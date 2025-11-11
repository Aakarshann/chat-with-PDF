Advanced RAG: Multi-Vector Retrieval for Text and Tables

This repository contains a Jupyter Notebook demonstrating an advanced Retrieval-Augmented Generation (RAG) pipeline. The core technique showcased is Multi-Vector Retrieval, which is designed to improve RAG performance on complex, multi-modal documents that contain both dense text and structured tables.

The pipeline uses unstructured to parse a PDF, ChatGoogleGenerativeAI (Gemini) to summarize the different elements, and a MultiVectorRetriever from LangChain to store summaries in Chroma while linking back to the original, full documents.

🎯 Core Concept: Multi-Vector RAG

Traditional RAG pipelines often struggle with complex documents. A single, large chunk might be too generic, while a small chunk might lack context. More importantly, embedding a raw table often results in a poor vector representation, making it difficult to retrieve.

This project solves that problem using a "parent document" or multi-vector approach:

Parse & Separate: The PDF is parsed into its constituent elements, separating text chunks from tables.

Summarize: A Gemini LLM generates a high-quality summary for each text chunk and each table (using its HTML representation).

Store:

Vector Store (Chroma): The summaries are embedded and stored. These are small, dense, and ideal for semantic search.

Document Store (InMemoryStore): The original, full documents (the raw text chunks and table objects) are stored in a simple key-value store.

Link: An ID is used to link each summary in the vector store to its original document in the docstore.

Retrieve & Generate:

A user's query is embedded and compared against the summaries.

The retriever finds the most relevant summaries and uses their IDs to fetch the original, full-context documents.

These full documents (not the summaries) are passed to the LLM to generate a high-quality, context-rich answer.

This method ensures we search over concise summaries but provide the LLM with the full, rich context for generation.

✨ Key Features

PDF Parsing: Uses unstructured for high-resolution PDF parsing, including table extraction.

Multi-Vector RAG: Implements the MultiVectorRetriever to store summaries for retrieval and original documents for context.

Multi-Modal Handling: Demonstrates a pipeline that separately processes text and tables from a single document.

Generative Summaries: Uses gemini-2.5-flash-lite to summarize all chunks, improving the relevance of vector search.

Vector Store: Leverages Chroma for efficient local vector storage.

LCEL: The entire retrieval and generation chain is built using the LangChain Expression Language (LCEL).

🛠️ Setup & Installation

1. Clone the Repository

git clone [https://github.com/your-username/your-repository-name.git](https://github.com/your-username/your-repository-name.git)
cd your-repository-name


2. OS-Level Dependencies

The unstructured library requires certain system packages to process PDFs and images.

For Debian/Ubuntu:

sudo apt-get update
sudo apt-get install -y poppler-utils tesseract-ocr


3. Create a Virtual Environment

python -m venv .venv
source .venv/bin/activate  # On Windows, use `.venv\Scripts\activate`


4. Install Python Dependencies

You can create a requirements.txt file from the notebook's installation commands.

unstructured[all-docs]
pillow
lxml
chromadb
tiktoken
langchain
langchain-community
langchain-google-genai
langchain-classic
python-dotenv
jupyterlab


Then, install them:

pip install -r requirements.txt


5. Set Up API Keys

Create a .env file in the root of the directory and add your Google Gemini API key.

# .env
GEMINI_KEY="your-google-api-key-here"

# Optional: For LangSmith tracing
LANGCHAIN_API_KEY="your-langchain-api-key-here"
LANGCHAIN_TRACING_V2="true"


6. Add Your Data

This notebook is configured to read a file named attention.pdf from a pdf/ directory.

Create the directory: mkdir pdf

Place your PDF file inside: pdf/attention.pdf

🚀 How to Run

Ensure your virtual environment is activated.

Start Jupyter Lab:

jupyter lab


Open the rag_with_langchain.ipynb notebook.

Run the cells from top to bottom to parse the document, build the retriever, and ask questions.

📂 File Structure

.
├── .env                 # (Your local file, contains API keys)
├── pdf/
│   └── attention.pdf    # (You must provide this document)
├── rag_with_langchain.ipynb
├── requirements.txt     # (Create this from the notebook)
└── README.md
