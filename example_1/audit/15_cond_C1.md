# 15. Condition — Initialization for continuous-time case (C1)

**Source:** L696–L712, label `cond:C1`. Subparts (C1a) at L708, (C1b) at L709–L710.
**Item Status:** 🟡 Warning — (C1b) stated but never derived; constants verified to match the main-theorem requirement.
**Phase-3 verification:** E15-C1B 🚨 confirmed: L708–L710 *state* both inequalities; L875 *uses* (C1b) ("Substituting the drift bounds and applying Condition (C1b) gives $\|Z^*(T^*) - Z^*(0)\|_F < \lambda_0/4$") without showing the substitution. My Audit S5 reconstructed the algebra: starting from drift bounds + [[13_lem_Zstar_perturb]], the required bound on $\lambda_0^3$ is exactly $\frac{16\bar\lambda_\mathbf u\|X\|_F^2\sqrt{2\mathcal L(0)}}{m^2}\bigl[1 + \tfrac{4\bar\lambda_U^2(\bar\lambda_A^2+1)}{m^2}\bigr]$, which matches L709–710 verbatim (using $\bar\lambda_{UA} = \bar\lambda_U\bar\lambda_A$).

---

## Statement

Let
$$
C_{\max} = \max\!\Bigl\{\tfrac{2\bar\lambda_{Uu A}}{m^2 R_A},\; \tfrac{2\bar\lambda_{Uu}}{m^2 R_B},\; \tfrac{\bar\lambda_\mathbf{u}}{m R_U},\; \tfrac{\bar\lambda_U}{m R_u}\Bigr\}.
$$
Conditions on $\lambda_0 = \sigma_{\min}(Z(0))$:
$$
(C1a):\quad \lambda_0^2 \geq 8\|X\|_F\sqrt{2\mathcal{L}(\theta_0)}\, C_{\max},
$$
$$
(C1b):\quad \lambda_0^3 \geq 16\|X\|_F^2\sqrt{2\mathcal{L}(\theta_0)}\,\frac{\bar\lambda_\mathbf{u}}{m^2}\!\left(\tfrac{4(\bar\lambda_{UA}^2+\bar\lambda_U^2)}{m^2} + 1\right).
$$

## Notes / Errors

- **Flag-C1-A (constant mismatch with proof).**
  The main theorem proof at L852 invokes (C1a) as
  $$\lambda_0^2 \geq 16\|X\|_F\sqrt{2\mathcal{L}(0)}\cdot\frac{\bar\lambda_{Uu A}}{m^2 R_A}.$$
  But Condition (C1a) as stated gives only $\lambda_0^2 \geq 8\|X\|_F\sqrt{2\mathcal L(0)}\cdot C_{\max} \geq 8\|X\|_F\sqrt{2\mathcal L(0)}\cdot\frac{2\bar\lambda_{Uu A}}{m^2 R_A} = 16\|X\|_F\sqrt{2\mathcal L(0)}\frac{\bar\lambda_{Uu A}}{m^2 R_A}$.
  These match (the factor of $2$ inside the max times the leading $8$ gives $16$). ✓ The proof works out, but it is easy to read it as a contradiction.
- **Flag-C1-B (factor checking).**
  The constant $8$ in (C1a) comes from the chain: drift bound is $\frac{2\bar\lambda\|X\|_F}{m^2}\cdot\frac{4\sqrt{2\mathcal L(0)}}{\lambda_0^2} = \frac{8\bar\lambda\|X\|_F\sqrt{2\mathcal L(0)}}{m^2\lambda_0^2}$, and we want this $\leq R/2$ to get strict inequality $<R$. So $\lambda_0^2 \geq 16\bar\lambda\|X\|_F\sqrt{2\mathcal L(0)}/(m^2 R)$, i.e., $\lambda_0^2 \geq 8\|X\|_F\sqrt{2\mathcal L(0)} \cdot \frac{2\bar\lambda}{m^2 R}$. Matches the $C_{\max}$ form. ✓
- **Flag-C1-C (C1b not used; computation unverified).** (C1b) is **stated** but the main proof L832–L881 invokes it only with "Substituting the drift bounds and applying Condition (C1b) gives $\|Z^*(T^*)-Z^*(0)\|_F < \lambda_0/4$" — the substitution itself is left out. The constant $16$, the factor $\bar\lambda_\mathbf u/m^2$, and the bracketed expression $4(\bar\lambda_{UA}^2 + \bar\lambda_U^2)/m^2 + 1$ are not derived. See [[16_thm_main_continuous]] Flag-MC3 for the computation gap.

## Audit (verification of constants)

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| C-S1 | From [[14_lem_drift]] eq (drift_A) and corollary [[17_cor_exp_decay]] eq (error_integral): $\|A(T) - A(0)\|_F \leq \tfrac{2\bar\lambda_{Uu A}\|X\|_F}{m^2}\cdot \tfrac{4\sqrt{2\mathcal L(0)}}{\lambda_0^2} = \tfrac{8\bar\lambda_{Uu A}\|X\|_F\sqrt{2\mathcal L(0)}}{m^2\lambda_0^2}$. | Drift bound, error integral. | ✓ |
| C-S2 | Demand $\|A(T)-A(0)\|_F \leq R_A/2 < R_A$, so $\tfrac{8\bar\lambda_{Uu A}\|X\|_F\sqrt{2\mathcal L(0)}}{m^2\lambda_0^2} \leq R_A/2 \Leftrightarrow \lambda_0^2 \geq \tfrac{16 \bar\lambda_{Uu A}\|X\|_F\sqrt{2\mathcal L(0)}}{m^2 R_A} = 8\|X\|_F\sqrt{2\mathcal L(0)}\cdot \tfrac{2\bar\lambda_{Uu A}}{m^2 R_A}$. | Algebra. | ✓ Matches (C1a). |
| C-S3 | Similarly for $B$: $\lambda_0^2 \geq 8\|X\|_F\sqrt{2\mathcal L(0)}\cdot\tfrac{2\bar\lambda_{Uu}}{m^2 R_B}$. | Analogous. | ✓ |
| C-S4 | For $U$: drift is $\tfrac{\bar\lambda_\mathbf u\|X\|_F}{m}\cdot\tfrac{4\sqrt{2\mathcal L(0)}}{\lambda_0^2}$, requiring $\lambda_0^2 \geq \tfrac{8\bar\lambda_\mathbf u\|X\|_F\sqrt{2\mathcal L(0)}}{m R_U} = 8\|X\|_F\sqrt{2\mathcal L(0)}\cdot\tfrac{\bar\lambda_\mathbf u}{m R_U}$. | Analogous. | ✓ |
| C-S5 | For $\mathbf u$: $\lambda_0^2 \geq 8\|X\|_F\sqrt{2\mathcal L(0)}\cdot\tfrac{\bar\lambda_U}{m R_u}$. | Analogous. | ✓ |

The four constraints collapse to a single condition with $C_{\max}$, exactly (C1a). ✓
