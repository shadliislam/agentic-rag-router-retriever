# Agentic RAG: Router-Retriever System

## Overview

This project implements an Agentic Retrieval-Augmented Generation (RAG) system using CrewAI.

The system contains two specialized agents:

- **Router Agent** - selects PDF or WEB retrieval.
- **Retriever Agent** - generates answers grounded in retrieved evidence.

## Architecture

User Question → Router Agent → PDF/Web Retrieval → Retriever Agent → Grounded Answer

## Features

- CrewAI multi-agent orchestration
- PDFSearchTool
- Sentence Transformer embeddings
- ChromaDB
- Tavily web search
- Azure OpenAI
- Custom CrewAI BaseLLM
- Operational trace logging

## Technologies

- Python 3.12.3
- Jupyter Notebook
- CrewAI
- PDFSearchTool
- all-MiniLM-L6-v2
- ChromaDB
- Tavily
- Azure OpenAI

## Test Results

| Question | Expected | Actual | Result |
|---|---|---|---|
| Scaled dot-product attention | PDF | PDF | PASS |
| Positional encoding | PDF | PDF | PASS |
| Latest Transformer developments | WEB | WEB | PASS |

## Challenges and Solutions

### PDF Embeddings

The course environment did not provide a separate embedding deployment.

**Solution:** Local `all-MiniLM-L6-v2` Sentence Transformer embeddings were used.

### Azure Integration

The standard CrewAI Azure integration was incompatible with the course API gateway.

**Solution:** A custom CrewAI `BaseLLM` implementation was created.

### Jupyter Async Execution

CrewAI synchronous execution conflicted with Jupyter's running event loop.

**Solution:** Asynchronous execution was used with `await crew.akickoff()`.

## Security

API credentials are not included in this repository.

Never commit:

- Azure API keys
- Tavily API keys
- credential.txt
- .env files

## Author

Shadli Islam
