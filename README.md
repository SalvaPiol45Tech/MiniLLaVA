# 🏥 Medical X-Ray Report Generator

### A Vision-Language Model for Chest X-Ray Image-to-Text Generation

<p align="center">

**CLIP → Vision-Language Projection → GPT-2**

</p>

<p align="center">

<a href="https://github.com/SalvaPiol45Tech">
<img src="https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github">
</a>

<a href="https://salvapiol45tech.github.io/medical-xray-report-generator/">
<img src="https://img.shields.io/badge/Project-Website-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white">
</a>

<a href="#-roadmap">
<img src="https://img.shields.io/badge/Roadmap-View-6C63FF?style=for-the-badge">
</a>

</p>

---

## 🔬 Overview

**Medical X-Ray Report Generator** is a multimodal AI research project exploring how a vision encoder and a language model can be connected to generate natural-language descriptions from chest X-ray images.

The current architecture combines:

```text
┌──────────────────────┐
│      Chest X-Ray     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│    CLIP ViT-B/32    │
│   Vision Encoder     │
│      Frozen ❄️       │
└──────────┬───────────┘
           │
        512-D
           │
           ▼
┌──────────────────────┐
│ Vision-Language      │
│    Projection        │
│    Trainable 🔥      │
└──────────┬───────────┘
           │
        768-D
           │
           ▼
┌──────────────────────┐
│        GPT-2         │
│   Language Decoder   │
│      Frozen ❄️       │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  Autoregressive Text │
│      Generation      │
└──────────────────────┘
```

The project focuses on the **engineering and research problem of multimodal alignment** rather than claiming clinical diagnostic capability.

> ⚠️ **This is a research prototype. It is not clinically validated and must not be used for diagnosis or patient-care decisions.**

---

# 🎯 Project Goal

The long-term goal is to investigate whether a relatively lightweight vision-language architecture can learn to transform chest X-ray visual representations into medically meaningful language.

The research direction is:

```text
Image
  ↓
Visual Representation
  ↓
Multimodal Alignment
  ↓
Language Representation
  ↓
Medical Language Generation
  ↓
Clinical Evaluation
```

The current implementation represents the **first experimental stage** of this roadmap.

---

# 🧠 Core Architecture

## Vision Encoder

The project uses:

```text
openai/clip-vit-base-patch32
```

CLIP converts the input image into a compact visual representation.

```text
Chest X-Ray
     ↓
CLIP Vision Transformer
     ↓
Visual Features
     ↓
512 dimensions
```

The CLIP model is frozen during the current experiment.

---

## 🔗 Vision-Language Connector

The visual representation cannot be passed directly into GPT-2 because the dimensionalities differ.

The project therefore introduces a trainable projection:

```text
512
 ↓
Linear Projection
 ↓
768
```

Implementation:

```python
class SimpleProjection(nn.Module):

    def __init__(self):
        super().__init__()

        self.linear = nn.Linear(
            512,
            768
        )

    def forward(self, x):
        return self.linear(x)
```

This connector contains:

```text
393,984 trainable parameters
```

Only this component is optimized in the current experiment.

---

# 🤖 Language Model

The language decoder uses:

```text
GPT-2
```

The projected visual representation becomes a visual token:

```text
[VISUAL TOKEN]
```

which is concatenated with the text embeddings:

```text
[VISUAL TOKEN] [TOKEN] [TOKEN] [TOKEN] ...
```

The resulting sequence is passed to GPT-2 through `inputs_embeds`.

GPT-2 then predicts the next token autoregressively.

---

# 🔄 Complete Inference Pipeline

```text
             INPUT IMAGE
                  │
                  ▼
          Image Preprocessing
                  │
                  ▼
             CLIP Encoder
                  │
                  ▼
          512-D Image Features
                  │
                  ▼
        Trainable Projection
                  │
                  ▼
          768-D Visual Token
                  │
                  ▼
        ┌─────────────────┐
        │   GPT-2 Decoder │
        └────────┬────────┘
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
        Generated Description
```

---

# 🧪 Current Experiment

The first experiment uses a deliberately small synthetic dataset to validate the complete multimodal pipeline.

The dataset contains four synthetic image-caption pairs:

```text
Normal chest X-ray with clear lungs

Pneumonia detected in lower left lobe

Mild pleural effusion on right side

Clear bilateral lungs without disease
```

The images are synthetic placeholders rather than real patient X-rays.

The purpose of this stage is to verify:

* image preprocessing
* CLIP encoding
* feature projection
* multimodal embedding construction
* GPT-2 forward pass
* loss computation
* backpropagation
* optimization
* autoregressive generation
* checkpoint saving/loading

---

# 📊 Current Results

The model successfully completed the complete training and inference pipeline.

### Training

| Epoch | Average Loss |
| ----: | -----------: |
|     1 |       8.0697 |
|     2 |       7.7373 |
|     3 |       7.5410 |

```text
Training Loss

8.0697  ────────┐
                │
7.7373  ────────┤ ↓
                │
7.5410  ────────┘
```

### Model

| Property             | Value             |
| -------------------- | ----------------- |
| Vision Encoder       | CLIP ViT-B/32     |
| Language Model       | GPT-2             |
| Connector            | Linear Projection |
| Trainable Parameters | 393,984           |
| CLIP                 | Frozen            |
| GPT-2                | Frozen            |
| Optimizer            | Adam              |
| Learning Rate        | `1e-4`            |
| Epochs               | 3                 |
| Batch Size           | 2                 |
| Sequence Length      | 50                |
| Device               | CUDA              |

---

# ⚠️ An Important Result

The prototype generated fluent text, but the generated text was not reliably grounded in the input image.

For example, the model produced unrelated general-domain text for different synthetic images.

This exposes an important limitation of the current architecture:

```text
Fluent Language
       ≠
Visual Grounding
```

A language model can produce grammatically coherent text without correctly interpreting the image.

Therefore, the current experiment demonstrates:

> **A functioning multimodal training and inference pipeline**

but does **not** demonstrate:

> **Reliable medical image understanding.**

This distinction is central to the next stages of the project.

---

# 🚧 Current Limitations

### Dataset

The current dataset contains only four synthetic examples.

### Visual Representation

The image is represented using a single projected visual token.

### Language Model

GPT-2 remains frozen and has not been medically adapted.

### Vision Encoder

CLIP remains frozen and was not specifically optimized for chest radiography.

### Medical Grounding

The model has not demonstrated reliable medical image grounding.

### Evaluation

No clinical evaluation has been performed.

### Safety

The generated text may contain hallucinations or medically incorrect statements.

---

# 🛣️ Roadmap

The project is being developed in stages.

## Phase 01 — Multimodal Prototype ✅

```text
CLIP
  ↓
Projection
  ↓
GPT-2
```

* [x] CLIP integration
* [x] GPT-2 integration
* [x] Vision-language projection
* [x] Frozen backbone training
* [x] Custom Dataset
* [x] DataLoader
* [x] Training loop
* [x] Autoregressive inference
* [x] CUDA execution
* [x] Checkpoint saving

---

## Phase 02 — Real Chest X-Ray Data 🔄

Move from synthetic images to real medical datasets.

### Planned datasets

* CheXpert
* MIMIC-CXR
* IU X-Ray

### Tasks

* [ ] Dataset downloader
* [ ] Metadata parser
* [ ] Image/report matching
* [ ] Train/validation/test split
* [ ] Medical report preprocessing
* [ ] Data quality checks
* [ ] Dataset caching
* [ ] Reproducible preprocessing pipeline

---

## Phase 03 — Better Visual Grounding 🔜

The current single-token representation is too restrictive.

The next architecture will investigate:

```text
Image
  ↓
Vision Transformer
  ↓
Patch Features
  ↓
Multiple Visual Tokens
  ↓
Multimodal Connector
  ↓
Language Model
```

Planned experiments:

* [ ] Patch-level visual features
* [ ] Multiple visual tokens
* [ ] MLP projection
* [ ] Cross-attention
* [ ] Q-Former-style connector
* [ ] Vision encoder comparison
* [ ] Visual grounding analysis

---

## Phase 04 — Medical Language Adaptation 🔜

Adapt the language generation component to the structure and terminology of radiology reports.

Planned work:

* [ ] Medical vocabulary analysis
* [ ] Radiology report formatting
* [ ] Findings generation
* [ ] Impression generation
* [ ] LoRA/PEFT experiments
* [ ] Medical-domain adaptation
* [ ] Structured report generation

Target format:

```text
FINDINGS:
...

IMPRESSION:
...
```

---

# 📈 Phase 05 — Evaluation

Evaluation will move beyond ordinary text-generation metrics.

### NLP Metrics

* [ ] BLEU
* [ ] ROUGE
* [ ] METEOR
* [ ] CIDEr
* [ ] BERTScore

### Medical Evaluation

* [ ] Clinical label agreement
* [ ] Finding-level accuracy
* [ ] Factual consistency
* [ ] Hallucination rate
* [ ] Negation accuracy
* [ ] Uncertainty handling
* [ ] Human/radiologist evaluation

The objective is to determine whether generated text is actually supported by the image rather than merely being linguistically fluent.

---

# 🧪 Phase 06 — Model Improvements

Future architecture:

```text
             X-RAY IMAGE
                  │
                  ▼
        ┌──────────────────┐
        │ Vision Transformer│
        └────────┬─────────┘
                 │
          Patch Representations
                 │
                 ▼
        ┌──────────────────┐
        │ Multimodal       │
        │ Connector        │
        └────────┬─────────┘
                 │
                 ▼
        ┌──────────────────┐
        │ Medical Language │
        │      Model       │
        └────────┬─────────┘
                 │
                 ▼
        Structured Report
```

Potential experiments:

* [ ] Better vision encoder
* [ ] Better language decoder
* [ ] Multi-token visual prefix
* [ ] Cross-attention
* [ ] LoRA
* [ ] QLoRA
* [ ] Contrastive alignment
* [ ] Instruction tuning
* [ ] Retrieval augmentation
* [ ] Uncertainty estimation
* [ ] Hallucination detection

---

# 🌐 Project Website

The project will have a dedicated website containing:

```text
HOME
 │
 ├── Overview
 │
 ├── Architecture
 │
 ├── Demo
 │
 ├── Results
 │
 ├── Roadmap
 │
 ├── Research
 │
 └── Documentation
```

### Website

**Live project website:**

`https://salvapiol45tech.github.io/medical-xray-report-generator/`

GitHub Pages can publish a project website directly from a repository, and GitHub Actions can automate deployment whenever the project is updated.

> The website should be activated once the repository and Pages deployment are configured; the URL above should not be presented as live until that deployment actually exists.

---

# 🖥️ Planned Interactive Demo

The next version will provide a browser interface:

```text
┌──────────────────────────────────────────┐
│      MEDICAL X-RAY REPORT GENERATOR      │
├──────────────────────────────────────────┤
│                                          │
│        ┌──────────────────────┐          │
│        │                      │          │
│        │    Upload X-Ray      │          │
│        │                      │          │
│        └──────────────────────┘          │
│                                          │
│              [ Analyze ]                 │
│                                          │
├──────────────────────────────────────────┤
│ Generated Report                         │
│                                          │
│ Findings:                                │
│ ...                                      │
│                                          │
│ Impression:                              │
│ ...                                      │
└──────────────────────────────────────────┘
```

The demo will only be enabled for appropriately evaluated models and will clearly display the research/educational status of the system.

---

# 💾 Checkpoint

The current trained checkpoint is:

```text
minillava_model.pt
```

Save:

```python
torch.save(
    model.state_dict(),
    "minillava_model.pt"
)
```

Load:

```python
model = MiniLLaVA_Simple()

model.load_state_dict(
    torch.load("minillava_model.pt")
)
```

---

# 📁 Repository Structure

```text
medical-xray-report-generator/
│
├── README.md
├── LICENSE
├── requirements.txt
│
├── notebooks/
│   └── medical_xray_vlm.ipynb
│
├── src/
│   ├── model.py
│   ├── dataset.py
│   ├── train.py
│   ├── inference.py
│   └── evaluation.py
│
├── checkpoints/
│   └── minillava_model.pt
│
├── data/
│   └── README.md
│
├── website/
│   ├── index.html
│   ├── style.css
│   └── script.js
│
└── outputs/
    ├── reports/
    └── evaluation/
```

---

# ⚙️ Installation

```bash
git clone https://github.com/SalvaPiol45Tech/medical-xray-report-generator.git

cd medical-xray-report-generator

pip install -r requirements.txt
```

Run the notebook:

```bash
jupyter notebook
```

Or run the training pipeline:

```bash
python src/train.py
```

---

# 🧰 Technology Stack

### Machine Learning

* Python
* PyTorch
* Hugging Face Transformers

### Computer Vision

* CLIP
* Vision Transformers
* PIL

### NLP

* GPT-2
* Transformer embeddings
* Autoregressive generation

### Engineering

* CUDA
* Git
* GitHub
* GitHub Actions
* GitHub Pages

### Future

* FastAPI
* Docker
* Cloud GPU
* Experiment tracking
* Model evaluation pipeline

---

# 🔬 Research Direction

This project is part of a broader exploration of:

```text
Computer Vision
       ↓
Multimodal Learning
       ↓
Vision-Language Models
       ↓
Reasoning
       ↓
Medical AI
```

The long-term research question is:

> **How can multimodal AI systems generate language that is both fluent and faithfully grounded in complex visual evidence?**

Medical imaging provides a challenging environment for studying this problem because a useful system must go beyond generating plausible language.

It must generate language that is:

```text
Visually Grounded
       +
Factually Consistent
       +
Medically Relevant
       +
Uncertainty Aware
```

---

# 📌 Project Philosophy

This project follows three principles:

### 1. Build From the Architecture

Understand how the multimodal system works rather than treating a pretrained model as a black box.

### 2. Measure What Actually Works

A lower training loss or fluent sentence does not automatically mean that the model understands the image.

### 3. Progress From Prototype → Real System

```text
Proof of Concept
      ↓
Real Data
      ↓
Better Architecture
      ↓
Evaluation
      ↓
Deployment
```

---

# ⚠️ Medical Disclaimer

This project is for **research and educational purposes only**.

It is not a medical device.

It has not been clinically validated.

The model may generate incorrect, incomplete, or hallucinated information.

The outputs must not be used for:

* diagnosis
* treatment
* triage
* patient management
* clinical decision-making

Any future clinical application would require appropriate medical validation, safety evaluation, regulatory review, and professional oversight.

---

# 👨‍💻 Author

## Salva Piol

**AI / Machine Learning Engineer**

### Areas of Focus

```text
Python
PyTorch
Deep Learning
Computer Vision
Transformers
Multimodal AI
Vision-Language Models
Medical AI
AI Agents
Reasoning & Planning
```

---

# 📊 Project
