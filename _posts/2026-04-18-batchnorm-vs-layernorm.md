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
Normalization layers mitigate internal covariate shift by enforcing consistent activation statistics. Batch Normalization (BatchNorm) and Layer Normalization (LayerNorm) differ fundamentally in the axis over which moments are computed, leading to distinct inductive biases. This post provides a rigorous comparison of their formulations, underlying statistical assumptions, implications for training dynamics, gradient behavior, and dynamic range preservation, with particular emphasis on why BatchNorm thrives in convolutional vision models while LayerNorm dominates transformers.
</p>

<h2>Core Mathematical Setup</h2>
<p>
Consider a mini-batch of activations <span class="eq">\mathbf{X} \in \mathbb{R}^{B \times d}</span>, where <em>B</em> is the batch size and <em>d</em> is the feature dimension. The goal of normalization is to transform <span class="eq">\mathbf{X}</span> such that its (estimated) mean is zero and variance is one:
</p>
<div class="eq">
\mathbb{E}[\hat{\mathbf{X}}] \approx \mathbf{0}, \quad \mathrm{Var}(\hat{\mathbf{X}}) \approx 1
</div>
<p>
The key distinction lies in the domain over which the expectation and variance are taken.
</p>

<h2>Batch Normalization: Batch-Wise Statistics</h2>
<p>
BatchNorm computes statistics independently for each feature dimension <em>j = 1, …, d</em> across the batch axis:
</p>
<div class="eq">
\mu_j = \frac{1}{B} \sum_{i=1}^{B} x_{ij}, \qquad
\sigma_j^2 = \frac{1}{B} \sum_{i=1}^{B} (x_{ij} - \mu_j)^2
</div>
<div class="eq">
\hat{x}_{ij} = \frac{x_{ij} - \mu_j}{\sqrt{\sigma_j^2 + \epsilon}}, \qquad
y_{ij} = \gamma_j \hat{x}_{ij} + \beta_j
</div>
<p>
During training, an exponential moving average maintains running estimates of <span class="eq">\mu_j</span> and <span class="eq">\sigma_j^2</span> for inference. This formulation implicitly assumes that the <em>B</em> samples are i.i.d. draws from the same underlying distribution.
</p>

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
In image data, low-level statistics (edge distributions, texture patterns, global contrast, color covariances) are largely invariant across samples. Formally:
</p>
<div class="eq">
\mathbb{E}_{\text{batch}}[x_{\cdot j}] \approx \mathbb{E}_{\text{data}}[x_{\cdot j}]
</div>
<p>
The mini-batch moments therefore serve as a Monte-Carlo estimator of the population moments. The resulting stochasticity in <span class="eq">\mu_j</span> and <span class="eq">\sigma_j^2</span> acts as a form of noise injection that regularizes the model (similar in spirit to jittering the loss landscape). This enables significantly larger learning rates and faster convergence, as originally demonstrated by Ioffe & Szegedy (2015).
</p>

<h2>Layer Normalization: Feature-Wise (Per-Sample) Statistics</h2>
<p>
LayerNorm computes statistics independently for each sample <em>i</em> across its feature vector:
</p>
<div class="eq">
\mu_i = \frac{1}{d} \sum_{j=1}^{d} x_{ij}, \qquad
\sigma_i^2 = \frac{1}{d} \sum_{j=1}^{d} (x_{ij} - \mu_i)^2
</div>
<div class="eq">
\hat{x}_{ij} = \frac{x_{ij} - \mu_i}{\sqrt{\sigma_i^2 + \epsilon}}, \qquad
y_{ij} = \gamma_i \hat{x}_{ij} + \beta_i
</div>
<p>
No information is shared across the batch. The operation is fully deterministic given a single input sample.
</p>

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
In transformer models, the batch dimension typically concatenates unrelated sequences (different sentences, documents, or tokens from diverse contexts). There is no shared statistical structure across batch elements. Batch-wise moments therefore become highly noisy and uninformative:
</p>
<div class="eq">
\mathrm{Var}_{\text{batch}}(\mu_j) \text{ is large and semantically meaningless}
</div>
<p>
This noise destabilizes the scaled dot-product attention mechanism, where small perturbations in input scale can lead to sharply different softmax distributions. LayerNorm avoids this by guaranteeing that every token’s feature vector has consistent scale and zero mean, independent of other tokens in the batch. This stability is crucial for both training convergence and autoregressive inference.
</p>

<h2>Consequences of Swapping Normalization Axes</h2>
<p>
<strong>BatchNorm in Transformers:</strong> The assumption of i.i.d. samples across batch is violated. Resulting issues include:
</p>
<ul>
<li>High-variance batch statistics → unstable attention logits and exploding/vanishing gradients</li>
<li>Poor scaling with batch size (especially problematic for large models trained on long sequences)</li>
<li>Inference-time running averages fail to capture per-token variability required by generation</li>
</ul>
<p>
<strong>LayerNorm in CNNs:</strong> The model loses access to cross-sample alignment of low-level statistics. Empirical consequences include:
</p>
<ul>
<li>Reduced regularization effect → higher risk of overfitting</li>
<li>Weaker exploitation of dataset-wide invariances → lower generalization performance</li>
<li>Typically 1–4% drop in ImageNet accuracy compared to BatchNorm-based baselines</li>
</ul>

<h2>Training Stability and Gradient Dynamics</h2>
<p>
Normalization controls the magnitude of gradients flowing backward. For a linear layer <span class="eq">\mathbf{y} = \mathbf{W} \hat{\mathbf{x}}</span>, the gradient with respect to the normalized input scales as:
</p>
<div class="eq">
\left\| \frac{\partial \mathcal{L}}{\partial \hat{x}_{ij}} \right\| \propto \frac{1}{\sigma} \cdot \left\| \frac{\partial \mathcal{L}}{\partial y} \right\| \approx O(1)
</div>
<p>
By keeping <span class="eq">\sigma \approx 1</span>, both methods prevent gradient explosion or vanishing due to scaling mismatches. However:
</p>
<ul>
<li>BatchNorm introduces stochasticity in the normalization statistics, which has been shown to smooth the optimization landscape (Santurkar et al., 2018).</li>
<li>LayerNorm provides deterministic, sample-independent stabilization, leading to smoother but sometimes less regularized trajectories.</li>
</ul>

<h2>Preservation of Dynamic Range Across Layers: A Deeper View</h2>
<p>
Consider a deep network without normalization. Let <span class="eq">\mathbf{h}^{(\ell+1)} = f(\mathbf{W}^{(\ell)} \mathbf{h}^{(\ell)})</span>. If the singular values of <span class="eq">\mathbf{W}^{(\ell)}</span> deviate from 1 on average, the variance of activations grows or shrinks exponentially with depth (exploding/vanishing variance problem). Normalization breaks this chain by resetting the first two moments at every layer:
</p>
<div class="eq">
\mathrm{Var}(\hat{\mathbf{h}}^{(\ell)}) \approx 1 \quad \Rightarrow \quad \mathrm{Var}(\mathbf{W} \hat{\mathbf{h}}^{(\ell)}) \text{ depends only on } \mathbf{W}
</div>
<p>
The affine parameters <span class="eq">(\gamma, \beta)</span> allow the network to re-introduce any desired scale and shift after normalization. This effectively decouples the <em>learned representation direction</em> from its <em>magnitude</em>, keeping activations in the sensitive, non-saturated region of the nonlinearity (e.g., away from the flat tails of ReLU or sigmoid). Consequently, networks can be trained with far deeper architectures and much higher learning rates.
</p>

<h2>Global vs Local Statistics: Limitations and Biological Inspiration</h2>
<p>
BatchNorm’s global (batch-wide) statistics can suppress rare, high-signal events. If a particular image contains unusual but semantically important local contrast, the global mean and variance pull its activations toward the batch average, potentially washing out discriminative information.
</p>
<p>
Biological visual systems solve this via <strong>local divisive normalization</strong>. Retinal ganglion cells and V1 neurons implement a center-surround mechanism:
</p>
<div class="diagram">
Center-surround divisive normalization (simplified)
          Center response (c)
   Output ≈ ─────────────────────────
            √(c² + ∑ surrounding² + ε)
</div>
<p>
This is mathematically closer to Instance Normalization or spatially localized GroupNorm than to BatchNorm. It enhances local contrast, suppresses uniform regions, and preserves edges irrespective of global illumination — exactly the behavior needed for robust perception. LayerNorm lies conceptually between global BatchNorm and fully local biological mechanisms: it is local to each sample but global across all features of that sample.
</p>

<h2>Final Theoretical Insight</h2>
<div class="eq">
BatchNorm : Aligns statistics <strong>across data</strong> (global, exploits shared distribution)  
LayerNorm  : Stabilizes geometry <strong>within sample</strong> (local, batch-independent)  
Biological vision : Local contrast normalization (center-surround divisive)
</div>
<p>
The superiority of one over the other is not universal but emerges from the alignment between the normalization axis and the intrinsic statistical structure of the data and architecture. Understanding these assumptions explains both the empirical successes and the failure modes when the layers are swapped, as well as why normalization enables stable training of extremely deep networks while preserving expressive dynamic range.
</p>

<p style="font-size:0.9em; color:#888; margin-top:50px;">
Key References:  
Ioffe & Szegedy (2015). Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift.  
Ba, Kiros & Hinton (2016). Layer Normalization.  
Vaswani et al. (2017). Attention Is All You Need.  
Santurkar et al. (2018). How Does Batch Normalization Help Optimization?  
Carandini & Heeger (2012). Normalization as a Canonical Neural Computation.
</p>