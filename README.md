# VetBench: A Comprehensive Benchmark for Veterinary Large Language Models

<div align="center">

![VetBench Taxonomy](assets/taxonomy_fig.png) [![Paper](https://img.shields.io/badge/Paper-ArXiv-red)](https://arxiv.org/abs/placeholder) [![License](https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg)](LICENSE)
[![Data](https://img.shields.io/badge/Data-HuggingFace-yellow)](https://huggingface.co/datasets/placeholder)

</div>

## 📖 Introduction

[cite_start]**VetBench** is the first comprehensive, multi-task benchmark designed to evaluate Large Language Models (LLMs) within the **"One Health"** framework of veterinary medicine[cite: 3, 12].

While LLMs show transformative potential in healthcare, their application in veterinary medicine is hindered by a lack of rigorous evaluation standards. [cite_start]VetBench addresses this gap by covering **3 core pillars** (Basic, Preventive, and Clinical Veterinary Medicine) across **10 sub-disciplines**[cite: 4, 33].

### Key Features
* [cite_start]**Full-Spectrum Coverage:** 3,036 high-quality questions derived from 276 expert-curated core passages[cite: 4, 53].
* [cite_start]**Hybrid Construction Pipeline:** Combines automated generation with rigorous expert validation (Human-in-the-loop)[cite: 43].
* [cite_start]**Multi-Task Evaluation:** Includes 11 distinct task types ranging from basic knowledge retrieval (QA) to complex clinical reasoning (Case Analysis)[cite: 15].
* [cite_start]**Rigorous Metrics:** Utilizes a "Meta-Evaluation" validated Judge Model (GPT-5.2) and specific metrics (Accuracy, Macro-F1, BERTScore) for different task paradigms[cite: 70, 71].



## 📂 Repository Structure

This repository contains the dataset, prompt templates, and evaluation scripts used in the VetBench paper.

```text
.
├── data/
│   ├── core_passages.json       # 276 High-quality veterinary text segments [cite: 53]
│   ├── questions/               # 3,036 Generated questions divided by task ID [cite: 61]
│   │   ├── qa1_single_choice.json
│   │   ├── qa2_multiple_choice.json
│   │   ├── ...
│   │   └── rc5_classification.json
│   └── few_shot_examples.json   # Expert-verified k-shot examples for ICL [cite: 56]
├── prompts/                     # Optimized System & User Prompts (JSON format)
│   ├── qa1_prompt.json
│   ├── qa3_prompt.json          # Includes Judge instructions
│   └── ...
├── scripts/                     # Evaluation scripts based on EvalScope 
└── README.md
