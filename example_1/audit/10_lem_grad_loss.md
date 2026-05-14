# 10. Lemma — Partial derivatives of $\mathcal{L}$ (grad_loss)

**Source:** L492–L513, label `lem:grad_loss`.
**Item Status:** 🔴 Contains formula error (E10-VEC) — formulas off by a commutation matrix under column-major $\operatorname{vec}$; bound consequences unaffected.
**Phase-3 verification:** E10-VEC 🚨 confirmed at L510. Explicit dimension check ($N=2, n=3$): with $\mathbf u \in \mathbb R^3$ and column-major $\operatorname{vec}[Z^*] = (Z^*_{11}, Z^*_{21}, Z^*_{31}, Z^*_{12}, Z^*_{22}, Z^*_{32})^\top$,
$(\mathbf u^\top \otimes I_2)\operatorname{vec}[Z^*] = (u_1 Z^*_{11} + u_2 Z^*_{31} + u_3 Z^*_{22},\; u_1 Z^*_{21} + u_2 Z^*_{12} + u_3 Z^*_{32})^\top$,
which does **not** equal $\hat{\mathbf y} = Z^{*\top}\mathbf u = (u_1 Z^*_{11} + u_2 Z^*_{21} + u_3 Z^*_{31},\; u_1 Z^*_{12} + u_2 Z^*_{22} + u_3 Z^*_{32})^\top$. The correct expression is $(I_N \otimes \mathbf u^\top)\operatorname{vec}[Z^*]$. The two forms are related by a commutation matrix $K^{(n,N)}$ which has spectral norm $1$, so the **bound** $\|(\mathbf u^\top \otimes I_N)Q^{-1}\| \leq \|\mathbf u\|/m$ still transfers — only the exact identities are off.

---

## Statement

Let $\mathbf{e} = \hat{\mathbf{y}} - \mathbf{y} \in \mathbb{R}^N$. Then:
$$
\begin{aligned}
\frac{\partial \mathcal{L}}{\partial A} &= \mathbf{e}^\top(\mathbf{u}^\top \otimes I_N)\,Q^{-1}\,D\,[Z^{*\top}\otimes I_n]\,P_A,\\
\frac{\partial \mathcal{L}}{\partial B} &= \mathbf{e}^\top(\mathbf{u}^\top \otimes I_N)\,Q^{-1}\,D\,[Z^{*\top}\otimes I_n]\,P_B,\\
\frac{\partial \mathcal{L}}{\partial U} &= \mathbf{e}^\top(\mathbf{u}^\top \otimes I_N)\,Q^{-1}\,D\,[X^\top\otimes I_n],\\
\frac{\partial \mathcal{L}}{\partial \mathbf{u}} &= Z^* \mathbf{e}.
\end{aligned}
$$

## Original Proof

Chain rule: $\partial\mathcal{L}/\partial\theta = (\partial\mathcal{L}/\partial\hat{\mathbf{y}}) \cdot (\partial\hat{\mathbf{y}}/\partial\operatorname{vec}[Z^*]) \cdot (\partial\operatorname{vec}[Z^*]/\partial\theta)$. Use $\partial\mathcal{L}/\partial\hat{\mathbf{y}} = \mathbf{e}^\top$, $\partial\hat{\mathbf{y}}/\partial\operatorname{vec}[Z^*] = \mathbf{u}^\top \otimes I_N$ (by K1), and [[09_lem_deri_Z]]. For $\mathbf{u}$: $\partial\hat{\mathbf{y}}/\partial\mathbf{u} = Z^{*\top}$, so $\nabla_\mathbf{u}\mathcal{L} = Z^* \mathbf{e}$.

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | $\mathcal{L} = \tfrac{1}{2}\|\hat{\mathbf{y}} - \mathbf{y}\|^2$, $\hat{\mathbf{y}} = Z^{*\top}\mathbf{u}$. | Setup (L237). | Definition. |
| S2 | $\partial \mathcal{L}/\partial \hat{\mathbf{y}} = (\hat{\mathbf{y}} - \mathbf{y})^\top = \mathbf{e}^\top$. | S1. | Standard. |
| S3 | $\hat{\mathbf{y}} = Z^{*\top}\mathbf{u}$ in vectorized form: $\hat{\mathbf{y}} = (\mathbf{u}^\top \otimes I_N)\operatorname{vec}[Z^*]$ (where $Z^* \in \mathbb{R}^{n\times N}$ and $\operatorname{vec}$ stacks columns). | K1: $\operatorname{vec}[ABC] = (C^\top \otimes A)\operatorname{vec}[B]$. | $Z^{*\top}\mathbf{u} = I_N Z^{*\top}\mathbf{u}$, view as $A=I_N, B=Z^{*\top}, C=\mathbf{u}$; then $\operatorname{vec}[I_N Z^{*\top}\mathbf{u}] = (\mathbf{u}^\top \otimes I_N)\operatorname{vec}[Z^{*\top}]$ — see Flag-L1. |
| S4 | $\partial \hat{\mathbf{y}}/\partial \operatorname{vec}[Z^*] = \mathbf{u}^\top \otimes I_N$. | S3, modulo Flag-L1. | ✓ if vec convention is **column-stacking** of $Z^*$ which is what the paper uses. |
| S5 | $\partial \mathcal{L}/\partial \operatorname{vec}[\theta_W]$ for $\theta_W \in \{A,B,U\}$: chain S2 → S4 → [[09_lem_deri_Z]]. | Chain rule. | ✓ |
| S6 | For $\mathbf{u}$: $\hat{\mathbf{y}} = Z^{*\top}\mathbf{u}$, so $\partial \hat{\mathbf{y}}/\partial \mathbf{u} = Z^{*\top}$ and $\nabla_\mathbf{u}\mathcal{L} = (Z^{*\top})^\top \mathbf{e} = Z^*\mathbf{e}$. | S2 and direct. | ✓ |

### Flag

- **Flag-L1 (vec convention).** The identity $\hat{\mathbf{y}} = (\mathbf{u}^\top \otimes I_N)\operatorname{vec}[Z^*]$ depends on whether $\operatorname{vec}$ stacks rows or columns of $Z^*$, and which dimension is $n$ vs $N$. Let $Z^* \in \mathbb{R}^{n \times N}$ (rows = neurons, cols = samples). Then $Z^{*\top}\mathbf{u}$ is in $\mathbb{R}^N$, $j$-th entry equals $\sum_i Z^*_{ij} u_i$. With $\operatorname{vec}[Z^*]$ stacking columns of $Z^*$ (column-major, the standard Matlab convention), $\operatorname{vec}[Z^*]_{(j-1)n + i} = Z^*_{ij}$. Then $(\mathbf{u}^\top \otimes I_N)$ has shape $N \times Nn$, with blocks... Let's verify:
  - $(\mathbf{u}^\top \otimes I_N)$ is $N \times Nn$. Yes. Apply to $\operatorname{vec}[Z^*] \in \mathbb{R}^{Nn}$. Result is $N$-dim.
  - Using K1 with $B = Z^*$, $A = I_N$, $C^\top = \mathbf{u}^\top$ requires $\operatorname{vec}[I_N \cdot Z^* \cdot \text{something}] = \dots$. Easier check: K1 says $\operatorname{vec}[ABC] = (C^\top \otimes A)\operatorname{vec}[B]$. Apply with $A = \mathbf{u}^\top \in \mathbb{R}^{1\times n}$, $B = Z^*$, $C = I_N$: $\operatorname{vec}[\mathbf{u}^\top Z^* I_N] = (I_N \otimes \mathbf{u}^\top)\operatorname{vec}[Z^*]$, with LHS $= \operatorname{vec}[\mathbf{u}^\top Z^*] = \mathbf{u}^\top Z^*$ which is the row vector $\hat{\mathbf{y}}^\top \in \mathbb{R}^{1\times N}$.
  - Equivalently $\hat{\mathbf{y}} = (Z^{*\top}\mathbf{u}) = ((\mathbf{u}^\top Z^*))^\top$, so $\hat{\mathbf{y}} = (I_N \otimes \mathbf{u}^\top)\operatorname{vec}[Z^*]^\top$... this gets tangled.
  - Cleanest: by K1 directly, $\hat{\mathbf{y}} = Z^{*\top}\mathbf{u} = \operatorname{vec}[Z^{*\top}\mathbf{u}] = (\mathbf{u}^\top \otimes I_N)\operatorname{vec}[Z^{*\top}]$. But the paper writes "$\partial \hat{\mathbf{y}}/\partial \operatorname{vec}[Z^*] = \mathbf{u}^\top \otimes I_N$" — using $\operatorname{vec}[Z^*]$, **not** $\operatorname{vec}[Z^{*\top}]$. These two differ by the commutation matrix $K^{(n,N)}$: $\operatorname{vec}[Z^{*\top}] = K^{(n,N)}\operatorname{vec}[Z^*]$.
  - **So the formula as written is off by a commutation matrix unless one redefines $\operatorname{vec}$ to be row-major.** A consistent reading would replace $\operatorname{vec}[Z^*]$ with $\operatorname{vec}[Z^{*\top}]$ throughout the gradient formulas, or absorb a $K^{(n,N)}$ into $Q^{-1}D$.
  - This convention slip propagates into [[09_lem_deri_Z]] and [[12_lem_grad_bounds]]. The bound arguments only ever use operator norms, and commutation matrices have norm $1$, so **the bounds are unaffected**. But the **exact formulas as stated are off by a commutation matrix** under the standard column-major $\operatorname{vec}$.
  - **Severity: Minor (convention slip, presentation; downstream-norm bounds unaffected since $\|K^{(n,N)}\|_2 = 1$).**

## Notes / Errors

Bounds-only consequences are unaffected; formulas need a consistent $\operatorname{vec}$ convention.
