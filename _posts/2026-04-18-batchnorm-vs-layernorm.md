---
layout: single
title: "BatchNorm vs LayerNorm — A Geometric & Statistical View"
date: 2026-04-18
categories: [Deep Learning, Theory]
---

<style>
.norm-box {
  background: #111;
  color: #eee;
  padding: 15px;
  border-radius: 10px;
  font-family: monospace;
  overflow-x: auto;
}

.diagram {
  background: #1a1a1a;
  padding: 15px;
  border-radius: 10px;
  font-family: monospace;
  white-space: pre;
  color: #8ef;
}

.section-block {
  margin-bottom: 40px;
}
</style>

<div class="section-block">
<h2>Motivation</h2>

<p>Given activations:</p>

<div class="norm-box">
x ∈ ℝ^(B × d)
</div>

<p>We want:</p>

<div class="norm-box">
E[x] ≈ 0,   Var[x] ≈ 1
</div>

<p>The key difference is <b>which axis we normalize over</b>.</p>
</div>

---

<div class="section-block">
<h2>Batch Normalization</h2>

<div class="norm-box">
μ_j = (1/B) Σ_i x_ij  
σ_j² = (1/B) Σ_i (x_ij − μ_j)²  

x̂_ij = (x_ij − μ_j) / √(σ_j² + ε)
y_ij = γ_j x̂_ij + β_j
</div>

<p><b>Interpretation:</b></p>
<ul>
<li>Statistics computed across batch</li>
<li>Each feature follows:</li>
</ul>

<div class="norm-box">
x_1j, ..., x_Bj ~ N(μ_j, σ_j²)
</div>

<div class="diagram">
BatchNorm (feature-wise across batch)

x_1j   x_2j   x_3j   ...   x_Bj
  |      |      |            |
  -------- mean,var ----------
             ↓
        normalize
</div>

<p><b>Key idea:</b> align feature distributions across samples.</p>
</div>

---

<div class="section-block">
<h2>Layer Normalization</h2>

<div class="norm-box">
μ_i = (1/d) Σ_j x_ij  
σ_i² = (1/d) Σ_j (x_ij − μ_i)²  

x̂_ij = (x_ij − μ_i) / √(σ_i² + ε)
</div>

<p><b>Interpretation:</b></p>

<ul>
<li>Statistics computed per sample</li>
<li>Each sample follows:</li>
</ul>

<div class="norm-box">
x_i1, ..., x_id ~ N(μ_i, σ_i²)
</div>

<div class="diagram">
LayerNorm (feature-wise inside sample)

x_i1   x_i2   x_i3   ...   x_id
  |      |      |            |
  -------- mean,var ----------
             ↓
        normalize
</div>

<p><b>Key idea:</b> normalize internal feature geometry.</p>
</div>

---

<div class="section-block">
<h2>Why BatchNorm Works for Images</h2>

<div class="norm-box">
x ∈ ℝ^(B × C × H × W)

μ_c = E_{b,h,w}[x_bchw]
</div>

<p>Assumption:</p>

<div class="norm-box">
x_bchw ~ N(μ_c, σ_c²)
</div>

<ul>
<li>Images share dataset-level statistics</li>
<li>Batch ≈ population estimate</li>
<li>Acts as stochastic regularizer</li>
</ul>
</div>

---

<div class="section-block">
<h2>Why Transformers Use LayerNorm</h2>

<div class="norm-box">
x ∈ ℝ^(B × T × d)
</div>

<ul>
<li>Tokens are unrelated across batch</li>
<li>Batch statistics are noisy</li>
<li>Variable sequence lengths</li>
</ul>

<p>Thus:</p>

<div class="norm-box">
∀ token i: normalize independently
</div>
</div>

---

<div class="section-block">
<h2>Geometric View</h2>

<p><b>BatchNorm:</b></p>
<div class="norm-box">
Normalize columns → align dataset distribution
</div>

<p><b>LayerNorm:</b></p>
<div class="norm-box">
Normalize rows → constrain each sample:

||x_i||² ≈ d
</div>
</div>

---

<div class="section-block">
<h2>Benefits vs Caveats</h2>

<table>
<tr>
<th>Property</th>
<th>BatchNorm</th>
<th>LayerNorm</th>
</tr>

<tr>
<td>Axis</td>
<td>Batch</td>
<td>Feature</td>
</tr>

<tr>
<td>Regularization</td>
<td>Strong (stochastic)</td>
<td>Weak</td>
</tr>

<tr>
<td>Batch size</td>
<td>Required</td>
<td>Not needed</td>
</tr>

<tr>
<td>Best for</td>
<td>CNNs</td>
<td>Transformers</td>
</tr>
</table>
</div>

---

<div class="section-block">
<h2>Unifying View</h2>

<div class="norm-box">
x̂ = (x − E_S[x]) / √(Var_S[x] + ε)
</div>

<p>Where:</p>

<ul>
<li>BatchNorm: S = batch axis</li>
<li>LayerNorm: S = feature axis</li>
</ul>
</div>

---

<div class="section-block">
<h2>Final Insight</h2>

<div class="norm-box">
BatchNorm → "How does this feature behave across data?"

LayerNorm → "How are features structured within this sample?"
</div>

</div>