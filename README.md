VetBench: A Comprehensive Benchmark for Veterinary Large Language ModelsVetBench is the first comprehensive, multi-task benchmark designed to evaluate Large Language Models (LLMs) in the domain of veterinary medicine.Constructed under the "One Health" framework, VetBench systematically covers 3 core pillars (Basic, Preventive, and Clinical Veterinary Medicine) across 10 sub-disciplines, comprising 3,036 high-quality questions generated via a "Hybrid Construction Pipeline" involving rigorous expert validation.(Note: Please upload Figure 1 from your paper to an assets folder and link it here)🔔 News[2026-01]: VetBench paper is released on arXiv![2026-01]: Dataset and evaluation code are open-sourced.🏆 LeaderboardHere is a summary of the performance of mainstream proprietary, open-source, and domain-specific models evaluated on VetBench.RankModelTypeAverage ScoreQA1 (Basic)Clinical Reasoning (RC1-RC4)🥇Claude 4.5 OpusProprietary87.2096.5898.86🥈Gemini 3.0 ProProprietary86.6898.5597.79🥉GPT-5.2Proprietary86.3496.3397.394GLM 4.7Open-Source85.2094.2096.485DeepSeek-v3.2Open-Source84.7097.1097.83..................-BioGPT-LargeDomain42.4220.2969.99Key Insight: Our results reveal a "Specialization Paradox" where generalist models (e.g., Claude 4.5 Opus) significantly outperform existing biology-focused models (e.g., BioGPT).📂 Repository StructureThis repository contains the data and prompts used in VetBench.Plaintext.
├── problem_bank/      # The core dataset containing 3,036 questions
│   ├── basic_vet/     # Anatomy, Physiology, Pathology, Pharmacology
│   ├── preventive/    # Microbiology, Parasitology, Public Health
│   └── clinical/      # Internal Medicine, Surgery, Obstetrics
├── prompt/            # Prompt templates for evaluation and generation
│   ├── inference/     # Prompts used for model inference (Zero-shot/CoT)
│   └── judge/         # Prompts used for the LLM-as-a-Judge (GPT-5.2)
├── few_shot/          # Examples used for In-Context Learning experiments
│   ├── 1_shot.json
│   ├── 3_shot.json
│   └── ...
├── text/              # Source corpora and core passages
│   └── core_passages.json # The 276 high-quality expert-verified text segments
└── README.md
🧠 Dataset DetailsVetBench covers the full spectrum of veterinary knowledge:Basic Veterinary Medicine: Anatomy & Histology, Physiology & Biochemistry, Pathology, Pharmacology.Preventive Veterinary Medicine: Microbiology & Immunology, Parasitology, Public Health.Clinical Veterinary Medicine: Clinical Internal Medicine, Veterinary Surgery, Obstetrics & Andrology.The benchmark includes 11 task types ranging from knowledge retrieval to complex reasoning:Q&A: Single Choice, Multiple Choice, Fill-in-the-Blank, Open-ended Generation.Summarization: Simple Summarization, Key Information Extraction.Reading Comprehension: Subcategory Classification, etc.🚀 Usage1. Installation(Assuming you might use a requirements.txt or specific environment)Bashgit clone https://github.com/YourUsername/VetBench.git
cd VetBench
pip install -r requirements.txt
2. EvaluationWe recommend using EvalScope for standardized evaluation. You can load the prompts from the prompt/ folder.Example script (pseudo-code):Pythonfrom eval_scope import Evaluator
from vetbench_loader import load_data

# Load questions from problem_bank
data = load_data('./problem_bank')

# Load Prompts
prompts = load_prompts('./prompt/inference')

# Run Evaluation
# Ensure you have API keys set for proprietary models or GPUs for open-source models
evaluator = Evaluator(model="deepseek-v3.2", dataset=data, prompts=prompts)
results = evaluator.run()
🔍 Key Analysis & Insights1. The Specialization ParadoxDomain-specific models like BioGPT and PMC-LLaMA currently underperform compared to state-of-the-art generalist models. This suggests that the "generalist capability" currently dominates in the veterinary domain due to the lack of high-quality, large-scale veterinary pre-training data.2. Anthropocentric BiasOur error analysis (refer to text/ or paper for case studies) reveals a critical bias where models incorrectly transfer human anatomical and medical knowledge to animal contexts (e.g., describing a dog's omentum as fused like a human's).3. Cost-Benefit AnalysisOpen-source models like DeepSeek-v3.2 demonstrate extreme cost-efficiency, challenging the dominance of proprietary models while maintaining high accuracy.(Note: Please upload Figure 7 from your paper here)🖊️ CitationIf you find VetBench useful for your research, please cite our paper:代码段@article{vetbench2026,
  title={VetBench: A Comprehensive Benchmark for Veterinary Large Language Models},
  author={Author One and Author Two and Author Three},
  journal={arXiv preprint arXiv:24xx.xxxxx},
  year={2026}
}
📄 LicenseThis dataset is licensed under the CC BY 4.0 License.🙏 AcknowledgementsWe thank the Red Bird MPhil Program at HKUST(GZ) for their support. We also acknowledge the use of EvalScope for our evaluation framework.
