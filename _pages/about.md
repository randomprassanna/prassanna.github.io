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


---
permalink: /
title: "Academic Pages is a ready-to-fork GitHub Pages template for academic personal websites"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

This is the front page of a website that is powered by the [Academic Pages template](https://github.com/academicpages/academicpages.github.io) and hosted on GitHub pages. [GitHub pages](https://pages.github.com) is a free service in which websites are built and hosted from code and data stored in a GitHub repository, automatically updating when a new commit is made to the repository. This template was forked from the [Minimal Mistakes Jekyll Theme](https://mmistakes.github.io/minimal-mistakes/) created by Michael Rose, and then extended to support the kinds of content that academics have: publications, talks, teaching, a portfolio, blog posts, and a dynamically-generated CV. You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

A data-driven personal website
======
Like many other Jekyll-based GitHub Pages templates, Academic Pages makes you separate the website's content from its form. The content & metadata of your website are in structured markdown files, while various other files constitute the theme, specifying how to transform that content & metadata into HTML pages. You keep these various markdown (.md), YAML (.yml), HTML, and CSS files in a public GitHub repository. Each time you commit and push an update to the repository, the [GitHub pages](https://pages.github.com/) service creates static HTML pages based on these files, which are hosted on GitHub's servers free of charge.

Many of the features of dynamic content management systems (like Wordpress) can be achieved in this fashion, using a fraction of the computational resources and with far less vulnerability to hacking and DDoSing. You can also modify the theme to your heart's content without touching the content of your site. If you get to a point where you've broken something in Jekyll/HTML/CSS beyond repair, your markdown files describing your talks, publications, etc. are safe. You can rollback the changes or even delete the repository and start over - just be sure to save the markdown files! Finally, you can also write scripts that process the structured data on the site, such as [this one](https://github.com/academicpages/academicpages.github.io/blob/master/talkmap.ipynb) that analyzes metadata in pages about talks to display [a map of every location you've given a talk](https://academicpages.github.io/talkmap.html).

Getting started
======
1. Register a GitHub account if you don't have one and confirm your e-mail (required!)
1. Fork [this template](https://github.com/academicpages/academicpages.github.io) by clicking the "Use this template" button in the top right. 
1. Go to the repository's settings (rightmost item in the tabs that start with "Code", should be below "Unwatch"). Rename the repository "[your GitHub username].github.io", which will also be your website's URL.
1. Set site-wide configuration and create content & metadata (see below -- also see [this set of diffs](http://archive.is/3TPas) showing what files were changed to set up [an example site](https://getorg-testacct.github.io) for a user with the username "getorg-testacct")
1. Upload any files (like PDFs, .zip files, etc.) to the files/ directory. They will appear at https://[your GitHub username].github.io/files/example.pdf.  
1. Check status by going to the repository settings, in the "GitHub pages" section

Site-wide configuration
------
The main configuration file for the site is in the base directory in [_config.yml](https://github.com/academicpages/academicpages.github.io/blob/master/_config.yml), which defines the content in the sidebars and other site-wide features. You will need to replace the default variables with ones about yourself and your site's github repository. The configuration file for the top menu is in [_data/navigation.yml](https://github.com/academicpages/academicpages.github.io/blob/master/_data/navigation.yml). For example, if you don't have a portfolio or blog posts, you can remove those items from that navigation.yml file to remove them from the header. 

Create content & metadata
------
For site content, there is one markdown file for each type of content, which are stored in directories like _publications, _talks, _posts, _teaching, or _pages. For example, each talk is a markdown file in the [_talks directory](https://github.com/academicpages/academicpages.github.io/tree/master/_talks). At the top of each markdown file is structured data in YAML about the talk, which the theme will parse to do lots of cool stuff. The same structured data about a talk is used to generate the list of talks on the [Talks page](https://academicpages.github.io/talks), each [individual page](https://academicpages.github.io/talks/2012-03-01-talk-1) for specific talks, the talks section for the [CV page](https://academicpages.github.io/cv), and the [map of places you've given a talk](https://academicpages.github.io/talkmap.html) (if you run this [python file](https://github.com/academicpages/academicpages.github.io/blob/master/talkmap.py) or [Jupyter notebook](https://github.com/academicpages/academicpages.github.io/blob/master/talkmap.ipynb), which creates the HTML for the map based on the contents of the _talks directory).

**Markdown generator**

The repository includes [a set of Jupyter notebooks](https://github.com/academicpages/academicpages.github.io/tree/master/markdown_generator
) that converts a CSV containing structured data about talks or presentations into individual markdown files that will be properly formatted for the Academic Pages template. The sample CSVs in that directory are the ones I used to create my own personal website at stuartgeiger.com. My usual workflow is that I keep a spreadsheet of my publications and talks, then run the code in these notebooks to generate the markdown files, then commit and push them to the GitHub repository.

How to edit your site's GitHub repository
------
Many people use a git client to create files on their local computer and then push them to GitHub's servers. If you are not familiar with git, you can directly edit these configuration and markdown files directly in the github.com interface. Navigate to a file (like [this one](https://github.com/academicpages/academicpages.github.io/blob/master/_talks/2012-03-01-talk-1.md) and click the pencil icon in the top right of the content preview (to the right of the "Raw | Blame | History" buttons). You can delete a file by clicking the trashcan icon to the right of the pencil icon. You can also create new files or upload files by navigating to a directory and clicking the "Create new file" or "Upload files" buttons. 

Example: editing a markdown file for a talk
![Editing a markdown file for a talk](/images/editing-talk.png)

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.
