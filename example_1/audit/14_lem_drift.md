# 14. Lemma — Parameter drift bounds under gradient flow (drift)

**Source:** L652–L665, label `lem:drift`.
**Item Status:** 🟢 Valid (inherits dependencies)

---

## Statement

Assume $\theta(t) \in \mathcal{B}(\theta(0), R_\theta)$ for all $t \in [0, T]$. Then:
$$
\begin{aligned}
\|A(T) - A(0)\|_F &\leq \frac{2\|X\|_F \bar\lambda_{UuA}}{m^2} \int_0^T \|\mathbf e(t)\|_2\, dt, \\
\|B(T) - B(0)\|_F &\leq \frac{2\|X\|_F \bar\lambda_{Uu}}{m^2} \int_0^T \|\mathbf e(t)\|_2\, dt, \\
\|U(T) - U(0)\|_F &\leq \frac{\bar\lambda_\mathbf{u}\|X\|_F}{m} \int_0^T \|\mathbf e(t)\|_2\, dt, \\
\|\mathbf u(T) - \mathbf u(0)\|_2 &\leq \frac{\bar\lambda_U\|X\|_F}{m} \int_0^T \|\mathbf e(t)\|_2\, dt.
\end{aligned}
$$

## Original Proof

By the fundamental theorem of calculus, $\|\theta(T) - \theta(0)\|_F \leq \int_0^T \|\dot\theta(t)\|_F\, dt = \int_0^T \|\nabla_\theta\mathcal{L}(t)\|_F\, dt$. Substitute [[12_lem_grad_bounds]].

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | For each parameter $\theta$: $\theta(T) - \theta(0) = \int_0^T \dot\theta(t)\, dt = -\int_0^T \nabla_\theta\mathcal{L}(t)\, dt$ (under gradient flow). | Gradient flow ODE. | ✓ |
| S2 | Frobenius norm of integral $\leq$ integral of Frobenius norm: $\|\theta(T) - \theta(0)\|_F \leq \int_0^T \|\nabla_\theta\mathcal{L}(t)\|_F\, dt$. | Minkowski / Jensen. | ✓ |
| S3 | Substitute the bound from [[12_lem_grad_bounds]] for each parameter. E.g. for $A$: $\|\nabla_A\mathcal{L}\|_F \leq (2\bar\lambda_{Uu A}\|X\|_F/m^2)\|\mathbf e(t)\|_2$. | [[12_lem_grad_bounds]]. | ✓ |
| S4 | $\|A(T) - A(0)\|_F \leq (2\bar\lambda_{Uu A}\|X\|_F/m^2)\int_0^T \|\mathbf e(t)\|_2\, dt$. Similarly for $B, U, \mathbf{u}$. | S2, S3. | ✓ |

### Flags

- **Flag-D1 (inherits [[12_lem_grad_bounds]] Critical).** The bound is correct only assuming [[12_lem_grad_bounds]] holds, which assumes [[11_lem_upperbound_VQ]]. The chain inherits the Critical concern.

## Notes / Errors

Straightforward consequence of FTC + previous lemma. Conclusions valid pending earlier dependencies.
