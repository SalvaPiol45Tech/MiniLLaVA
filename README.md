# 🩻 Medical MiniLLaVA

### Vision-Language Model Prototype for Chest X-Ray → Text Generation

> **A research-oriented multimodal AI prototype combining CLIP vision features with GPT-2 language generation through a trainable visual projection layer.**

[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-ee4c2c?logo=pytorch)](https://pytorch.org/)
[![Transformers](https://img.shields.io/badge/Hugging%20Face-Transformers-yellow?logo=huggingface)](https://huggingface.co/docs/transformers)
[![CLIP](https://img.shields.io/badge/CLIP-ViT--B%2F32-purple)](https://huggingface.co/openai/clip-vit-base-patch32)
[![GPT-2](https://img.shields.io/badge/GPT--2-Language%20Model-green)](https://huggingface.co/openai-community/gpt2)
[![CUDA](https://img.shields.io/badge/CUDA-GPU%20Training-76b900?logo=nvidia)](https://developer.nvidia.com/cuda)

---

## 🧠 Overview

**Medical MiniLLaVA** is a small research prototype exploring how a pretrained vision encoder and a pretrained language model can be connected to create an image-conditioned text generation system.

The architecture is inspired by the general idea behind multimodal language models such as LLaVA:

```text
                    IMAGE
                      │
                      ▼
             ┌─────────────────┐
             │   CLIP ViT-B/32 │
             │  Vision Encoder │
             │     FROZEN      │
             └────────┬────────┘
                      │
                 512-D Feature
                      │
                      ▼
             ┌─────────────────┐
             │ Visual Projection│
             │    512 → 768     │
             │    TRAINABLE     │
             └────────┬────────┘
                      │
                Visual Token
                      │
                      ▼
             ┌─────────────────┐
             │      GPT-2      │
             │ Language Model  │
             │     FROZEN      │
             └────────┬────────┘
                      │
                      ▼
                 GENERATED TEXT
```

The project is intentionally small.

It is designed to demonstrate the **engineering and research principles of multimodal model construction**, rather than claim clinical-level medical report generation.

---

# ⚠️ Important: Current Work vs Future Work

This distinction is important.

### ✅ Implemented in the current project

* CLIP image encoding
* GPT-2 language generation
* 512 → 768 visual projection
* Visual token injection into GPT-2
* Frozen pretrained CLIP
* Frozen pretrained GPT-2
* Trainable multimodal projection
* PyTorch training loop
* CUDA training
* Checkpoint saving/loading
* Autoregressive generation
* End-to-end image → text pipeline

### 🚧 NOT implemented yet

* Real chest X-ray dataset training
* Large-scale medical report dataset
* Train/validation/test split
* Clinical evaluation
* BLEU/ROUGE/METEOR/CIDEr evaluation
* Clinical accuracy evaluation
* Expert radiologist evaluation
* Production deployment
* Medical diagnosis capability

Those are **future development stages**, not current results.

---

# 🔬 Current Research Prototype

## Model

The final model used in this experiment is:

```text
CLIP ViT-B/32
      │
      │ 512 dimensions
      ▼
Linear Projection
      │
      │ 768 dimensions
      ▼
GPT-2
      │
      ▼
Text Generation
```

### Components

| Component      | Model                                | Status    |
| -------------- | ------------------------------------ | --------- |
| Vision Encoder | `openai/clip-vit-base-patch32`       | Frozen    |
| Image Feature  | CLIP projected visual representation | 512-D     |
| Projection     | Linear `512 → 768`                   | Trainable |
| Language Model | `gpt2`                               | Frozen    |
| Tokenizer      | GPT-2 tokenizer                      | Used      |
| Framework      | PyTorch + Transformers               | Used      |
| Hardware       | CUDA GPU                             | Used      |

---

# 🧩 The Trainable Component

The actual trainable module is intentionally simple:

```python
class SimpleProjection(nn.Module):
    def __init__(self):
        super().__init__()
        self.linear = nn.Linear(512, 768)

    def forward(self, x):
        return self.linear(x)
```

This produces:

```text
Input:
512-dimensional CLIP representation

        ↓

Linear Layer

        ↓

Output:
768-dimensional GPT-2 embedding
```

### Trainable parameters

```text
393,984 parameters
```

Only the projection layer is trained.

CLIP and GPT-2 remain frozen.

---

# 🧠 Multimodal Conditioning

The projected image representation becomes a visual token.

The system constructs:

```text
[VISUAL TOKEN] + [TEXT TOKENS]
```

The resulting embeddings are passed to GPT-2 through:

```python
inputs_embeds
```

The attention mask is also extended so that GPT-2 can process the visual token together with the text sequence.

This creates the fundamental multimodal pathway:

```text
Image
  ↓
CLIP
  ↓
Visual Embedding
  ↓
Projection
  ↓
Visual Token
  ↓
GPT-2
  ↓
Generated Language
```

---

# 📊 Current Dataset

## ⚠️ Synthetic Prototype Dataset

The current experiment does **not** use a real medical dataset.

It uses four synthetic RGB images created with PIL:

```python
Image.new("RGB", (224, 224), color=(50, 50, 50))
Image.new("RGB", (224, 224), color=(100, 100, 100))
Image.new("RGB", (224, 224), color=(120, 120, 120))
Image.new("RGB", (224, 224), color=(140, 140, 140))
```

The corresponding captions were:

```text
1. Normal chest X-ray with clear lungs

2. Pneumonia detected in lower left lobe

3. Mild pleural effusion on right side

4. Clear bilateral lungs without disease
```

### Dataset size

```text
Images:        4
Captions:      4
Image size:    224 × 224
Batch size:    2
Max sequence:  50 tokens
```

This dataset is suitable only for demonstrating the **pipeline and implementation**.

It is not sufficient to learn medical visual-language alignment.

---

# ⚙️ Training Configuration

```text
Epochs:              3
Batch size:          2
Learning rate:       1e-4
Optimizer:           Adam
Loss:                Cross Entropy
Gradient clipping:   1.0
Trainable params:    393,984
Device:              CUDA
```

Training was performed using GPU acceleration.

---

# 📉 Actual Training Results

The measured training losses were:

| Epoch | Average Loss |
| ----: | -----------: |
|     1 |       8.0697 |
|     2 |       7.7373 |
|     3 |       7.5410 |

The loss decreased during training:

```text
8.0697
   ↓
7.7373
   ↓
7.5410
```

This confirms that the training loop and optimization process were functioning.

However:

> **A decreasing training loss on four synthetic images does not demonstrate that the model learned medical image understanding.**

---

# 🧪 Inference Results

The trained model was tested using the image → text generation pipeline.

The model successfully generated text autoregressively.

However, the generated text was not reliably related to the input image.

For example, generated text contained unrelated general-domain language such as references to:

```text
"The U.S. Department of Justice..."
```

rather than reliable chest X-ray descriptions.

---

# 🔎 What This Result Means

This is an important research finding.

The pipeline works technically:

```text
Image
  ↓
CLIP
  ↓
Projection
  ↓
GPT-2
  ↓
Text
```

But the current experiment does **not** establish strong visual grounding.

The likely reasons include:

### 1. Extremely small dataset

Only four synthetic images were used.

### 2. Synthetic images

The images do not contain real radiographic structures.

### 3. Frozen language model

GPT-2 cannot significantly adapt to the visual representation because it remains frozen.

### 4. Tiny trainable interface

Only:

```text
393,984 parameters
```

are optimized.

### 5. Weak image-text alignment

A single linear projection is a very small interface between a vision encoder and a language model.

### 6. GPT-2 language prior dominates

Because GPT-2 is already a pretrained language model, it can generate fluent text without necessarily using the image representation correctly.

---

# 🧪 Research Interpretation

The current result should therefore be described as:

> **A working multimodal engineering proof-of-concept with unsuccessful visual grounding on a four-sample synthetic dataset.**

It should **not** be described as:

> ❌ A clinically accurate medical report generator

or:

> ❌ A validated medical AI system.

The current project is a foundation for future experimentation.

---

# 💾 Model Checkpoint

The trained model is saved using:

```python
torch.save(
    model.state_dict(),
    "minillava_model.pt"
)
```

It can later be restored with:

```python
model = MiniLLaVA_Simple()

model.load_state_dict(
    torch.load("minillava_model.pt")
)
```

---

# 🚀 Installation

## Requirements

```bash
pip install torch
pip install transformers
pip install pillow
pip install tqdm
```

Or:

```bash
pip install torch transformers pillow tqdm
```

For Jupyter:

```bash
pip install jupyterlab
```

---

# ▶️ Running the Project

Start Jupyter:

```bash
jupyter lab
```

Then open the notebook and execute the cells in order.

The pipeline is:

```text
1. Load CLIP
        ↓
2. Load GPT-2
        ↓
3. Freeze pretrained models
        ↓
4. Create projection layer
        ↓
5. Create dataset
        ↓
6. Train projection
        ↓
7. Save checkpoint
        ↓
8. Reload checkpoint
        ↓
9. Generate text
```

---

# 🏗️ Project Structure

A production-ready version of the project can evolve toward:

```text
medical-minillava/
│
├── README.md
├── requirements.txt
├── config.yaml
│
├── src/
│   ├── models/
│   │   ├── clip_encoder.py
│   │   ├── projection.py
│   │   ├── decoder.py
│   │   └── minillava.py
│   │
│   ├── data/
│   │   ├── dataset.py
│   │   ├── preprocessing.py
│   │   └── transforms.py
│   │
│   ├── training/
│   │   ├── train.py
│   │   ├── losses.py
│   │   └── checkpoint.py
│   │
│   ├── evaluation/
│   │   ├── metrics.py
│   │   └── evaluation.py
│   │
│   └── inference/
│       └── generate.py
│
├── notebooks/
│   └── medical_minillava.ipynb
│
├── checkpoints/
│
├── tests/
│
└── demos/
```

The current notebook is the prototype implementation. The structure above is a **planned engineering evolution**, not a claim that all these files already exist.

---

# 🛣️ FUTURE DEVELOPMENT ROADMAP

## Phase 1 — Real Medical Dataset

Replace the synthetic dataset with a properly licensed/accessible medical image-report dataset.

Potential research directions include:

```text
Chest X-Ray
    ↓
Real Images
    +
Real Radiology Reports
    ↓
Paired Dataset
```

Candidate sources to investigate include:

* CheXpert
* IU X-Ray
* MIMIC-CXR
* Other appropriately licensed medical imaging datasets

Dataset access, licensing, and usage restrictions must be checked before training or redistribution.

---

# Phase 2 — Proper Dataset Pipeline

Implement:

```text
Dataset
   ↓
Train / Validation / Test
   ↓
Image preprocessing
   ↓
Text preprocessing
   ↓
Tokenization
   ↓
DataLoader
```

Target:

```text
Train
Validation
Test
```

instead of training on the complete dataset.

---

# Phase 3 — Improve the Multimodal Architecture

Current:

```text
CLIP
 ↓
Single Linear Layer
 ↓
One Visual Token
 ↓
GPT-2
```

Future architecture:

```text
CLIP
 ↓
Multiple Visual Tokens
 ↓
Projection / Resampler
 ↓
Cross-Attention
 ↓
Language Model
 ↓
Medical Report
```

Potential experiments:

* MLP projection
* multi-layer projection
* multiple visual tokens
* Q-Former-style adapters
* cross-attention
* LoRA
* partial decoder fine-tuning
* modern instruction-tuned language models
* multimodal instruction tuning

---

# Phase 4 — Improve Training

Future experiments should investigate:

### Better loss handling

The current experiment uses:

```python
nn.CrossEntropyLoss()
```

A production training pipeline should properly handle padding tokens using an appropriate ignored label.

### Better token handling

Use explicit:

```text
BOS
EOS
PAD
```

semantics rather than relying on GPT-2 defaults.

### Better generation

Experiment with:

```text
Greedy decoding
Beam search
Temperature
Top-k
Top-p
Repetition penalty
```

---

# Phase 5 — Evaluation

Future versions should evaluate both language quality and medical correctness.

Possible metrics:

```text
BLEU
ROUGE
METEOR
CIDEr
```

alongside medical/clinical evaluation where appropriate.

The evaluation system should also include:

```text
Hallucination analysis
Clinical concept accuracy
Disease mention accuracy
Negation accuracy
Anatomical location accuracy
Error analysis
```

Most importantly:

> Text similarity alone is not enough to establish medical usefulness.

---

# Phase 6 — Experiment Tracking

Future experiments should record:

```text
Model configuration
Dataset version
Training parameters
GPU
Training time
Loss
Validation metrics
Checkpoints
Git commit
Random seed
```

Possible tools:

```text
MLflow
Weights & Biases
TensorBoard
```

---

# Phase 7 — Demo

A future demonstration interface:

```text
┌─────────────────────────────────────────┐
│       Medical MiniLLaVA Demo            │
├─────────────────────────────────────────┤
│                                         │
│       [ Upload Chest X-Ray ]            │
│                                         │
│               ↓                         │
│                                         │
│       Vision-Language Model             │
│                                         │
│               ↓                         │
│                                         │
│       Generated Report                  │
│                                         │
└─────────────────────────────────────────┘
```

Possible stack:

```text
Model
  ↓
Python
  ↓
FastAPI
  ↓
REST API
  ↓
Web / Gradio Interface
  ↓
Docker
```

---

# 🧠 AI ENGINEER ROADMAP

The goal is not to relearn Python from zero.

The focus is to transform existing Python/AI knowledge into **production AI engineering ability**.

```text
Python
   ↓
Git + Project Structure
   ↓
APIs + FastAPI
   ↓
Databases
   ↓
Testing
   ↓
Async Programming
   ↓
Docker
   ↓
Linux
   ↓
Cloud / GPU
   ↓
PyTorch
   ↓
LLM Engineering
   ↓
RAG
   ↓
Agents
   ↓
Multimodal AI
   ↓
MLOps / LLMOps
   ↓
Production AI Systems
```

### Core AI Engineer projects

Build:

* AI REST API
* RAG system
* LLM application
* multimodal application
* agent with tools
* evaluation pipeline
* model serving API
* Dockerized AI service
* GPU inference service
* production AI platform

---

# 🔬 AI RESEARCHER / AI RESEARCH ENGINEER ROADMAP

The research path is different from simply building applications.

```text
Mathematics
   ↓
Machine Learning
   ↓
Deep Learning
   ↓
Transformers
   ↓
Computer Vision
   ↓
Multimodal Learning
   ↓
LLMs
   ↓
Reasoning
   ↓
Planning
   ↓
Agents
   ↓
World Models
   ↓
Reinforcement Learning
   ↓
Research Reproduction
   ↓
Novel Experiments
   ↓
Ablation Studies
   ↓
Research Reports / Papers
```

### Research workflow

Every research project should follow:

```text
Problem
  ↓
Hypothesis
  ↓
Baseline
  ↓
Architecture
  ↓
Experiment
  ↓
Evaluation
  ↓
Ablation
  ↓
Error Analysis
  ↓
Conclusion
  ↓
Reproducible Code
```

The objective is not simply:

> "I built an AI model."

The stronger research question is:

> "What did I test, why did I test it, what changed, and what evidence supports the conclusion?"

---

# 💻 SOFTWARE ENGINEERING ROADMAP FOR AI

Software engineering is treated here as the infrastructure needed to build reliable AI systems.

```text
Python
 ↓
Git
 ↓
Clean Architecture
 ↓
Typing
 ↓
Testing
 ↓
Logging
 ↓
Configuration
 ↓
REST APIs
 ↓
FastAPI
 ↓
SQL / Databases
 ↓
Async
 ↓
Docker
 ↓
Linux
 ↓
CI/CD
 ↓
Cloud
 ↓
Monitoring
```

The goal is:

```text
Research Prototype
       ↓
Reliable Software
       ↓
Production AI System
```

---

# 📚 LEARN — AI / ML

## Python

Official Python downloads:

https://www.python.org/downloads/

---

## PyTorch

Use PyTorch for:

* neural networks
* training
* GPU computation
* computer vision
* transformers
* research experimentation

Official installation:

https://pytorch.org/get-started/locally/

Official tutorials:

https://docs.pytorch.org/tutorials/

---

## Hugging Face

Useful for:

* Transformers
* pretrained models
* datasets
* tokenizers
* multimodal models
* model sharing

Hugging Face Learn:

https://huggingface.co/learn

Recommended areas:

```text
LLMs
Computer Vision
Agents
Deep RL
Robotics
3D ML
```

---

# 📊 DATASETS

## Hugging Face Datasets

Use for discovering and working with machine-learning datasets:

https://huggingface.co/docs/datasets

---

## Kaggle

Useful for:

```text
Datasets
Competitions
Notebooks
GPU experiments
Machine learning projects
```

https://www.kaggle.com/

---

# 🧪 NOTEBOOK DEVELOPMENT

## Jupyter

Useful for:

* experiments
* research notebooks
* visualization
* model testing
* reproducible experiments

Official installation:

https://jupyter.org/install

---

# 🖥️ DEVELOPMENT ENVIRONMENT

## Visual Studio Code

Primary editor for:

```text
Python
PyTorch
FastAPI
Git
Docker
AI projects
```

Download:

https://code.visualstudio.com/download

---

# ⚡ GPU / CUDA

CUDA enables GPU-accelerated workloads for NVIDIA hardware.

Official CUDA Toolkit:

https://developer.nvidia.com/cuda/toolkit

Use it for:

```text
PyTorch GPU training
LLM inference
Computer vision
CUDA programming
GPU optimization
```

---

# 🚀 BACKEND

## FastAPI

FastAPI is useful for turning trained AI models into APIs.

Typical architecture:

```text
Client
  ↓
FastAPI
  ↓
Model Service
  ↓
PyTorch / Transformers
  ↓
Prediction
```

Official tutorial:

https://fastapi.tiangolo.com/tutorial/

---

# 🐳 DEPLOYMENT

## Docker

Use Docker to package:

```text
Python
Dependencies
AI Model
API
Runtime
```

Architecture:

```text
AI Application
      ↓
Docker Image
      ↓
Container
      ↓
Server / Cloud
```

Official Docker documentation:

https://docs.docker.com/get-started/

---

# 🌐 FUTURE AI LAB WEBSITE

The long-term goal is to build an AI research and engineering laboratory website.

Possible structure:

```text
AI LAB
│
├── Home
│
├── Research
│   ├── Multimodal AI
│   ├── LLMs
│   ├── Computer Vision
│   ├── Agents
│   ├── Reasoning
│   ├── World Models
│   └── Embodied AI
│
├── Projects
│
├── Models
│
├── Datasets
│
├── Experiments
│
├── Demos
│
├── Tools
│
├── Downloads
│
├── Documentation
│
└── Learning
```

---

# 🛠️ AI LAB TOOL CENTER

The future website can contain a centralized tool directory:

| Category      | Resources             |
| ------------- | --------------------- |
| Programming   | Python, Git           |
| IDE           | VS Code               |
| Deep Learning | PyTorch               |
| Transformers  | Hugging Face          |
| Datasets      | Kaggle, HF Datasets   |
| Notebooks     | Jupyter               |
| Backend       | FastAPI               |
| Containers    | Docker                |
| GPU           | CUDA                  |
| Research      | Papers, Repositories  |
| Deployment    | Cloud / GPU platforms |

The goal is to make the website useful not only as a portfolio, but also as a **learning and engineering resource hub**.

---

# 🧰 PROJECT DEVELOPMENT STANDARD

Future AI projects should follow a consistent structure:

```text
01 — Problem
      ↓
02 — Research
      ↓
03 — Dataset
      ↓
04 — Baseline
      ↓
05 — Architecture
      ↓
06 — Training
      ↓
07 — Evaluation
      ↓
08 — Error Analysis
      ↓
09 — Improvement
      ↓
10 — Deployment
      ↓
11 — Documentation
      ↓
12 — Release
```

Every project should clearly state:

```text
What exists
What was tested
What worked
What failed
What was measured
What remains
```

---

# 📌 Current Project Status

## Medical MiniLLaVA

| Area                    | Status             |
| ----------------------- | ------------------ |
| CLIP integration        | ✅ Complete         |
| GPT-2 integration       | ✅ Complete         |
| Image encoder           | ✅ Complete         |
| Projection layer        | ✅ Complete         |
| Visual token            | ✅ Complete         |
| Training loop           | ✅ Complete         |
| CUDA training           | ✅ Complete         |
| Checkpoint saving       | ✅ Complete         |
| Inference               | ✅ Complete         |
| Synthetic dataset       | ✅ Complete         |
| Real medical dataset    | 🚧 Planned         |
| Proper validation       | 🚧 Planned         |
| Medical evaluation      | 🚧 Planned         |
| Strong visual grounding | ❌ Not achieved yet |
| Production deployment   | 🚧 Planned         |
| Clinical validation     | 🚧 Future research |

---

# 🔬 Research Direction

The long-term research direction is:

```text
Small Multimodal Prototype
          ↓
Real Vision-Language Dataset
          ↓
Better Visual Tokenization
          ↓
Cross-Attention
          ↓
Efficient Fine-Tuning
          ↓
Medical VLM
          ↓
Evaluation
          ↓
Reliable Multimodal AI
```

The same principles can then be extended beyond medical imaging to:

```text
Computer Vision
      ↓
Multimodal AI
      ↓
LLMs
      ↓
Agents
      ↓
Reasoning
      ↓
Planning
      ↓
World Models
      ↓
Embodied AI
```

---

# ⚠️ Medical Safety

This repository is a **research and engineering prototype**.

It is not:

* a medical device
* a diagnostic system
* a clinical decision-support system
* a replacement for a radiologist
* clinically validated

Generated text must not be used to make medical decisions.

Future clinical experimentation would require appropriate datasets, evaluation methodology, expert review, privacy safeguards, and regulatory considerations.

---

# 🎯 Long-Term Vision

The goal of this project is not simply to create another chatbot.

The broader objective is to build increasingly capable AI systems that combine:

```text
Vision
  +
Language
  +
Memory
  +
Reasoning
  +
Planning
  +
Tools
  +
Learning
```

Ultimately:

```text
AI Research
      +
AI Engineering
      +
Open Source
      +
Reproducible Experiments
      ↓
AI Research & Engineering Lab
```

---

# 👨‍💻 Author

**Salva Piol**

AI Engineer / AI Research Engineer

Focus:

```text
Python
PyTorch
Deep Learning
Transformers
LLMs
Computer Vision
Multimodal AI
3D Vision
Reasoning
Planning
AI Agents
World Models
```

---

# ⭐ Repository Philosophy

> **Build it. Measure it. Document it. Improve it.**

Not:

```text
"I plan to build an AI system."
```

But:

```text
"I implemented it.
I tested it.
Here is what happened.
Here is what failed.
Here is the evidence.
Here is what I will improve next."
```

That is the standard this repository follows.

---

## 🔗 Core Resources

* Python — https://www.python.org/
* PyTorch — https://pytorch.org/
* Hugging Face — https://huggingface.co/
* Hugging Face Learn — https://huggingface.co/learn
* Kaggle — https://www.kaggle.com/
* Jupyter — https://jupyter.org/
* VS Code — https://code.visualstudio.com/
* FastAPI — https://fastapi.tiangolo.com/
* Docker — https://www.docker.com/
* NVIDIA CUDA — https://developer.nvidia.com/cuda
* GitHub — https://github.com/

---

**Status:** 🧪 Research Prototype
**Domain:** Multimodal AI / Vision-Language Models / Medical AI
**Framework:** PyTorch + Hugging Face Transformers
**Current Dataset:** Synthetic 4-sample prototype
**Current Result:** End-to-end generation pipeline implemented; reliable visual grounding not yet achieved
**Next Milestone:** Real paired medical image-report dataset + proper evaluation
