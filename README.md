# 🏥 Medical X-Ray Report Generator

### Vision-Language Model for Chest X-Ray Caption Generation

A research prototype that connects **computer vision and language modeling** to generate text descriptions from chest X-ray images.

The system uses a pretrained **CLIP vision encoder**, a trainable **projection layer**, and a pretrained **GPT-2 language model** to create a lightweight multimodal architecture inspired by the design principles behind models such as LLaVA.

> ⚠️ **Research Prototype — Not a Clinical Diagnostic System**
> This project is intended for research and educational purposes. It has not been clinically validated and must not be used for medical diagnosis or patient-care decisions.

---

## 🧠 Overview

Medical imaging contains complex visual information that can be difficult to translate into natural-language reports.

This project explores a simple Vision-Language architecture:

```text
             Chest X-Ray
                  │
                  ▼
        ┌──────────────────┐
        │   CLIP Vision    │
        │     Encoder      │
        └────────┬─────────┘
                 │
            512-d features
                 │
                 ▼
        ┌──────────────────┐
        │    Projection    │
        │      Layer       │
        └────────┬─────────┘
                 │
            768-d features
                 │
                 ▼
        ┌──────────────────┐
        │      GPT-2       │
        │  Language Model  │
        └────────┬─────────┘
                 │
                 ▼
        Generated Caption
```

The key idea is to transform visual features into the same embedding space used by GPT-2 and provide the projected image representation as a **visual token** before the text sequence.

---

# 🚀 Features

* 🖼️ Image-to-text generation
* 👁️ CLIP-based visual representation
* 🧠 GPT-2 language generation
* 🔗 Trainable vision-language projection
* ❄️ Frozen pretrained CLIP encoder
* ❄️ Frozen pretrained GPT-2
* ⚡ GPU acceleration with CUDA
* 📦 PyTorch implementation
* 🧪 Custom PyTorch Dataset and DataLoader
* 🔄 Autoregressive token generation
* 💾 Model checkpoint saving/loading
* 🏗️ Lightweight multimodal architecture

---

# 🏗️ Architecture

The model consists of three major components.

## 1. CLIP Vision Encoder

The project uses:

```text
openai/clip-vit-base-patch32
```

CLIP converts the input image into a dense visual representation.

```python
image_features = CLIP(image)
```

The resulting representation is passed to the projection network.

---

## 2. Vision-Language Projection

The projection layer maps the CLIP representation into GPT-2's embedding dimension.

```text
CLIP
512 dimensions
      │
      ▼
Linear Projection
      │
      ▼
GPT-2
768 dimensions
```

Implementation:

```python
class SimpleProjection(nn.Module):
    def __init__(self):
        super().__init__()
        self.linear = nn.Linear(512, 768)

    def forward(self, x):
        return self.linear(x)
```

Only this component is trained in the current experiment.

### Trainable Parameters

```text
393,984
```

CLIP and GPT-2 remain frozen.

---

## 3. GPT-2 Language Decoder

The language component uses:

```text
gpt2
```

The projected image representation is inserted as a visual token:

```text
[VISUAL TOKEN] [TEXT TOKENS]
```

These embeddings are passed into GPT-2 through:

```python
inputs_embeds=combined
```

GPT-2 then predicts the next token autoregressively.

---

# 🔬 Training Strategy

The current prototype uses parameter-efficient multimodal alignment.

Instead of fine-tuning billions of parameters, the experiment freezes the pretrained models:

```text
CLIP          → Frozen ❄️
GPT-2         → Frozen ❄️
Projection    → Trainable 🔥
```

This significantly reduces the number of parameters that need to be optimized.

### Optimization

```text
Optimizer: Adam
Learning Rate: 1e-4
Epochs: 3
Batch Size: 2
Trainable Parameters: 393,984
Device: CUDA
```

---

# 🧪 Current Experiment

The initial experiment uses a **small synthetic dataset** containing four dummy images and example captions.

Example captions:

```text
Normal chest X-ray with clear lungs

Pneumonia detected in lower left lobe

Mild pleural effusion on right side

Clear bilateral lungs without disease
```

The purpose of this experiment was to verify the complete multimodal pipeline:

```text
Image
  ↓
CLIP
  ↓
Projection
  ↓
Visual Token
  ↓
GPT-2
  ↓
Autoregressive Generation
```

It is **not sufficient for medical performance evaluation**.

---

# 📊 Training Results

The prototype successfully completed three training epochs.

| Epoch | Average Loss |
| ----: | -----------: |
|     1 |       8.0697 |
|     2 |       7.7373 |
|     3 |       7.5410 |

The decreasing loss confirms that the trainable projection layer was being optimized during the experiment.

However, these results should **not** be interpreted as evidence of medical understanding or clinical accuracy.

---

# ⚠️ Important Experimental Limitation

The current training data is intentionally tiny and synthetic.

Only four dummy images were used.

Therefore:

* ❌ No medical diagnostic accuracy can be established
* ❌ No clinical conclusions can be drawn
* ❌ The generated reports are not medically reliable
* ❌ The experiment does not represent real-world X-ray performance
* ❌ The model has not been clinically validated

The experiment demonstrates the **engineering pipeline**, not a production medical AI system.

---

# 🤖 Inference

After training, the model can generate text autoregressively.

The generation process is:

```text
Input X-Ray
    │
    ▼
CLIP Image Features
    │
    ▼
Projection Layer
    │
    ▼
Visual Token
    │
    ▼
GPT-2
    │
    ▼
Next Token
    │
    ▼
Next Token
    │
    ▼
...
    │
    ▼
Generated Caption
```

Example:

```python
caption = generate_caption(
    model,
    image,
    max_tokens=40
)
```

---

# 💾 Model Checkpoint

The trained model can be saved with:

```python
torch.save(
    model.state_dict(),
    "minillava_model.pt"
)
```

Load it later with:

```python
model = MiniLLaVA_Simple()

model.load_state_dict(
    torch.load("minillava_model.pt")
)
```

---

# 🛠️ Technology Stack

| Technology                | Purpose           |
| ------------------------- | ----------------- |
| Python                    | Programming       |
| PyTorch                   | Deep Learning     |
| Hugging Face Transformers | CLIP + GPT-2      |
| CLIP                      | Vision Encoder    |
| GPT-2                     | Language Decoder  |
| PIL                       | Image Processing  |
| CUDA                      | GPU Acceleration  |
| tqdm                      | Training Progress |

---

# 📁 Project Structure

A recommended repository structure:

```text
medical-xray-report-generator/
│
├── notebook/
│   └── medical_xray_vlm.ipynb
│
├── src/
│   ├── model.py
│   ├── dataset.py
│   ├── train.py
│   └── inference.py
│
├── checkpoints/
│   └── minillava_model.pt
│
├── README.md
├── requirements.txt
└── LICENSE
```

---

# ⚙️ Installation

Clone the repository:

```bash
git clone https://github.com/SalvaPiol45Tech/medical-xray-report-generator.git

cd medical-xray-report-generator
```

Install dependencies:

```bash
pip install torch torchvision transformers pillow tqdm
```

For GPU training, install the appropriate CUDA-enabled PyTorch version for your system.

---

# ▶️ Running the Project

Start the notebook:

```bash
jupyter notebook
```

Then run the cells sequentially.

The pipeline will:

```text
1. Load CLIP
2. Load GPT-2
3. Freeze pretrained models
4. Create projection layer
5. Build dataset
6. Train projection
7. Generate captions
8. Save checkpoint
```

---

# 🧬 From Prototype to Real Medical AI

The next stage is replacing the synthetic experiment with a real chest X-ray dataset.

Potential research datasets include:

* CheXpert
* MIMIC-CXR
* IU X-Ray
* Open-i

A real training pipeline would require:

```text
Real X-Ray Images
        +
Medical Reports
        ↓
Dataset Cleaning
        ↓
Train / Validation / Test Split
        ↓
Image Preprocessing
        ↓
CLIP Feature Extraction
        ↓
Vision-Language Alignment
        ↓
GPT-2 / Modern LLM Fine-Tuning
        ↓
Medical Report Generation
        ↓
Evaluation
```

---

# 📈 Future Improvements

## Vision-Language Architecture

* Replace the single visual token with multiple visual tokens
* Use CLIP patch-level features
* Add cross-attention
* Add a Q-Former-style connector
* Experiment with larger vision encoders
* Experiment with modern vision-language models

## Language Model

* Replace GPT-2 with a stronger decoder
* Experiment with instruction-tuned LLMs
* Fine-tune using LoRA/QLoRA
* Improve medical vocabulary handling
* Add structured report generation

## Medical Training

* Train on real chest X-ray/report pairs
* Handle multiple findings per image
* Model uncertainty
* Address class imbalance
* Separate findings from clinical impressions
* Evaluate hallucination rates

## Evaluation

Potential metrics include:

```text
BLEU
ROUGE
METEOR
CIDEr
BERTScore
Clinical efficacy metrics
CheXpert-based label agreement
Hallucination analysis
```

For medical report generation, text similarity alone is not enough; clinical correctness and factual consistency are important research considerations.

---

# 🔬 Research Questions

This project can be extended into several research directions:

### 1. Visual Token Alignment

Can a lightweight projection layer effectively align visual representations with a frozen language model?

### 2. Parameter-Efficient Multimodal Learning

How much multimodal performance can be obtained while training only a small connector?

### 3. Medical Vision-Language Generation

Can general-purpose vision and language representations be adapted to medical report generation?

### 4. Hallucination Reduction

How can a vision-language model reduce clinically incorrect findings that are not supported by the image?

### 5. Efficient VLM Training

Can parameter-efficient methods such as LoRA produce competitive results with substantially lower computational requirements?

---

# 🧠 What This Project Demonstrates

This prototype demonstrates the fundamental mechanics of a Vision-Language Model:

```text
Computer Vision
       +
Representation Learning
       +
Multimodal Alignment
       +
Language Modeling
       =
Vision-Language Generation
```

More specifically, it demonstrates how a visual representation can be transformed into a language-model-compatible representation and injected into a pretrained autoregressive language model.

---

# ⚠️ Medical Disclaimer

This repository is a research and educational project.

It is **not a medical device**, diagnostic system, clinical decision-support system, or substitute for professional medical advice.

Generated text may be incorrect, incomplete, misleading, or clinically unsafe.

Do not use outputs from this project to diagnose, treat, or make decisions about patients.

---

# 📚 References

* Radford et al. — *Learning Transferable Visual Models From Natural Language Supervision*
* Radford et al. — *Improving Language Understanding by Generative Pre-Training*
* Liu et al. — *Visual Instruction Tuning*
* Irvin et al. — *CheXpert: A Large Chest Radiograph Dataset with Uncertainty Labels and Expert Comparison*
* Johnson et al. — *MIMIC-CXR: A Large Publicly Available Database of Labeled Chest Radiographs*

---

# 👨‍💻 Author

**Salva Piol**

AI / Machine Learning Engineer focused on:

```text
Python
PyTorch
Deep Learning
Computer Vision
Transformers
Multimodal AI
Vision-Language Models
AI Agents
Reasoning & Planning
```

---

## ⭐ Project Status

```text
Status: Research Prototype
Architecture: CLIP + Projection + GPT-2
Training: Completed on synthetic demonstration data
Inference: Working
Medical Validation: Not performed
Real Dataset Training: Planned
```

If you find the project useful, consider giving the repository a ⭐.
