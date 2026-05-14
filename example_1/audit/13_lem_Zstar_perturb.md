# 13. Lemma — Perturbation of $Z^*$ and $\hat{\mathbf{y}}$ (Zstar_perturb)

**Source:** L575–L606, label `lem:Zstar_perturb`.
**Item Status:** 🔴 Contains Fatal Error (proof gap).
**Phase-3 verification:**
- E13-GAP 🚨 **confirmed (Critical) at L595:** the proof literally writes
  `(I-W)(Z-Z') = (W-W')Z' + (U-U')X + (\text{ReLU correction}),`
  with `(\text{ReLU correction})` as a LaTeX placeholder text. There is no definition of what the ReLU correction is and no derivation of how the bound that follows ("applying $\|(I-W)^{-1}\|_2 \leq 1/m$") is rigorously obtained. The correct argument requires firm non-expansiveness of ReLU as the proximal operator of the indicator of $\mathbb{R}_{\geq 0}^n$, which gives the inner-product inequality $\|\Delta\|^2 \leq \langle \Delta, W\Delta + (W-W')Z' + (U-U')X\rangle$ and then strong monotonicity $\langle \Delta, (I-W)\Delta\rangle \geq m\|\Delta\|^2$ to extract $\|\Delta\| \leq (\|W-W'\|\|Z'\| + \|U-U'\|\|X\|)/m$.
- E13-NOT 🚨 confirmed at L576: statement uses $\theta = (W, U, \mathbf u)$ where the main paper L196 uses $\theta = (A, B, U, \mathbf u)$. The reduction $\|W - W'\|_2 \leq 2\bar\lambda_A\|A-A'\|_2 + 2\|B-B'\|_2$ is deferred to L869–L873.

---

## Statement

Let $\theta = (W, U, \mathbf{u})$ and $\theta' = (W', U', \mathbf{u}')$. Let $Z = Z(\theta)$, $Z' = Z(\theta')$ be the corresponding equilibrium feature matrices. Then
$$
\|Z - Z'\|_F \leq \frac{\|X\|_F\|U-U'\|_2}{m} + \frac{\|X\|_F\|W-W'\|_2\|U'\|_2}{m^2},
$$
$$
\|\hat{\mathbf y} - \hat{\mathbf y}'\|_2 \leq \frac{\|X\|_F\|\mathbf u\|_2\|U-U'\|_2}{m} + \frac{\|X\|_F\|\mathbf u\|_2\|W-W'\|_2\|U'\|_2}{m^2} + \frac{\|X\|_F\|U'\|_2\|\mathbf u - \mathbf u'\|_2}{m}.
$$

## Original Proof

"From $Z - Z' = \sigma(WZ+UX) - \sigma(W'Z'+U'X)$, non-expansiveness of ReLU gives $\|Z-Z'\|_F \leq \|WZ + UX - W'Z' - U'X\|_F$. Rewriting,
$$(I-W)(Z-Z') = (W-W')Z' + (U-U')X + (\text{ReLU correction}),$$
and applying $\|(I-W)^{-1}\|_2 \leq 1/m$ yields the bound, together with $\|Z'\|_F \leq \|U'\|_2\|X\|_F/m$."

For $\hat{\mathbf y}$: $\hat{\mathbf y}-\hat{\mathbf y}' = (Z-Z')^\top \mathbf u + Z^{'\top}(\mathbf u - \mathbf u')$. Then apply the $Z$ bound.

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | $Z = \sigma(WZ + UX)$, $Z' = \sigma(W'Z' + U'X)$. | Fixed-point definitions. | ✓ |
| S2 | $Z - Z' = \sigma(WZ + UX) - \sigma(W'Z' + U'X)$. | S1, subtract. | ✓ |
| S3 | (Paper:) $\|Z-Z'\|_F \leq \|WZ + UX - W'Z' - U'X\|_F$. | ReLU 1-Lipschitz. | ✓ |
| S4 | Rewrite $WZ - W'Z' = WZ - WZ' + WZ' - W'Z' = W(Z-Z') + (W-W')Z'$. | Algebraic identity. | ✓ |
| S5 | Therefore $WZ + UX - W'Z' - U'X = W(Z-Z') + (W-W')Z' + (U-U')X$. | S4. | ✓ |
| S6 | Combining S3, S5: $\|Z-Z'\|_F \leq \|W(Z-Z') + (W-W')Z' + (U-U')X\|_F$. | S3, S5. | ✓ |
| **S7** | **(Paper's leap.)** $\|(I-W)(Z-Z')\|_F \leq \|(W-W')Z'\|_F + \|(U-U')X\|_F + \text{"ReLU correction"}$. | ??? | **See Flag-ZP1: this step is not justified by S6.** From S6, all we have is $\|Z-Z'\|_F \leq \|W(Z-Z') + (W-W')Z' + (U-U')X\|_F$, **not** $\|(I-W)(Z-Z')\|_F$ on the LHS. The paper effectively wants to move $W(Z-Z')$ to the LHS, but **ReLU non-expansiveness only gives $\|Z-Z'\|_F$, not equality**. The "ReLU correction" hand-waves this. |
| S7-corr | (Correction.) Let $\Delta = Z - Z'$. From ReLU non-expansiveness and $\sigma$ being a *proximal operator* (firmly non-expansive), we have $\|\Delta\|_F^2 \leq \langle \Delta, W\Delta + (W-W')Z' + (U-U')X\rangle$. Then $\|\Delta\|_F^2 - \langle \Delta, W\Delta\rangle \leq \|\Delta\|_F \cdot (\|W-W'\|_2\|Z'\|_F + \|U-U'\|_2\|X\|_F)$. Strong monotonicity gives $\langle \Delta, (I-W)\Delta\rangle \geq m\|\Delta\|_F^2$, so $\|\Delta\|_F^2 \cdot m \leq \|\Delta\|_F \cdot \text{RHS}$, yielding $\|\Delta\|_F \leq (\text{RHS})/m$. This is the **firmly-non-expansive argument** used in the original monDEQ paper. The paper here under-uses ReLU's structure. | Firm non-expansiveness of $\sigma = \prox_{f}$ for $f$ indicator of $\mathbb R_{\geq 0}^n$. | This produces the correct bound but requires a stronger ReLU property than mere Lipschitz. |
| S8 | Given S7-corr, $\|Z-Z'\|_F \leq \|W-W'\|_2\|Z'\|_F/m + \|U-U'\|_2\|X\|_F/m$. | S7-corr. | ✓ |
| S9 | $\|Z'\|_F \leq \|U'\|_2\|X\|_F/m$. | [[04_lem_Zstar_norm]]. | ✓ |
| S10 | $\|Z-Z'\|_F \leq \|U-U'\|_2\|X\|_F/m + \|W-W'\|_2\|U'\|_2\|X\|_F/m^2$. | S8, S9. | ✓ |
| S11 | $\hat{\mathbf y} - \hat{\mathbf y}' = (Z-Z')^\top \mathbf u + Z^{'\top}(\mathbf u - \mathbf u')$. | $\hat{\mathbf y} = Z^\top \mathbf u$, expand. | ✓ |
| S12 | $\|\hat{\mathbf y} - \hat{\mathbf y}'\|_2 \leq \|Z-Z'\|_F\|\mathbf u\|_2 + \|Z'\|_F\|\mathbf u - \mathbf u'\|_2$. | S11, sub-mult. | $\|(Z-Z')^\top \mathbf u\|_2 \leq \|Z-Z'\|_2\|\mathbf u\|_2 \leq \|Z-Z'\|_F\|\mathbf u\|_2$. ✓ |
| S13 | Combine S10, S12, S9 to get the $\hat{\mathbf y}$ bound. | Substitution. | ✓ |

### Flags on this proof

- **Flag-ZP1 (load-bearing gap).** The paper writes "$(I-W)(Z-Z') = (W-W')Z' + (U-U')X + (\text{ReLU correction})$" without specifying what the ReLU correction is. As written, this is **not** an equality and not even an inequality — it's a rewrite that's only valid if ReLU is *applied in the same active pattern* on both sides, which generally fails when $\theta \neq \theta'$. The correct path (above S7-corr) uses **firm non-expansiveness** of $\sigma$ (as a proximal operator), giving an inequality involving the inner product, not the literal equality. The end **conclusion is correct**, but the proof is invalid as written.
  - This argument is exactly the form used in the original monDEQ paper (Winston & Kolter, NeurIPS 2020, Eq. (10) and following) and is non-trivial to skip.
  - **Severity: Critical** (proof step does not justify the claim; conclusion correct but requires firm non-expansiveness which the paper does not invoke).
- **Flag-ZP2 (notation mismatch).** Statement uses $\theta = (W, U, \mathbf u)$, but in the rest of the paper $\theta = (A, B, U, \mathbf u)$. The lemma is happy to treat $W$ as a single variable for the perturbation, but consistency with $W = (1-m)I - A^\top A + B - B^\top$ requires reducing $\|W - W'\|_2$ to $\|A-A'\|_2, \|B-B'\|_2$ when applied downstream (this is what [[16_thm_main_continuous]] does via $\|W(T^*) - W(0)\|_2 \leq 2\bar\lambda_A\|A(T^*) - A(0)\|_2 + 2\|B(T^*) - B(0)\|_2$, dropping a quadratic term — see Flag-MC2 in [[16_thm_main_continuous]]). **Severity: Minor.**

## Notes / Errors

Final inequality is correct, but the proof skipped a non-trivial step (firm non-expansiveness of ReLU).
