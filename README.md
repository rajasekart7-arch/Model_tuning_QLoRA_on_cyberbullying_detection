**🛡️ Trust & Safety Classification using QLoRA**

**📌 Overview**

This project builds a Trust & Safety (T&S) classifier to detect harmful content in social media text.
The model classifies content into:

normal → safe or non-harmful content
offensive → abusive or insulting language toward an individual
hatespeech → harmful or discriminatory content targeting a group

Using QLoRA (Quantized Low-Rank Adaptation), the project efficiently fine-tunes a large language model on a curated dataset.

**🚀 Key Results**

Metric	Before QLoRA	After QLoRA
Accuracy	0.52	0.64
Precision	0.50	0.63
Recall	0.52	0.64
F1 Score	0.49	0.63

👉 Achieved ~12% improvement in accuracy and significant gains in recall and F1.

**🧠 Key Challenges**

1. Label Noise

* Many hatespeech examples were incorrectly labeled as offensive
* Safe content (criticism/opinion) mislabeled as offensive

2. Class Boundary Confusion

* Model initially failed to distinguish:
    "This idea is stupid" → normal
    "You are stupid" → offensive
    "People like you are stupid" → hatespeech

3. Model Collapse
* Early versions predicted a single dominant class (e.g., offensive)

**🛠️ Approach**

🔹 1. Data Cleaning
* Removed noisy tokens (<user>, <number>, encoding artifacts)
* Normalized text for better model understanding

🔹 2. Weak Relabeling

* Introduced rule-based correction to fix label noise:
* Identity + attack → hatespeech
* Opinion without insult → normal

🔹 3. Contrastive Learning (Key Innovation)

* Added examples to explicitly teach class boundaries:
  "You are stupid" → offensive  
  "This idea is stupid" → normal  
  "People like you are stupid" → hatespeech  
  This significantly improved class separation.

🔹 4. Prompt Engineering

  Designed structured prompts with:
  clear label definitions
  strict output constraints
  decision rules for classification

🔹 5. QLoRA Fine-Tuning

**Used:**

  Low-rank adapters (r=8)
  4-bit quantization
  efficient training on limited GPU resources
  
**⚙️ Model Configuration**

* Model: Qwen/Qwen2.5-1.5B-Instruct
* LoRA Rank: 8
* Learning Rate: 5e-5
* Epochs: 2
* Batch Size: 2 (effective 8 with accumulation)
* Optimizer: paged_adamw_8bit

**📊 Evaluation**

Evaluation performed using:

* Accuracy
* Precision (weighted)
* Recall (weighted)
* F1 Score

Also used:

* Confusion Matrix
* Error Analysis
  
**🔍 Error Analysis Insights**
  Initial model over-predicted offensive
  After tuning:
  Improved detection of hatespeech
  Reduced false positives on normal
  Balanced classification across all classes

**📈 Final Outcome**

**The model now:**

Accurately distinguishes normal vs offensive vs hatespeech
Handles real-world noisy and ambiguous language
Generalizes better with improved recall and precision

**🧠 Key Learnings**

Data quality > model complexity
Label noise can significantly degrade performance
Contrastive examples are critical for semantic boundary learning
Over-tuning can reduce generalization

**🔮 Future Improvements**

Add more diverse identity-based datasets
Improve multi-lingual support
Incorporate context-aware classification
Deploy as real-time moderation API

**📂 Tech Stack**

Python
PyTorch
Hugging Face Transformers
PEFT (QLoRA)
TRL
scikit-learn
pandas

**💡 Conclusion**

This project demonstrates how efficient fine-tuning + data-centric improvements can significantly enhance Trust & Safety systems, even with limited compute.
