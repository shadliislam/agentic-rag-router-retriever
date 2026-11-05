# Agentic RAG: Router-Retriever System with PDF and Web Search

A multi-agent Retrieval-Augmented Generation (RAG) system built with **CrewAI** that routes user questions between a Transformer research paper and current web information.

## Project Overview

This project implements an Agentic RAG workflow using two specialized AI agents:

- **Router Agent** - determines whether a question should use PDF or WEB retrieval.
- **Retriever Agent** - generates an answer grounded in retrieved evidence.

The system supports two retrieval paths:

- **PDF** - questions related to the supplied Transformer research paper.
- **WEB** - questions requiring current, recent, or external information.

## Architecture

```text
User Question
      |
      v
Router Agent
      |
      +------ PDF ------> PDFSearchTool ------> PDF Evidence
      |
      +------ WEB ------> Tavily Search ------> Web Evidence
                                                  |
                                                  v
                                           Retriever Agent
                                                  |
                                                  v
                                           Grounded Answer
```

## Features

- Multi-agent orchestration with CrewAI
- Intelligent PDF/Web routing
- PDF semantic search with PDFSearchTool
- Local Sentence Transformer embeddings
- ChromaDB vector storage
- Tavily web search
- Azure OpenAI integration through the course gateway
- Custom CrewAI `BaseLLM`
- Grounded answer generation
- Asynchronous agent execution in Jupyter
- Operational trace logging

## Technologies Used

| Technology | Purpose |
|---|---|
| Python 3.12.3 | Programming language |
| Jupyter Notebook | Development and demonstration |
| CrewAI | Multi-agent orchestration |
| PDFSearchTool | PDF semantic retrieval |
| Sentence Transformers | Local embeddings |
| all-MiniLM-L6-v2 | Embedding model |
| ChromaDB | Vector storage and similarity search |
| Tavily | Current web search |
| Azure OpenAI | LLM used by the agents |
| CrewAI BaseLLM | Custom course gateway integration |

## Knowledge Source

The PDF retrieval system uses the research paper **Attention Is All You Need**.

The document contains information about:

- Transformer architecture
- Scaled dot-product attention
- Multi-head attention
- Positional encoding
- Encoder-decoder architecture
- Training
- Experimental results

## Router Agent

The Router Agent analyzes each question and selects one retrieval source:

- `PDF` for information contained in the Transformer research paper.
- `WEB` for recent, current, changing, or external information.

## Retriever Agent

The Retriever Agent receives evidence from the selected retrieval source and generates a grounded response.

The Retriever Agent is instructed to:

- Use only the supplied evidence
- Avoid unsupported information
- Produce clear and concise answers
- Identify the information source
- State when the available evidence is insufficient

## PDF Retrieval

PDF retrieval is implemented using `PDFSearchTool`.

The project uses the local Sentence Transformer model `all-MiniLM-L6-v2` for embeddings. ChromaDB stores the generated embeddings and supports semantic similarity search.

Using a local embedding model eliminates the need for a separate embedding API.

## Web Retrieval

Tavily Search is used for questions requiring current or external information. The retrieved web evidence is passed to the Retriever Agent for grounded response generation.

## Azure OpenAI Integration

The course environment provides Azure OpenAI access through an API Management gateway. The standard CrewAI Azure integration was not compatible with the supplied gateway URL structure.

To resolve this, a custom CrewAI LLM implementation named `CourseAzureLLM` was created using `BaseLLM`. This allows the Router Agent and Retriever Agent to communicate with the course-provided endpoint.

## End-to-End Workflow

The final system provides a reusable asynchronous function:

```python
await run_agentic_rag(question)
```

The workflow:

1. Accepts a user question.
2. Sends the question to the Router Agent.
3. Selects either `PDF` or `WEB`.
4. Executes the selected retrieval tool.
5. Collects retrieved evidence.
6. Sends the evidence to the Retriever Agent.
7. Generates a grounded response.
8. Records an operational trace.

## Test Results

| Test Question | Expected | Actual | Result |
|---|---|---|---|
| What is scaled dot-product attention? | PDF | PDF | PASS |
| Why does the Transformer use positional encoding? | PDF | PDF | PASS |
| What are the latest developments in Transformer architectures? | WEB | WEB | PASS |

## Logging and Traceability

Each execution records:

- Timestamp
- User question
- Router decision
- Retrieval tool
- Information source
- Final answer
- Execution duration

This provides workflow traceability without exposing private model chain-of-thought.

## Challenges and Solutions

### Python Environment

The course VM used an externally managed Python installation, preventing direct installation of the necessary packages.

**Solution:** A Python 3.12 virtual environment was created for the project.

### PDF Embeddings

`PDFSearchTool` initially expected an OpenAI embedding configuration.

**Solution:** Local Sentence Transformer embeddings using `all-MiniLM-L6-v2` were configured.

### Azure API Gateway

CrewAI's standard Azure provider generated a URL structure that was incompatible with the course API Management gateway.

**Solution:** A custom CrewAI `BaseLLM` adapter was created to communicate directly with the course endpoint.

### Jupyter Asynchronous Execution

Synchronous CrewAI execution conflicted with Jupyter's active event loop.

**Solution:** The agents were executed asynchronously using:

```python
await crew.akickoff()
```

## Example Questions

### PDF Retrieval

- What is multi-head attention?
- What is scaled dot-product attention?
- Why does the Transformer use positional encoding?

### Web Retrieval

- What are the latest developments in Transformer architectures?

## Engineering Trade-offs

- Local embeddings eliminate the need for another embedding API but may differ in retrieval quality from larger commercial embedding models.
- PDF retrieval provides controlled and reproducible evidence but is limited to the supplied document.
- Web retrieval provides current information but depends on search-result quality.
- Specialized agents improve modularity and traceability but require additional LLM calls.
- The custom LLM adapter provides compatibility with the course Azure gateway but adds implementation complexity.

## Security

API credentials are intentionally **not included in this repository**.

Never commit:

- `.env`
- `credential.txt`
- `credentials.txt`
- Azure API keys
- Tavily API keys

Sensitive credentials should be provided securely at runtime.

## Project Deliverables

- Jupyter Notebook source code
- Project write-up PDF
- Execution screenshots PDF
- Source-code PDF
- Transformer research paper dataset

## Author

**Shadli Islam**

## Conclusion

This project demonstrates a working Agentic RAG architecture combining multi-agent orchestration with multiple retrieval sources.

The Router Agent intelligently chooses between PDF and web retrieval, while the Retriever Agent produces answers grounded in retrieved evidence.

The final system demonstrates:

- Multi-agent coordination
- Intelligent source routing
- Semantic PDF retrieval
- Current web search
- Local vector embeddings
- Grounded response generation
- Azure OpenAI integration
- Operational traceability
