# 17. Corollary — Exponential decay of $\mathcal{L}$ along gradient flow (exp_decay)

**Source:** L746–L770, label `cor:exp_decay`.
**Item Status:** 🟡 Warning — argument correct; hypothesis-scope mismatch (E17-SCOPE).
**Phase-3 verification:** E17-SCOPE 🚨 confirmed: L747 states the hypothesis "for all $t \in [0, T)$" (potentially finite $T$), but L750 writes the conclusion as an integral over $[0, \infty)$. Inside the induction in [[16_thm_main_continuous]], the corollary is invoked at $T = T^*$ and the bound holds on $[0, T^*]$ — practically fine, but the standalone statement is mis-scoped.

---

## Statement

Suppose $\sigma_{\min}(Z^*(t)) \geq \lambda_0/2$ for all $t \in [0, T)$. Then
$$
\mathcal{L}(t) \leq e^{-\lambda_0^2 t/2}\mathcal{L}(0),
\qquad
\int_0^\infty \|\mathbf e(t)\|_2\, dt \leq \frac{4\sqrt{2\mathcal L(0)}}{\lambda_0^2}.
$$

## Original Proof

$\dot{\mathcal L} = -\|\nabla_\theta\mathcal L\|^2_F \leq -\|\nabla_\mathbf u\mathcal L\|^2_2 = -\|Z^*\mathbf e\|^2_2 \leq -\sigma_{\min}^2(Z^*)\|\mathbf e\|^2 \leq -(\lambda_0^2/4)\|\mathbf e\|^2 = -(\lambda_0^2/4)\cdot 2\mathcal L = -(\lambda_0^2/2)\mathcal L$. Grönwall: $\mathcal L(t) \leq e^{-\lambda_0^2 t/2}\mathcal L(0)$. For the integral: $\|\mathbf e(t)\|_2 = \sqrt{2\mathcal L(t)} \leq \sqrt{2\mathcal L(0)}e^{-\lambda_0^2 t/4}$, integrate.

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | Along gradient flow: $\dot{\mathcal L} = \langle \nabla_\theta\mathcal L, \dot\theta\rangle = -\|\nabla_\theta\mathcal L\|^2_F$. | Definition of gradient flow. | ✓ |
| S2 | $\|\nabla_\theta\mathcal L\|^2_F \geq \|\nabla_\mathbf u\mathcal L\|^2_2$. | Frobenius norm is sum of squared partials over all params. | ✓ |
| S3 | $\nabla_\mathbf u\mathcal L = Z^*\mathbf e$. | [[10_lem_grad_loss]]. | ✓ |
| S4 | $\|Z^*\mathbf e\|^2_2 \geq \sigma_{\min}^2(Z^*)\|\mathbf e\|^2_2$. | Properties of singular values. | If $Z^* \in \mathbb{R}^{n \times N}$ has rank $\geq N$ (over-parameterized, $n \geq N$), $\sigma_{\min}(Z^*)$ denotes the smallest singular value, and $\|Z^*\mathbf e\|^2 = \mathbf e^\top Z^{*\top} Z^* \mathbf e \geq \sigma_{\min}^2(Z^*)\|\mathbf e\|^2$. ✓ **But this requires $n \geq N$**, the over-parameterization assumption stated at L667. |
| S5 | Hypothesis: $\sigma_{\min}(Z^*(t)) \geq \lambda_0/2$, so $\sigma_{\min}^2(Z^*) \geq \lambda_0^2/4$. | Hypothesis. | ✓ |
| S6 | $\dot{\mathcal L} \leq -(\lambda_0^2/4)\|\mathbf e\|^2_2 = -(\lambda_0^2/4)\cdot 2\mathcal L = -(\lambda_0^2/2)\mathcal L$. | $\|\mathbf e\|^2 = 2\mathcal L$ since $\mathcal L = \frac{1}{2}\|\mathbf e\|^2$. | ✓ |
| S7 | Grönwall: $\mathcal L(t) \leq e^{-\lambda_0^2 t/2}\mathcal L(0)$. | S6, integrate. | ✓ |
| S8 | $\|\mathbf e(t)\|_2 = \sqrt{2\mathcal L(t)} \leq \sqrt{2\mathcal L(0)}\,e^{-\lambda_0^2 t/4}$. | S7, $\sqrt{\cdot}$ monotone. | ✓ |
| S9 | $\int_0^\infty \|\mathbf e(t)\|_2\, dt \leq \sqrt{2\mathcal L(0)}\int_0^\infty e^{-\lambda_0^2 t/4}\, dt = \sqrt{2\mathcal L(0)}\cdot \frac{4}{\lambda_0^2} = \frac{4\sqrt{2\mathcal L(0)}}{\lambda_0^2}$. | S8. | ✓ |

### Flags

- **Flag-ED1 (hypothesis scope).** The corollary is stated for $t \in [0, T)$ (with potentially finite $T$), but the integral is over $[0, \infty)$. The integral bound is well-defined only if Property 2 ($\sigma_{\min}(Z^*) \geq \lambda_0/2$) holds for all $t \in [0,\infty)$, **which is exactly what the main theorem [[16_thm_main_continuous]] sets out to prove**. So the corollary as stated is **slightly mis-scoped**: with the stated hypothesis, the integral up to $T$ is bounded by $\sqrt{2\mathcal L(0)}\cdot(1 - e^{-\lambda_0^2 T/4})/(\lambda_0^2/4) \leq \tfrac{4\sqrt{2\mathcal L(0)}}{\lambda_0^2}$ — so the inequality holds for all $T$, but reading it as an *integral to $\infty$* requires that the hypothesis hold globally, which is the conclusion of the main theorem, not its premise. The corollary is used inside the **continuous-induction** argument of [[16_thm_main_continuous]] at $T = T^*$, where the integral over $[0, T^*]$ is bounded by the same constant. So **practically OK**, but the statement glosses over the scope mismatch. **Severity: Minor.**
- **Flag-ED2 (chained inequalities use the *full* gradient).** Step S2 throws away the contributions of $\nabla_A, \nabla_B, \nabla_U$. This is the *PL-on-readout* trick: $\|\nabla_\theta\|^2 \geq \|\nabla_\mathbf u\|^2$ alone provides the descent rate. Sound — but it implicitly assumes those other gradient components don't cancel against $\nabla_\mathbf u$, which they can't since the sum of squares is a sum of non-negatives. ✓ **Severity: None.**
- **Flag-ED3 ($\sigma_{\min}(Z^*)$ vs $\sigma_{\min}(Z(0))$).** The hypothesis bounds $\sigma_{\min}(Z^*(t)) \geq \lambda_0/2$, where $\lambda_0 := \sigma_{\min}(Z(0))$ (a constant). The corollary's *output* uses $\lambda_0^2/2$ in the exponential rate; the corollary correctly does **not** use $\sigma_{\min}^2(Z^*(t))/2$ — that would be time-dependent and not yield a clean exponential decay. ✓ **Severity: None.**

## Notes / Errors

Correct conditional on the over-parameterization assumption $n \geq N$ and on the singular-value hypothesis. Stated as a corollary because of the slight scope mismatch (Flag-ED1).
