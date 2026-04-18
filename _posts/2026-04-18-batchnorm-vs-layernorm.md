---
layout: single
title: "Batch Normalization vs Layer Normalization: A Statistical & Geometric View"
date: 2026-04-18
categories: [Deep Learning, Theory]
---

<style>
.eq {
  background: #111;
  color: #00ffcc;
  padding: 12px;
  border-radius: 8px;
  font-family: monospace;
  margin: 10px 0;
}

.diagram {
  background: #1a1a1a;
  color: #8fd;
  padding: 15px;
  border-radius: 10px;
  font-family: monospace;
  white-space: pre;
  margin: 15px 0;
}

p {
  line-height: 1.7;
}
</style>

<h2>1. Problem Setting</h2>

<p>
Let activations of a neural network layer be:
</p>

<div class="eq">
x ∈ ℝ^(B × d)
</div>

<p>
where <b>B</b> is batch size and <b>d</b> is feature dimension.
Training instability arises because the distribution of x drifts during optimization.
Normalization enforces:
</p>

<div class="eq">
E[x] ≈ 0,   Var[x] ≈ 1
</div>

<p>
The key question is: <b>over which axis is this expectation taken?</b>
</p>

---

<h2>2. Batch Normalization</h2>

<p>
BatchNorm normalizes each feature across the batch:
</p>

<div class="eq">
μ_j = (1/B) Σ_i x_ij  
σ_j² = (1/B) Σ_i (x_ij − μ_j)²
</div>

<div class="eq">
x̂_ij = (x_ij − μ_j) / √(σ_j² + ε)
</div>

<p>
This implies:
</p>

<div class="eq">
x_1j, x_2j, ..., x_Bj ~ 𝒩(μ_j, σ_j²)
</div>

<p>
Thus, BatchNorm assumes a <b>batch-wise Gaussian distribution per feature</b>.
</p>

<div class="diagram">
BatchNorm (normalize column-wise)

        feature j
x_1j    x_2j    x_3j   ...   x_Bj
  |       |       |            |
  ----------- μ,σ -------------
              ↓
          normalize
</div>

<p>
Geometrically, BatchNorm aligns the distribution of each feature across samples:
</p>

<div class="eq">
Var_batch(x_.j) → 1
</div>

---

<h2>3. Layer Normalization</h2>

<p>
LayerNorm instead normalizes across features within each sample:
</p>

<div class="eq">
μ_i = (1/d) Σ_j x_ij  
σ_i² = (1/d) Σ_j (x_ij − μ_i)²
</div>

<div class="eq">
x̂_ij = (x_ij − μ_i) / √(σ_i² + ε)
</div>

<p>
This implies:
</p>

<div class="eq">
x_i1, x_i2, ..., x_id ~ 𝒩(μ_i, σ_i²)
</div>

<p>
Thus, LayerNorm assumes a <b>feature-wise Gaussian distribution per sample</b>.
</p>

<div class="diagram">
LayerNorm (normalize row-wise)

sample i →

x_i1   x_i2   x_i3   ...   x_id
  |      |      |            |
  -------- μ,σ --------------
           ↓
       normalize
</div>

<p>
Geometrically:
</p>

<div class="eq">
||x_i||² ≈ d
</div>

<p>
Each sample lies on a normalized hypersphere.
</p>

---

<h2>4. Why BatchNorm Works for Images</h2>

<p>
For CNN activations:
</p>

<div class="eq">
x ∈ ℝ^(B × C × H × W)
</div>

<div class="eq">
μ_c = E_{b,h,w}[x_bchw]
</div>

<p>
BatchNorm assumes:
</p>

<div class="eq">
x_bchw ~ 𝒩(μ_c, σ_c²)
</div>

<p>
This works because:
</p>

<ul>
<li>Images in a batch share global statistics (lighting, textures)</li>
<li>Batch acts as a population estimate</li>
<li>Noise in μ,σ acts as regularization</li>
</ul>

---

<h2>5. Why LayerNorm Dominates Transformers</h2>

<p>
For sequence models:
</p>

<div class="eq">
x ∈ ℝ^(B × T × d)
</div>

<p>
Problems with BatchNorm:</p>

<ul>
<li>Tokens across batch are unrelated</li>
<li>Sequence lengths vary</li>
<li>Batch statistics become noisy</li>
</ul>

<p>
LayerNorm avoids this by enforcing:
</p>

<div class="eq">
∀ token i: normalize independently
</div>

---

<h2>6. Statistical Contrast</h2>

<table>
<tr>
<th>Aspect</th>
<th>BatchNorm</th>
<th>LayerNorm</th>
</tr>

<tr>
<td>Normalization axis</td>
<td>Batch</td>
<td>Feature</td>
</tr>

<tr>
<td>Distribution assumption</td>
<td>Batch Gaussian</td>
<td>Feature Gaussian</td>
</tr>

<tr>
<td>Dependency</td>
<td>Across samples</td>
<td>Within sample</td>
</tr>

<tr>
<td>Stochasticity</td>
<td>Yes</td>
<td>No</td>
</tr>
</table>

---

<h2>7. Failure Modes</h2>

<p><b>BatchNorm:</b></p>

<div class="eq">
B → 1 ⇒ σ² → 0  (degeneracy)
</div>

<ul>
<li>Small batch instability</li>
<li>Train-test mismatch (running stats)</li>
</ul>

<p><b>LayerNorm:</b></p>

<ul>
<li>No cross-sample regularization</li>
<li>Less effective in CNNs</li>
<li>Assumes feature symmetry</li>
</ul>

---

<h2>8. Unified View</h2>

<p>
Both methods are instances of:
</p>

<div class="eq">
x̂ = (x − E_S[x]) / √(Var_S[x] + ε)
</div>

<p>
where:</p>

<ul>
<li>BatchNorm: S = batch axis</li>
<li>LayerNorm: S = feature axis</li>
</ul>

---

<h2>9. Final Insight</h2>

<div class="eq">
BatchNorm → aligns distributions across data

LayerNorm → stabilizes representation geometry within a sample
</div>

<p>
They impose Gaussian structure — but along <b>orthogonal statistical axes</b>.
</p>