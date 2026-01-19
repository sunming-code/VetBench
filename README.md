🐾 VetBench: A Comprehensive Benchmark for Veterinary Large Language Models<div align="center">[🏠 Home Page] | [🤗 Dataset] | [📖 Paper]</div>📖 IntroductionVetBench is the first comprehensive, multi-task benchmark specifically designed for the veterinary domain under the "One Health" framework1.To bridge the gap between "technological potential" and "clinical productivity," VetBench systematically evaluates Large Language Models (LLMs) across the three core pillars of Basic, Preventive, and Clinical Veterinary Medicine, covering 10 sub-disciplines22. The benchmark comprises 3,036 high-quality questions generated via a "Hybrid Construction Pipeline" involving rigorous expert validation3333.<div align="center"><img src="assets/taxonomy.png" width="80%" alt="VetBench Taxonomy"><em>Figure 1: The Taxonomy of VetBench covering 10 expert-curated subcategories4.</em></div>🔔 News[2026-01-xx] 🚀 VetBench paper is released on arXiv.[2026-01-xx] 📂 The full dataset (problem_bank), source texts (text), and evaluation prompts (prompt) are open-sourced.🏆 LeaderboardWe evaluated mainstream proprietary, open-source, and domain-specific models. Below is the performance overview (Sorted by Average Score)5555.RankModelTypeSizeAverageBasic (QA1)Clinical Reasoning (RC1-RC4)🥇Claude 4.5 OpusProprietary-87.2096.5898.86🥈Gemini 3.0 ProProprietary-86.6898.5597.79🥉GPT-5.2Proprietary-86.3496.3397.394GLM 4.7Open-Source358B85.2094.2096.485DeepSeek-v3.2Open-Source685B84.7097.1097.836Qwen3Open-Source235B84.4996.3898.07.....................-BioGPT-LargeDomain1.5B42.4220.2969.99📉 The Specialization Paradox: Generalist models significantly outperform existing biology-focused models (e.g., BioGPT, PMC-LLaMA) in veterinary tasks6.📂 Repository StructureThis repository is organized as follows:Plaintext.
├── 📂 problem_bank/       # [Core Dataset] Contains 3,036 curated questions
│   ├── basic/             # Anatomy, Physiology, Pathology, Pharmacology
│   ├── preventive/        # Microbiology, Parasitology, Public Health
│   └── clinical/          # Internal Med, Surgery, Obstetrics
│
├── 📂 text/               # [Source Material] 
│   └── core_passages.json # 276 expert-selected core passages from papers & books [cite: 53]
│
├── 📂 prompt/             # [Evaluation Prompts]
│   ├── inference.json     # Prompts for model inference (Standard & CoT)
│   └── judge.json         # Prompts for GPT-5.2 as the "Optimal Judge" [cite: 71]
│
├── 📂 few_shot/           # [ICL Samples]
│   ├── 0_shot.json
│   ├── 1_shot.json        # Demonstrations used for In-Context Learning analysis [cite: 126]
│   └── 5_shot.json
│
└── README.md
🧠 Data & TasksVetBench evaluates models on 11 distinct task types across 3 paradigms, ensuring a panoramic assessment of cognitive capabilities7777.1. Task TaxonomyDiscriminative Tasks: Single Choice (QA1), Multiple Choice (QA2), Classification (RC5).Lexical Precision Tasks: Fill-in-the-Blank (QA3) — Requires exact terminology retrieval.Generative Tasks: Open-ended QA (QA4), Summarization (SUM1/SUM2), Reading Comprehension.2. Dataset StatisticsThe dataset is balanced across disciplines based on veterinary curricula8:Basic Vet Med: 38.76%Preventive Vet Med: 30.08%Clinical Vet Med: 31.16%🔍 Key Insights from the Paper1. Anthropocentric BiasOur error analysis reveals that models often incorrectly transfer human anatomical knowledge to animal contexts.Example: Describing a dog's greater omentum as fused (human-like) instead of having separate leaves9999.2. The "Cost-Intelligence" Trade-offWhile Claude 4.5 Sonnet (Reasoning) achieves the highest accuracy, it incurs significant costs. DeepSeek-v3.2 offers extreme cost-efficiency, situated on the Pareto Frontier10101010.<div align="center"><img src="assets/cost_benefit.png" width="60%" alt="Cost Benefit Analysis"></div>3. Reasoning vs. FormattingFew-shot learning (ICL) primarily helps with format correction (e.g., label space mapping) rather than unlocking deep reasoning capabilities for weaker models11.🚀 Quick StartInstallationBashgit clone https://github.com/YourOrg/VetBench.git
cd VetBench
pip install -r requirements.txt
Loading DataPythonimport json

# Load the problem bank
with open('problem_bank/clinical/internal_medicine.json', 'r') as f:
    data = json.load(f)

# Load the corresponding prompt
with open('prompt/inference.json', 'r') as f:
    prompts = json.load(f)

print(f"Loaded {len(data)} questions.")
🖊️ CitationIf you use VetBench in your research, please cite our paper:代码段@article{vetbench2026,
  title={VetBench: A Comprehensive Benchmark for Veterinary Large Language Models},
  author={Your Name and Co-Authors},
  journal={arXiv preprint arXiv:2501.xxxxx},
  year={2026}
}
📄 LicenseThis project is licensed under the Creative Commons Attribution 4.0 International (CC BY 4.0).<div align="center"><sub>Detailed definitions of veterinary competencies can be found in Appendix A of the paper.</sub></div>
