# LetSQL


https://www.letsql.com/posts/candle-image-segmentation/

Making Deep Learning Workflows, Relational
Continuing our theme of built-in UDFs, we preview Segment Anything Model UDF as a foray into Rust’s ML capabilities with HuggingFace’s candle Library. We show how to integrate the SAM model into a relational workflow, enabling portability and complex data processing.




TL;DR
In this post, we build on top of Rust-based ML ecosystem, showing the capabilities of candle as performant integration with Python. We turn a PyTorch+pandas workflow with the Segment Anything Model (SAM) into relational operations; this enables portability and performance. We show 4x speed-ups over the external Pytorch/pandas based workflow. Finally, we explore pa.binary type for representing images.

Here is the code snippet that demonstrates the workflow:

    t.select(
        [
            "id",
            "sensitivity",
            sam_udf(str(model_path), t.image, [0.5, 0.6]).name("segmented"),
        ]
    )
    .filter([t.sensitivity >= 0.5])
    .limit(3)
    .cache(storage)
    .filter([_.segmented.iou_score > 0.5])
    .select([_.id, _.sensitivity, _.segmented.iou_score, _.segmented.mask])
)

You can drop into an IPython shell with the latest to try out the built-in UDFs.

nix run github:letsql/letsql

or check try out our latest experimental release here:

pip install letsql

Code example: GitHub repository

Get involved:

LETSQL GitHub repository
LETSQL Zulip chat
Introduction
Unstructured data, including text documents, videos, and images, constitutes an overwhelming 90% of the world’s information. Despite its prevalence, 95% of organizations need help to effectively manage and extract value from this data. This challenge is particularly acute when it comes to image data.

The complexity arises from several factors:

High Computational Demands: Extracting meaningful insights from images often relies on sophisticated AI and deep learning techniques. These computational methods require substantial processing power and resources, particularly when handling large-scale image datasets.
Complex Data Pipelines: Integrating images, often stored separately from structured data, into analytics workflows requires sophisticated data pipelines. These pipelines, which encompass complex Extract, Transform, Load (ETL) processes and extend into advanced Machine Learning Operations (MLOps) pipelines, are crucial in workflow management.
The impact of successfully integrating image analysis with traditional data processing could be transformative across various industries:

Drug Discovery: Deep learning approaches have contributed significantly to morphological profiling data analysis through segmenting cellular images, learning robust image representations, and integrating morphological data with other data modalities.

Manufacturing: In 3D printing, analyzing design images alongside production data could significantly improve the printing process, enhancing efficiency and quality.

These multi-modal workflows, involving images and tabular data, represent a frontier in data science. By addressing the challenges of unified data and image processing, organizations can unlock new insights and capabilities.

The Problem
To illustrate the problem and its solution, let’s explore a practical scenario involving a dataset where each record is associated with an image. This type of data structure is common in various fields, such as:

Patient medical records linked to CT scan images
3D printing projects with model data and design plans
In these scenarios, users typically need to perform three types of operations:

Selecting records based on specific criteria (e.g., patients over a certain age, 3D models with specific dimensions, or properties within a price range)
Applying ML inference to the images associated with the filtered results (e.g., tumor segmentation in CT scans, 3D model complexity analysis, or feature detection in real estate photos)
Integrating with relational databases or storage systems
Traditionally, these operations required separate systems or tools, one for data querying and another for image processing. This separation often leads to workflow inefficiencies and added complexity.

To illustrate this challenge, consider a table with multiple columns, including:

image: Contains image data in binary format

sensitivity: A value between 0 and 1

The task can be divided into multiple steps:

Filter the rows to include only those with a sensitivity above 0.5.

Select the first three rows from this filtered set.

Apply the Segment Anything Model to process the images from the selected rows.

(Bonus) Filter based on the IOU score.

Interlude: The Segment Anything Model
The Segment Anything Model (SAM) is an AI model developed by Meta that can identify and outline objects in images based on various types of prompts. It’s designed to be flexible and can segment any object in a photo when given a point, box, or text prompt. SAM can operate on various image segmentation tasks without additional training, making it a versatile tool for computer vision applications.



In the left image, the model receives a prompt (indicated by the red dot) and provides a mask that enables image segmentation (right). A SAM model generally returns a mask and the associated IOU score.

How to solve it today: pandas + PyTorch
Back to the problem at hand, we’ll show an example of how to solve it using Pandas and PyTorch, adapted from this tutorial.

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
            point_coords=input_point, point_labels=input_label, multimask_output=True
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
model_path = checkpoint
urllib.request.urlretrieve(SAM_MODEL_URL, model_path)

sam = sam_model_registry[model_type](checkpoint=checkpoint)
sam.to(device="cpu")
predictor = SamPredictor(sam)

t = t[t["sensitivity"] >= 0.5]
df = t.assign(segmented=pandas_sam_predict(predictor, t["image"], [0.5, 0.6]))[
["id", "sensitivity", "segmented"]
].head(3)

# https://stackoverflow.com/a/55355928/4001592
expanded_info = pd.json_normalize(df["segmented"])
df = pd.concat([df.drop("segmented", axis=1), expanded_info], axis=1)

df = df[df["iou_score"] > 0.5]

This code is good for the following reasons:

It’s easy to understand and write and shows the power of Python as a glue language.

It uses the Pandas DataFrame API, which is familiar to data scientists and analysts. It also shows an often neglected pattern of how people use pandas: they stick their objects in the columns (even if it is not performant).

It uses PyTorch, a popular deep-learning library, to run the image segmentation model.

However, it comes with some issues:

This is not very portable. If you want to run this code in a different environment, you will need to install the required libraries and make sure they are compatible with your system. This can be a significant barrier to entry for users who need to become more familiar with these libraries.

The code could be more efficient. The pandas_sam_predict function is called row-by-row with one Python function called for each image. This can be slow for large datasets and computationally expensive models.

The current implementation filters the dataset before applying the pandas_sam_predict function. While this order is correct, it’s not enforced structurally. A less careful developer could inadvertently reorder these operations, using segmentation to all images before filtering. This would waste significant computational resources on images that are ultimately discarded.

The solution: How does it work?


How it works
Our solution combines the operational machinery from database processing with the complex processing workflow, executing the model inference as UDFs.

The workflow is represented as a relational model, which allows for piping to other SQL engines and extensions and enables users to express complex data processing and machine learning tasks using familiar SQL-like syntax. This makes it more accessible to data analysts and scientists comfortable with database query languages.

Arrow, DataFusion, and Candle
In developing LETSQL, we adopt and bundle DataFusion as our query engine, teaching it to register expressions that can consume Arrow RecordBatch. These attributes position Rust as a compelling alternative to more established ecosystems in data analysis and machine learning.

Furthermore, DataFusion’s extensibility is a key asset. We showcase this flexibility by implementing a User Defined Function (UDF) that integrates the Candle machine learning library, demonstrating the seamless incorporation of advanced ML capabilities into our query engine. Candle is:

a minimalist ML framework for Rust focusing on performance (including GPU support) and ease of use.

We chose Candle for two key reasons, as explained in the library’s README:

It’s shipped as lightweight binaries designed for serverless computing. There’s no figuring out PyTorch deployment or Cluster. It can even run on WASM.
It is written in Rust, aligning with DataFusion, removing the need for Python overhead.
The Code
In this section, we demonstrate how such a pipeline could look like in a LETSQL pipeline. For it, we use the SAM model from the Candle project.

The setup and image loading are handled in the initial part of the code. The code then processes all images in the specified folder, converting them to binary data and organizing their IDs, names, and content into structured lists. Additionally, there are two functions: the built-in UDF sam_udf and the function get_blob, which transforms Apache Arrow binary data to store images within a table.

Binary data types and standards such as JPEG and PNG facilitate seamless image data transfer across Python and Rust. Alternatively, we could implement an Arrow ExtensionType to augment the binary image data with metadata. This approach would enable more sophisticated operations directly within the Arrow ecosystem, such as filtering images by format or other attributes.

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
def sam_udf(path: str, img: dt.binary, s: list[float]) -> dt.Struct({"mask": dt.Array[float], "iou_score": float}):
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

This section creates a PyArrow table from the processed image data and registers it with the name “images” within LETSQL. The code then downloads and saves a Segment Anything Model (SAM) for image segmentation.

table = pa.Table.from_arrays(
[
pa.array(ids),
pa.array(names),
pa.array([random.uniform(0.3, 1) for _ in range(5000)], type=pa.float64()),
pa.array(images, type=pa.binary()),
],
names=["id", "name", "sensitivity", "image"],
)

con = ls.connect()
t = con.register(table, table_name="images")

# download model
SAM_MODEL_URL = "https://storage.googleapis.com/letsql-assets/models/mobile_sam-tiny-vitt.safetensors"
model_path = "mobile_sam-tiny-vitt.safetensors"
urllib.request.urlretrieve(SAM_MODEL_URL, model_path)

This final section applies the image segmentation process to the filtering expression’s result. Using the sam_udf function on the image data creates an expression, utilizing the SAM model and the point [0.5, 0.6] as a seed.

It is important to note that this is a demonstration; in a real-world scenario, you must provide actual positive example points for accurate segmentation.

For the bonus task, we push the data to a DuckDB (via a SourceStorage cache) to take advantage of its capabilities for nested data manipulation.

storage = SourceStorage(source=ls.duckdb.connect())
expr = (
t.select(
[
"id",
"sensitivity",
sam_udf(str(model_path), t.image, [0.5, 0.6]).name("segmented"),
]
)
.filter([t.sensitivity >= 0.5])
.limit(3)
.cache(storage)
.filter([_.segmented.iou_score > 0.5])
.select([_.id, _.sensitivity, _.segmented.iou_score, _.segmented.mask])
)

result = expr.execute()

A (micro) benchmark
A exploratory benchmark reveals that the proposed approach is approximately 4 times faster1 than the pandas + PyTorch alternative:

%timeit -n 5 -r 5 letsql_workflow()
37.5 s ± 185 ms per loop (mean ± std. dev. of 5 runs, 5 loops each)

%timeit -n 5 -r 5 pandas_workflow()
2min 22s ± 1.23 s per loop (mean ± std. dev. of 5 runs, 5 loops each)

The code to reproduce such timings is the following:

def to_measure(table):
table = table[table["sensitivity"] >= 0.5]
df = table.assign(segmented=pandas_sam_predict(predictor, table["image"], [0.5, 0.6]))[
["id", "sensitivity", "segmented"]
].head(3)
# https://stackoverflow.com/a/55355928/4001592
expanded_info = pd.json_normalize(df["segmented"])
df = pd.concat([df.drop("segmented", axis=1), expanded_info], axis=1)
df = df[df["iou_score"] >= 0.8]
return df

pandas_workflow = partial(to_measure, t)

expr = (
t.select(
[
"id",
"sensitivity",
sam_udf(str(model_path), t.image, [0.5, 0.6]).name("segmented"),
]
)
.filter([t.sensitivity >= 0.5])
.limit(3)
.filter([_.segmented.iou_score > 0.5])
.select([_.id, _.sensitivity, _.segmented.iou_score, _.segmented.mask])
)

letsql_workflow = lambda : expr.execute()

The results are merely preliminary, and accurate benchmarking is challenging, so take these numbers with a grain of salt. Nevertheless, the benchmark provides a general idea of the relative performance of both approaches.

The complete code example is on GitHub.

Benefits
High-Performance Integration
Seamless integration with the Rust-based candle deep learning package
Enables close-to-metal compute capabilities
Enhanced Portability
The workflow is a relational model that can be piped to other SQL engines and extended as conceptualized with Ibis.
There is no need to install Python packages like PyTorch or others since the UDF is integrated with Rust, and the engine is shipped as a portable Rust binary.
Allows for database-style optimizations.
Versatile Data Representation:
Utilizes Arrow Data Types.
Supports both Array and non-tabular data structures.
Conclusion
By unlocking Rust and adopting DataFusion’s execution engine for UDFs, we can integrate with performant Rust-based Deep Learning libraries. We have shown a promising approach to handling complex data like images and combining the safer execution offered by database machinery with deep learning inference. Beyond enabling expressive DSL in Python to build portable pipelines, this approach automatically enables database-style optimizations like projection pushdown. We also show the viability of using pa.binary to allow data to transport with Arrow.

Future Work
Our future development plans focus on enhancing the integration between LETSQL (based on DataFusion) and Candle while expanding support for additional libraries and data formats. This effort aims to improve our system’s overall composability and extensibility.

One promising avenue we’re exploring is using structured and semi-structured data in generative AI applications. Candle’s robust support for cutting-edge Large Language Models (LLMs), demonstrated in their extensive examples, makes this exciting prospect possible. Further, the benefits of this approach can be realized by using GPUs for inference tasks.

On Data Types, we’d like to understand further the benefits of creating more precise types using Arrow Extension type for images as shown by LanceDB

We’re considering extending LETSQL to incorporate various image processing functions to broaden our capabilities in handling image data. These functions would be sourced from the Rust ecosystem’s image crate, enabling more comprehensive image manipulation and analysis within our framework.

Finally, we would also like to explore integration with Daft as a multi-engine option for processing on images.

Get Involved
You can get involved by engaging with us on GitHub or chatting with us on Zulip. We would like your feedback on the built-in UDFs and how they can be improved to serve your needs better. We are also looking for contributors who are interested in helping us build the ecosystem around the built-in UDFs. We are excited about this feature’s potential and its impact on the data community.

Appendix and References
Image source: COCO Dataset 2017 validation images

Key technologies:

Segment Anything Model (SAM)
Candle
DataFusion
Apache Arrow
Related LETSQL blog posts:

XGBoost integration
Multi-engine data stacks
Code example: GitHub repository

Get involved:

LETSQL GitHub repository
LETSQL Zulip chat
Additional resources:

DuckDB JSON handling
LanceDB image tensor types
Daft image processing capabilities
Footnotes
The benchmark is run on 16 core, 32GB laptop running 13th Gen Intel(R) Core(TM) i5-1340P↩︎