## Mini RAG

Author: [Felipe Gonzalez Roldan](https://felipeg17.github.io/index.html)<br>
Repo: [mini-rag](https://github.com/felipe-roldan/mini-rag)<br>

### Description

This project enables the creation of a mini-RAG (Retrieval-Augmented Generation) system using an in-memory vector store. In general terms, it allows an LLM to incorporate specific context from a knowledge base—in this case, documents in various formats (e.g., .csv, .pdf)—so that the answers it provides are grounded in real information, not just the model’s training data.

### Approach

`Langchain` is the framework of choice. It enables the systematic use of LLM APIs like OpenAI.
It provides:

- Model access via the `ChatOpenAI` class  
- A vector database using `InMemoryVectorStore`  
- Embedding support with `OpenAIEmbeddings`  
- Document processing using `Document` and `RecursiveCharacterTextSplitter`

Key components of this implementation:

- The selected model was `gpt-4o-mini` due to its low token cost.
- The embedding model used was `text-embedding-ada-002`, which is fully compatible with the OpenAI ecosystem and provides vector representations for chunks of tokens.
- The vector database used was `InMemoryVectorStore`, which offers a lightweight interface for storing embeddings from source documents and exposes the necessary interfaces to perform similarity searches and semantic retrieval.
- Question answering (QA) was implemented using the `RetrievalQA` chain, which allows the LLM to use a prompt enriched with the most relevant context from the vector store and respond to user queries grounded in the original source documents.

In the [notebook](/mini_rag.ipynb), two types of documents were used: a `.csv` file containing condensed tax rates from Missouri, and a PDF file that provides a taxation guide for the state of Washington.

Both documents are processed differently:

- For the `.csv` file, embeddings are created for each row.
- For the PDF file, the content is split into chunks, embeddings are generated for each chunk, and then stored in the vector database.

In both cases, a query is made to compare the answer based on the knowledge base with a zero-shot response from the LLM, demonstrating the actual difference.


Question with RAG
```
Query: ¿What is county tourism sales tax rate in Missouri?
Answer: The county tourism sales tax rate in Missouri can be up to 7/8%.
Source documents:
COUNTY TOURISM SALES TAX UP TO 7/8% 67.671-67.685
ST LOUIS COUNTY ZOOGICAL SALES TAX 1/8, 1/4, 3/8, 1/2% (Cannot exceed 1/8% beginning 8/28/17) (Cannot be in excess of 1% combined) 67.547
ST LOUIS COUNTY ADDITIONAL SALES TAX 275/1000% 67.581
```

Straingt question to the LLM
```
In Missouri, the county tourism sales tax rate can vary by county, as each county has the authority to impose its own sales tax for tourism-related purposes. Generally, the rates can range from 1% to 5%, depending on the specific county's regulations and decisions. To find the exact tourism sales tax rate for a particular county in Missouri, it's best to check with the local county government or their official website for the most accurate and up-to-date information.
```

### Requirements

- OPENAI APY-KEY: Located in the .env file
- [Folder](/documents/) with documents 

### Installation

Required: `python 3.10+`

Virtual enviroment setup:
```bash
python -m venv v_mini_rag
```

Dependancies installation:

```bash
pip install -r requirements.txt
```