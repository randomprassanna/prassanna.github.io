<hr>

<h2>10. Why Batch Statistics Work for Images</h2>

<p>
Consider convolutional activations:
</p>

<div class="eq">
x ∈ ℝ^(B × C × H × W)
</div>

<p>
BatchNorm computes:
</p>

<div class="eq">
μ_c = E_{b,h,w}[x_bchw],   σ_c² = Var_{b,h,w}[x_bchw]
</div>

<p>
Implicit assumption:
</p>

<div class="eq">
x_bchw ~ 𝒩(μ_c, σ_c²)
</div>

<p>
This assumption is not arbitrary. It relies on a <b>dataset-level stationarity</b>:
</p>

<ul>
<li>Images are sampled from a shared distribution P(image)</li>
<li>Local patches across images share similar statistics (edges, textures)</li>
<li>Thus batch ≈ i.i.d. samples from same distribution</li>
</ul>

<p>
Hence:
</p>

<div class="eq">
E_batch[x] ≈ E_data[x]
</div>

<p>
BatchNorm is therefore a <b>Monte Carlo estimator of population statistics</b>.
</p>

---

<h2>11. Why This Fails for Transformers</h2>

<p>
For sequence models:
</p>

<div class="eq">
x ∈ ℝ^(B × T × d)
</div>

<p>
Each token embedding satisfies:
</p>

<div class="eq">
x_i ~ P(token_i)
</div>

<p>
But tokens across batch are:
</p>

<div class="eq">
x_i ⟂ x_j  (independent, heterogeneous semantics)
</div>

<ul>
<li>Different sentences → different distributions</li>
<li>No shared spatial structure</li>
<li>Non-stationary across batch</li>
</ul>

<p>
Thus:
</p>

<div class="eq">
E_batch[x] ≠ meaningful estimate
</div>

<p>
BatchNorm introduces noise in representation alignment, destabilizing attention layers.
</p>

---

<h2>12. Why LayerNorm Works in Transformers</h2>

<p>
LayerNorm assumes:
</p>

<div class="eq">
x_i1, ..., x_id ~ 𝒩(μ_i, σ_i²)
</div>

<p>
Interpretation:
</p>

<ul>
<li>Features represent a distributed embedding</li>
<li>Only <b>relative scale within a token</b> matters</li>
<li>Absolute scale across tokens is irrelevant</li>
</ul>

<p>
Thus normalization enforces:
</p>

<div class="eq">
||x_i|| ≈ constant
</div>

<p>
This stabilizes:
</p>

<ul>
<li>Dot-product attention (scale-sensitive)</li>
<li>Residual connections</li>
<li>Gradient propagation depth-wise</li>
</ul>

---

<h2>13. What Happens If We Swap Them?</h2>

<p><b>BatchNorm in Transformers:</b></p>

<ul>
<li>Attention logits fluctuate due to batch noise</li>
<li>Sequence-dependent instability</li>
<li>Poor convergence for small batch sizes</li>
</ul>

<div class="eq">
softmax(QKᵀ / √d) becomes unstable
</div>

<p><b>LayerNorm in CNNs:</b></p>

<ul>
<li>Ignores cross-image statistics</li>
<li>Removes useful contrast differences between samples</li>
<li>Reduces implicit regularization</li>
</ul>

<p>
Empirically:
</p>

<div class="eq">
CNN + LN ⇒ slower convergence, worse generalization
</div>

---

<h2>14. Training Stability & Gradient Dynamics</h2>

<p>
Normalization affects gradients through scale invariance:
</p>

<div class="eq">
∂L/∂x ∝ 1 / σ
</div>

<p>
For BatchNorm:
</p>

<ul>
<li>σ estimated over batch → stochastic gradients</li>
<li>Acts as noise injection</li>
<li>Improves generalization</li>
</ul>

<p>
For LayerNorm:
</p>

<ul>
<li>σ deterministic per sample</li>
<li>Stable but less regularizing</li>
</ul>

<p>
Thus:
</p>

<div class="eq">
BN ≈ stochastic preconditioning  
LN ≈ deterministic conditioning
</div>

---

<h2>15. Dynamic Range & Signal Propagation</h2>

<p>
Without normalization:
</p>

<div class="eq">
x^(l+1) = W x^(l)
</div>

<p>
Variance grows exponentially:
</p>

<div class="eq">
Var[x^(l)] → exploding / vanishing
</div>

<p>
Normalization enforces:
</p>

<div class="eq">
Var[x^(l)] ≈ 1
</div>

<p>
Thus preserving <b>dynamic range</b>:
</p>

<ul>
<li>Prevents saturation (ReLU dead zones, sigmoid collapse)</li>
<li>Maintains gradient flow</li>
<li>Keeps representations in high-sensitivity region</li>
</ul>

<p>
Interpretation:
</p>

<div class="eq">
Normalization ≈ projection onto a stable manifold
</div>

---

<h2>16. The Problem with Global Statistics</h2>

<p>
BatchNorm uses global statistics across samples:
</p>

<div class="eq">
μ_global = E_batch[x]
</div>

<p>
But this can wash out:
</p>

<ul>
<li>Rare features</li>
<li>Outliers (medically important signals!)</li>
<li>Local contrast variations</li>
</ul>

<p>
Thus:
</p>

<div class="eq">
Signal-to-noise ratio ↓ for rare patterns
</div>

---

<h2>17. Biological Parallel: Human Vision</h2>

<p>
The human visual system does <b>local normalization</b>, not global.
</p>

<p>
In retinal ganglion cells:
</p>

<div class="eq">
response ∝ (center − surround)
</div>

<p>
This is known as:
</p>

<div class="eq">
Divisive normalization
</div>

<ul>
<li>Normalization is local in space</li>
<li>Enhances edges and contrast</li>
<li>Preserves important anomalies</li>
</ul>

<div class="diagram">
Biological analogy:

   center pixel
       ↓
   [ strong response ]

surrounding region
   [ suppressive field ]

output = center / (local variance)
</div>

<p>
Comparison:
</p>

<ul>
<li>BatchNorm → global population statistics</li>
<li>LayerNorm → per-sample normalization</li>
<li>Biology → <b>local spatial normalization</b> (closer to InstanceNorm / GroupNorm)</li>
</ul>

---

<h2>18. Final Synthesis</h2>

<div class="eq">
BatchNorm → assumes dataset-level stationarity  
LayerNorm → assumes feature-level exchangeability  
Biological systems → assume local spatial coherence
</div>

<p>
Thus, normalization choice reflects a deeper assumption about:
</p>

<ul>
<li>What constitutes a “population”</li>
<li>Where invariance should be enforced</li>
<li>What structure should be preserved</li>
</ul>

<p>
In essence:
</p>

<div class="eq">
BN = statistical alignment across data  
LN = geometric stabilization within representation  
Biology = contrast normalization in local neighborhoods
</div>