# 01. Definition — Monotone DEQ (def_mondeq)

**Source:** Section 3.1 "Setting", `\begin{definition}[Monotone DEQ]` at L175–L197, label `def:mondeq`.
**Item Status:** 🟡 Warning (Needs Review)
**Phase-3 verification:** E01-CONV 🚨 confirmed at L187 ("$I-W \succeq mI$") and L728 (same wording in main theorem). Confirmed: $W$ contains the skew part $B - B^\top \neq 0$ in general, so $I - W$ is not symmetric, hence "$I - W \succeq mI$" cannot be a literal PSD condition; the intended condition is operator-theoretic strong monotonicity ($((I-W) + (I-W)^\top)/2 \succeq mI$), as the commented paragraph at L199–L209 makes clear.

---

## Statement

The monDEQ is a weight-tied, input-injected network with iteration
$$
\mathbf{z}^{[k+1]} = \sigma\bigl(W\mathbf{z}^{[k]} + U\mathbf{x}\bigr),
$$
where $\mathbf{x} \in \mathbb{R}^d$, $U \in \mathbb{R}^{n\times d}$, $W \in \mathbb{R}^{n\times n}$, and $\sigma$ is the element-wise ReLU. The model output is the fixed point
$$
\mathbf{z}^* = \sigma(W\mathbf{z}^* + U\mathbf{x}).
$$
To enforce strong monotonicity $I - W \succeq m I$ (in the symmetrized sense), $W$ is parametrized as
$$
W = (1-m)\,I - A^\top A + B - B^\top, \qquad A, B \in \mathbb{R}^{n\times n}.
$$
The network output is $f(\theta, \mathbf{x}) = \mathbf{u}^\top \mathbf{z}^*$ with trainable parameters $\theta = (\mathbf{u}, A, B, U)$.

## Original Proof

(Definition — no proof.)

## Notes / Errors

- **Convention drift.** The paper switches between "$I - W$ is $m$-strongly monotone" (an *operator*-theoretic statement requiring $\frac{1}{2}((I-W) + (I-W)^\top) \succeq mI$) and writing "$I - W \succeq mI$" as if $I-W$ were symmetric. Since $W = (1-m)I - A^\top A + B - B^\top$ is **not symmetric** (the skew part $B - B^\top$ is nonzero), $I - W$ is not symmetric either, so the literal PSD inequality "$I-W \succeq mI$" is ambiguous / abusive. The author's commented-out paragraph (L199–L209) clarifies the intended meaning: it is the symmetrized condition $(I-W) + (I-W)^\top \succeq 2mI$, i.e. strong monotonicity as a linear operator.
  - **Flag:** Convention slip rather than error — but downstream uses (especially $\|(I-W)^{-1}\|_2 \leq 1/m$) need the operator-norm version, not PSD, and the paper conflates them.
- **Notation.** $\mathbf{z}^{[k]}$ is used both as iteration index *and* in the same definition without ambiguity; OK.
- **Existence/uniqueness of fixed point.** The definition asserts that "$\mathbf{z}^*$ exists" but the well-posedness is deferred to Appendix Section \ref{sec:well_poseness_of_monDEQ}. OK as a forward reference.
