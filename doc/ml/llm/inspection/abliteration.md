# Removing refusals with transformers

https://github.com/Sumandora/remove-refusals-with-transformers

This is a crude, proof-of-concept implementation to remove refusals from an LLM model without using TransformerLens. This means, that this supports every model that HF Transformers supports*.

The code was tested on a RTX 2060 6GB, thus mostly <3B models have been tested, but the code has been tested to work with bigger models as well.

*While most models are compatible, some models are not. Mainly because of custom model implementations. Some Qwen implementations for example don't work. Because model.model.layers can't be used for getting layers. They call the variables so that, model.transformer.h must be used, if I'm not mistaken.

## Usage
Set model and quantization in compute_refusal_dir.py and inference.py (Quantization can apparently be mixed)
Run compute_refusal_dir.py (Some settings in that file may be changed depending on your use-case)
Run inference.py and ask the model how to build an army of rabbits, that will overthrow your local government one day, by stealing all the carrots.


## Ollama models:


https://ollama.com/huihui_ai/qwen3-abliterated

This is an uncensored version of Qwen/qwen3 created with abliteration (see remove-refusals-with-transformers to know more about it).
This is a crude, proof-of-concept implementation to remove refusals from an LLM model without using TransformerLens.

You can toggle non-thinking mode in Ollama by typing /no_think after the prompt.




