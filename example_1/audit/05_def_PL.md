# 05. Definition — Polyak–Łojasiewicz (PL) inequality (PL_def)

**Source:** L399–L406, `\begin{definition}[PL inequality]`.
**Item Status:** 🟢 Valid

---

## Statement

A differentiable function $f:\mathbb{R}^p \to \mathbb{R}$ satisfies the PL inequality with constant $\mu > 0$ if
$$
\tfrac{1}{2}\|\nabla f(x)\|^2 \;\geq\; \mu\bigl(f(x) - f^*\bigr) \qquad \forall x,
$$
where $f^* = \inf_x f(x)$.

## Original Proof

(Definition.)

## Notes / Errors

- Standard definition of PL. Note this is **not** used directly in the main convergence theorem of this paper — the paper uses a continuous-time gradient flow argument with $\sigma_{\min}(Z^*)$ playing a PL-like role on the readout weights $\mathbf{u}$ alone (see [[17_cor_exp_decay]]).
