# End-to-end-Medical-Chatbot-Generative-AI
# End-to-End Medical Chatbot

## Overview

This project implements an end-to-end medical chatbot designed to provide users with information derived from a comprehensive medical knowledge base. The chatbot leverages advanced natural language processing (NLP) techniques, including vector embeddings and large language models (LLMs), to understand user queries and deliver relevant and accurate responses.

## Project Architecture

The project is divided into two main components:

### 1.  Backend Data Architecture

* **Data Source:** The primary data source is the "Gale Encyclopedia of Medicine," supplemented by a medical book PDF.
* **Data Processing:**
    * The medical book PDF was processed to extract all documents, focusing on content from "chance 1" to "chance 3." This suggests a categorization or prioritization of the information within the book.
    * The extracted documents, along with the Gale Encyclopedia of Medicine data, were then transformed into a suitable format for generating vector embeddings.
* **Embedding Model:** The `sentence-transformers==4.1.0` library was used to create dense vector representations (embeddings) of the text data. These embeddings capture the semantic meaning of the text.
* **Semantic Indexing:** The generated vector embeddings were used to build a semantic index.
* **Knowledge Base:** The semantic index is stored in a Pinecone vector database. Pinecone provides efficient storage and retrieval of high-dimensional vectors, enabling fast similarity search. Specifically, `pinecone[grpc]` and `langchain-pinecone` were used to interface with Pinecone.

### 2.  Frontend Data Architecture

* **Query Processing:**
    * User queries are received by the system.
    * The `sentence-transformers==4.1.0` library is again used to generate a vector embedding of the user's query. This transforms the query into a numerical representation that can be compared to the embeddings in the knowledge base.
* **Information Retrieval:**
    * The query embedding is used to perform a similarity search in the Pinecone vector database. This retrieves the most relevant documents from the knowledge base based on semantic similarity to the query.
* **LLM Integration:**
    * The retrieved documents are then passed to an LLM (accessed via `langchain_openai` and `langchain_experimental`).
    * The LLM uses the retrieved information to formulate a coherent and informative response to the user's query. Langchain helps in orchestrating the interaction with the LLM.
* **Response Delivery:** The LLM's response is delivered to the user.
* **Framework:** Flask is used as the web framework.

## Technologies Used

* `sentence-transformers==4.1.0`: For generating text embeddings.
* `langchain`: A framework for developing applications powered by language models.
* `flask`: A web framework for building the chatbot application.
* `pypdf`: For processing and extracting data from PDF files.
* `python-dotenv`: For managing environment variables (e.g., API keys).
* `pinecone[grpc]`: A vector database for storing and querying embeddings.
* `langchain-pinecone`: Integration between Langchain and Pinecone.
* `langchain_community`: Community driven tools for Langchain
* `langchain_openai`: Integration between Langchain and OpenAI.
* `langchain_experimental`: Experimental features in Langchain.
* `-e .`: Indicates a local install of the package.
* OpenAI LLM: The specific LLM used (e.g., GPT-3.5, GPT-4).
* CICD: Continuous Integration/Continuous Deployment

## What I Did

* **Data Preparation:** I processed the "Gale Encyclopedia of Medicine" and a medical book PDF, extracting and cleaning the relevant text data.
* **Embedding Generation:** I used the Sentence Transformers library to convert the text data into vector embeddings, capturing the semantic meaning of the medical information.
* **Knowledge Base Construction:** I built a knowledge base by indexing the generated embeddings in a Pinecone vector database, enabling efficient similarity search.
* **Chatbot Development:** I developed a chatbot application using Flask that:
    * Takes user queries as input.
    * Generates embeddings for the queries.
    * Retrieves relevant information from the Pinecone knowledge base.
    * Leverages an OpenAI LLM to generate natural language responses.
* **Integration:** I integrated the various components, including the data processing pipeline, the vector database, and the LLM, using Langchain.
* **CICD:** I implemented a CI/CD pipeline.

## How I Did It

1.  **Data Extraction and Preprocessing:**
    * Used `pypdf` to extract text from the medical book PDF.
    * Cleaned the text data by removing irrelevant characters, formatting inconsistencies, and stop words.
    * Structured the data into a format suitable for embedding generation.

2.  **Embedding Generation:**
    * Installed the `sentence-transformers` library.
    * Loaded a pre-trained Sentence Transformer model (specifically version 4.1.0, as specified).
    * Applied the model to the processed text data to generate vector embeddings.

3.  **Knowledge Base Setup:**
    * Created a Pinecone account and obtained the necessary API key and environment.
    * Installed the `pinecone` and `langchain-pinecone` libraries.
    * Initialized a Pinecone index with an appropriate dimensionality to store the generated embeddings.
    * Loaded the embeddings into the Pinecone index.

4.  **Chatbot Development (Flask):**
    * Set up a Flask application to handle user requests.
    * Created an API endpoint to receive user queries.
    * Implemented the query processing logic:
        * Generated embeddings for the user query using the same Sentence Transformer model.
        * Performed a similarity search in the Pinecone index using the query embedding.
        * Retrieved the most relevant documents from Pinecone.
        * Used Langchain to interact with the OpenAI LLM, providing the retrieved documents as context.
        * The LLM generates a response based on the retrieved information.
    * Returned the LLM's response to the user.
    * Implemented necessary environment variable management using `python-dotenv`.
    * Setup CI/CD.

## Impact (STAR Method)

* **Situation:** Accessing accurate and reliable medical information can be challenging and time-consuming for users. Traditional methods often involve sifting through lengthy articles or consulting with healthcare professionals, which may not always be feasible.
* **Task:** I aimed to develop a chatbot that could provide users with quick and easy access to medical information from a trusted source (the Gale Encyclopedia of Medicine and a medical book). The goal was to create a tool that could answer user questions in a clear, concise, and informative manner.
* **Action:**
    * I designed and implemented an end-to-end system that combines the power of vector embeddings, a vector database, and a large language model.
    * I carefully processed and indexed the medical knowledge base to ensure efficient retrieval of relevant information.
    * I used Langchain to orchestrate the interaction between the different components, enabling seamless communication between the user, the knowledge base, and the LLM.
    * I developed a Flask-based web application to provide a user-friendly interface for interacting with the chatbot.
    * I setup a CI/CD pipeline for efficient development.
* **Result:** The resulting chatbot provides users with a convenient and efficient way to access medical information. It can answer a wide range of questions related to diseases, treatments, symptoms, and other medical topics. The chatbot has the potential to:
    * Improve user access to reliable medical information.
    * Reduce the time and effort required to find answers to medical questions.
    * Support users in making informed decisions about their health.
    * Potentially assist healthcare professionals by providing quick access to relevant information.




# How to run?
### STEPS:

Clone the repository

```bash
Project repo: https://github.com/
```
### STEP 01- Create a conda environment after opening the repository

```bash
conda create -n medibot python=3.12.7 -y
```

```bash
conda activate medibot
```


### STEP 02- install the requirements
```bash
pip install -r requirements.txt
```


### Create a `.env` file in the root directory and add your Pinecone & openai credentials as follows:

```ini
PINECONE_API_KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
OPENAI_API_KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```


```bash
# run the following command to store embeddings to pinecone
python store_index.py
```

```bash
# Finally run the following command
python app.py
```

Now,
```bash
open up localhost:
```
