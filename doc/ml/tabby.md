# TabbyML

https://github.com/TabbyML/tabby


Tabby is a self-hosted AI coding assistant, offering an open-source and on-premises alternative to GitHub Copilot. It boasts several key features:

Self-contained, with no need for a DBMS or cloud service.
OpenAPI interface, easy to integrate with existing infrastructure (e.g Cloud IDE).
Supports consumer-grade GPUs.
Open in Playground

Demo

🔥 What's New
04/22/2024 v0.10.0 released, featuring the latest Reports tab with team-wise analytics for Tabby usage.
04/19/2024 📣 Tabby now incorporates locally relevant snippets(declarations from local LSP, and recently modified code) for code completion!
04/17/2024 CodeGemma and CodeQwen model series have now been added to the official registry!
Archived
👋 Getting Started
You can find our documentation here.

📚 Installation
💻 IDE/Editor Extensions
⚙️ Configuration
Run Tabby in 1 Minute
The easiest way to start a Tabby server is by using the following Docker command:

docker run -it \
--gpus all -p 8080:8080 -v $HOME/.tabby:/data \
tabbyml/tabby \
serve --model TabbyML/StarCoder-1B --device cuda
For additional options (e.g inference type, parallelism), please refer to the documentation page.

🤝 Contributing

