# Experimental Samples

This directory contains the fixed sample manifests used in the project experiments.

## Experimental Sample V1

**File:** `experimental_sample_v1.csv`

**Rows:** 120

**Selection seed:** `20260928`

**SHA-256:**

`eaed00fa4b0f2093b8bd3d004879f7ca5bd7a378c9d9a9622f00861ebd56e436`

The sample contains:

- 60 Single-Document QA questions
- 60 Multi-Document QA questions
- 40 short-context questions
- 40 medium-context questions
- 40 long-context questions

Each of the six QA-type/context-length combinations contains exactly 20 questions.

The sample was selected and frozen **before any question-answering model performance was observed**. This prevents later replacement of difficult questions based on model results and keeps the experiment reproducible.
