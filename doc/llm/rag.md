# RAGAPP

python + typescript

https://github.com/ragapp/ragapp


The easiest way to use Agentic RAG in any enterprise.

As simple to configure as OpenAI's custom GPTs, but deployable in your own cloud infrastructure using Docker. Built using LlamaIndex.


To run, start a docker container with our image:

docker run -p 8000:8000 ragapp/ragapp
Then, access the Admin UI at http://localhost:8000/admin to configure your RAGapp.

You can use hosted AI models from OpenAI or Gemini, and local models using Ollama.

Note: To avoid running into any errors, we recommend using the latest version of Docker and (if needed) Docker Compose.

Endpoints
The docker container exposes the following endpoints:

Admin UI: http://localhost:8000/admin
Chat UI: http://localhost:8000
API: http://localhost:8000/docs
Note: The Chat UI and API are only functional if the RAGapp is configured.







# RAG


RAG on Complex PDF using LlamaParse, Langchain and Groq


https://medium.com/the-ai-forum/rag-on-complex-pdf-using-llamaparse-langchain-and-groq-5b132bd1f9f3


Plaban Nayak
The AI Forum
Plaban Nayak

·
Follow

Published in
The AI Forum

·
13 min read
·
Apr 7, 2024
717


10






Retrieval-Augmented Generation (RAG) is a new approach that leverages Large Language Models (LLMs) to automate knowledge search, synthesis, extraction, and planning from unstructured data sources. This method has gained prominence over the past year due to its ability to enhance LLM applications with contextual information. The RAG data stack consists of several key components:

Loading Data: Initially, data is ingested from various sources, such as text documents, websites, or databases. This data can be in a raw or preprocessed format.
Processing Data: The data undergoes preprocessing steps to clean and structure it for further analysis. This may include tasks like tokenization, stemming, and removing stop words.
Embedding Data: Each piece of data is converted into a numerical representation called an embedding. This embedding captures semantic information about the data, making it easier for the LLM to understand and process.
Vector Database: The embeddings are stored in a vector database, which allows for efficient retrieval based on similarity metrics. This database enables quick access to relevant data points during the generation process.
Retrieval and Prompting: During the generation process, the LLM can retrieve relevant data points from the vector database based on the context of the current input. This retrieval mechanism helps the LLM provide more accurate and contextually relevant outputs.
Overall, the RAG approach enhances the capabilities of LLMs by enabling them to leverage external knowledge sources in a systematic and efficient manner. This can lead to more powerful and contextually aware applications in various domains, such as natural language understanding, information retrieval, and decision-making.

Building a Production grade RAG remains a complex and subtle problem. Some of the challenges associated is as follows:-

Results aren’t accurate enough: The application was not able to produce satisfactory results for a long-tail of input tasks/queries.
The number of parameters to tune is overwhelming: It’s not clear which parameters across the data parsing, ingestion, retrieval.
PDFs are specifically a problem: I have complex docs with lots of messy formatting. How do I represent this in the right way so the LLM can understand it?
Data syncing is a challenge: Production data often updates regularly, and continuously syncing new data brings a new set of challenges.
With a sole intent to solve the above problems in February 20, 2024 LlamaIndex came up with LalmaCloud and LlamaParse a new generation of managed parsing, ingestion, and retrieval services, designed to bring production-grade context-augmentation to our LLM and RAG applications.

The main intuition behind the inception of LalmaCloud was to focus on focus on writing the business logic and not on data wrangling. Process large volumes of production data, immediately leading to better response quality. It has following two components:

LlamaParse: Proprietary parsing for complex documents with embedded objects such as tables and figures. LlamaParse directly integrates with LlamaIndex ingestion and retrieval to let you build retrieval over complex, semi-structured documents. It is promised to be able to answer complex questions that simply weren’t possible previously.
Managed Ingestion and Retrieval API: An API which allows you to easily load, process, and store data for your RAG app and consume it in any language. Backed by data sources in LlamaHub, including LlamaParse, and data storage integrations.

What is LlamaParse ?
LlamaParse is a proprietary parsing service that is incredibly good at parsing PDFs with complex tables into a well-structured markdown format.

This service is available in a public preview mode: available to everyone, but with a usage limit (1k pages per day) with 7,000 free pages per week. Then $0.003 per page ($3 per 1,000 pages).It operates as a standalone service that can also be plugged into the managed ingestion and retrieval API

from llama_parse import LlamaParse

parser = LlamaParse(
api_key="llx-...",  # can also be set in your env as LLAMA_CLOUD_API_KEY
result_type="markdown",  # "markdown" and "text" are available
verbose=True
)
Currently LlamaParse primarily support PDFs with tables, but they are also building out better support for figures, and and an expanded set of the most popular document types: .docx, .pptx, .html as a part of next enhancements.

What is Groq ?
Groq, founded in 2016 and headquartered in Mountain View, California, is an AI solutions startup focused on ultra-low latency AI inference. The company has made significant advancements in AI computing performance and is a notable participant in the AI technology sector. Groq has registered its name as a trademark and has assembled a global team dedicated to democratizing access to AI

Groq’s Language Processing Unit (LPU) is a cutting-edge technology designed to significantly enhance AI computing performance, especially for Large Language Models (LLMs). The primary goal of the Groq LPU system is to provide real-time, low-latency experiences with exceptional inference performance. One of Groq’s achievements includes surpassing the benchmark of over 300 tokens per second per user on Meta AI’s Llama-2 70B model, which is a significant advancement in the industry.

The Groq LPU system is particularly notable for its ultra-low latency capabilities, which are crucial for supporting AI technologies. It is specifically tailored for sequential and compute-intensive GenAI language processing, outperforming traditional GPU solutions. This makes it highly efficient for tasks such as natural language creation and understanding.

At the core of the Groq LPU system is the first-generation GroqChip, which features a tensor streaming architecture optimized for speed, efficiency, accuracy, and cost-effectiveness. This chip has set new records in foundational LLM speed, measured in tokens per second per user, surpassing existing solutions in the market. Groq has ambitious plans to deploy 1 million AI inference chips within two years, showcasing its dedication to advancing AI acceleration technologies.

What is the LPU Inference Engine ?
An LPU Inference Engine, with LPU standing for Language Processing Unit™, is a new type of end-to-end processing unit system that provides the fastest inference for computationally intensive applications with a sequential component to them, such as AI language applications (LLMs).

Why is it faster than GPUs ?
The LPU is designed to overcome the two LLM bottlenecks: compute density and memory bandwidth. An LPU has greater compute capacity than a GPU and CPU in regards to LLMs. This reduces the amount of time per word calculated, allowing sequences of text to be generated much faster. Additionally, eliminating external memory bottlenecks enables the LPU Inference Engine to deliver orders of magnitude better performance on LLMs compared to GPUs.

For a more technical read about our architecture, download our ISCA-awarded 2020 and 2022 papers.

Getting Started with Groq
Right now, Groq is providing free-to-use API endpoints to the Large Language Models running on the Groq LPU — Language Processing Unit.

Groq guarantees to beat any published price per million tokens by published providers of the equivalent listed models. Other models, such as Mistral and CodeLlama, are available for specific customer requests.

To get started, visit this page and click on login. The page looks like the one below:



What is Langchain?
LangChain is an open-source framework designed to simplify the creation of applications using large language models (LLMs). It provides a standard interface for chains, lots of integrations with other tools, and end-to-end chains for common applications.

Langchain Key Concepts:
Components: Components are modular building blocks that are ready and easy to use to build powerful applications. Components include LLM Wrappers, Prompt Template and Indexes for relevant information retrieval.
Chains: Chains allow us to combine multiple components together to solve a specific task. Chains make it easy for the implementation of complex applications by making it more modular and simple to debug and maintain.
Agents: Agents allow LLMs to interact with their environment. For example, using an external API to perform a specific action.
Code Implementation
The code is implemented in Google Colab (cpu)

Install required dependencies

%%writefile requirements.txt
langchain
langchain-community
llama-parse
fastembed
chromadb
python-dotenv
langchain-groq
chainlit
fastembed
unstructured[md]
!pip install -r requirements.txt
Set up the environment variables

from google.colab import userdata

llamaparse_api_key = userdata.get('LLAMA_CLOUD_API_KEY')
groq_api_key = userdata.get("GROQ_API_KEY")
Import required dependencies

##### LLAMAPARSE #####
from llama_parse import LlamaParse

from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.embeddings.fastembed import FastEmbedEmbeddings
from langchain_community.vectorstores import Chroma
from langchain_community.document_loaders import DirectoryLoader
from langchain_community.document_loaders import UnstructuredMarkdownLoader
from langchain.prompts import PromptTemplate
from langchain.chains import RetrievalQA
#
from groq import Groq
from langchain_groq import ChatGroq
#
import joblib
import os
import nest_asyncio  # noqa: E402
nest_asyncio.apply()
LlamaParse Parameters


* api_key: str = Field(
  default="",

  description="The API key for the LlamaParse API.",

  )

* base_url: str = Field(
  default=DEFAULT_BASE_URL,
  description="The base URL of the Llama Parsing API.",
  )
* result_type: ResultType = Field(
  default=ResultType.TXT, description="The result type for the parser."
  )
* num_workers: int = Field(
  default=4,
  gt=0,
  lt=10,
  description="The number of workers to use sending API requests for parsing."
  )
* check_interval: int = Field(
  default=1,
  description="The interval in seconds to check if the parsing is done.",
  )
* max_timeout: int = Field(
  default=2000,
  description="The maximum timeout in seconds to wait for the parsing to finish.",
  )
* verbose: bool = Field(
  default=True, description="Whether to print the progress of the parsing."
  )
* language: Language  = Field(
  default=Language.ENGLISH, description="The language of the text to parse."
  )
* parsing_instruction: Optional[str] = Field(
  default="",
  description="The parsing instruction for the parser."
  )
  Helper function to load and parse the input data

!mkdir data
#
def load_or_parse_data():
data_file = "./data/parsed_data.pkl"

    if os.path.exists(data_file):
        # Load the parsed data from the file
        parsed_data = joblib.load(data_file)
    else:
        # Perform the parsing step and store the result in llama_parse_documents
        parsingInstructionUber10k = """The provided document is a quarterly report filed by Uber Technologies,
        Inc. with the Securities and Exchange Commission (SEC).
        This form provides detailed financial information about the company's performance for a specific quarter.
        It includes unaudited financial statements, management discussion and analysis, and other relevant disclosures required by the SEC.
        It contains many tables.
        Try to be precise while answering the questions"""
        parser = LlamaParse(api_key=llamaparse_api_key,
                            result_type="markdown",
                            parsing_instruction=parsingInstructionUber10k,
                            max_timeout=5000,)
        llama_parse_documents = parser.load_data("./data/uber_10q_march_2022 (1).pdf")


        # Save the parsed data to a file
        print("Saving the parse results in .pkl format ..........")
        joblib.dump(llama_parse_documents, data_file)

        # Set the parsed data to the variable
        parsed_data = llama_parse_documents

    return parsed_data
Helper function to load chunks into vectorstore.

# Create vector database
def create_vector_database():
"""
Creates a vector database using document loaders and embeddings.

    This function loads urls,
    splits the loaded documents into chunks, transforms them into embeddings using OllamaEmbeddings,
    and finally persists the embeddings into a Chroma vector database.

    """
    # Call the function to either load or parse the data
    llama_parse_documents = load_or_parse_data()
    print(llama_parse_documents[0].text[:300])

    with open('data/output.md', 'a') as f:  # Open the file in append mode ('a')
        for doc in llama_parse_documents:
            f.write(doc.text + '\n')

    markdown_path = "/content/data/output.md"
    loader = UnstructuredMarkdownLoader(markdown_path)

#loader = DirectoryLoader('data/', glob="**/*.md", show_progress=True)
documents = loader.load()
# Split loaded documents into chunks
text_splitter = RecursiveCharacterTextSplitter(chunk_size=2000, chunk_overlap=100)
docs = text_splitter.split_documents(documents)

    #len(docs)
    print(f"length of documents loaded: {len(documents)}")
    print(f"total number of document chunks generated :{len(docs)}")
    #docs[0]

    # Initialize Embeddings
    embed_model = FastEmbedEmbeddings(model_name="BAAI/bge-base-en-v1.5")

    # Create and persist a Chroma vector database from the chunked documents
    vs = Chroma.from_documents(
        documents=docs,
        embedding=embed_model,
        persist_directory="chroma_db_llamaparse1",  # Local mode with in-memory storage only
        collection_name="rag"
    )

    #query it
    #query = "what is the agend of Financial Statements for 2022 ?"
    #found_doc = qdrant.similarity_search(query, k=3)
    #print(found_doc[0][:100])
    #print(qdrant.get())

    print('Vector DB created successfully !')
    return vs,embed_model
Process the data and create Vector Store

vs,embed_model = create_vector_database()
Instantiate LLM


chat_model = ChatGroq(temperature=0,
model_name="mixtral-8x7b-32768",
api_key=userdata.get("GROQ_API_KEY"),)
The above code does the following:

Creates a new ChatGroq object named chat_model
Sets the temperature parameter to 0, indicating that the responses should be more predictable
Sets the model_name parameter to “mixtral-8x7b-32768“, specifying the language model to use
Instantiate Vectorstore

vectorstore = Chroma(embedding_function=embed_model,
persist_directory="chroma_db_llamaparse1",
collection_name="rag")
#
retriever=vectorstore.as_retriever(search_kwargs={'k': 3})
Create a Custom Prompt Template

custom_prompt_template = """Use the following pieces of information to answer the user's question.
If you don't know the answer, just say that you don't know, don't try to make up an answer.

Context: {context}
Question: {question}

Only return the helpful answer below and nothing else.
Helpful answer:
"""
Helper Function to format the prompt

def set_custom_prompt():
"""
Prompt template for QA retrieval for each vectorstore
"""
prompt = PromptTemplate(template=custom_prompt_template,
input_variables=['context', 'question'])
return prompt
#
prompt = set_custom_prompt()
prompt

########################### RESPONSE ###########################
PromptTemplate(input_variables=['context', 'question'], template="Use the following pieces of information to answer the user's question.\nIf you don't know the answer, just say that you don't know, don't try to make up an answer.\n\nContext: {context}\nQuestion: {question}\n\nOnly return the helpful answer below and nothing else.\nHelpful answer:\n")
Instantiate the Retrieval Question Answering Chain

qa = RetrievalQA.from_chain_type(llm=chat_model,
chain_type="stuff",
retriever=retriever,
return_source_documents=True,
chain_type_kwargs={"prompt": prompt})
Invoke the Retriveal QA Chain

response = qa.invoke({"query": "what is the Balance of UBER TECHNOLOGIES, INC.as of December 31, 2021?"})

Response Synthesized

response['result']

########################### RESPONSE ###########################
Based on the provided balance sheet of Uber Technologies, Inc. as of December 31, 2021, the total assets are $38,774 million, total liabilities are $23,425 million, and total equity is $9,613 million.
Question 2


response = qa.invoke({"query": "What is the Cash flows from operating activities associated with bad expense specified in the document ?"})
response['result']

######################## RESPONSE ###############################
The Cash flows from operating activities associated with bad debt expense is 23 for the year 2021 and 18 for the year 2022.
Question 3

response = qa.invoke({"query": "what is Loss (income) from equity method investments, net ?"})
response["result"]

############################### RESPONSE ##############################
The loss from equity method investments, net, is calculated as the sum of the impairment of equity method investment and the revaluation of MLU B.V. call option, which amounted to $182 million and $181 million, respectively. This results in a total loss from equity method investments, net, of $363 million. This loss is included in the net loss attributable to Uber Technologies, Inc. of $5.9 billion.
Question 4


response = qa.invoke({"query": "What is the Total cash and cash equivalents, and restricted cash and cash equivalents for reconciliation ?"})
response['result']

######################## RESPONSE #####################################
The total cash and cash equivalents, and restricted cash and cash equivalents for reconciliation is $6,607 million. This amount is obtained by adding the cash and cash equivalents of $4,836 million and the restricted cash and cash equivalents - current of $247 million, and the restricted cash and cash equivalents - non-current of $1,524 million.
Question 5


response = qa.invoke({"query":"Based on the CONDENSED CONSOLIDATED STATEMENTS OF REDEEMABLE NON-CONTROLLING INTERESTS AND EQUITY what is the Balance as of March 31, 2021?"})
print(response['result'])

############# RESPONSE ##################
The balance as of March 31, 2021 was $473 for Redeemable Non-Controlling Interests, 1,867,369 shares for Common Stock, $— for Additional Paid-In Capital, $36,182 for Other Comprehensive Income (Loss), $654 for Non-Controlling Interests, and $654 for Total Equity.
Question 6


response = qa.invoke({"query":"Based on the condensed consolidated statements of comprehensive Income(loss) what is the  Comprehensive income (loss) attributable to Uber Technologies, Inc.for the three months ended March 31, 2022"})
response['result']

######################### RESPONSE ####################################
The Comprehensive income (loss) attributable to Uber Technologies, Inc. for the three months ended March 31, 2022 was $(5,911) million. This information can be found on the Uber Technologies, Inc. - Condensed Consolidated Statements of Comprehensive Income (Loss) provided in the quarterly report.
Question 7

response = qa.invoke({"query":"Based on the condensed consolidated statements of comprehensive Income(loss) what is the  Comprehensive income (loss) attributable to Uber Technologies?"})
response['result']

##################### RESPONSE #################################
The Comprehensive income (loss) attributable to Uber Technologies, Inc. for the three months ended March 31, 2021 is $1,081 million, and for the three months ended March 31, 2022 is -$5,911 million.
Question 8

response = qa.invoke({"query":"Based on the condensed consolidated statements of comprehensive Income(loss) what is the Net loss including non-controlling interests"})
response['result']

################ RESPONSE #######################################
The Net loss including non-controlling interests is $(122) million for the three months ended March 31, 2021 and $(5,918) million for the three months ended March 31, 2022.
Question 9


response = qa.invoke({"query":"what is the Net cash used in operating activities for Mrach 31,2021? "})
response['result']

############## RESPONSE ###############################
Net cash used in operating activities for March 31, 2021 was $611 million.
Question 10


query = "Based on the CONDENSED CONSOLIDATED STATEMENTS OF CASH FLOWS What is the value of Purchases of property and equipment ?"
response = qa.invoke({"query":query})
response['result']

####################### RESPONSE ######################################
The value of purchases of property and equipment for the three months ended March 31, 2021 and 2022 can be found in the 'Cash flows from investing activities' section of the condensed consolidated statements of cash flows.

For the three months ended March 31, 2021: $71 million
For the three months ended March 31, 2022: $62 million
Question 11

query = "Based on the CONDENSED CONSOLIDATED STATEMENTS OF CASH FLOWS what is the Purchases of property and equipment for the year 2022?"
response = qa.invoke({"query":query})
response['result']

########### RESPONSE #####################################
The purchases of property and equipment for the year 2022 based on the CONDENSED CONSOLIDATED STATEMENTS OF CASH FLOWS is -62.
From the above implementation we can assume that LLamaParse is comparatively good at parsing complex pdf documents. Although we have to experiment more with different tabular structures. Here ia comparison of LlamaParse with PyPDF


Source : LLAMAPARSE
Conclusion
From the above implementation we can conclude that LlamaParse is incredibly good at parsing PDFs with complex tables into a well-structured markdown format. This representation directly plugs into the advanced Markdown parsing and recursive retrieval algorithms available in the open-source library. The end result is that we are able to build RAG over complex documents that can answer questions over both tabular and unstructured data.

References:
Launching the first GenAI-native document parsing platform - LlamaIndex, Data Framework for LLM…
LlamaIndex is a simple, flexible data framework for connecting custom data sources to large language models (LLMs).
www.llamaindex.ai

GroqCloud
Experience the fastest inference in the world
console.groq.com

Introduction | 🦜️🔗 LangChain
LangChain is a framework for developing applications powered by large language models (LLMs).
python.langchain.com

connect with me



--------

# RAG 2.0: Retrieval Augmented Language Models
Vishal Rajput
AIGuys
Vishal Rajput


https://medium.com/aiguys/rag-2-0-retrieval-augmented-language-models-3762f3047256



Language models have led to amazing progress, but they also have important shortcomings. One solution for many of these shortcomings is retrieval augmentation. There have been plenty of articles written about Retrieval Augmented Generation (RAG) pipelines, which as a technology is quite cool. But today, we are taking it one step further and truly exploring what’s next for the technology of RAG. What if we can create models with trainable retrievers, or in short, the entire RAG pipeline is customizable like fine-tuning an LLM? The problem with current RAGs is that they are not fully in tune within it’s submodules, it’s like a Frankenstein monster, it somehow works, but the parts are not in harmony and perform quite suboptimally together. So, to tackle all the issues with Frankenstein RAG, let’s take a deep dive into RAG 2.0.

Table of Contents
What Are RAGs?
What RAG 2.0 Achieves?
How Does RAG Solve Issues of Intelligence?
Better Retrieval Strategies
SOTA Retrieval Algorithms
Contextualizing the Retriever for the Generator
Combined Contextualized Retriever and Generator
SOTA Contextualizaton
Conclusion
Please take a moment to share the article and follow me, only if you see the hard work. It takes weeks to write such articles, some love would be highly appreciated.


