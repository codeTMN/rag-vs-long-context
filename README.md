# RAG vs. Long-Context Prompting

## Overview

Large language models can now process increasingly large amounts of text at once. This raises an interesting question: if a model can read a very long document directly, do we still need Retrieval-Augmented Generation (RAG)?

This project investigates that question by comparing two approaches for answering questions from long documents:

- **Long-context prompting:** giving the language model the full available context and asking it to find the information it needs.
- **Retrieval-Augmented Generation (RAG):** first searching the source material for the most relevant passages and then giving only those passages to the language model.

The goal is not simply to build a RAG system. The goal is to experimentally determine how the two approaches compare as the amount of available context increases.

## Research Question

**How does Retrieval-Augmented Generation compare with direct long-context prompting for question answering as the amount of available context increases?**

The project will examine both effectiveness and efficiency. Important measures will include answer accuracy, input size, response time, and other retrieval-related measurements where appropriate.

## Why This Matters

RAG is widely used because it allows a language model to work with large collections of information without placing everything into a single prompt. However, newer language models support much larger context windows than earlier models.

This creates a practical tradeoff. Supplying the full context may avoid retrieval errors, but extremely large prompts can be expensive and difficult for a model to use effectively. RAG can greatly reduce the amount of information sent to the model, but it can fail if the retrieval system does not find the information needed to answer the question.

Understanding when each approach works well can help developers make better decisions when building systems that answer questions from large documents or collections of documents.

## Dataset

The project is currently using **LongBench v2** as the candidate evaluation benchmark.

The complete benchmark contains **503 examples** across several types of long-context tasks. Initial exploration showed that many of these tasks do not directly match the question-answering focus of this project.

The candidate pool was therefore narrowed to:

- **175 Single-Document QA examples**
- **125 Multi-Document QA examples**
- **300 total question-answering examples**

The QA subset contains:

| Context Category | Examples |
| ---------------- | -------: |
| Short            |      132 |
| Medium           |      121 |
| Long             |       47 |
| **Total**        |  **300** |

The dataset analysis also showed that context sizes vary substantially. Some QA contexts contain approximately 8,000 words, while the largest contains more than 1.1 million words.

Because language-model context limits are measured in tokens rather than words, the final experimental sample will be selected only after a model and its tokenizer have been chosen.

## Current Progress

The first stage of the project focused on understanding the benchmark before building the experimental systems.

Completed work includes:

- Created the project repository and research structure.
- Loaded LongBench v2 in Google Colab.
- Examined the structure of individual benchmark examples.
- Analyzed all 503 examples by task type, difficulty, and context length.
- Narrowed the candidate benchmark to 300 question-answering examples.
- Examined Single-Document QA and Multi-Document QA separately.
- Analyzed context-length and sub-domain distributions.
- Verified basic dataset quality.
- Confirmed that the QA subset contains no duplicate IDs, missing answers, or invalid answer labels.
- Created an initial context-length distribution figure.

## Planned Experiment

The core experiment will compare the same questions under different ways of supplying context to a language model.

The initial experimental conditions are expected to include:

1. **Direct long-context prompting**
2. **RAG using the top retrieved passages**
3. Additional RAG retrieval depths where useful

The same language model, questions, answer format, and evaluation procedure will be used across conditions so that the main variable being tested is how the context is provided.

The experiment will also examine performance across different context lengths and across both Single-Document and Multi-Document QA.

## Next Steps

The next stage of the project is model selection and experimental preparation.

The immediate work will be to:

1. Select a suitable language model for the controlled experiment.
2. Use that model's tokenizer to calculate exact context sizes in tokens.
3. Determine which benchmark examples fit within the model's supported context window.
4. Create a reproducible experimental sample.
5. Implement the direct long-context baseline.
6. Build the RAG retrieval pipeline.
7. Run pilot experiments before scaling to the main evaluation.

Examples that cannot fit in the selected model's context window will not be silently truncated and treated as full-context examples.

## Repository Structure

```text
rag-vs-long-context/
├── figures/        # Figures and visualizations from the experiments
├── notebooks/      # Research and experimental notebooks
├── README.md
└── requirements.txt
```
