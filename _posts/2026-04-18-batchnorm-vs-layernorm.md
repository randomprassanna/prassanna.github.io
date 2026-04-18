---
layout: single
title: "BatchNorm vs LayerNorm: Theory, Assumptions, and Dynamics"
date: 2026-04-18
categories: [Deep Learning]
---

<style>
.eq {
  background: #111;
  color: #00e6b8;
  padding: 10px;
  border-radius: 6px;
  font-family: monospace;
  margin: 10px 0;
}
.diagram {
  background: #1a1a1a;
  color: #8fd;
  padding: 12px;
  border-radius: 8px;
  font-family: monospace;
  white-space: pre;
  margin: 15px 0;
}
</style>

<h2>Core Idea</h2>

<p>
Neural activations drift during training. Normalization enforces controlled statistics:
</p>

<div class="eq">
x in R^(B x d)
</div>

<div class="eq">
mean(x) approx 0, var(x) approx 1
</div>

<p>
The difference is where normalization is applied:
</p>

<ul>
<li><b>BatchNorm:</b> across samples (batch axis)</li>
<li><b>LayerNorm:</b> across features (within sample)</li>
</ul>

---

<h2>Batch Normalization</h2>

<div class="eq">
mu_j = (1/B) sum_i x_ij
</div>

<div class="eq">
sigma_j^2 = (1/B) sum_i (x_ij - mu_j)^2
</div>

<p>
Assumption:
</p>

<div class="eq">
x_1j ... x_Bj follow same distribution
</div>

<div class="diagram">
BatchNorm

x_1j   x_2j   x_3j   ...   x_Bj
  |      |      |            |
  -------- mean,var ---------
             |
        normalize
</div>

<p>
Interpretation: each feature is aligned across the dataset.
</p>

---

<h2>Why BatchNorm Works for Images</h2>

<p>
Images share global structure:
</p>

<ul>
<li>edges</li>
<li>textures</li>
<li>lighting statistics</li>
</ul>

<p>
Thus batch acts like samples from same distribution:
</p>

<div class="eq">
E_batch[x] approx E_data[x]
</div>

<p>
BatchNorm becomes a stochastic estimator of dataset statistics.
</p>

---

<h2>Layer Normalization</h2>

<div class="eq">
mu_i = (1/d) sum_j x_ij
</div>

<div class="eq">
sigma_i^2 = (1/d) sum_j (x_ij - mu_i)^2
</div>

<p>
Assumption:
</p>

<div class="eq">
features inside one sample form a distribution
</div>

<div class="diagram">
LayerNorm

x_i1   x_i2   x_i3   ...   x_id
  |      |      |            |
  -------- mean,var ---------
             |
        normalize
</div>

<p>
Interpretation: normalize internal representation geometry.
</p>

---

<h2>Why Transformers Use LayerNorm</h2>

<ul>
<li>tokens across batch are unrelated</li>
<li>no shared spatial structure</li>
<li>batch statistics become noisy</li>
</ul>

<p>
Thus BatchNorm breaks:
</p>

<div class="eq">
mean_batch becomes unstable
</div>

<p>
LayerNorm instead ensures:
</p>

<div class="eq">
each token has stable scale
</div>

---

<h2>What Happens If We Swap?</h2>

<p><b>BatchNorm in Transformers:</b></p>
<ul>
<li>attention instability</li>
<li>noisy gradients</li>
<li>poor convergence</li>
</ul>

<p><b>LayerNorm in CNNs:</b></p>
<ul>
<li>loses cross-image statistics</li>
<li>weaker generalization</li>
</ul>

---

<h2>Training Stability</h2>

<p>
Normalization controls gradient scale:
</p>

<div class="eq">
grad approx 1 / sigma
</div>

<ul>
<li>BatchNorm: noisy but regularizing</li>
<li>LayerNorm: stable but less regularization</li>
</ul>

---

<h2>Dynamic Range Preservation</h2>

<p>
Without normalization:
</p>

<div class="eq">
variance explodes or vanishes
</div>

<p>
Normalization enforces:
</p>

<div class="eq">
variance approx constant across layers
</div>

<p>
This keeps activations in sensitive regions (avoids dead neurons).
</p>

---

<h2>Global vs Local Statistics Problem</h2>

<p>
BatchNorm uses global statistics:
</p>

<ul>
<li>can suppress rare signals</li>
<li>can wash out anomalies</li>
</ul>

---

<h2>Biological Analogy (Human Vision)</h2>

<p>
Human vision uses local normalization:
</p>

<div class="diagram">
center pixel -> strong signal
surrounding pixels -> suppression

output = center / local activity
</div>

<ul>
<li>enhances contrast</li>
<li>preserves edges</li>
<li>not global normalization</li>
</ul>

<p>
Closer to local normalization methods than BatchNorm.
</p>

---

<h2>Final Insight</h2>

<div class="eq">
BatchNorm = align across data
LayerNorm = stabilize within sample
Biology = local contrast normalization
</div>