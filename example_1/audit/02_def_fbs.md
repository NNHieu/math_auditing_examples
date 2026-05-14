# 02. Definition — Forward–Backward Splitting iteration (def_fbs)

**Source:** Section 3.2, `\begin{definition}[FBS iteration]` at L294–L300.
**Item Status:** 🟢 Valid

---

## Statement

Given $(W, U)$ and $X$, initialize $Z^{[0]}$ (e.g., $0$) and iterate
$$
Z^{[\ell+1]} = \sigma\!\bigl(T_\alpha Z^{[\ell]} + \alpha U X\bigr),
$$
where $T_\alpha = I - \alpha(I - W)$ and $\alpha > 0$.

## Original Proof

(Definition — no proof.)

## Notes / Errors

- Standard forward–backward splitting form (forward step in $I-W$, backward through the proximal operator of the indicator of $\mathbb{R}_{\geq 0}^n$, which equals ReLU). The relation between this fixed point and the [[01_def_mondeq]] fixed point is justified in Appendix \ref{sec:well_poseness_of_monDEQ}.
- Uses the same data matrix $X$ as columns of inputs, so the iteration is applied batch-wise (matrix $Z^{[\ell]} \in \mathbb{R}^{n \times N}$ as in (\ref{eq:eq_point})).
