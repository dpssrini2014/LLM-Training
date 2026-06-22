# Day 1–5 Runbook: Execute and Understand the LLM-Training Demos

This document explains how to run and understand the materials in the LLM-Training repository (Day 1 → Day 5). It mirrors the exploration performed and provides reproducible steps, prerequisites, and troubleshooting notes.

## Summary of repository contents (at provided ref)
- day1/
  - code/: many plain-text examples demonstrating curl usage, streaming, tokenization, temperature effects, and file handling.
  - slides/
  - readme.txt
- day2/
  - code/
    - B2-VirtualKey-demo/: docker-compose demo with app, config, and tests.
    - B1-B7-demo/: (directory placeholder)
- day3/ & day4/
  - code/ and slides/ (placeholders; conceptual material)
- day5/
  - keyword_search_demo1.zip, pgvector_demo.zip, sample PDFs, slides/

## Prerequisites
- OS: Linux/macOS or Windows (WSL recommended)
- Tools: git, python 3.8+, pip, virtualenv (optional), docker, docker-compose, unzip, curl, jq
- API keys: export your LLM API key(s) as environment variables (example: OPENAI_API_KEY)

## Day 1 — Basic LLM interaction examples
Purpose: show raw HTTP/curl interactions, streaming behavior, tokenization, temperature, and examples of sending different file types to an LLM.

Key files:
- day1/code/0_Beginning_LLM_curl_commands.txt
- day1/code/1_ConnectToDifferentLLM.txt
- day1/code/1a_ConnectToLLMInStreamingMode.txt
- day1/code/2_LLM_200_Response.txt
- day1/code/3_LLM_4xx_Response.txt
- day1/code/4_TokenizationDemo.txt
- day1/code/5_SendDifferentFileContentToLLM.txt
- day1/code/6_Temperature_0_7.txt
- sample files: sample_claim.txt, sample_invoice.pdf, sample_invoice.png

Steps to run the examples:
1. Open the text files to inspect sample commands and JSON payloads.
2. Replace placeholder variables (e.g., $API_KEY, $MODEL_ENDPOINT) with your real keys and endpoints.
3. Example curl pattern:
   - export API_KEY="your_key"
   - curl -s -X POST "https://api.example.com/v1/completions" \
       -H "Authorization: Bearer $API_KEY" \
       -H "Content-Type: application/json" \
       -d '{"model":"gpt-xxx","prompt":"Hello","max_tokens":50}'
4. Tokenization demo: install the tokenizer library referenced (e.g., tiktoken) and run snippet(s) in 4_TokenizationDemo.txt to observe token counts.
5. Try different temperature values from 6_Temperature_0_7.txt to compare deterministic vs creative responses.
6. File-handling examples: test prompts from 5_SendDifferentFileContentToLLM.txt to see how text, images, and PDFs are passed to the model (embedding, summarization, extraction patterns).

Concepts to check:
- HTTP status handling (2xx vs 4xx)
- Streaming: incremental token arrival and assembly
- Token limits, truncation, and cost implications

## Day 2 — VirtualKey demo (dockerized app)
Purpose: demo app showing virtual key/model routing in a small service.

Key files:
- day2/code/B2-VirtualKey-demo/.env
- day2/code/B2-VirtualKey-demo/config.yaml
- day2/code/B2-VirtualKey-demo/docker-compose.yml
- day2/code/B2-VirtualKey-demo/startup_steps.txt
- day2/code/B2-VirtualKey-demo/app/app.py
- day2/code/B2-VirtualKey-demo/app/app_model_routing_test.py
- day2/code/B2-VirtualKey-demo/app/app_virtual_key_basic_test.py

Steps:
1. Inspect .env and config.yaml to identify required environment variables.
2. Create a local .env with your API keys (do not commit keys).
3. Start the demo with Docker Compose:
   - cd day2/code/B2-VirtualKey-demo
   - docker-compose up --build
4. Optional: run tests locally without Docker:
   - python -m venv venv && source venv/bin/activate
   - pip install -r requirements.txt  (if present)
   - pytest -q app/app_model_routing_test.py app/app_virtual_key_basic_test.py
5. Read app.py and the tests to understand routing logic and expected behavior.

Concepts to check:
- Model selection and routing rules (config.yaml)
- Virtual key abstraction and how it proxies to downstream LLMs

## Day 3 & Day 4
- Primarily slides and placeholders. Open the slides directories to review concepts that complement the demos.

## Day 5 — Search & vector demos
Purpose: Compare keyword search and vector (embedding) search; demonstrate pgvector usage.

Key files:
- day5/keyword_search_demo1.zip
- day5/pgvector_demo.zip
- day5/sample_banking_reference.pdf
- day5/sample_drug_reference.pdf
- day5/sample_solar_system.pdf

Steps:
1. unzip keyword_search_demo1.zip -d day5/keyword_search_demo1
2. unzip pgvector_demo.zip -d day5/pgvector_demo
3. Inspect each demo's README or scripts for exact instructions.
4. For pgvector demo:
   - Run a Postgres with pgvector (via Docker or local install)
   - Create DB and enable extension: CREATE EXTENSION IF NOT EXISTS vector;
   - Install demo dependencies and run the script that builds embeddings from the sample PDFs and stores vectors in Postgres.
   - Run similarity search queries and compare results to keyword search.

Concepts to check:
- Document chunking strategy and how it's embedded
- Embeddings vs keyword matching
- Indexing vectors and NN search parameters

## Troubleshooting tips
- Docker: use docker-compose logs <service> to inspect failures; ensure env vars are present.
- Missing packages: check Dockerfile and requirements.txt to install correct dependencies.
- Tests failing: verify network services are running or mock endpoints.
- API errors: inspect HTTP response codes and body for error details; 4xx usually indicates auth or invalid payload.

## Next steps and how I can help further
- I can expand this runbook into a notebook (Colab/Jupyter) with runnable examples.
- I can create scripts to automate demo setup (docker, DB, env file templates).
- I can walk through any day/file line-by-line and annotate the code.

---
Generated by an automated review of the repository at ref 232b47650f85ab5ed49c8b6d364797fa353ea423.
