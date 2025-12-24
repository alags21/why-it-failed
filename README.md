# why-it-failed
Code and results from "Why It Failed: A Benchmark to Evaluate Interpretability"


## Gemma-2-2B Benchmark Evaluations

This repository tracks my experiments running the `google/gemma-2-2b` model across three reasoning datasets (BoolQ, WinoGrande, and Social IQa). Each run streams through the source dataset, scores prompts with Gemma, and collects 2,000 correct and 2,000 incorrect generations so I can inspect balanced slices of the model's behavior.

## Evaluation Recipe

- The scripts in `evaluation_scripts/` handle dataset-specific prompting and scoring:
  - `evalute_gemma_boolq.py` for BoolQ.
  - `evaluate_gemma_winogrande.py` for WinoGrande.
  - `evaluate_gemma_siqa.py` for Social IQa (SIQA).
- Every script logs in to Hugging Face (if an `HF_TOKEN` is set), loads Gemma-2-2B, and picks the best answer via log-likelihood comparisons.
- As soon as both the `correct` and `incorrect` buckets contain 2,000 samples each, the sweep stops and the buckets are written to `evaluation_results/<dataset>_2000_correct_incorrect.json`.

## Datasets Used

### BoolQ – Yes/No Reading Comprehension

- **Dataset:** [google/boolq](https://huggingface.co/datasets/google/boolq) contains short passages paired with yes/no questions sourced from Google search queries.
- **Dataset:** [allenai/winogrande](https://huggingface.co/datasets/allenai/winogrande) is a fill-in-the-blank test of pronoun/coreference resolution that probes common-sense reasoning.
- **Dataset:** [allenai/social_i_qa](https://huggingface.co/datasets/allenai/social_i_qa) presents short social situations and multiple-choice questions about intentions, reactions, or descriptions.

## Result Files

The balanced slices for each dataset live under `evaluation_results/`. They can be readily loaded with `json.load` for error analysis, fine-tuning data prep, or qualitative inspection of what triggers correct vs. incorrect behavior.