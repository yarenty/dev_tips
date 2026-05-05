---
title: Test the function
main_link: https://huggingface.co/JetBrains/Mellum-4b-base
keywords: [jetbrains-mellum, models, fill, middle]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Test the function

**Main link:** <https://huggingface.co/JetBrains/Mellum-4b-base>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[deepseek]] — Deepseek _(score 27.5)_
- [[strawberry]] — Strawberry _(score 27.5)_
- [[olmo]] — OLMo - Open Language Models _(score 27.5)_
- [[mplug]] — mPLUG _(score 27.5)_
- [[llama_2]] — LLAMA _(score 27.5)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#jetbrains-mellum` `#models` `#llm` `#ml` `#fill` `#middle` `#sample` `#jetbrains`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->


https://huggingface.co/JetBrains/Mellum-4b-base

Syntax-Aware Fill-in-the-Middle (SAFIM)

Sample Usage
Here are examples of how to run and sample from the model.

Generic generaion
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
"Too few arguments. You should provide: \n1. dataset_filename" +
"\n2. output_data_filename"
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
        "Too few arguments. You should provide: \n1. dataset_filename" +
        "\n2. output_data_filename"
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
Fill in the middle with additional files as context generation
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
