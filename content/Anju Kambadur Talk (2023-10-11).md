---
title: Anju Kambadur Talk (2023-10-11)
date: 2023-10-11
tags:
  - seed
enableToc: false
---
- Queries and documents are tightly coupled and should be preprocessed as such
- Sparsity gets you precision but lacks context
- Preprocessing is very hard
- Documents are not just plaintext
- Enrichment is extremely important
- The pipeline is indexing, document enrichment, retrieval based on query, relevance ranking based on query context
- Dense retrieval is a lot more approximate (nearest neighbor)
- Can’t create vectors without losing lots of information
- LLMs aren’t a silver bullet because of hallucination, instability, model updates, latency, making them impossible to sub in for traditional pipelines
- They allow finetuning but data protection, latency, and cost are issues, fine tuned vendor models are way more expensive
- Paradigms: BERT = encoder, T5 = encoder decoder, GPT = decoder
- Bloomberg will build all of these with various data and sizes and allow for applications to be built on them
- Use the LLM at the end after traditional inference steps (retrieval, ranking) for summarization and QA (this is RAG)
- Do training while doing inference
- LLMs are useful annotators and can be used as teacher models, distillation to small transformers
- Queries will be more complex questions, we can have LLMs for input and output at the UI level
- All companies who are selling LLM products have a vested interest in making their papers and blog posts thinly veiled advertisements