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

📊 Dataset & TaxonomyVetBench is structured into three primary categories and further divided into 10 sub-disciplines1:CategorySub-disciplinesBasic Vet MedAnatomy & Histology, Physiology & Biochemistry, Pathology, PharmacologyPreventive Vet MedMicrobiology & Immunology, Parasitology, Public HealthClinical Vet MedInternal Medicine, Surgery, Obstetrics & AndrologyThe benchmark includes 11 Task Types designed to probe different cognitive capabilities:IDTask TypeMetricCountQA-1Single ChoiceAccuracy276QA-2Multiple AnswerMacro-F1276QA-3Fill-in-the-BlankLLM Judge (A/B/C)276QA-4Open-ended GenerationRubric-based Score276SUMSummarization & ExtractionBERTScore552RCReading ComprehensionWeighted Avg1380🚀 Getting Started1. InstallationBashgit clone [https://github.com/your-username/VetBench.git](https://github.com/your-username/VetBench.git)
cd VetBench
pip install -r requirements.txt
2. Prompt FormatWe utilize a Global System Message strategy to enforce role-play and formatting constraints. For tasks requiring reasoning (e.g., QA-2, QA-4), we implement a configurable CoT (Chain-of-Thought) Switch.Example Configuration (prompts/qa2_multiple_choice.json):JSON{
  "inference": {
    "system_message": "You are an expert in the field of veterinary medicine... Select ALL correct options...",
    "cot_trigger": "Think like a clinical veterinarian and explain your reasoning step-by-step...",
    "current_input": "Question: {question}\nOptions: {options}\nCorrect answer:"
  }
}
3. Running EvaluationWe recommend using EvalScope for standardized assessment2.Bash# Example: Evaluate a model on QA-1 tasks
python scripts/eval_vetbench.py \
    --model_name_or_path "deepseek-ai/deepseek-v3" \
    --task "qa1" \
    --shot 5 \
    --use_cot True
🏆 Leaderboard (Selected)Below is a summary of model performance from our paper3.ModelTypeAverage ScoreCost EfficiencyClaude 4.5 OpusProprietary87.20LowGemini 3.0 ProProprietary86.68MediumGLM 4.7Open-Source85.20HighDeepSeek-v3.2Open-Source84.70ExtremeBioGPT-LargeDomain-Specific42.42-Note: Our results reveal a "Specialization Paradox" where generalist models significantly outperform existing biology-focused models4.⚖️ LicenseThis dataset is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).✍️ CitationIf you find VetBench useful for your research, please cite our paper:代码段@article{vetbench2025,
  title={VetBench: A Comprehensive Benchmark for Veterinary Large Language Models},
  author={Your Name and Co-authors},
  journal={ArXiv preprint},
  year={2025}
}
🙏 AcknowledgementsThis work is supported by the Red Bird MPhil Program at the Hong Kong University of Science and Technology (Guangzhou)5. We thank the veterinary experts who contributed to the data validation process.
