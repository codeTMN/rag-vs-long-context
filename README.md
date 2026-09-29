# RAG vs. Long-Context Prompting

## Overview

Large language models can now process very large amounts of text at once. That raises a practical question: if a model can read a long document directly, when is Retrieval-Augmented Generation (RAG) still useful?

This project compares two ways of giving information to a language model for question answering:

- **Long-context prompting:** give the model the full source context and ask it to find the information it needs.
- **Retrieval-Augmented Generation (RAG):** search the source material first, retrieve the most relevant passages, and give only those passages to the model.

The goal is not simply to build a RAG system. The goal is to experimentally measure how the two approaches compare as the amount of available context increases.

## Research Question

**How does Retrieval-Augmented Generation compare with direct long-context prompting for question answering as the amount of available context increases?**

The final comparison will focus on measures such as answer accuracy, input size, response time, and retrieval-related measurements where they can be evaluated reliably.

## Why This Matters

RAG is widely used because it lets a language model work with large collections of information without placing everything into one prompt. However, newer models now support much larger context windows.

That creates a real tradeoff. Giving the model the full context avoids retrieval failures, but very large prompts can be slower, more expensive, and harder for the model to use effectively. RAG reduces the amount of text sent to the model, but it can fail when the retrieval step does not find the information needed to answer the question.

This project is designed to test that tradeoff under controlled conditions rather than assume that either method is always better.

## Dataset

The project uses **LongBench v2**.

The full benchmark contains **503 examples** across several long-context task types. Because this research focuses specifically on question answering, the candidate pool was narrowed to:

- **175 Single-Document QA examples**
- **125 Multi-Document QA examples**
- **300 total QA candidates**

The QA candidate pool contains:

| Context category | Examples |
| --- | ---: |
| Short | 132 |
| Medium | 121 |
| Long | 47 |
| **Total** | **300** |

The original contexts vary greatly in size, from thousands of words to more than one million words.

## Model Feasibility and Exact Tokenization

Several long-context model options were reviewed for context capacity, cost, accessibility, and reproducibility. **Gemini 3.8 Flash** was selected as the primary model for the current experiment.

Because model context limits are measured in tokens rather than words, all 300 QA candidates were measured using Gemini's actual token-counting endpoint.

Gemini 3.8 Flash reports an input limit of **1,048,576 tokens**.

The exact-tokenization results were:

- **300 / 300** candidate prompts successfully measured
- **299** prompts fit within Gemini's full input limit
- **1** prompt exceeded the limit and was excluded from full-context sampling

Median prompt sizes were approximately:

| Context category | Median exact tokens |
| --- | ---: |
| Short | 24,484 |
| Medium | 108,439 |
| Long | 352,030 |

The largest candidate prompt contained approximately **1.87 million tokens**, so it could not be used as a genuine full-context prompt without truncation. It was excluded rather than shortened, because truncation could remove information needed to answer the question and unfairly weaken the long-context condition.

## Frozen Experimental Sample

From the 299 Gemini-feasible candidates, the final experimental sample was selected **before any question-answering model results were observed**.

The sample contains **120 unique questions**, balanced across QA type and context length:

| QA type | Short | Medium | Long | Total |
| --- | ---: | ---: | ---: | ---: |
| Single-Document QA | 20 | 20 | 20 | 60 |
| Multi-Document QA | 20 | 20 | 20 | 60 |
| **Total** | **40** | **40** | **40** | **120** |

Sampling approximately preserves sub-domain representation within each QA-type/context-length group and uses the fixed random seed:

`20260928`

The frozen manifest is stored at:

`samples/experimental_sample_v1.csv`

Its SHA-256 checksum is:

`eaed00fa4b0f2093b8bd3d004879f7ca5bd7a378c9d9a9622f00861ebd56e436`

The checksum provides a reproducible fingerprint of the exact sample used for the experiment.

## Current Progress

Completed work:

- Created the project repository and research structure.
- Loaded and explored all 503 LongBench v2 examples.
- Narrowed the benchmark to 300 relevant QA candidates.
- Analyzed task type, context length, sub-domain distribution, and data quality.
- Compared candidate long-context language models for feasibility.
- Selected Gemini 3.8 Flash as the primary experimental model.
- Measured exact Gemini token counts for all 300 QA candidates.
- Identified 299 prompts that fit the full Gemini context window.
- Constructed a balanced, reproducible 120-question experimental sample.
- Validated that all 120 selected IDs are unique and fit the Gemini input limit.
- Saved and checksummed the frozen sample before observing model-answering performance.

No main question-answering results have been collected yet.

## Research Notebooks

The project is organized as a sequence of reproducible notebooks:

- `notebooks/01_dataset_exploration.ipynb` — explores LongBench v2 and defines the QA candidate pool.
- `notebooks/02_model_feasibility.ipynb` — compares model/context feasibility and rough experiment cost.
- `notebooks/03_exact_tokenization.ipynb` — measures exact Gemini prompt sizes and determines the feasible pool.
- `notebooks/04_experimental_sampling.ipynb` — constructs and validates the frozen 120-question sample.

## Planned Experiment

The same frozen questions and the same Gemini model will be used across experimental conditions so that the main variable is **how source information is supplied**.

The planned comparison is:

1. **Direct long-context prompting**
2. **RAG with a small set of top-ranked retrieved passages**
3. Additional retrieval depths if they are useful and remain within the project scope

Results will be analyzed across:

- Single-Document vs. Multi-Document QA
- Short vs. Medium vs. Long contexts

## Next Steps

The next stage is the first actual model-inference experiment.

1. Build the direct long-context baseline.
2. Run a small pilot to validate answer parsing, latency measurement, logging, retries, and result storage.
3. Run the baseline on the frozen 120-question sample.
4. Build the RAG retrieval pipeline.
5. Run the same frozen questions under the RAG conditions.
6. Compare accuracy and efficiency across methods and context lengths.
7. Perform error analysis and summarize the final findings.

## Repository Structure

```text
rag-vs-long-context/
├── figures/        # Figures and visualizations
├── notebooks/      # Research and experimental notebooks
├── samples/
│   └── experimental_sample_v1.csv
├── README.md
└── requirements.txt
```

## Tools Used So Far

- Python
- Google Colab Pro
- Hugging Face Datasets
- Pandas / NumPy
- Matplotlib
- Gemini API / REST API
- Git and GitHub
