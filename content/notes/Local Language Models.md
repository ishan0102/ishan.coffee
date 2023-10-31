---
title: Local Language Models
date: 2023-10-04
tags:
  - seed
enableToc: false
---
Will keep track of local LM releases here.
- Mistral-7B: https://github.com/mistralai/mistral-src
- MPT-30B
- LLaMa 2-7B
- Code-LLaMa
- Hermes 2: https://huggingface.co/teknium/OpenHermes-2-Mistral-7B
- [skunkworks model](https://huggingface.co/SkunkworksAI/BakLLaVA-1)
- mistral 7b instruct hosted on pplx
- pplx is gonna have a RAG api as well
- [](https://t.co/5zT0C3QBjQ)

Good way to run open source models:
- https://ollama.ai/

An idea for how to make model inference cheaper and faster:
- wouldn’t that involve caching each prompt + storing activation state for each prompt or most frequent prompts? I imagine it’s rare for two prompts to be the exact same unless they cache “similar enough” prompts or something, which would take up a lot of storage potentially
- this is a cool idea for speeding up open source models though: 
	- embed user inputs
	- cluster to figure out most frequent inputs and cache a model activation for one of them
	- on new input, check if you can find a close enough activation
- 

Mistral funding memo: https://drive.google.com/file/d/1gquqRqiT-2Be85p_5w0izGQGgHvVzncQ/view?usp=drivesdk
was wondering what the strategy was for startups raising 100m+ (eg mistral adept) who have only released FOSS models, no product