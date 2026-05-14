# Error Index — Audit of `sample_paper.tex` (Phase 3 validated)

Paper: **On Global Convergence Theory for Monotone DEQ** (Nguyen).

Legend:
- **Status:** ⬜ To validate · 🚨 Confirmed Error · 🟢 False Alarm
- **Severity:** Critical · Major · Minor
- **Confidence:** High · Medium · Low

| File | Error ID | One-line description | Status | Severity | Confidence | Source line(s) |
|------|----------|----------------------|--------|----------|------------|----------------|
| 01_def_mondeq | E01-CONV | "$I-W \succeq mI$" written as PSD, but $W$ has skew part $B - B^\top$ so $I-W$ is non-symmetric; intended condition is operator strong-monotonicity (symmetrized). | 🚨 Confirmed Error | Minor | High | L187, L728 |
| 03_prop_upper_LT | E03-INT | "Contractive for $\alpha \in (0, 2m/L^2]$" — at $\alpha = 2m/L^2$, $L[T_\alpha] \leq 1$ but not strictly $< 1$. | 🚨 Confirmed Error | Minor | High | L330 (cf. L329 open interval) |
| 04_lem_Zstar_norm | E04-LHOP | L'Hôpital step substitutes $\alpha = 0$ into the unsimplified derivative ratio; the cleaner version takes the limit of the simplified ratio. End answer $1/m$ is correct. | 🚨 Confirmed Error | Minor | High | L363–L364 |
| 04_lem_Zstar_norm | E04-MONO | "Differentiating and simplifying ... shows $f'(\alpha) > 0$" — citing "$\sqrt h < 1$" alone does NOT imply $f' > 0$. The actual condition is $\alpha^2(L^2 - m^2) > 0$. | 🚨 Confirmed Error | Minor | High | L368 |
| 04_lem_Zstar_norm | E04-LIM | Identifies $\lim_\ell Z^{[\ell]} = Z^*$ implicitly; FBS convergence needed to pass the bound to the limit. | 🚨 Confirmed Error | Minor | Medium | L344–L352 |
| 07_def_residual | E07-IFT | ReLU's $\sigma'$ is undefined at $0$; IFT used in smooth-derivative form via $D$ without addressing the measure-zero non-differentiability. | 🚨 Confirmed Error | Minor | Medium | L437–L455 (no discussion) |
| 08_lem_deri_g | E08-SIGN | Phase 1 worry: appendix sign mishandling. **Verified:** the equality $-[\mathbf z^\top \otimes J]P_A = J[\dots](I+K)(I\otimes A^\top)$ is correct because $-P_A = +(I+K)(I\otimes A^\top)$. | 🟢 False Alarm | — | High | L1102 |
| 08_lem_deri_g | E08-BATCH | Appendix proves single-sample case; batch lift to $N$-sample matrix form (L461–L473) is stated without separate proof. | 🚨 Confirmed Error | Minor | High | L1041 (`g_\theta(\mathbf z, \mathbf x)`) vs L461 (batch) |
| 09_lem_deri_Z | E09-INV | L452: "Invertibility [of $Q$] is guaranteed by the strong monotonicity of $I-W$" — bare assertion. The correct argument needs an active-subspace restriction (ReLU dead neurons). | 🚨 Confirmed Error | Major | High | L452 |
| 10_lem_grad_loss | E10-VEC | $\partial \hat{\mathbf y}/\partial \operatorname{vec}[Z^*] = \mathbf u^\top \otimes I_N$ is correct only under **row-major** $\operatorname{vec}$, off by $K^{(n,N)}$ under standard column-major. Verified with explicit dimension check ($N=2, n=3$): the actual K1 application yields $I_N \otimes \mathbf u^\top$, not $\mathbf u^\top \otimes I_N$ for column-major. Norm bounds are unaffected. | 🚨 Confirmed Error | Major | High | L510 |
| 11_lem_upperbound_VQ | E11-PI | L1167: "Take the limit $\mu \to 0^+$, we have $\|\Pi\| \le 1$" — **false** for inactive ReLU coords ($J_{ii}=0$, $\Pi_{ii} = 1/\mu \to \infty$). | 🚨 Confirmed Error | Critical | High | L1167 |
| 11_lem_upperbound_VQ | E11-JLT1 | L1137: "$J_{ii} < 1\;\forall i \in [n]$" — for ReLU $J_{ii} \in \{0, 1\}$, so $=1$ in the active state, not $<1$. The "$\Pi \in D^+$" argument is unaffected, but the claim is literally wrong. | 🚨 Confirmed Error | Major | High | L1137 |
| 11_lem_upperbound_VQ | E11-RG | $R_G = (I + G)^{-1}$ for $G$ not verified maximal monotone; firmly-non-expansive bound on $R_G$ not justified. | 🚨 Confirmed Error | Critical | Medium | L1155–L1157 |
| 11_lem_upperbound_VQ | E11-TYPO | L1183: `\time` typo (should be `\times`). | 🚨 Confirmed Error | Minor | High | L1183 |
| 12_lem_grad_bounds | E12-CHN | Inherits the unproven [[11_lem_upperbound_VQ]]: all four gradient bounds use $\|(\mathbf u^\top\otimes I_N)Q^{-1}\| \leq \|\mathbf u\|/m$. | 🚨 Confirmed Error (inherited) | Critical (inherited) | High | L548, L551, L557 |
| 13_lem_Zstar_perturb | E13-GAP | L595: "$(I-W)(Z-Z') = (W-W')Z' + (U-U')X + (\text{ReLU correction})$" with `(\text{ReLU correction})` literally a placeholder. Correct proof requires **firm** non-expansiveness of ReLU as a proximal operator. | 🚨 Confirmed Error | Critical | High | L595 |
| 13_lem_Zstar_perturb | E13-NOT | Statement uses $\theta = (W, U, \mathbf u)$ but rest of paper uses $\theta = (A, B, U, \mathbf u)$; reduction of $\|W - W'\|$ to $\|A-A'\|, \|B-B'\|$ deferred to main theorem. | 🚨 Confirmed Error | Minor | Medium | L576, vs L196 |
| 15_cond_C1 | E15-C1B | (C1b) stated but never derived from drift + perturbation in body. **Verified:** constants do match the main-theorem requirement exactly (audit re-derived in 16_thm_main_continuous Audit S5). | 🚨 Confirmed Error | Minor | High | L708–L710 (stated); L875 (used without derivation) |
| 16_thm_main_continuous | E16-QUAD | Phase-1 worry: dropped quadratic in $\|A^\top A - A_0^\top A_0\|$. **Verified false alarm in math:** identity used at L871 is exact ($A_1^\top A_1 - A_0^\top A_0 = (A_1 - A_0)^\top A_1 + A_0^\top (A_1 - A_0)$), giving $2\bar\lambda_A\|A_1-A_0\|_2$ without dropping anything. The parenthetical L873 "dropping the quadratic term for small drift" is **misleading description** — no quadratic term is actually being dropped. | 🟢 False Alarm (math) / 🚨 Confirmed Error (annotation) | Minor | High | L871–L873 |
| 16_thm_main_continuous | E16-ALG | L875: "Substituting the drift bounds and applying Condition (C1b) gives $\|Z^*(T^*) - Z^*(0)\|_F < \lambda_0/4$" — no calculation shown; reader must do 6 lines of algebra. (Constants verify; see audit S5.) | 🚨 Confirmed Error | Major | High | L875 |
| 16_thm_main_continuous | E16-FMT | L877: "$\sigma_{\min} \geq \lambda_0 - \lambda_0/4 = \lambda_0 3/4 > \lambda_0/2$" — math right, formatting "$\lambda_0 3/4$" should be $3\lambda_0/4$. | 🚨 Confirmed Error | Minor | High | L877 |
| 16_thm_main_continuous | E16-WIDTH | L728: "the width of the hidden layer is $O(N)$" — proof needs $n \geq N$ for $\lambda_0 > 0$. $O(N)$ is asymptotically misleading. | 🚨 Confirmed Error | Minor | Medium | L728 |
| 16_thm_main_continuous | E16-INIT | (C1a), (C1b) never instantiated for any random init. "Linear convergence under over-parameterization" is conditional on an existential $\lambda_0$ lower bound never derived. | 🚨 Confirmed Error | Major | High | L696–L712 (conditions) + absence of init section |
| 16_thm_main_continuous | E16-GFGD | Abstract (L75) and intro (L110) claim "gradient descent converges at linear rate"; theorem (L726) is "Global Convergence under Gradient Flow" with continuous-time proof. Discrete-time GD result not closed. | 🚨 Confirmed Error | Major | High | L75, L110 vs L726, L832 |
| 17_cor_exp_decay | E17-SCOPE | Hypothesis stated on $[0, T)$ at L747; integral on $[0, \infty)$ at L750. Works because used at $T = T^*$ inside induction (in the limit the hypothesis extends), but the standalone corollary is mis-scoped. | 🚨 Confirmed Error | Minor | High | L747 vs L750 |
| structure | E-STRUCT-1 | Vec convention not stated paper-wide; column-major vs row-major affects exact-formula correctness (but $\|K^{(n,N)}\|_2 = 1$ so bounds unaffected). | 🚨 Confirmed Error | Minor | High | global |
| structure | E-STRUCT-2 | "Strong monotonicity" used ambiguously — PSD interpretation in some places, $\|(I-W)^{-1}\|_2 \leq 1/m$ form needs the symmetrized/operator version. | 🚨 Confirmed Error | Minor | High | L187, L597 |

---

## Summary of Phase 3 results

**26 errors logged; 24 confirmed, 2 false alarms (one of which had a confirmed sub-issue in its annotation).**

### Critical (3 confirmed)
- **E11-PI**: $\|\Pi\| \leq 1$ in limit is *false*. The whole inner-lemma proof of [[11_lem_upperbound_VQ]] collapses; the conclusion happens to be correct via a different (active-subspace) argument.
- **E11-RG**: resolvent norm bound for $R_G$ not justified — companion gap.
- **E13-GAP**: literal `(\text{ReLU correction})` placeholder in the perturbation-lemma proof; needs firm non-expansiveness, which the paper never invokes.

### Major (7 confirmed)
- **E09-INV**: $Q$ invertibility asserted, not proved.
- **E10-VEC**: vec convention mismatch (formula off by $K^{(n,N)}$).
- **E11-JLT1**: $J_{ii} < 1$ claim is wrong for ReLU.
- **E12-CHN**: inherited Critical from E11-*.
- **E16-ALG**: hidden substitution in main proof (verified constants work).
- **E16-INIT**: (C1a)/(C1b) not instantiated for any random init.
- **E16-GFGD**: discrete-GD vs gradient-flow mismatch between intro/abstract and theorem.

### Minor (14 confirmed)
Notation, presentation, scope, and convention slips throughout.

### False alarms (2)
- **E08-SIGN**: signs do work out via $-P_A = +(I+K)(I\otimes A^\top)$.
- **E16-QUAD (math)**: identity $A_1^\top A_1 - A_0^\top A_0 = (A_1-A_0)^\top A_1 + A_0^\top(A_1-A_0)$ is exact; the parenthetical "dropping the quadratic term" is a misleading note but no quadratic term is actually being dropped.

---

## Conclusion

The **main theorem** is plausibly true, and most of the proof chain works. But as written, the paper has:
- **Two critical proof gaps** (E11-PI/RG; E13-GAP) where the displayed argument does not justify the claim. Both have known correct alternative arguments in the monDEQ literature.
- **Several major rigor gaps** (E09-INV, E16-ALG, E16-INIT, E16-GFGD) where load-bearing steps are asserted rather than derived.
- **One asymptotic-claim mismatch** between the abstract (discrete GD) and the actual theorem (gradient flow).

The paper would benefit from:
1. Filling in [[11_lem_upperbound_VQ]] with the active-subspace argument from Winston & Kolter (NeurIPS 2020).
2. Replacing the `(\text{ReLU correction})` placeholder in [[13_lem_Zstar_perturb]] with the firmly-non-expansive proximal-operator argument.
3. Providing an explicit invertibility-of-$Q$ argument.
4. Writing out the 6-line substitution justifying (C1b) → $\lambda_0/4$ in the main theorem.
5. Either proving a random-init analysis or rephrasing the main result as a *conditional* convergence theorem.
6. Reconciling the gradient-flow theorem with the discrete-GD claim in the abstract.
