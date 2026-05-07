---
title: JetBrains Mellum-4B-base — coding fill-in-the-middle model
main_link: https://huggingface.co/JetBrains/Mellum-4b-base
keywords: [jetbrains-mellum, mellum, fill-in-the-middle, fim, coding, jetbrains, ai-assistant, junie]
status: reviewed
review_date: 2026/05/03
---

# JetBrains Mellum-4B-base — coding fill-in-the-middle model

**Main link:** <https://huggingface.co/JetBrains/Mellum-4b-base>

## Summary

Mellum-4B-base is JetBrains' open-weights, code-specialised LLM tuned for **fill-in-the-middle (FIM)** completion — the canonical task for IDE inline-suggestion. It's a 4B-parameter model trained with a SAFIM (Syntax-Aware FIM) objective, so it has a strong sense of where in the syntactic structure a completion is happening (function body vs argument list vs class scope), and accepts cross-file context via `<filename>` markers in the prompt. JetBrains released the base weights to Hugging Face in 2024-2025 as part of their broader AI Assistant / Junie agent strategy; Mellum is the local-first inline-completion engine, distinct from the cloud-only Junie agent and AI Assistant chat features.

## Insight

Mellum is the JetBrains-flavoured answer to a crowded niche: open-weights coding models small enough (4B) to run locally with reasonable latency for inline suggestions. The competitive landscape is intense — **DeepSeek-Coder-V2-Lite** (16B/2.4B-active MoE), **Codestral / Codestral-Mamba** (Mistral, 22B/7B), **Qwen2.5-Coder** (1.5B/7B/14B/32B), **StarCoder2** (3B/7B/15B by BigCode) — but Mellum's value-add is the **SAFIM training objective** and the **`<filename>` / `<fim_prefix>` / `<fim_suffix>` / `<fim_middle>` prompt format** that's already wired up for multi-file IDE context, which most of the alternatives only support via after-the-fact prompt engineering.

The strategic story is JetBrains hedging: they ship AI Assistant (cloud, multi-provider chat), Junie (cloud agent for multi-file edits), and Mellum (local FIM completion) as a portfolio. The base model is `Mellum-4b-base` — for production inline completion you'd typically pair it with a per-language fine-tune; HuggingFace lists JetBrains-published variants for Python and a few others. Don't reach for Mellum if you want a chat / agent / reasoning model — it's a base-model FIM completer, not instruction-tuned.

## Similar / related topics

- DeepSeek-Coder-V2-Lite — strong open-weights coder; MoE-active 2.4B is competitive with dense 7B coders.
- Codestral / Codestral-Mamba (Mistral) — 22B / 7B coder; Mamba variant has different latency profile.
- Qwen2.5-Coder — Alibaba's coder family; small sizes (1.5B, 7B) dominate single-GPU coding benchmarks in 2025.
- StarCoder2 (BigCode) — 3B/7B/15B; the canonical "open dataset + open weights" coder line.
- TabbyML — self-hosted coding-completion server; can serve Mellum / StarCoder2 / DeepSeek-Coder.

## Internal links
- [[README|llm/models]]
- [[../README|llm]]
- [[../runtimes/README|llm/runtimes]] — Ollama / vLLM / TabbyML for serving.
- [[../../tools/README|ml/tools]]
- [[llama]] — see Code Llama section for the older equivalent.

## Keywords

`#jetbrains-mellum` `#mellum` `#fill-in-the-middle` `#fim` `#coding` `#jetbrains` `#ai-assistant` `#junie`

## References / raw notes

### Model card

- HF model: <https://huggingface.co/JetBrains/Mellum-4b-base>
- JetBrains org on HF: <https://huggingface.co/JetBrains>

Trained with **Syntax-Aware Fill-in-the-Middle (SAFIM)**.

### Sample usage

#### Generic generation

```python
from transformers import AutoTokenizer, AutoModelForCausalLM

example = """
import sys
import os
import time

sys.path.append(os.getcwd())

from cluster.prepare_data import get_headers_pairs_list, write_dist_matrix
from cluster.token_edit_distance import get_distance_matrix

if len(sys.argv) < 3:
    print(
        "Too few arguments. You should provide: \\n1. dataset_filename" +
        "\\n2. output_data_filename"
    )
    sys.exit()

start = time.perf_counter()
dataset_filename_ = sys.argv[1]
output_data_filename_ = sys.argv[2]

headers_pairs = get_headers_pairs_list(dataset_filename_, verbose=True)

dist_matrix, max_dist = get_distance_matrix(
    list(map(lambda x: x[1], headers_pairs)),
    verbose=True
)

write_dist_matrix(dist_matrix, max_dist, output_data_filename_, verbose=True)

end = time.perf_counter()
"""

tokenizer = AutoTokenizer.from_pretrained('JetBrains/Mellum-4b-base')
model = AutoModelForCausalLM.from_pretrained('JetBrains/Mellum-4b-base')

encoded_input = tokenizer(example, return_tensors='pt', return_token_type_ids=False)
input_len = len(encoded_input["input_ids"][0])
out = model.generate(
    **encoded_input,
    max_new_tokens=100,
)
print("### Context")
print(tokenizer.decode(out[0][:input_len]))
print("### Prediction")
print(tokenizer.decode(out[0][input_len:]))
```

#### Fill-in-the-middle with additional files as context

```python
example = """<filename>utils.py
def multiply(x, y):
    return x * y
<filename>config.py
DEBUG = True
MAX_VALUE = 100
<filename>example.py
<fim_suffix>

# Test the function
result = calculate_sum(5, 10)
print(result)<fim_prefix>def calculate_sum(a, b):
<fim_middle>"""

encoded_input = tokenizer(example, return_tensors='pt', return_token_type_ids=False)
out = model.generate(
    **encoded_input,
    max_new_tokens=100,
)
```

The `<filename>...` markers let you give the model cross-file context; `<fim_prefix>` / `<fim_suffix>` / `<fim_middle>` are the standard FIM control tokens with the position-of-cursor convention reversed from the model's text order (suffix appears first in the prompt because that's what the SAFIM training objective expects).
