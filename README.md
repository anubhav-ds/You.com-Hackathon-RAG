#Project Name: RAG Pipeline V2 Project**

This repo contain retrieval-augmented generation (RAG) pipeline made for You.com Agentic Hackathon. It is a RAG Pipeline focused on Financial and Legal documents, focusing on high relative accuracy and retriever performance. We used LlamaIndex for the overall RAG pipeline and used three different you.com agents, all based on GPT 5 Mini, which we integrated in the pipeline and made over 1700 API calls during the project.

We obtained 100% Relative Numerical Accuracy, 85% MMR, 100% HitRate@10, and 87% Recall@10 with 20.5 seconds mean latency. RAG Pipelne parsed documents more than 1000 pages and was optimized for multiple document types including xls, xlsx, doc, docx, pdf, jpg, png, pdf, and html. Main architecture of the pipeline:

1. Used LlamaParse to handle document types by using their Agentic AI for parser to produce results in a markdown format while using high quality OCR on cloud.
2. Used You.com custom agents for Logical Document Creation and Metadata Tagging which involved RAG_DoctypeClassifier and RAG_PageContinue custom Agents.
3. Made a custom retriever with Hybrid retrieval (FAISS dense + BM25 lexical) with query fusion, Metadata filtering and query routing, and cross-encoder reranking.
4. Used gemini-2.5-flash for RAG LLM and bge big eng v1 model for embeddings.
5. Created modular SubQuestionQueryEngine that decomposes complex questions and routes them to per-tool query engines.

In this repo we have have provided pickle objects for Documents and Logical Documents, saving the need of parsing the documents and making API Calls for a review.

## What is in this Repo?
1. you_rag.ipynb — end-to-end notebook: load data → chunk → build index → hybrid retrieval → rerank → SubQuestion query → evaluation.
3. logical_train.pkl — logical documents already grouped and ready to chunk. Use this to skip parsing.
3. documents_train_meta.pkl, metadata_train.pkl — metadata used in experiments (optional for a quick run).
4. requirements.txt — pip dependencies.
5. environment.yaml — Conda environment (alternative to requirements.txt).
6. README.md — this file.

## Prerequisites
1. Python 3.10+ recommended
2. Optionally Conda (miniconda/anaconda) if you prefer environment.yaml
3. Internet access for model downloads (sentence-transformers reranker) and Gemini API calls

## Environmental 
Both requirements and environment.yaml files have been provided.

Use following code for requirements:
pip install -U pip
pip install -r requirements.txt

And following for conda environment:
conda env create -f environment.yaml
conda activate rag

## Configure API Keys
You.com API and Gemini API keys are essential, LlamaParse API is needed if reader wants to parse new documents.

Save following data in an .env text file in root folder for API.

GOOGLE_API_KEY=your_gemini_key
YOU_API_KEY=your_youcom_key
LLAMA_CLOUD_API_KEY=your_llamacloud_key

## Quickstart
We have provided logical documents in pickle format, featuring 630 logical documents and they can be subsequently used in the jupyter notebook in a conda environment in this form:
1. Load Logical documents
2. Run Chunking (Recursive + Semantic)
3. Build FAISS Index and BM25, with and without metadata filtering
4. Query with SubQuestionQueryEngine which break the complex queries into parts and use different query engines to process them, while using soft metadata filtering with Global BM25 Fallback if metadata are messy

## Evaluation
We used 15 test queries to evaluate the pipeline. We evaluated SubQuestionQueryEngine for accuracy and relative accuracy metrics, obtained 87% accuracy and 100% relative numerical accuracy. And we created a base retriever without metadat filtering to test general retriever performance. We obtained 85% MMR and 100% HitRate@10 alongwith 87% Recall@10, signifying that our retriver was picking up a lot of right gold nuggets near the top.

We could improve the pipeline, espeically Recall, by actually improving the RAG Pipeline Agents as RAG_PageContinue especially was not able to decide properly if a document continues or not in case of similar sounding documents like employee contract, contractor contract, and company policies, all which can contain very similar information. This area needs more testing, but nevertheless by using you.com Custom Agent API I was able to create a highly scalable RAG pipeline which worked quite well with a lot of scope of improvement.  
