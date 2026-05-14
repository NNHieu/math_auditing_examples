# 12. Lemma — Gradient norm upper bounds (grad_bounds)

**Source:** L527–L563, label `lem:grad_bounds`.
**Item Status:** 🔴 Inherits Critical from [[11_lem_upperbound_VQ]].
**Phase-3 verification:** E12-CHN 🚨 confirmed: L548, L551, L557 explicitly invoke "$\|(\mathbf{u}^\top \otimes I_N)Q^{-1}D\|_2 \leq \bar\lambda_\mathbf u/m$" (via [[11_lem_upperbound_VQ]] + $\|D\|_2 \leq 1$). Since the proof of the underlying lemma is invalid (E11-PI/RG), all four gradient bounds inherit the gap.

---

## Statement

If $\theta \in \mathcal{B}(\theta(0), R_\theta)$, then
$$
\begin{aligned}
\|\nabla_\mathbf{u}\mathcal{L}\|_2 &\leq \|Z^*\|_F\|\mathbf{e}\|_2 \leq \tfrac{\bar\lambda_U\|X\|_F}{m}\|\mathbf{e}\|_2, \\
\|\nabla_U\mathcal{L}\|_F &\leq \tfrac{\bar\lambda_\mathbf{u}\|X\|_F}{m}\|\mathbf{e}\|_2, \\
\|\nabla_B\mathcal{L}\|_F &\leq \tfrac{2\bar\lambda_\mathbf{u}\|Z^*\|_F}{m}\|\mathbf{e}\|_2 \leq \tfrac{2\bar\lambda_\mathbf{u}\bar\lambda_U\|X\|_F}{m^2}\|\mathbf{e}\|_2, \\
\|\nabla_A\mathcal{L}\|_F &\leq \tfrac{2\bar\lambda_\mathbf{u}\|Z^*\|_F\bar\lambda_A}{m}\|\mathbf{e}\|_2 \leq \tfrac{2\bar\lambda_\mathbf{u}\bar\lambda_U\bar\lambda_A\|X\|_F}{m^2}\|\mathbf{e}\|_2.
\end{aligned}
$$

## Original Proof

(See L545–L563.) Sub-multiplicativity of operator norm, $\|P_B\|_2 \leq 2$, $\|P_A\|_2 \leq 2\bar\lambda_A$, $\|K^{(n)}\|_2 = 1$, [[11_lem_upperbound_VQ]] and [[04_lem_Zstar_norm]].

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | $\|\nabla_\mathbf{u}\mathcal{L}\|_2 = \|Z^*\mathbf{e}\|_2 \leq \|Z^*\|_2\|\mathbf{e}\|_2 \leq \|Z^*\|_F\|\mathbf{e}\|_2$. | [[10_lem_grad_loss]]. | $\|Az\| \leq \|A\|_2\|z\|$, $\|A\|_2 \leq \|A\|_F$. |
| S2 | $\|Z^*\|_F \leq \bar\lambda_U \|X\|_F/m$. | [[04_lem_Zstar_norm]] with $\|U\| \leq \bar\lambda_U$ inside the ball. | ✓ |
| S3 | For $U$: $\nabla_U\mathcal{L} = \mathbf{e}^\top(\mathbf{u}^\top\otimes I_N)Q^{-1}D[X^\top\otimes I_n]$ ([[10_lem_grad_loss]]). Frobenius norm of matrix grad equals $\ell_2$ norm of vectorized: $\|\nabla_U\mathcal{L}\|_F = \|\mathbf{e}^\top (\dots)\|_2 \leq \|\mathbf{e}\|_2 \cdot \|(\mathbf{u}^\top\otimes I_N)Q^{-1}D[X^\top\otimes I_n]\|_2$. | Sub-multiplicativity. | ✓ |
| S4 | $\|(\mathbf{u}^\top\otimes I_N)Q^{-1}\|_2 \leq \bar\lambda_\mathbf{u}/m$. | [[11_lem_upperbound_VQ]] inside the ball where $\|\mathbf{u}\| \leq \bar\lambda_\mathbf{u}$. | See Flag-GB1: the bound [[11_lem_upperbound_VQ]] gives $\|\mathbf u\|/m$; using $\|\mathbf u\| \leq \bar\lambda_\mathbf u$ gives $\bar\lambda_\mathbf u/m$. |
| S5 | $\|D\|_2 \leq 1$. | $D$ is a $0/1$ diagonal matrix. | ✓ |
| S6 | $\|X^\top \otimes I_n\|_2 = \|X^\top\|_2 = \|X\|_2 \leq \|X\|_F$. | K2. | ✓ |
| S7 | $\|\nabla_U \mathcal{L}\|_F \leq \|\mathbf{e}\|_2 \cdot \bar\lambda_\mathbf{u}/m \cdot 1 \cdot \|X\|_F = \bar\lambda_\mathbf{u}\|X\|_F\|\mathbf e\|_2/m$. | S3–S6. | ✓ |
| S8 | For $B$: $\|P_B\|_2 = \|I - K^{(n)}\|_2 \leq \|I\|_2 + \|K^{(n)}\|_2 = 1 + 1 = 2$ since $K^{(n)}$ is a permutation matrix. | Spectral norm of permutation matrix is $1$. | ✓ |
| S9 | $\|\nabla_B \mathcal{L}\|_F \leq \|\mathbf e\|_2 \cdot \bar\lambda_\mathbf u/m \cdot \|Z^{*\top}\otimes I_n\|_2 \cdot \|P_B\|_2 \leq (\bar\lambda_\mathbf u/m)\|Z^*\|_F \cdot 2 \cdot \|\mathbf e\|_2 = (2\bar\lambda_\mathbf u/m)\|Z^*\|_F\|\mathbf e\|_2$. Apply S2 for the second bound. | Sub-mult., S2. | ✓ |
| S10 | For $A$: $\|P_A\|_2 = \|(I + K^{(n)})(I_n \otimes A^\top)\|_2 \leq \|I + K^{(n)}\|_2\|I_n\otimes A^\top\|_2 \leq 2 \cdot \|A^\top\|_2 \leq 2\bar\lambda_A$. | Sub-mult., $\|I + K\|_2 \leq 2$, $\|I_n \otimes A^\top\|_2 = \|A\|_2$ (K2). | ✓ |
| S11 | $\|\nabla_A \mathcal{L}\|_F \leq (\bar\lambda_\mathbf u/m)\|Z^*\|_F \cdot 2\bar\lambda_A \cdot \|\mathbf e\|_2$. | As S9 with $P_A$ instead. | ✓ |

### Flags on this proof

- **Flag-GB1 (depends on [[11_lem_upperbound_VQ]]).** Steps S4 inherits the **fatal flaw** in the proof of [[11_lem_upperbound_VQ]] (the bound itself is correct but unjustified in the paper). All gradient bounds in this lemma depend on that result. **Severity: Inherits Critical.**
- **Flag-GB2 ($D$ as $0/1$ matrix).** S5 uses $\|D\|_2 \leq 1$. For ReLU's $\sigma' \in \{0,1\}$ a.e., this holds. At the kink, the subdifferential is $[0,1]$ — paper does not address this. **Severity: Minor.**
- **Flag-GB3 (Frobenius vs spectral on $Z^*$).** S9 uses $\|Z^{*\top}\otimes I_n\|_2 = \|Z^*\|_2 \leq \|Z^*\|_F$. ✓ (with potential looseness; this is the standard step.) **Severity: None.**

## Notes / Errors

Reasoning is correct **modulo** the unproven [[11_lem_upperbound_VQ]]. Inherits its Critical flag.
