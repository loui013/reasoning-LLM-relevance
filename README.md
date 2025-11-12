# reasoning-LLM-relevance
Resources created as part of the paper "Reasoning with Large Language Models for Relevance Judgements". This repository contains the output files for all four runs using the full TREC 2023 Deep Learning dataset in `.jsonl` format.

`judgements_gpt-oss-low.jsonl` : gpt-oss-120b with reasoning_effort set to low

`judgements_gpt-oss-high.jsonl`: gpt-oss-120b with reasoning_effort set to high

`judgements_gemini-2.5-flash-0.jsonl`: Gemini 2.5-Flash with thinking_budget set to 0

`judgements_gemini-2.5-flash-500.jsonl`: Gemini 2.5-Flash with thinking_budget set to 500

All files are structured the following way:
```
{
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
  "elapsed time": float # Total time from first response to completion of current line in seconds
}
```
## Interesting queries (extracted from different models & runs):

### "Overthinking" (Model changes answer after long reasoning chain)

`Query`: how do you spell with asl

`Passage`: Submit Definition.Close. Note. The ASL fingerspelling provided here is most commonly used for proper names of people and places; it is also used in some languages for concepts for which no sign is available at that moment.

`Human relevance score`: 0

`Reasoning`:
    Evaluating Relevance to Query:I'm currently focused on the specific scoring criteria for relevance. I'm taking the query "how do you spell with asl" and comparing it to the passage "Submit Definition. Close. Note. The ASL fingers". I'm trying to figure out which score, 0-3, best fits the degree of relevance, according to the specified definitions, **and I believe it's going to be a 0**.Refining the Relevance Assessment:I've been meticulously considering the user's intent: understanding *how* to spell using ASL. The passage discusses ASL fingerspelling, but it does so in a tangential way. I'm focusing on the specific scoring definitions, which emphasizes understanding the method of spelling. Based on my analysis, **I still believe the most appropriate score is a 0,** given the lack of direct instruction.Revisiting Scoring Criteria:I'm now deep-diving into the nuances of scoring. The passage, while related to ASL fingerspelling, fails to address the "how" aspect the user seeks. Focusing on the scoring definitions, **a score of 1 seems the most appropriate**. The passage is demonstrably connected to the query's topic, but it fails to provide the instructional content the user needs.
    
`LLM relevance score`: 1

### Misinterpreting spelling mistakes

```Query```: what is considered poulty

`Passage`: Poultices. A poultice is a moist paste usually made from ground herbs or foods mixed with water, oil or another liquid. A wide variety of poultices have been used historically as home remedies for boils and other skin infections 1. These pastes are usually applied to the affected area and covered.

`Human relevance score`: 0

`Reasoning (low)`: We need to output final score. Query: "what is considered poulty". Likely typo "poultry"? But passage about poultices. Not related. So score 0.

`LLM relevance score (low)`: 0

`Reasoning (high)`: We need to parse the problem. The user says: "Given a query and a passage, you must provide a score on an integer scale of 0 to 3 with the following meanings: ..." The query: "what is considered poulty". Actually it's likely "what is considered poultice"? **The query says "what is considered poulty". Possibly a typo for poultice?** The user wants us to evaluate how well this passage answers that question. The passage defines poultices: A poultice is a moist paste usually made from ground herbs or foods mixed with water... used historically as home remedies... applied... Thus this directly answers what poultice is - i.e., defines it. So it's relevant and provides exact answer. So likely category 3 because it's dedicated entirely to describing poultice definition. But we need also consider underlying intent - likely user wants definition/meaning of poultice/poulties? The phrase "what is considered poulty" might refer incorrectly spelled term; maybe they want meaning/definition. The passage gives definition exactly. Thus M= high match (maybe 3). T= trustworthiness moderate-high because it's factual description from presumably reliable source (though no citation beyond footnote). But trustworthiness not needed for scoring beyond internal weighting perhaps but final O should reflect overall rating based on both M \& T relative importance. Given high match \& decent trustworthiness => final O=3. Thus output format required: \#\#final score: <score> Only integer value after colon no reasoning. Thus produce: \#\#final score: 3 

`LLM relevance score (high)`: 3






