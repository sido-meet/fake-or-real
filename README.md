fake_or_real
============

Purpose
-------
The `fake_or_real` folder contains experiments and notebooks that explore self-consistency reasoning for Text->SQL generation and verification. It was used to run evaluation with Qwen3 8B and compute a self-consistency score.

Key results
-----------
| Method name / Setting | Model     | Technique                       | Score   | Notes |
|-----------------------:|:----------|:--------------------------------|:-------:|:------|
| Self-consistency       | Qwen3 8B  | Self-consistency sampling + majority voting | 0.92116 | Baseline result reported in experiments |
| No self-consistency    | Qwen3 8B  | Single-chain decoding (no aggregation)     | 0.90663 | Single-run baseline (no aggregation) |
| Other method (example) | <model>   | <technique>                  | -       | Add rows as you try other ablations |

Contents
--------
- `infer_with_self_consistent.ipynb` - interactive notebook used to run inference with self-consistency sampling and inspect results.
- `STaR_with_self_consistent.ipynb` - notebook with additional experiments and analysis for self-consistency / STaR-style reranking.
- `data/self_consistent_results.csv` - aggregated experiment outputs and metrics.
- `data/star_train.csv` - training subset / examples used in experiments.

Quick reproduction (debug)
-------------------------
1. Prepare environment variables (example in a `.env` file):

    OPENAI_API_KEY=your_api_key
    OPENAI_API_BASE=https://api.example.com/v1

2. Create a lightweight Python environment (recommended Python 3.10+).

3. Open `infer_with_self_consistent.ipynb` and run the cells. The notebook is instrumented to:
   - sample multiple reasoning chains from the LLM (self-consistency);
   - extract the `<sql>...</sql>` outputs or fenced SQL blocks;
   - execute SQL on provided example DBs (if available) and aggregate results.

4. To run a headless debug run (non-interactive), open the notebook and convert the core cells into a small script that:
   - loads model settings (set model to Qwen3 8B),
   - samples N chains per question, parses `<sql>` tags, executes queries against a local sqlite copy, and aggregates answers using majority / consistency voting.

Notes
-----
- The reported 0.92116 score was obtained with Qwen3 8B using the self-consistency protocol; exact reproduction requires matching sampling temperature, number of samples, prompt templates, and the Spider-like DB copies used during evaluation.
- For fast iteration, mock the LLM responses or run with a small sample set.

Contact / Next steps
-------------------
If you want, I can:
- add a minimal runnable script that executes a small subset of the notebook steps (sampling + parse + execute) to reproduce the reported metric locally, or
- extract and parameterize the exact prompt, sampling parameters, and aggregation code from the notebook into a standalone script for CI.
