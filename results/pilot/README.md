# Pilot Experiment Results

This folder contains the results from the paired pilot comparing Retrieval-Augmented Generation (RAG) with direct long-context prompting.

The pilot evaluated 12 LongBench v2 questions using Gemini 3.8 Flash under both methods.

## Results

| Method       | Correct Answers | Accuracy |
| ------------ | --------------: | -------: |
| Long Context |            7/12 |    58.3% |
| RAG Top-5    |            8/12 |    66.7% |

All 24 evaluations were completed successfully and passed the data integrity checks.

The results are preliminary and do not establish that one method is generally better than the other.

## Files

- `paired_pilot_12_v1.csv` — Question-level predictions, correctness, input-token usage, generation latency, and retrieval measurements.

The research methodology, pilot implementation, and error analysis are documented in `notebooks/05_paired_pilot.ipynb`.
