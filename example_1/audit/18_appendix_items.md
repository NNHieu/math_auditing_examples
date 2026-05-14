# 18. Appendix — Supporting items (well-posedness)

**Source:** Appendix L1004–L1031 (well-posedness, problem:splitting, lem:derv_maxiaml, lem:linear_maxiaml).
**Item Status:** 🟢 Valid (auxiliary standard results)

---

## Item 18.1 — Problem (operator splitting)

**Source:** L1010–L1020.

Statement: Find $\mathbf z^*$ such that $0 \in (F + G)(\mathbf z^*)$ with $F(\mathbf z) = (I-W)\mathbf z - (U\mathbf x + \mathbf b)$ and $G = \partial f$ (subdifferential of a convex closed proper function $f$).

**Note:** This is the operator-splitting reformulation of the monDEQ fixed-point equation. Standard in convex analysis.

---

## Item 18.2 — Lemma `lem:derv_maxiaml`

**Source:** L1023–L1026.

Statement: A subdifferential $\partial f$ is maximal monotone iff $f$ is convex, closed, proper (CCP).

**Note:** Standard (Rockafellar 1970). Cited without proof. ✓

---

## Item 18.3 — Lemma `lem:linear_maxiaml`

**Source:** L1028–L1031.

Statement: A linear operator $F(\mathbf x) = W\mathbf x + h$ is monotone iff $W + W^\top \succeq 0$, strongly monotone if $W + W^\top \succeq mI$.

**Notes:**
- **Flag-LM1 (typo).** Statement says "maximal monotone iff $W + W^\top \succeq 0$". As stated this is correct **for the linear operator $\mathbf x \mapsto W\mathbf x + h$** on a finite-dimensional Euclidean space — every monotone linear operator on $\mathbb{R}^n$ is automatically maximal. Reads slightly oddly. **Severity: None.**

## Item 18.4 — Backward pass (`sec:backward_pass`)

**Source:** L952–L1002.

The appendix derives the RBP backward-pass formula based on the implicit function theorem and the fixed-point equation. Standard derivation; not a proof in the audit-worthy sense, but a tutorial inclusion.

**Notes:**
- L963 says "$\nabla_\mathbf z g(\mathbf z^*, \mathbf u) = J_g(\mathbf z^*) = J_f(\mathbf z^*) - I$ is nonsingular". This is an *assumption* required for IFT; in our setting it follows from strong monotonicity of $I - W$.
- The exposition mixes notations: $\mathbf u$ as input parameter (RBP convention) vs the trainable readout vector in the main paper. **Severity: Minor (potential reader confusion).**
