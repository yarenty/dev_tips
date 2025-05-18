# MCP


## MCP USE 

🌐 MCP-Use is the open source way to connect any LLM to any MCP server and build custom agents that have tool access, without using closed source or application clients.

💡 Let developers easily connect any LLM to tools like web browsing, file operations, and more.

https://github.com/mcp-use/mcp-use






# Features

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| 🔄 [**Ease of use**](#quick-start) | Create your first MCP capable agent you need only 6 lines of code |
| 🤖 [**LLM Flexibility**](#installing-langchain-providers) | Works with any langchain supported LLM that supports tool calling (OpenAI, Anthropic, Groq, LLama etc.) |
| 🌐 [**Code Builder**](https://mcp-use.io/builder) | Explore MCP capabilities and generate starter code with the interactive [code builder](https://mcp-use.io/builder). |
| 🔗 [**HTTP Support**](#http-connection-example) | Direct connection to MCP servers running on specific HTTP ports |
| ⚙️ [**Dynamic Server Selection**](#dynamic-server-selection-server-manager) | Agents can dynamically choose the most appropriate MCP server for a given task from the available pool |
| 🧩 [**Multi-Server Support**](#multi-server-support) | Use multiple MCP servers simultaneously in a single agent |
| 🛡️ [**Tool Restrictions**](#tool-access-control) | Restrict potentially dangerous tools like file system or network access |
| 🔧 [**Custom Agents**](#build-a-custom-agent) | Build your own agents with any framework using the LangChain adapter or create new adapters |


# Quick start

With pip:

```bash
pip install mcp-use
```

Or install from source:

```bash
git clone https://github.com/pietrozullo/mcp-use.git
cd mcp-use
pip install -e .
```

### Installing LangChain Providers

mcp_use works with various LLM providers through LangChain. You'll need to install the appropriate LangChain provider package for your chosen LLM. For example:

```bash
# For OpenAI
pip install langchain-openai

# For Anthropic
pip install langchain-anthropic

# For other providers, check the [LangChain chat models documentation](https://python.langchain.com/docs/integrations/chat/)
```

and add your API keys for the provider you want to use to your `.env` file.

```bash
OPENAI_API_KEY=
ANTHROPIC_API_KEY=
```

> **Important**: Only models with tool calling capabilities can be used with mcp_use. Make sure your chosen model supports function calling or tool use.

### Spin up your agent:

```python
import asyncio
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from mcp_use import MCPAgent, MCPClient

async def main():
    # Load environment variables
    load_dotenv()

    # Create configuration dictionary
    config = {
      "mcpServers": {
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest"],
          "env": {
            "DISPLAY": ":1"
          }
        }
      }
    }

    # Create MCPClient from configuration dictionary
    client = MCPClient.from_dict(config)

    # Create LLM
    llm = ChatOpenAI(model="gpt-4o")

    # Create agent with the client
    agent = MCPAgent(llm=llm, client=client, max_steps=30)

    # Run the query
    result = await agent.run(
        "Find the best restaurant in San Francisco",
    )
    print(f"\nResult: {result}")

if __name__ == "__main__":
    asyncio.run(main())
```

You can also add the servers configuration from a config file like this:

```python
client = MCPClient.from_config_file(
        os.path.join("browser_mcp.json")
    )
```

Example configuration file (`browser_mcp.json`):

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"],
      "env": {
        "DISPLAY": ":1"
      }
    }
  }
}
```

For other settings, models, and more, check out the documentation.


# Example Use Cases

## Web Browsing with Playwright

```python
import asyncio
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from mcp_use import MCPAgent, MCPClient

async def main():
    # Load environment variables
    load_dotenv()

    # Create MCPClient from config file
    client = MCPClient.from_config_file(
        os.path.join(os.path.dirname(__file__), "browser_mcp.json")
    )

    # Create LLM
    llm = ChatOpenAI(model="gpt-4o")
    # Alternative models:
    # llm = ChatAnthropic(model="claude-3-5-sonnet-20240620")
    # llm = ChatGroq(model="llama3-8b-8192")

    # Create agent with the client
    agent = MCPAgent(llm=llm, client=client, max_steps=30)

    # Run the query
    result = await agent.run(
        "Find the best restaurant in San Francisco USING GOOGLE SEARCH",
        max_steps=30,
    )
    print(f"\nResult: {result}")

if __name__ == "__main__":
    asyncio.run(main())
```

## Airbnb Search

```python
import asyncio
import os
from dotenv import load_dotenv
from langchain_anthropic import ChatAnthropic
from mcp_use import MCPAgent, MCPClient

async def run_airbnb_example():
    # Load environment variables
    load_dotenv()

    # Create MCPClient with Airbnb configuration
    client = MCPClient.from_config_file(
        os.path.join(os.path.dirname(__file__), "airbnb_mcp.json")
    )

    # Create LLM - you can choose between different models
    llm = ChatAnthropic(model="claude-3-5-sonnet-20240620")

    # Create agent with the client
    agent = MCPAgent(llm=llm, client=client, max_steps=30)

    try:
        # Run a query to search for accommodations
        result = await agent.run(
            "Find me a nice place to stay in Barcelona for 2 adults "
            "for a week in August. I prefer places with a pool and "
            "good reviews. Show me the top 3 options.",
            max_steps=30,
        )
        print(f"\nResult: {result}")
    finally:
        # Ensure we clean up resources properly
        if client.sessions:
            await client.close_all_sessions()

if __name__ == "__main__":
    asyncio.run(run_airbnb_example())
```

Example configuration file (`airbnb_mcp.json`):

```json
{
  "mcpServers": {
    "airbnb": {
      "command": "npx",
      "args": ["-y", "@openbnb/mcp-server-airbnb"]
    }
  }
}
```

## Blender 3D Creation

```python
import asyncio
from dotenv import load_dotenv
from langchain_anthropic import ChatAnthropic
from mcp_use import MCPAgent, MCPClient

async def run_blender_example():
    # Load environment variables
    load_dotenv()

    # Create MCPClient with Blender MCP configuration
    config = {"mcpServers": {"blender": {"command": "uvx", "args": ["blender-mcp"]}}}
    client = MCPClient.from_dict(config)

    # Create LLM
    llm = ChatAnthropic(model="claude-3-5-sonnet-20240620")

    # Create agent with the client
    agent = MCPAgent(llm=llm, client=client, max_steps=30)

    try:
        # Run the query
        result = await agent.run(
            "Create an inflatable cube with soft material and a plane as ground.",
            max_steps=30,
        )
        print(f"\nResult: {result}")
    finally:
        # Ensure we clean up resources properly
        if client.sessions:
            await client.close_all_sessions()

if __name__ == "__main__":
    asyncio.run(run_blender_example())
```

# Configuration File Support

MCP-Use supports initialization from configuration files, making it easy to manage and switch between different MCP server setups:

```python
import asyncio
from mcp_use import create_session_from_config

async def main():
    # Create an MCP session from a config file
    session = create_session_from_config("mcp-config.json")

    # Initialize the session
    await session.initialize()

    # Use the session...

    # Disconnect when done
    await session.disconnect()

if __name__ == "__main__":
    asyncio.run(main())
```

## HTTP Connection Example

MCP-Use now supports HTTP connections, allowing you to connect to MCP servers running on specific HTTP ports. This feature is particularly useful for integrating with web-based MCP servers.

Here's an example of how to use the HTTP connection feature:

```python
import asyncio
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from mcp_use import MCPAgent, MCPClient

async def main():
    """Run the example using a configuration file."""
    # Load environment variables
    load_dotenv()

    config = {
        "mcpServers": {
            "http": {
                "url": "http://localhost:8931/sse"
            }
        }
    }

    # Create MCPClient from config file
    client = MCPClient.from_dict(config)

    # Create LLM
    llm = ChatOpenAI(model="gpt-4o")

    # Create agent with the client
    agent = MCPAgent(llm=llm, client=client, max_steps=30)

    # Run the query
    result = await agent.run(
        "Find the best restaurant in San Francisco USING GOOGLE SEARCH",
        max_steps=30,
    )
    print(f"\nResult: {result}")

if __name__ == "__main__":
    # Run the appropriate example
    asyncio.run(main())
```

This example demonstrates how to connect to an MCP server running on a specific HTTP port. Make sure to start your MCP server before running this example.

# Multi-Server Support

MCP-Use allows configuring and connecting to multiple MCP servers simultaneously using the `MCPClient`. This enables complex workflows that require tools from different servers, such as web browsing combined with file operations or 3D modeling.

## Configuration

You can configure multiple servers in your configuration file:

```json
{
  "mcpServers": {
    "airbnb": {
      "command": "npx",
      "args": ["-y", "@openbnb/mcp-server-airbnb", "--ignore-robots-txt"]
    },
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"],
      "env": {
        "DISPLAY": ":1"
      }
    }
  }
}
```

## Usage

The `MCPClient` class provides methods for managing connections to multiple servers. When creating an `MCPAgent`, you can provide an `MCPClient` configured with multiple servers.

By default, the agent will have access to tools from all configured servers. If you need to target a specific server for a particular task, you can specify the `server_name` when calling the `agent.run()` method.

```python
# Example: Manually selecting a server for a specific task
result = await agent.run(
    "Search for Airbnb listings in Barcelona",
    server_name="airbnb" # Explicitly use the airbnb server
)

result_google = await agent.run(
    "Find restaurants near the first result using Google Search",
    server_name="playwright" # Explicitly use the playwright server
)
```

## Dynamic Server Selection (Server Manager)

For enhanced efficiency and to reduce potential agent confusion when dealing with many tools from different servers, you can enable the Server Manager by setting `use_server_manager=True` during `MCPAgent` initialization.

When enabled, the agent intelligently selects the correct MCP server based on the tool chosen by the LLM for a specific step. This minimizes unnecessary connections and ensures the agent uses the appropriate tools for the task.

```python
import asyncio
from mcp_use import MCPClient, MCPAgent
from langchain_anthropic import ChatAnthropic

async def main():
    # Create client with multiple servers
    client = MCPClient.from_config_file("multi_server_config.json")

    # Create agent with the client
    agent = MCPAgent(
        llm=ChatAnthropic(model="claude-3-5-sonnet-20240620"),
        client=client,
        use_server_manager=True  # Enable the Server Manager
    )

    try:
        # Run a query that uses tools from multiple servers
        result = await agent.run(
            "Search for a nice place to stay in Barcelona on Airbnb, "
            "then use Google to find nearby restaurants and attractions."
        )
        print(result)
    finally:
        # Clean up all sessions
        await client.close_all_sessions()

if __name__ == "__main__":
    asyncio.run(main())
```

# Tool Access Control

MCP-Use allows you to restrict which tools are available to the agent, providing better security and control over agent capabilities:

```python
import asyncio
from mcp_use import MCPAgent, MCPClient
from langchain_openai import ChatOpenAI

async def main():
    # Create client
    client = MCPClient.from_config_file("config.json")

    # Create agent with restricted tools
    agent = MCPAgent(
        llm=ChatOpenAI(model="gpt-4"),
        client=client,
        disallowed_tools=["file_system", "network"]  # Restrict potentially dangerous tools
    )

    # Run a query with restricted tool access
    result = await agent.run(
        "Find the best restaurant in San Francisco"
    )
    print(result)

    # Clean up
    await client.close_all_sessions()

if __name__ == "__main__":
    asyncio.run(main())
```

# Build a Custom Agent:

You can also build your own custom agent using the LangChain adapter:

```python
import asyncio
from langchain_openai import ChatOpenAI
from mcp_use.client import MCPClient
from mcp_use.adapters.langchain_adapter import LangChainAdapter
from dotenv import load_dotenv

load_dotenv()


async def main():
    # Initialize MCP client
    client = MCPClient.from_config_file("examples/browser_mcp.json")
    llm = ChatOpenAI(model="gpt-4o")

    # Create adapter instance
    adapter = LangChainAdapter()
    # Get LangChain tools with a single line
    tools = await adapter.create_tools(client)

    # Create a custom LangChain agent
    llm_with_tools = llm.bind_tools(tools)
    result = await llm_with_tools.ainvoke("What tools do you have avilable ? ")
    print(result)


if __name__ == "__main__":
    asyncio.run(main())


```





-----------


What is MCP (Model Context Protocol)?
The Model Context Protocol (MCP), introduced by Anthropic, defines a standardized interface for supplying structured, real-time context to large language models (LLMs).


![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*MOisdNK2yj5pwi37B4gVuQ.png)

https://modelcontextprotocol.io/
Core Functionalities
Contextual Data Injection
MCP lets you pull in external resources — like files, database rows, or API responses — right into the prompt or working memory. All of it comes through a standardized interface, so your LLM can stay lightweight and clean.

Function Routing & Invocation
MCP also lets models call tools dynamically. You can register capabilities like searchCustomerData or generateReport, and the LLM can invoke them on demand. It’s like giving your AI access to a toolbox, but without hardwiring the tools into the model itself.

Prompt Orchestration
Rather than stuffing your prompt with every possible detail, MCP helps assemble just the context that matters. Think modular, on-the-fly prompt construction — smarter context, fewer tokens, better outputs.

Implementation Characteristics
Operates over HTTP(S) with JSON-based capability descriptors
Designed to be model-agnostic — any LLM with a compatible runtime can leverage MCP-compliant servers
Compatible with API gateways and enterprise authentication standards (e.g., OAuth2, mTLS)
Engineering Use Cases
➀ LLM integrations for internal APIs: Enable secure, read-only or interactive access to structured business data without exposing raw endpoints.

➁ Enterprise agents: Equip autonomous agents with runtime context from tools like Salesforce, SAP, or internal knowledge bases.

➂ Dynamic prompt construction: Tailor prompts based on user session, system state, or task pipeline logic

What is ACP (Agent Communication Protocol)?
The Agent Communication Protocol (ACP) is an open standard originally proposed by BeeAI and IBM to enable structured communication, discovery, and coordination between AI agents operating in the same local or edge environment.

Unlike cloud-oriented protocols such as A2A or context-routing protocols like MCP, ACP is designed for local-first, real-time agent orchestration with minimal network overhead and tight integration across agents deployedc within a shared runtime.

Protocol Design & Architecture
ACP defines a decentralized agent environment in which:

Each agent advertises its identity, capabilities, and state using a local broadcast/discovery layer.
Agents communicate through event-driven messaging, often using a local bus or IPC (inter-process communication) system.
A runtime controller (optional) can orchestrate agent behavior, aggregate telemetry, and enforce execution policies.
ACP agents typically operate as lightweight, stateless services or containers with a shared communication substrate.

Implementation Characteristics
Designed for low-latency environments (e.g., local orchestration, robotics, offline edge AI)
Can be implemented over gRPC, ZeroMQ, or custom runtime buses
Emphasizes local sovereignty — no cloud dependency or external service registration required
Supports capability typing and semantic descriptors for automated task routing
Engineering Use Cases
➀ Multi-agent orchestration on edge devices (e.g., drones, IoT clusters, or robotic fleets)

➁ Local-first LLM systems coordinating model invocations, sensor inputs, and action execution

➂ Autonomous runtime environments where agents must coordinate without centralized cloud infrastructure

In short, ACP offers a runtime-local protocol layer for modular AI systems — prioritizing low-latency coordination, resilience, and composability. It’s a natural fit for privacy-sensitive, autonomous, or edge-first deployments where cloud-first protocols are impractical.

What is A2A (Agent-to-Agent Protocol)?
The Agent-to-Agent (A2A) Protocol, introduced by Google, is a cross-platform specification for enabling AI agents to communicate, collaborate, and delegate tasks across heterogeneous systems.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*xrExmJ7_zySXEDl_YHkyRQ.png)

https://google.github.io/
Unlike ACP’s local-first focus or MCP’s tool integration layer, A2A addresses horizontal interoperability — standardizing how agents from different vendors or runtimes can exchange capabilities and coordinate workflows over the open web.

Protocol Overview
A2A defines a HTTP-based communication model where agents are treated as interoperable services. Each agent exposes an “Agent Card” — a machine-readable JSON descriptor detailing its identity, capabilities, endpoints, and authentication requirements.

Agents use this information to:

Discover each other programmatically
Negotiate tasks and roles
Exchange messages, data, and streaming updates
A2A is transport-layer agnostic in principle, but currently specifies JSON-RPC 2.0 over HTTPS as its core mechanism for interaction.

Core Components
Agent Cards
JSON documents describing an agent’s capabilities, endpoints, supported message types, auth methods, and runtime metadata.

A2A Client/Server Interface
Each agent may function as a client (task initiator), a server (task executor), or both, enabling dynamic task routing and negotiation.

Message & Artifact Exchange
Supports multipart tasks with context, streaming output (via SSE), and persistent artifacts (e.g., files, knowledge chunks).

User Experience Negotiation
Agents can adapt message format, content granularity, and visualization to match downstream agent capabilities.

Security Architecture
OAuth 2.0 and API key-based authorization
Capability-scoped endpoints — agents only expose functions required for declared interactions
Agents can operate in “opaque” mode — hiding internal logic while revealing callable services
Implementation Characteristics
Web-native by design: built on HTTP, JSON-RPC, and standard web security
Model-agnostic: works with any agent system (LLM or otherwise) that implements the protocol
Supports task streaming and multi-turn collaboration with lightweight payloads
Engineering Use Cases
➀ Cross-platform agent ecosystems where agents from different teams or vendors need to interoperate securely

➁ Distributed agent orchestration in cloud-native AI environments (e.g., Vertex AI, LangChain, HuggingFace Agents)

➂ Multi-agent collaboration frameworks, such as enterprise AI workflows that span multiple systems (e.g., CRM, HR, IT agents)

Protocols Compared Side-by-Side
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*TU0BHEiLmZZ-a2brrGQ1xw.png)


Complementary or Competitive?
A2A + MCP
A2A and MCP aren’t fighting each other — they’re solving totally different parts of the agentic AI puzzle, and they actually fit together pretty nicely.


![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*bWg9FT2_smxDuWwntjFWVg.png)

https://google.github.io/
Think of MCP as the protocol that lets AI agents plug into the world. It gives them access to files, APIs, databases — basically, all the structured context they need to do something useful. Whether it’s pulling real-time sales data or generating a custom report, MCP handles the connection to tools and data.

Now layer on A2A. This is where agents start collaborating. A2A gives them a shared language and set of rules to discover each other, delegate tasks, and negotiate how they’ll work together — even if they’re built by different vendors or running on different platforms.

So here’s a simple way to think about it:
⟢ MCP connects AI to tools.
⟢ A2A connects AI to other AI.

Together, they form a strong modular base for building smart, collaborative systems.

What About ACP?
Then there’s ACP, which takes a different approach altogether. It’s all about local-first agent coordination — no cloud required. Instead of using HTTP and web-based discovery, ACP enables agents to find and talk to each other right inside a shared runtime.

This is perfect for situations where:

You have limited bandwidth or need low-latency (like in robotics or on-device assistants),
Privacy matters and you want to keep everything offline,
Or you’re deploying in environments cut off from the internet (e.g., factory floors, edge nodes).
ACP isn’t trying to compete with A2A — it just fills a different niche. But in some setups, especially in tightly controlled environments, ACP might replace A2A entirely, because it skips the overhead of web-native protocols and just gets the job done locally.

Convergence or Fragmentation?
As more teams adopt these protocols, a few possible futures are taking shape.

Best case? We see convergence. Imagine a unified agent platform where A2A handles the back-and-forth between agents, MCP manages access to tools and data, and ACP-style runtimes plug in for edge or offline scenarios. Everything just works, and developers can build on top without worrying which protocol is doing what behind the scenes.

Worst case? Things fragment. Different vendors push their own flavors of A2A or MCP, and we end up with a mess — like the early days of web services, when nothing talked to anything else without a lot of glue code.

The middle ground? Open-source tools and middleware could save the day. These projects would sit between agents and protocols, abstracting the differences and giving devs a clean, unified API — while translating under the hood depending on where and how your agents run.

In short: we’re early. But how we build and adopt these standards now will shape whether AI agents become a cohesive ecosystem — or a patchwork of silos.
