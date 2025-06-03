---
permalink: /
title: "About Me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---



Your Hidden Research Work Summary

Let me also extract your research skills more explicitly:

Area	Depth
Generative Models	ProGAN, Pix2Pix, SPADE, CP-VTON, MADF, LAMA, WGAN, VAE
Vision Transformers	Multi-modal VITON-HD transformer, attention models, optical flow transformers
Segmentation	UNet, custom contour extraction, small lesion segmentation losses
Detection	YOLOv5 with custom NMS, multi-class anatomical detection
Interpretability	Captum-based regulatory interpretability
Post-Processing	Rule-based smoothing, temporal filtering
Optimization	Mixed precision, model pruning, ONNX graph editing
Deployment	Full-stack deployment with CUDA/CuPy, Docker, FastAPI, AWS EC2
Regulatory AI	FDA/MDR-oriented test pipelines, documentation standards
Clinical Data Curation	Dataset pipelines, GT tools, clinician feedback loop
✅ With these 3 stories + research summary, your profile becomes:

Full-stack AI architect
Deep researcher (not just applied engineer)
Regulatory-compliant product builder
GPU performance optimizer
Clinical data operations designer



1️⃣ Mirrorsize (First Job: Computer Vision Research Trainee) — The Beginning of Full-Stack AI Research

Storyline:
How you transitioned from pure research papers to production-grade implementations across complex multi-modal transformer-based models.
Key Pointers:
Entered the team as a research trainee, but independently implemented multi-modal vision transformer architecture combining image, pose, and semantic maps (total ~500M parameters).
Translated cutting-edge papers into production-level code:
VITON-HD, Pix2Pix, SPADE, CP-VTON, LAMA.
Designed custom loss functions (WGAN, Hinge Loss, LSGAN, Wasserstein Loss).
Integrated normalization layers (BatchNorm, InstanceNorm, ConditionalNorm).
Developed custom UNet-based generator (21M params) and ResNet-based structure generator (100M params).
Conducted paper-to-production translation: identified mathematical gaps, implemented custom components for alignment, warping, feature aggregation.
Optimized data pipeline to handle high-dimensional semantic labels for garment transfer.
Fine-tuned models on AWS EC2 with distributed training on multi-GPU setups.
Optimized training runtime from 72 hours → 24 hours through advanced scheduling, mixed precision training, and efficient checkpointing.
Deployed full stack system with FastAPI backend, Docker containerization, and EC2 deployment.
Quantification (for your resume):
📊 ~500M parameter multi-modal transformer model implemented.
⚙️ Reduced training time by ~65% via optimization.
🔬 Implemented >10 custom loss functions and normalization techniques.
🚀 Delivered full-stack AI system (research → production deployment).
2️⃣ Endovision (Data Scientist Role: Oct 2022 – Mar 2024) — From Applied Research to First Clinical Deployment

Storyline:
First successful delivery of real-time AI models into medical production system — mixing academic depth with real-world deployment constraints.
Key Pointers:
Led model research and applied translation of:
UNet for segmentation with real-time post-processing contour extraction.
YOLOv5 detection models with custom NMS logic for high-sensitivity anatomical class detection.
Introduced temporal rule-based stability layer for smoothing predictions across frames.
Designed full data curation pipeline:
Built Python-based data filtering tool:
Blurry frame rejection (via OpenCV + variance of Laplacian, etc.).
Auto-sorting by procedure/video metadata.
Dataset versioning for model reproducibility.
Semi-automated clinician ground truth labeling tool for clinical annotation QA.
Managed data strategy for:
50+ endoscopy procedures, curated 100K+ frames for training.
Developed reproducible dataset versioning system for model audit trail.
Built multiple ONNX inference pipelines with direct ONNX graph editing for fusing models and trimming model outputs to optimize runtime.
Coordinated cross-functional collaboration with clinical teams for model QA cycles.
Quantification (for your resume):
📊 Curated 100K+ frame datasets across 50+ procedures.
🧪 Delivered 2 production-grade models (UNet, YOLOv5).
🚀 Improved clinical inference stability with custom rule-based smoothing.
⚙️ Reduced dataset curation effort by ~70% through automation tools.
🔬 First full-stack clinical AI pipeline promoted to real-time application.
3️⃣ Endovision (Senior Data Scientist: Mar 2024 – Ongoing) — Full AI Product Owner, Research + Engineering Leadership

Storyline:
End-to-end ownership of AI clinical modules — driving both research innovation and full-stack engineering across multiple AI models.
Key Pointers:
Independently led new clinical module for Atrophic Gastritis & Intestinal Metaplasia detection:
Designed data collection protocols with clinicians.
Designed custom loss functions for small lesion detection.
Implemented advanced multi-loss architectures combining cross-entropy, Dice, and attention-based spatial regularization.
Researched Transformer-based temporal modules and optical flow for real-time lesion tracking:
Experimented with transformer-based optical flow, compared against rule-based modules.
Built real-time interpretability framework using Captum for regulatory-grade model explainability.
Designed FDA-compliant testing framework based on real-world video segmentations.
Led optimization of rendering pipeline:
Original OpenCV-based rendering (25ms) → CuPy (10ms) → CUDA kernel-based rendering (2ms).
Designed full internal AI lifecycle for team:
Model promotion frameworks.
Dataset curation standards.
Model packaging for deployment.
Regulatory documentation pipeline.
Setup full-stack AI operations including visualization tools, versioning, benchmark dashboards, workflow automation for labeling and testing.
Led internal workshops for AI feasibility estimation, adoption pathway design, and regulatory impact quantification.
