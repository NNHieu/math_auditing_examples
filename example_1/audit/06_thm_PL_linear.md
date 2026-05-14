# 06. Theorem — Linear convergence under PL [Hamed2016] (PL_linear_thm)

**Source:** L415–L430. `\begin{theorem}[\cite{Hamed2016}]`.
**Item Status:** 🟡 Warning (Needs Review) — minor inconsistency in the proof statement.

---

## Statement

If $f$ has an $L$-Lipschitz gradient and satisfies PL with constant $\mu$, then GD with step size $1/L$ satisfies
$$
f(x_k) - f^* \;\leq\; \bigl(1 - \tfrac{\mu}{L}\bigr)^k\bigl(f(x_0) - f^*\bigr).
$$

## Original Proof

Descent lemma with $L$-smoothness gives $f(x_{k+1}) \leq f(x_k) - \frac{1}{2L}\|\nabla f(x_k)\|^2$. Applying PL: $\frac{1}{2L}\|\nabla f(x_k)\|^2 \geq \frac{\mu}{L}(f(x_k) - f^*)$, so $f(x_{k+1}) - f^* \leq (1 - \mu/L)(f(x_k) - f^*)$. Iterate.

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | (Descent lemma, $L$-smoothness) For step size $\eta = 1/L$: $f(x_{k+1}) \leq f(x_k) - \frac{1}{2L}\|\nabla f(x_k)\|^2$. | $\nabla f$ is $L$-Lipschitz. | Standard descent lemma: $f(x_k - \tfrac{1}{L}\nabla f(x_k)) \leq f(x_k) - \tfrac{1}{L}\|\nabla f(x_k)\|^2 + \tfrac{L}{2}\cdot\tfrac{1}{L^2}\|\nabla f(x_k)\|^2 = f(x_k) - \tfrac{1}{2L}\|\nabla f(x_k)\|^2$. |
| S2 | PL gives $\tfrac{1}{2}\|\nabla f(x_k)\|^2 \geq \mu(f(x_k) - f^*)$. | [[05_def_PL]]. | Definition. |
| S3 | Combining: $f(x_{k+1}) - f^* \leq (1 - \mu/L)(f(x_k) - f^*)$. | S1, S2. | $f(x_{k+1}) \leq f(x_k) - \frac{1}{2L}\|\nabla f(x_k)\|^2 \leq f(x_k) - \frac{\mu}{L}(f(x_k) - f^*)$; subtract $f^*$. |
| S4 | $f(x_k) - f^* \leq (1 - \mu/L)^k (f(x_0) - f^*)$. | S3, induction. | ✓ |

### Flag

- **Flag-PL1 (minor).** In the proof at L425, the line "Applying PL: $\frac{1}{2L}\|\nabla f(x_k)\|^2 \geq \frac{\mu}{L}(f(x_k) - f^*)$" is a strict re-scaling of the PL inequality. Strictly, PL gives $\frac{1}{2}\|\nabla f\|^2 \geq \mu(f - f^*)$, which divided by $L$ yields exactly this. OK — no error. **Severity: None.**

## Notes / Errors

- This theorem is cited but **not directly used** in the main result; the actual convergence argument in [[16_thm_main_continuous]] runs a continuous-time variant. Listing it for completeness.
