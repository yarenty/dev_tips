# smolagents

https://huggingface.co/blog/smolagents


smolagents is a library that enables you to run powerful agents in a few lines of code. It offers:

✨ Simplicity: the logic for agents fits in ~1,000 lines of code (see agents.py). We kept abstractions to their minimal shape above raw code!

🧑‍💻 First-class support for Code Agents. Our CodeAgent writes its actions in code (as opposed to "agents being used to write code"). To make it secure, we support executing in sandboxed environments via E2B or via Docker.

🤗 Hub integrations: you can share/pull tools or agents to/from the Hub for instant sharing of the most efficient agents!

🌐 Model-agnostic: smolagents supports any LLM. It can be a local transformers or ollama model, one of many providers on the Hub, or any model from OpenAI, Anthropic and many others via our LiteLLM integration.

👁️ Modality-agnostic: Agents support text, vision, video, even audio inputs! Cf this tutorial for vision.

🛠️ Tool-agnostic: you can use tools from LangChain, MCP, you can even use a Hub Space as a tool.





