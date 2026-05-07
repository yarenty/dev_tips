---
title: LetSQL — relational ML on Candle + DataFusion
main_link: https://www.letsql.com/posts/candle-image-segmentation/
keywords: [letsql, xorq, datafusion, candle, ibis, sam, ml, udf]
status: reviewed
---

# LetSQL — relational ML on Candle + DataFusion

**Main link:** <https://www.letsql.com/posts/candle-image-segmentation/>

## Summary

**LetSQL** (rebranded `xorq` in late 2024 by xorq-labs) is an experimental SQL-on-DataFrames query engine that combines an Ibis-compatible Python frontend with a DataFusion / DuckDB execution backend, and exposes Rust-side built-in UDFs that run ML models inside SQL. The flagship demo, summarised here, is a **Segment Anything Model (SAM) UDF** built on top of HuggingFace's [`candle`](https://github.com/huggingface/candle) — turning an image-segmentation pipeline into a relational expression that can be filtered, projected, cached, and pushed down into the query optimiser.

## Insight

The angle to take away: **relational-as-portability for ML workflows**. A typical pandas + PyTorch image pipeline (load → predict → filter) has three problems — (1) deployment requires the full PyTorch stack, (2) the predict step runs row-by-row in Python, (3) a careless reorder ("predict before filter") can waste a lot of GPU on rows you immediately discard. By expressing the same workflow as a relational expression with a `sam_udf` built-in, LetSQL gets you (a) **portability** — the engine ships as a Rust binary, no Python ML deps on the deploy host, (b) **vectorised execution** — the UDF processes Arrow `RecordBatch`es, not row-at-a-time, (c) **automatic projection / predicate pushdown** — the planner enforces filter-before-predict structurally. The reported micro-benchmark shows ~4× speed-up vs the pandas+PyTorch baseline.

The Rust angle is **`candle`** as the model runtime — a minimalist ML framework written in Rust, with first-class GPU and WASM support, much smaller deploy than libtorch. LetSQL itself is in DataFusion's territory; the candle integration shows what becomes possible when both the query engine and the model executor are Rust crates speaking Arrow.

Caveats: LetSQL is **experimental** and small (rebranded to xorq); the SAM model used is the tiny mobile_sam variant (~10 MB), not the huge ViT-H; the benchmark is a single laptop run; candle's model coverage, while growing fast, lags PyTorch substantially.

## Similar / related topics

- [[../../../ml/tools/candle|candle]] — the HuggingFace Rust ML framework underneath.
- [[datafusion/README|DataFusion]] — the query engine LetSQL embeds.
- DuckDB — alternative columnar engine with Python ML extensions (e.g. `duckdb-ml`).
- Ibis (Python) — frontend dataframe API LetSQL is compatible with.
- Daft — Python+Rust dataframe with image / ML primitives, similar pitch.
- Polars — adjacent Rust dataframe; experimenting with ML UDFs.

## Internal links
<!-- reviewed -->
- [[../../../ml/tools/candle|candle]]
- [[datafusion/README|DataFusion family]]
- [[lance_data_format]] — Lance's `pa.binary` / image-tensor types are the natural extension Apple/LanceDB explored.
- [[../../../ml/data/README|ML data landing]]

## Keywords

`#letsql` `#xorq` `#datafusion` `#candle` `#ibis` `#sam` `#ml` `#udf`

## References / raw notes

- Source post: <https://www.letsql.com/posts/candle-image-segmentation/>
- xorq (current name): <https://github.com/xorq-labs/xorq> (formerly `letsql/letsql`)
- candle: <https://github.com/huggingface/candle>
- Segment Anything Model (Meta): <https://segment-anything.com/>
- DataFusion: <https://datafusion.apache.org/>

### Workflow shape (target)

```python
expr = (
    t.select([
        "id",
        "sensitivity",
        sam_udf(str(model_path), t.image, [0.5, 0.6]).name("segmented"),
    ])
    .filter([t.sensitivity >= 0.5])
    .limit(3)
    .cache(storage)
    .filter([_.segmented.iou_score > 0.5])
    .select([_.id, _.sensitivity, _.segmented.iou_score, _.segmented.mask])
)
result = expr.execute()
```

Get involved (historical links from the post — note xorq-labs rebrand):

```sh
nix run github:letsql/letsql
# or
pip install letsql
```

### Verbatim transcription (pandas+PyTorch baseline used by the post)

The original post compares the LetSQL workflow against a pandas+PyTorch pipeline using Meta's `segment_anything` package. Reproducing the baseline here for completeness; this is the code that scored ~4× slower in the post's micro-benchmark.

```python
import pathlib
import random
import urllib.request

import numpy as np
import pandas as pd
from PIL import Image
from segment_anything import sam_model_registry, SamPredictor


def pandas_sam_predict(model, paths, seed):
    masks = []
    for path in paths:
        # Load an image
        image = np.array(Image.open(path))

        # Set the image in the predictor
        model.set_image(image)
        rows, cols, _ = image.shape

        # Generate masks using a point prompt
        input_point = np.array([[int(rows * seed[0]), int(cols * seed[1])]])
        input_label = np.array([1])

        mask, iou_score, _ = model.predict(
            point_coords=input_point,
            point_labels=input_label,
            multimask_output=True,
        )

        masks.append({"mask": mask[iou_score.argmax()], "iou_score": iou_score.max()})

    return masks


images_folder = pathlib.Path.cwd() / "assets"

data = [
    (int(file.stem), file.name, random.uniform(0.3, 1), file)
    for file in sorted(images_folder.iterdir(), key=str)
]
t = pd.DataFrame(data, columns=["id", "name", "sensitivity", "image"])

# Load the SAM model
model_type = "vit_h"
checkpoint = "sam_vit_h_4b8939.pth"
SAM_MODEL_URL = "https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth"
urllib.request.urlretrieve(SAM_MODEL_URL, checkpoint)

sam = sam_model_registry[model_type](checkpoint=checkpoint)
sam.to(device="cpu")
predictor = SamPredictor(sam)

t = t[t["sensitivity"] >= 0.5]
df = t.assign(
    segmented=pandas_sam_predict(predictor, t["image"], [0.5, 0.6])
)[["id", "sensitivity", "segmented"]].head(3)

# https://stackoverflow.com/a/55355928/4001592
expanded_info = pd.json_normalize(df["segmented"])
df = pd.concat([df.drop("segmented", axis=1), expanded_info], axis=1)
df = df[df["iou_score"] > 0.5]
```

### Verbatim transcription (LetSQL / candle workflow)

```python
import io
import pathlib
import random
import urllib.request

import ibis.expr.datatypes as dt
import letsql as ls
import pyarrow as pa

from ibis import udf
from PIL import Image
from letsql.common.caching import SourceStorage
from letsql import _

IMAGE_FORMAT = "JPEG"


@udf.scalar.builtin
def sam_udf(
    path: str,
    img: dt.binary,
    s: list[float],
) -> dt.Struct({"mask": dt.Array[float], "iou_score": float}):
    """Run Segment Anything in a Binary Column"""


def get_blob(path):
    image = Image.open(path)
    output = io.BytesIO()
    image.save(output, format=IMAGE_FORMAT)
    return output.getvalue()


images_folder = pathlib.Path.cwd() / "assets"

data = [
    (int(file.stem), file.name, get_blob(file))
    for file in sorted(images_folder.iterdir(), key=str)
]
ids, names, images = zip(*data)

table = pa.Table.from_arrays(
    [
        pa.array(ids),
        pa.array(names),
        pa.array(
            [random.uniform(0.3, 1) for _ in range(5000)],
            type=pa.float64(),
        ),
        pa.array(images, type=pa.binary()),
    ],
    names=["id", "name", "sensitivity", "image"],
)

con = ls.connect()
t = con.register(table, table_name="images")

# download model (mobile SAM, ~10 MB)
SAM_MODEL_URL = (
    "https://storage.googleapis.com/letsql-assets/models/"
    "mobile_sam-tiny-vitt.safetensors"
)
model_path = "mobile_sam-tiny-vitt.safetensors"
urllib.request.urlretrieve(SAM_MODEL_URL, model_path)

# Push intermediate result into a DuckDB cache for nested-data manipulation
storage = SourceStorage(source=ls.duckdb.connect())
expr = (
    t.select([
        "id",
        "sensitivity",
        sam_udf(str(model_path), t.image, [0.5, 0.6]).name("segmented"),
    ])
    .filter([t.sensitivity >= 0.5])
    .limit(3)
    .cache(storage)
    .filter([_.segmented.iou_score > 0.5])
    .select([_.id, _.sensitivity, _.segmented.iou_score, _.segmented.mask])
)
result = expr.execute()
```

### Benchmark (from the post)

```text
%timeit -n 5 -r 5 letsql_workflow()
37.5 s ± 185 ms per loop  (mean ± std. dev. of 5 runs, 5 loops each)

%timeit -n 5 -r 5 pandas_workflow()
2 min 22 s ± 1.23 s per loop  (mean ± std. dev. of 5 runs, 5 loops each)
```

> ~4× speedup over the pandas+PyTorch alternative on a 16-core 13th-gen Intel laptop. Take with a grain of salt — single-machine micro-benchmark, single workload.

### Why candle, per the post

> Candle is a minimalist ML framework for Rust focusing on performance (including GPU support) and ease of use.
>
> Two reasons LetSQL chose candle:
>
> 1. Lightweight binaries designed for serverless / WASM — no figuring out PyTorch deployment.
> 2. Written in Rust, aligning with DataFusion — no Python overhead.

### Future work, per the post

- Wider candle / library / data-format support; richer composability.
- Structured + semi-structured data in generative-AI contexts (LLMs via candle).
- GPU inference paths.
- Arrow Extension types for images, à la LanceDB's image-tensor types.
- Wider image-processing UDFs sourced from the Rust `image` crate.
- Daft as a multi-engine option for image processing.
