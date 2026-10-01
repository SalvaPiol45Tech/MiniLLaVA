# 🧠 AI Research & Engineering

<p align="center">
  <img src="https://img.shields.io/badge/AI-Research%20%26%20Engineering-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Python-PyTorch-yellow?style=for-the-badge&logo=python">
  <img src="https://img.shields.io/badge/LLMs-Multimodal-purple?style=for-the-badge">
  <img src="https://img.shields.io/badge/Agents-Systems-green?style=for-the-badge">
</p>

<p align="center">

### Building AI systems from fundamentals to real-world applications.

**LLMs · VLMs · Multimodal AI · Agents · Reasoning · Computer Vision · Medical AI**

</p>

<p align="center">

<a href="#-ai-roadmap">Roadmap</a> • <a href="#-projects">Projects</a> • <a href="#-ai-tools">AI Tools</a> • <a href="#-website">Website</a> • <a href="#-research-direction">Research</a>

</p>

---

# 👋 About

I build and study **AI systems across language, vision, multimodal learning, reasoning, and autonomous agents**.

My approach is focused on understanding systems from the inside:

```text
Fundamentals
     ↓
Architecture
     ↓
Implementation
     ↓
Training
     ↓
Evaluation
     ↓
Deployment
     ↓
Real AI Systems
```

The goal is not simply to use AI APIs.

The goal is to understand how modern AI systems are constructed and turn those ideas into **working research prototypes, tools, models, and applications**.

---

# 🧭 AI Research Roadmap

My long-term roadmap follows the evolution of modern AI systems:

```text
                    AI ENGINEERING
                          │
                          ▼
                  ┌───────────────┐
                  │     LLMs      │
                  └───────┬───────┘
                          │
                          ▼
                ┌──────────────────┐
                │ Multimodal AI    │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │      VLMs        │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │ Reasoning        │
                │ Planning         │
                │ Memory           │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │ AI Agents        │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │ Multi-Agent      │
                │ Systems          │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │ Production AI    │
                │ Systems          │
                └──────────────────┘
```

---

# 🧠 Phase 1 — LLMs

### Goal

Understand how language models work by implementing important components rather than treating LLMs as black boxes.

### Topics

* Tokenization
* Embeddings
* Positional Encoding
* Self-Attention
* Multi-Head Attention
* Transformer Blocks
* Causal Masking
* Layer Normalization
* Feed-Forward Networks
* GPT architectures
* Training loops
* Optimization
* Sampling
* Fine-tuning
* LoRA / PEFT
* Evaluation

### Projects

```text
01 ─ Tokenizer
02 ─ Embedding Model
03 ─ Attention From Scratch
04 ─ Transformer From Scratch
05 ─ GPT From Scratch
06 ─ GPT Training Pipeline
07 ─ Fine-Tuning Pipeline
08 ─ LLM Evaluation
```

---

# 👁️ Phase 2 — Computer Vision

Build visual intelligence from fundamental architectures to modern vision models.

### Topics

* CNNs
* ResNet
* Vision Transformers
* ViT
* Object Detection
* Image Segmentation
* Representation Learning
* Contrastive Learning
* CLIP
* 3D Vision

### Projects

```text
CNN
 ↓
ResNet
 ↓
ViT
 ↓
CLIP
 ↓
3D Vision
 ↓
World Models
```

---

# 🌐 Phase 3 — Multimodal AI

Connect different modalities into a single AI system.

```text
Text
 │
 ├──────────────┐
 │              │
 ▼              ▼
Language       Vision
 │              │
 └──────┬───────┘
        ▼
   Multimodal AI
```

### Areas

* Vision + Language
* Image + Text
* Audio + Text
* Video + Language
* 3D + Language
* Multimodal Embeddings
* Cross-Modal Attention
* Vision-Language Alignment

### Projects

```text
CLIP
Medical AI
MiniLLaVA
Vision-Language Models
Multimodal Agents
```

---

# 👁️‍🗨️ Phase 4 — VLMs

Build increasingly capable Vision-Language Models.

### Architecture

```text
Image
  ↓
Vision Encoder
  ↓
Visual Features
  ↓
Multimodal Connector
  ↓
Language Model
  ↓
Text
```

### Research Areas

* Visual Tokens
* Patch Embeddings
* Vision-Language Alignment
* Cross-Attention
* Q-Former
* Visual Instruction Tuning
* Multimodal Fine-Tuning
* Visual Grounding
* Multimodal Reasoning

### Current Project

## 🏥 Medical X-Ray Report Generator

```text
Chest X-Ray
     ↓
CLIP
     ↓
Projection Layer
     ↓
GPT-2
     ↓
Medical Text
```

Current implementation demonstrates the complete multimodal pipeline and serves as a foundation for future training on real medical datasets.

---

# 🧩 Phase 5 — Reasoning & Planning

Move beyond simple generation.

```text
Observation
     ↓
State
     ↓
Reasoning
     ↓
Planning
     ↓
Action
     ↓
Observation
```

### Research Areas

* Reasoning
* Planning
* Search
* Memory
* World Models
* Reinforcement Learning
* Model-Based RL
* Tree Search
* Tool Use
* Self-Evaluation

---

# 🤖 Phase 6 — AI Agents

The next stage is building systems that can **use models as components inside an autonomous loop**.

```text
              ┌──────────────┐
              │     Goal     │
              └──────┬───────┘
                     ↓
              ┌──────────────┐
              │  Reasoning   │
              └──────┬───────┘
                     ↓
              ┌──────────────┐
              │    Plan      │
              └──────┬───────┘
                     ↓
              ┌──────────────┐
              │     Tool     │
              │     Call     │
              └──────┬───────┘
                     ↓
              ┌──────────────┐
              │ Observation  │
              └──────┬───────┘
                     │
                     └──────────► Repeat
```

### Agent Stack

```text
LLM
 ↓
Tool Calling
 ↓
Planning
 ↓
Memory
 ↓
RAG
 ↓
Reasoning
 ↓
Multi-Agent
 ↓
Evaluation
 ↓
Deployment
```

### Agent Research

* Tool Calling
* Function Calling
* Planning
* Short-Term Memory
* Long-Term Memory
* RAG
* Agent State
* Reflection
* Evaluation
* Multi-Agent Systems
* Agent Environments
* Autonomous Workflows

---

# 🌍 Phase 7 — Production AI

Research prototypes eventually need engineering infrastructure.

```text
AI Model
   ↓
API
   ↓
Backend
   ↓
Database
   ↓
Authentication
   ↓
Docker
   ↓
Linux
   ↓
Cloud
   ↓
Monitoring
   ↓
Production
```

### Engineering Stack

* Python
* FastAPI
* REST APIs
* PostgreSQL
* Redis
* Docker
* Linux
* Git
* CI/CD
* Cloud GPU
* MLflow
* Model evaluation
* Observability

---

# 🧪 Projects

## 🧠 LLM From Scratch

A GPT-style language model implemented from fundamental Transformer components.

```text
Tokenizer
   ↓
Embeddings
   ↓
Self-Attention
   ↓
Transformer Blocks
   ↓
Language Head
   ↓
Next Token Prediction
```

**Focus:**

* Transformer architecture
* Causal language modeling
* Training
* Validation
* Text generation
* Reasoning experiments

---

## 👁️ Multimodal AI

Experiments combining visual and textual representations.

**Focus:**

* CLIP
* Vision encoders
* Language models
* Multimodal embeddings
* Cross-modal representation learning

---

## 🏥 Medical AI

Vision-language experiments for medical imaging.

```text
X-Ray
 ↓
Vision Encoder
 ↓
Multimodal Alignment
 ↓
Language Model
 ↓
Report Generation
```

---

## 🧠 ARIA

### Adaptive Reasoning & Imagination Agent

A research prototype exploring:

```text
Vision
+
State Representation
+
Memory
+
World Model
+
Planning
+
Reinforcement Learning
```

ARIA is a research prototype and **not a claim of general AGI**.

---

## 🧩 ARC Research

Experiments investigating how AI systems can solve novel abstract reasoning problems.

Focus areas:

* Abstract reasoning
* Pattern discovery
* Program synthesis
* Search
* Generalization
* Test-time adaptation

---

# 🛠️ AI Tools

The long-term goal is to build a collection of reusable AI engineering tools.

```text
                    AI TOOLBOX
                        │
        ┌───────────────┼────────────────┐
        │               │                │
        ▼               ▼                ▼
      LLMs             VLMs            Agents
        │               │                │
        ▼               ▼                ▼
   Training Tools   Vision Tools    Agent Tools
        │               │                │
        └───────────────┼────────────────┘
                        ▼
                  Developer Tools
```

### Planned Tools

| Tool                 | Purpose                            | Status |
| -------------------- | ---------------------------------- | ------ |
| LLM Training Toolkit | Train/evaluate small LLMs          | 🔄     |
| Transformer Toolkit  | Educational Transformer components | 🔄     |
| VLM Toolkit          | Vision-language experiments        | 🔄     |
| Dataset Tools        | Prepare AI datasets                | 🔄     |
| Evaluation Toolkit   | Evaluate models                    | 🔜     |
| Agent Toolkit        | Build agent workflows              | 🔜     |
| RAG Toolkit          | Retrieval pipelines                | 🔜     |
| Memory Toolkit       | Agent memory                       | 🔜     |
| Multimodal Toolkit   | Vision + language systems          | 🔜     |
| Deployment Toolkit   | Deploy AI systems                  | 🔜     |

---

# 🌐 AI Research Website

The portfolio will eventually have a central website connecting the entire ecosystem.

```text
                    AI LAB WEBSITE
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ▼                 ▼                 ▼
     Research          Projects           Tools
        │                 │                 │
        ▼                 ▼                 ▼
      Papers            Demos           Downloads
        │                 │                 │
        └─────────────────┼─────────────────┘
                          ▼
                       Models
                          │
                          ▼
                       Datasets
```

### Website Sections

```text
HOME
│
├── Research
│
├── LLMs
│
├── VLMs
│
├── Multimodal
│
├── Agents
│
├── Projects
│
├── AI Tools
│
├── Models
│
├── Datasets
│
├── Demos
│
├── Documentation
│
└── Downloads
```

The website can be hosted with GitHub Pages, which supports project sites directly from GitHub repositories and can use GitHub Actions for automated deployment.

---

# 📦 AI Tools & Downloads

The website will provide a central download area for open-source work.

### Downloads

```text
┌────────────────────────────────────────────┐
│              AI TOOLBOX                    │
├────────────────────────────────────────────┤
│                                            │
│  🧠 LLM Toolkit                            │
│  Build and train small language models     │
│                                            │
│  👁️ VLM Toolkit                            │
│  Vision-language experimentation           │
│                                            │
│  🤖 Agent Toolkit                           │
│  Build tool-using AI agents                │
│                                            │
│  📊 Evaluation Toolkit                     │
│  Evaluate AI systems                       │
│                                            │
│  🗂️ Dataset Toolkit                        │
│  Prepare datasets for training             │
│                                            │
└────────────────────────────────────────────┘
```

Each tool will eventually include:

```text
Source Code
Documentation
Installation
Examples
Model Weights
Configuration
Benchmarks
License
Release History
```

---

# 🚀 Demos

Interactive AI demonstrations will be provided where practical.

Examples:

```text
LLM Playground
      ↓
VLM Playground
      ↓
Medical AI Demo
      ↓
Agent Playground
      ↓
Multimodal Playground
```

For ML demos, Hugging Face Spaces is one deployment option because Spaces supports ML applications and provides Git-based workflows, including Gradio, Docker, and static applications.

---

# 📚 Research Documentation

Each major project will document:

```text
Problem
 ↓
Research Question
 ↓
Architecture
 ↓
Implementation
 ↓
Dataset
 ↓
Training
 ↓
Experiments
 ↓
Evaluation
 ↓
Failure Analysis
 ↓
Future Work
```

The goal is to make the repository useful not only as a portfolio but also as a **technical research notebook**.

---

# 🗺️ Long-Term Roadmap

```text
2026
 │
 ├── LLMs
 │    ├── Transformer From Scratch
 │    ├── GPT From Scratch
 │    └── LLM Training
 │
 ├── Computer Vision
 │    ├── CNN
 │    ├── ViT
 │    └── CLIP
 │
 └── Multimodal
      └── VLM Prototype
            │
            ▼
2027
 │
 ├── Strong VLMs
 │
 ├── Multimodal Reasoning
 │
 ├── AI Agents
 │
 ├── Memory
 │
 ├── Planning
 │
 ├── Tool Use
 │
 └── Multi-Agent Systems
      │
      ▼
2027+
 │
 ├── Advanced Reasoning
 ├── World Models
 ├── Embodied AI
 ├── Autonomous Agents
 ├── Production AI
 └── Open AI Tools
```

The dates are development targets rather than guarantees; the sequence is the important part.

---

# 🔬 Research Direction

The central research direction is:

```text
How can AI systems perceive,
represent, reason, plan,
use tools, learn from feedback,
and act in complex environments?
```

This leads toward:

```text
LLMs
 ↓
VLMs
 ↓
Multimodal Models
 ↓
Reasoning
 ↓
Planning
 ↓
Memory
 ↓
Agents
 ↓
World Models
 ↓
Embodied AI
```

---

# 🏗️ AI System Architecture

The long-term architecture I am working toward is:

```text
                 ┌───────────────────┐
                 │       USER        │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │      AGENT        │
                 │   Orchestrator    │
                 └─────────┬─────────┘
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
          Reasoning      Memory        Planning
             │             │             │
             └─────────────┼─────────────┘
                           ▼
                    ┌──────────────┐
                    │    Models    │
                    ├──────────────┤
                    │     LLM      │
                    │     VLM      │
                    │ Multimodal   │
                    └──────┬───────┘
                           │
                           ▼
                     ┌───────────┐
                     │   Tools   │
                     ├───────────┤
                     │ Search    │
                     │ Code      │
                     │ Database  │
                     │ APIs      │
                     │ Files     │
                     └─────┬─────┘
                           │
                           ▼
                      Environment
```

---

# 📈 Engineering Principles

### Build

Don't only call a model.

Understand the architecture.

### Measure

Don't assume that fluent output means intelligence.

Evaluate it.

### Experiment

Compare architectures and document failures.

### Ship

Turn successful research prototypes into usable tools.

### Open

Publish code, experiments, documentation, and reproducible results whenever possible.

---

# 🔗 Ecosystem

### Code

GitHub repositories contain the implementations, experiments, and research projects.

### Models

Model checkpoints and trained artifacts can be published through appropriate model repositories.

### Datasets

Dataset configurations and preprocessing tools will be documented separately.

### Demos

Interactive applications will be deployed through the project website and ML demo platforms.

### Tools

Reusable AI engineering tools will eventually be collected into a central AI toolbox.

---

# 📌 Current Status

| Area                | Status       |
| ------------------- | ------------ |
| Python / PyTorch    | 🟢 Active    |
| LLMs                | 🟢 Active    |
| Transformers        | 🟢 Active    |
| Computer Vision     | 🟢 Active    |
| Multimodal AI       | 🟢 Active    |
| VLMs                | 🟢 Active    |
| Medical AI          | 🟢 Active    |
| Reasoning           | 🟡 Research  |
| Planning            | 🟡 Research  |
| AI Agents           | 🟡 Building  |
| Multi-Agent Systems | 🔵 Planned   |
| AI Tooling          | 🔵 Planned   |
| AI Website          | 🔵 Planned   |
| Production AI       | 🔵 Planned   |
| Embodied AI         | 🔵 Long-Term |

---

# 🧰 Current Core Stack

```text
Python
PyTorch
Transformers
Hugging Face
NumPy
Pandas
CUDA
FastAPI
Docker
Git
Linux
```

---

# 👨‍💻 Author

## Salva Piol

**AI Engineer · AI Research · Multimodal Systems**

### Focus

```text
LLMs
Computer Vision
Transformers
Multimodal AI
Vision-Language Models
Medical AI
AI Agents
Reasoning
Planning
World Models
Embodied AI
```

---

# ⭐ Vision

The long-term goal is to build an open collection of:

```text
        MODELS
          +
        TOOLS
          +
       RESEARCH
          +
        DEMOS
          +
     DOCUMENTATION
```

forming a single AI engineering ecosystem where people can:

**learn → experiment → download → run → modify → build.**

---

## ⚠️ Research Disclaimer

The projects in this repository are research and engineering experiments.

Individual models may be incomplete, experimental, or unsuitable for production use.

Medical AI projects are not clinically validated and must not be used for diagnosis or medical decision-making.
