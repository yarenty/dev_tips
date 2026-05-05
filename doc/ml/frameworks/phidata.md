# PhiData


https://github.com/phidatahq/phidata/blob/main/README.md


![](https://private-user-images.githubusercontent.com/22579644/322947450-295187f6-ac9d-41e0-abdb-38e3291ad1d1.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTU3NjI3MzgsIm5iZiI6MTcxNTc2MjQzOCwicGF0aCI6Ii8yMjU3OTY0NC8zMjI5NDc0NTAtMjk1MTg3ZjYtYWM5ZC00MWUwLWFiZGItMzhlMzI5MWFkMWQxLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA1MTUlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNTE1VDA4NDAzOFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTBkNzJjMDliNDcyYTI1ODY5YTU3MTI5M2JjN2EyYzliYjJlNGQxNmUwYmQ3OTg3NDUxOTQzOTRiZmI5Yjc3YjMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.eCxLbFybMxt6s6_MhlTI_-NE2JrjAop2MeM_Bzpo7Po)

What is phidata?
Phidata is a framework for building Autonomous Assistants (i.e. Agents) using LLMs.

Why phidata?
Problem: LLMs have limited context and cannot take actions.

Solution: Add memory, knowledge and tools.

Memory: Stores chat history in a database and enables LLMs to have long-term conversations.
Knowledge: Stores information in a vector database and provides LLMs with business context.
Tools: Enable LLMs to take actions like pulling data from an API, sending emails or querying a database.
How it works
Step 1: Create an Assistant
Step 2: Add Tools (functions), Knowledge (vectordb) and Storage (database)
Step 3: Serve using Streamlit, FastApi or Django to build your AI application
Installation
pip install -U phidata
Quickstart: Assistant that can search the web
Create a file assistant.py

from phi.assistant import Assistant
from phi.tools.duckduckgo import DuckDuckGo

assistant = Assistant(tools=[DuckDuckGo()], show_tool_calls=True)
assistant.print_response("Whats happening in France?", markdown=True)
Install libraries, export your OPENAI_API_KEY and run the Assistant

pip install openai duckduckgo-search

export OPENAI_API_KEY=sk-xxxx

python assistant.py
Documentation and Support
Read the docs at docs.phidata.com
Chat with us on discord
Examples
LLM OS: Using LLMs as the CPU for an emerging Operating System.
Autonomous RAG: Gives LLMs tools to search its knowledge, web or chat history.
Local RAG: Fully local RAG with Ollama and PgVector.
Investment Researcher: Generate investment reports on stocks using Llama3 and Groq.
News Articles: Write News Articles using Llama3 and Groq.
Video Summaries: YouTube video summaries using Llama3 and Groq.
Research Assistant: Write research reports using Llama3 and Groq.