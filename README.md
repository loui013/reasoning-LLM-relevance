# reasoning-LLM-relevance
Resources created as part of the paper "Reasoning with Large Language Models for Relevance Judgements". This repository contains the output files for all four runs using the full TREC 2023 Deep Learning dataset in `.jsonl` format.

`judgements_gpt-oss-low.jsonl` : gpt-oss-120b with reasoning_effort set to low

`judgements_gpt-oss-high.jsonl`: gpt-oss-120b with reasoning_effort set to high

`judgements_gemini-2.5-flash-0.jsonl`: Gemini 2.5-Flash with thinking_budget set to 0

`judgements_gemini-2.5-flash-500.jsonl`: Gemini 2.5-Flash with thinking_budget set to 500

All files are structured the following way:
```{
  "query_id": str, # Query ID
  "query": str, # Query
  "passage_id": str, # Passage ID
  "passage": str, # Passage
  "content": str, # LLM-generated response
  "reasoning": str, # LLM-generated reasoning
  "human_relevance": int, # Relevance label by human TREC judge
  "llm_relevance": int, # Relevance label assigned by LLM (-1 if not in format ##final score: X)
  "total tokens": int, # Total tokens used, i.e. input + output tokens
  "output tokens": int, # Tokens used for reasoning and final answer
  "elapsed time": float # Total time from first response to completion current line in seconds
}`
