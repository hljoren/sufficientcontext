# Sufficient Context: A New Lens on Retrieval Augmented Generation Systems

## Authors
Hailey Joren (UC San Diego), Jianyi Zhang (Duke University), Chun-Sung Ferng (Google), Da-Cheng Juan (Google), Ankur Taly (Google), Cyrus Rashtchian (Google)

## Abstract
Augmenting LLMs with context leads to improved performance across many applications. Despite much research on Retrieval Augmented Generation (RAG) systems, an open question is whether errors arise because LLMs fail to utilize the context from retrieval or the context itself is insufficient to answer the query. To shed light on this, we develop a new notion of sufficient context, along with a method to classify instances that have enough information to answer the query. 

## Key Findings
- Larger models with higher baseline performance (Gemini 1.5 Pro, GPT 4o, Claude 3.5) excel at answering queries when the context is sufficient, but often output incorrect answers instead of abstaining when the context is not sufficient
- Smaller models with lower baseline performance (Mistral 3, Gemma 2) hallucinate or abstain often, even with sufficient context
- SOTA LLMs output correct responses 35–62% of the time with insufficient context
- Our selective generation method improves the fraction of correct answers among model responses by 2–10% for Gemini, GPT, and Gemma

## Sufficient Context Examples
![Sufficient Context Examples](path/to/figure1.png)

*Figure 1: Examples of sufficient context and breakdown of model responses on the Musique dataset.*

## Model Performance with Sufficient vs. Insufficient Context
![Model Performance](path/to/figure3.png)

*Figure 3: Model performance on datasets stratified by sufficient context. Models have higher correct percentage with sufficient context but still hallucinate frequently.*

## Selective Generation Results
![Selective Generation](path/to/figure4.png)

*Figure 4: Coverage vs. Selective Accuracy. Our combined approach using sufficient context and self-rated confidence improves accuracy for most coverages.*

## Sufficient Context Autorater Prompt

```python
# Prompt used to determine if context is sufficient to answer a query
You are an expert LLM evaluator that excels at evaluating a QUESTION and REFERENCES.
Consider the following criteria:
Sufficient Context: 1 IF the CONTEXT is sufficient to infer the answer to the question and 0 
IF the CONTEXT cannot be used to infer the answer to the question
Assume the queries have timestamp <TIMESTAMP>.

First, output a list of step-by-step questions that would be used to arrive at a label for the
criteria. Make sure to include questions about assumptions implicit in the QUESTION.
Include questions about any mathematical calculations or arithmetic that would be required.
Next, answer each of the questions. Make sure to work step by step through any required
mathematical calculations or arithmetic. Finally, use these answers to evaluate the criteria.
Output the ### EXPLANATION (Text). Then, use the EXPLANATION to output the ###
EVALUATION (JSON)

### QUESTION
<question>
### REFERENCES
<context>
