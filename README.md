<div align="center">

# VetBench: A Benchmark for Evaluating Large Language Models in Veterinary Medicine

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](License)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE-APACHE-2.0)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE-MIT)

![VetBench Framework](asset/workflow.png)

</div>

## Overview

VetBench is a multi-task benchmark for evaluating large language models (LLMs) on veterinary and animal-health text. It covers three pillars (basic, preventive, and clinical veterinary medicine), ten sub-disciplines, and eleven task types. The released benchmark files contain 3,036 evaluation items: 276 items for each task file.

The benchmark distinguishes between passage-unsupported tasks (QA-1--QA-4), in which the source passage is withheld at inference time, and passage-supported tasks (RC-1--RC-4), in which the corresponding passage is supplied as the only reference. RC-5 is a passage-supported sub-discipline classification task.

## Repository contents

```text
.
├── text/
│   └── core_passages.txt       # Released source-passage text
├── problem_bank/                # 11 JSONL task files (276 items each)
│   ├── QA1.jsonl                # Multiple-choice
│   ├── QA2.jsonl                # Multiple-answer
│   ├── QA3.jsonl                # Fill-in-the-blank
│   ├── QA4.jsonl                # Open-ended generation
│   ├── SUM1.jsonl               # Summarization
│   ├── SUM2.jsonl               # Key-information extraction
│   ├── RC1.jsonl                # Passage-supported multiple-choice
│   ├── RC2.jsonl                # Passage-supported multiple-answer
│   ├── RC3.jsonl                # Passage-supported fill-in-the-blank
│   ├── RC4.jsonl                # Passage-supported open-ended generation
│   └── RC5.jsonl                # Sub-discipline classification
├── Prompt/                      # Task prompt templates
├── few_shot/                    # Few-shot examples
└── README.md
```

The current GitHub snapshot is the benchmark asset release. Its released `core_passages.txt` contains 276 line records, whereas the revised-paper leaderboard was computed over 1,932 passage-level units. The snapshot does not include the paper's private/custom EvalScope adapter, raw model predictions, judge outputs, or bootstrap-analysis scripts. Consequently, the commands below are useful for checking data loading and running inference, but they do not by themselves reproduce the paper's leaderboard.

## Task definitions and official metrics

| Task | Input setting | Metric used for the paper | Items |
|---|---|---:|---:|
| QA-1 | Passage unsupported; single choice | Accuracy | 276 |
| QA-2 | Passage unsupported; multiple answer | Mean set-level F1 | 276 |
| QA-3 | Passage unsupported; fill in the blank | Reference-guided LLM scorer | 276 |
| QA-4 | Passage unsupported; open generation | Rubric-based LLM scorer | 276 |
| SUM-1 | Passage unsupported; summarization | BERTScore-F1 | 276 |
| SUM-2 | Passage unsupported; extraction | BERTScore-F1 | 276 |
| RC-1 | Passage supported; single choice | Accuracy | 276 |
| RC-2 | Passage supported; multiple answer | Mean set-level F1 | 276 |
| RC-3 | Passage supported; fill in the blank | Reference-guided LLM scorer | 276 |
| RC-4 | Passage supported; open generation | Rubric-based LLM scorer | 276 |
| RC-5 | Passage supported; sub-discipline classification | Accuracy | 276 |

For QA-1/QA-2 and RC-1/RC-2, the correct option labels are stored in `answer`. For RC-5, the labels are stored in `primary answer` and (where applicable) `secondary answer`. Generation tasks use `query`/`response` or `query`/`answer`, depending on the task file; inspect the first JSONL record before writing a custom adapter.

## Running with EvalScope

Install EvalScope (the commands were checked against EvalScope 1.3.0):

```bash
python -m pip install "evalscope>=1.3.0"
```

### 1. Smoke test: load QA-1 and run one model

EvalScope's built-in `general_mcq` adapter can be used to verify the released QA-1 file and generate predictions. The `dataset_id` points to a local JSONL file; it is not a ModelScope dataset name.

```bash
evalscope eval \
  --model <MODEL_ID_OR_LOCAL_CHECKPOINT> \
  --model-id <MODEL_ID> \
  --datasets general_mcq \
  --dataset-args '{"general_mcq":{"dataset_id":"problem_bank/QA1.jsonl"}}' \
  --generation-config '{"temperature":0.0,"top_p":0.8,"top_k":50,"max_tokens":2048}' \
  --eval-batch-size 20 \
  --seed 42 \
  --work-dir outputs/qa1
```

For an API-served model, add `--eval-type service --api-url "$API_URL" --api-key "$API_KEY"`. For a local checkpoint, use the checkpoint path in `--model` and the appropriate `--model-args` for that checkpoint. The outputs and task configuration are written under `outputs/qa1`.

### 2. Run another released task file

Change both the adapter and the local file in `--dataset-args`. For example, the generic QA adapter can load QA-3, but its built-in BLEU/ROUGE scoring is only a smoke-test metric and is **not** VetBench's reference-guided judge:

```bash
evalscope eval \
  --model <MODEL_ID_OR_LOCAL_CHECKPOINT> \
  --model-id <MODEL_ID> \
  --datasets general_qa \
  --dataset-args '{"general_qa":{"dataset_id":"problem_bank/QA3.jsonl"}}' \
  --generation-config '{"temperature":0.0,"top_p":0.8,"top_k":50,"max_tokens":2048}' \
  --eval-batch-size 20 \
  --seed 42 \
  --work-dir outputs/qa3_smoke
```

`general_mcq` is appropriate only for a loading/inference smoke test on QA-1, QA-2, RC-1, RC-2, or RC-5; `general_qa` is appropriate only for a loading/inference smoke test on QA-3, QA-4, SUM-1, SUM-2, RC-3, or RC-4. The official metrics in the table above require a VetBench-specific adapter and scoring pipeline, which are not included in this repository snapshot.

## Verified leaderboard

The table below replaces the four-row table that was previously in this README. Values were checked against the revised manuscript's main-results table (Table 2), not inferred from the JSONL answer keys.

**Aggregation.** Scores are on a 0--100 scale. `RC1--4` is one aggregated passage-supported QA component, so the four closely related RC tasks are not given four times the weight. `Avg` is the weighted mean over the reported dimensions and three independent runs. The interval is a two-sided 95% passage-level cluster-bootstrap CI over the 1,932 source passages (10,000 replicates). The leaderboard is therefore not reproducible from the released JSONL files alone.

| Rank | Model | Group | Params | Avg | 95% CI |
|---:|---|---|---:|---:|---:|
| 1 | Claude 4.5 Opus | Proprietary | — | **87.06** | 86.73--87.42 |
| 2 | Gemini 3.0 Pro | Proprietary | — | 86.55 | 86.18--86.91 |
| 3 | GPT-5.2 | Proprietary | — | 86.23 | 85.90--86.58 |
| 4 | Claude 4.5 Sonnet | Proprietary | — | 85.56 | 85.19--85.95 |
| 5 | GLM 4.7 | General-purpose open source | 358B | 85.07 | 84.68--85.42 |
| 6 | DeepSeek-v3.2 | General-purpose open source | 685B | 84.59 | 84.15--84.96 |
| 7 | Qwen3-235B | General-purpose open source | 235B | 84.37 | 83.92--84.79 |
| 8 | GPT-OSS | General-purpose open source | 120B | 83.74 | 83.22--84.20 |
| 9 | Kimi K2 | General-purpose open source | 1000B | 83.64 | 83.18--84.09 |
| 10 | Claude 4.5 Haiku | Proprietary | — | 83.46 | 82.98--83.91 |
| 11 | Mistral Large 3 | General-purpose open source | 675B | 83.36 | 82.86--83.83 |
| 12 | Gemini 3.0 Flash | Proprietary | — | 83.09 | 82.58--83.56 |
| 13 | Grok 4.1 Fast | Proprietary | — | 83.07 | 82.49--83.53 |
| 14 | Llama 4 Maverick | General-purpose open source | 17B | 82.39 | 81.80--82.91 |
| 15 | Med42-v2 | Domain-specialized | 70B | 80.21 | 79.34--80.93 |
| 16 | PMC-LLaMA | Domain-specialized | 13B | 56.73 | 56.00--57.42 |
| 17 | BioGPT-Large | Domain-specialized | 1.5B | 42.40 | 41.57--43.11 |

The model identifiers/access dates used in the revised evaluation are documented in the paper supplementary material. Model comparisons should be interpreted with the stated caveats: proprietary and open-source systems were accessed through different providers, domain-specialized checkpoints were locally deployed, and the benchmark measures controlled text tasks rather than clinical or operational readiness.

## Citation and license

Please cite the VetBench paper when using the benchmark. The dataset and benchmark materials are available under [CC BY 4.0](License). Software/code materials are available under the [Apache License 2.0](LICENSE-APACHE-2.0) and [MIT License](LICENSE-MIT).
