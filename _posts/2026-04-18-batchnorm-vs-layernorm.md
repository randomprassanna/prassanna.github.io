---
layout: single
title: "BatchNorm vs LayerNorm: Theory, Assumptions, and Dynamics"
date: 2026-04-19
categories: [Deep Learning]
---
<style>
.eq {
  background: #111;
  color: #00e6b8;
  padding: 12px;
  border-radius: 6px;
  font-family: monospace;
  margin: 12px 0;
  overflow-x: auto;
}
.diagram {
  background: #1a1a1a;
  color: #8fd;
  padding: 14px;
  border-radius: 8px;
  font-family: monospace;
  white-space: pre;
  margin: 18px 0;
  line-height: 1.4;
}
</style>

<h2>Abstract</h2>
<p>
Normalization layers mitigate internal covariate shift by enforcing consistent activation statistics. Batch Normalization (BatchNorm) and Layer Normalization (LayerNorm) differ fundamentally in the axis over which moments are computed, leading to distinct inductive biases. This post provides a rigorous comparison of their formulations, underlying statistical assumptions, implications for training dynamics, gradient behavior, and dynamic range preservation.
</p>

<h2>Core Mathematical Setup</h2>
<p>
Consider a mini-batch of activations \(\mathbf{X} \in \mathbb{R}^{B \times d}\), where \(B\) is the batch size and \(d\) is the feature dimension. The goal of normalization is to transform \(\mathbf{X}\) such that its (estimated) mean is zero and variance is one:
</p>
$$ \mathbb{E}[\hat{\mathbf{X}}] \approx \mathbf{0}, \quad \mathrm{Var}(\hat{\mathbf{X}}) \approx 1 $$

<h2>Batch Normalization: Batch-Wise Statistics</h2>
<p>
BatchNorm computes statistics independently for each feature dimension \(j = 1, \dots, d\) across the batch axis:
</p>
$$ \mu_j = \frac{1}{B} \sum_{i=1}^{B} x_{ij}, \qquad
\sigma_j^2 = \frac{1}{B} \sum_{i=1}^{B} (x_{ij} - \mu_j)^2 $$
$$ \hat{x}_{ij} = \frac{x_{ij} - \mu_j}{\sqrt{\sigma_j^2 + \epsilon}}, \qquad
y_{ij} = \gamma_j \hat{x}_{ij} + \beta_j $$

<div class="diagram">
BatchNorm (per channel)
x_{1j}   x_{2j}   …   x_{Bj}
  │        │           │
  ─────── batch mean & variance ───────
                   │
             normalize + affine (γⱼ, βⱼ)
</div>

<h2>Why BatchNorm Aligns with Convolutional Architectures</h2>
<p>
In image data, low-level statistics are largely invariant across samples. Formally:
</p>
$$ \mathbb{E}_{\text{batch}}[x_{\cdot j}] \approx \mathbb{E}_{\text{data}}[x_{\cdot j}] $$

<h2>Layer Normalization: Feature-Wise (Per-Sample) Statistics</h2>
<p>
LayerNorm computes statistics independently for each sample \(i\) across its feature vector:
</p>
$$ \mu_i = \frac{1}{d} \sum_{j=1}^{d} x_{ij}, \qquad
\sigma_i^2 = \frac{1}{d} \sum_{j=1}^{d} (x_{ij} - \mu_i)^2 $$
$$ \hat{x}_{ij} = \frac{x_{ij} - \mu_i}{\sqrt{\sigma_i^2 + \epsilon}}, \qquad
y_{ij} = \gamma_i \hat{x}_{ij} + \beta_i $$

<div class="diagram">
LayerNorm (per sample/token)
x_{i1}   x_{i2}   …   x_{id}
  │        │           │
  ─────── feature mean & variance ───────
                   │
             normalize + affine (γᵢ, βᵢ)
</div>

<h2>Why Transformers Require LayerNorm</h2>
<p>
In transformers, batch-wise moments become highly noisy and uninformative:
</p>
$$ \mathrm{Var}_{\text{batch}}(\mu_j) \text{ is large and semantically meaningless} $$

<h2>Consequences of Swapping Normalization Axes</h2>
<p>
The mismatch leads to instability or loss of regularization, as detailed in the previous versions.
</p>

<h2>Training Stability and Gradient Dynamics</h2>
<p>
Normalization controls gradient magnitude. For a linear layer \(\mathbf{y} = \mathbf{W} \hat{\mathbf{x}}\), the gradient with respect to the normalized input scales as:
</p>
$$ \left\| \frac{\partial \mathcal{L}}{\partial \hat{x}_{ij}} \right\| \propto \frac{1}{\sigma} \cdot \left\| \frac{\partial \mathcal{L}}{\partial y} \right\| \approx O(1) $$

<h2>Preservation of Dynamic Range Across Layers</h2>
<p>
Without normalization, activation variance can grow or shrink exponentially with depth. Normalization resets the moments at every layer:
</p>
$$ \mathrm{Var}(\hat{\mathbf{h}}^{(\ell)}) \approx 1 \quad \Rightarrow \quad \mathrm{Var}(\mathbf{W} \hat{\mathbf{h}}^{(\ell)}) \text{ depends only on } \mathbf{W} $$

<p>
The affine parameters \((\gamma, \beta)\) then allow the network to recover any desired scale and shift, keeping activations in the sensitive region of the nonlinearity.
</p>

<h2>Global vs Local Statistics and Biological Inspiration</h2>
<p>
BatchNorm’s global statistics can suppress rare signals. Biological systems use local divisive normalization, e.g.:
</p>
$$ \text{Output} \approx \frac{\text{center response}}{\sqrt{\text{center}^2 + \sum \text{surrounding}^2 + \epsilon}} $$

<h2>Final Theoretical Insight</h2>
$$ \begin{align*}
\text{BatchNorm} &\colon \text{align statistics across data (global)} \\
\text{LayerNorm} &\colon \text{stabilize geometry within sample (local)} \\
\text{Biological vision} &\colon \text{local contrast normalization (center-surround)}
\end{align*} $$

<p style="font-size:0.9em; color:#888; margin-top:50px;">
Key References:  
Ioffe & Szegedy (2015). Batch Normalization.  
Ba, Kiros & Hinton (2016). Layer Normalization.  
Vaswani et al. (2017). Attention Is All You Need.  
Santurkar et al. (2018). How Does Batch Normalization Help Optimization?
</p>