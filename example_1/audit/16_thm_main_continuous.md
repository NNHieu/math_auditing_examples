# 16. Theorem — Global Convergence of monDEQ under Gradient Flow (main_continuous)

**Source:** L726–L740 (statement), L832–L881 (proof). Label `theorem:main_continuous`.
**Item Status:** 🔴 Contains gaps — major load-bearing steps are unjustified (E16-ALG), abstract claims more than the theorem (E16-GFGD), and the chain inherits Critical gaps from [[11_lem_upperbound_VQ]] and [[13_lem_Zstar_perturb]].
**Phase-3 verification:**
- E16-QUAD: 🟢 **Mathematics is a false alarm.** L871 uses the *exact* identity $A_1^\top A_1 - A_0^\top A_0 = (A_1 - A_0)^\top A_1 + A_0^\top(A_1 - A_0)$, which gives $\|\cdot\|_2 \leq 2\bar\lambda_A \|A_1 - A_0\|_2$ **without dropping anything**. However, the parenthetical at L873 ("dropping the quadratic term for small drift") 🚨 misdescribes what the algebra is doing — it appears to be leftover language from an earlier version (see commented L867 which did include a quadratic term). The active math is rigorous; the annotation is wrong.
- E16-ALG 🚨 **confirmed (Major) at L875:** "Substituting the drift bounds and applying Condition (C1b) gives $\|Z^*(T^*) - Z^*(0)\|_F < \lambda_0/4$." No substitution shown. Verified in audit S5 that the (C1b) constants exactly match the requirement (Phase-1 fear of mismatch turned out correct constants but missing derivation).
- E16-FMT 🚨 confirmed at L877: literal text "$\lambda_0 3/4$" (i.e., $\lambda_0 \cdot 3/4 = 3\lambda_0/4$). Math is correct, formatting is awkward.
- E16-WIDTH 🚨 confirmed at L728: "the width of the hidden layer is $O(N)$"; whereas the proof requires $n \geq N$ (over-parameterization regime stated at L667 says "$n \ge N$"). $O(N)$ asymptotic notation here is technically vacuous (any constant multiple of $N$ qualifies, including $n < N$).
- E16-INIT 🚨 confirmed (by absence): Conditions L696–L712 are existential constraints on $\lambda_0$; no section of the paper analyzes their satisfiability under any random-init scheme.
- E16-GFGD 🚨 confirmed: abstract L75 ("the gradient descent converges to the global optimum at a linear rate") and intro L110 ("global linear convergence under gradient descent by leveraging over-parameterization") **vs** theorem L726 ("Global Convergence of monDEQ under Gradient Flow") with proof L609 onwards explicitly continuous-time ("gradient flow or the gradient descent with an infinitesimal step-size").

---

## Statement

Suppose the monotone DEQ is parameterized such that $I - W \succeq mI$, the hidden width is $O(N)$ (with $n \geq N$), and the initialization satisfies Condition [[15_cond_C1]]. Then for all $t \geq 0$:
1. (Bounded drift) $\theta(t) \in \mathcal B(\theta(0), R_\theta)$.
2. (Stable representation) $\sigma_{\min}(Z(t)) \geq \lambda_0/2$.
3. (Exponential convergence) $\mathcal L(\theta(t)) \leq e^{-\lambda_0^2 t/2}\mathcal L(\theta(0))$.

## Original Proof (L832–L881)

Continuous induction. Define $T^* = \sup\{T \geq 0 : \text{P1, P2, P3 hold on } [0,T]\}$. At $t = 0$ they hold strictly, so $T^* > 0$. Assume $T^* < \infty$.
- **P3 at $T^*$:** P2 on $[0, T^*]$ + [[17_cor_exp_decay]] gives $\mathcal L(T^*) \leq e^{-\lambda_0^2 T^*/2}\mathcal L(0)$, strict for $T^* > 0$.
- **P1 at $T^*$:** Apply [[14_lem_drift]] eq (drift_A) over $[0, T^*]$ with the integral bound from [[17_cor_exp_decay]] eq (error_integral): $\|A(T^*) - A(0)\|_F \leq \tfrac{8\bar\lambda_{Uu A}\|X\|_F\sqrt{2\mathcal L(0)}}{m^2\lambda_0^2}$. By (C1a), this is $\leq R_A/2 < R_A$. Repeat for $B, U, \mathbf u$.
- **P2 at $T^*$:** Bound $\|Z^*(T^*) - Z^*(0)\|_F$ via [[13_lem_Zstar_perturb]]:
$$\|Z^*(T^*)-Z^*(0)\|_F \leq \frac{\|X\|_F\|U(T^*)-U(0)\|_2}{m} + \frac{\|X\|_F\|W(T^*)-W(0)\|_2\|U(0)\|_2}{m^2}.$$
$U$-drift bounded by (drift_U). $W$-drift via $W = (1-m)I - A^\top A + B - B^\top$: $\|W(T^*) - W(0)\|_2 \leq 2\bar\lambda_A\|A(T^*) - A(0)\|_2 + 2\|B(T^*) - B(0)\|_2$, "dropping the quadratic term for small drift". Substituting + applying (C1b) gives $\|Z^*(T^*) - Z^*(0)\|_F < \lambda_0/4$. Weyl: $\sigma_{\min}(Z^*(T^*)) \geq \lambda_0 - \lambda_0/4 = 3\lambda_0/4 > \lambda_0/2$, strict.

All three properties hold strictly at $T^*$; continuity contradicts $T^* < \infty$.

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | $T^* > 0$ (strict initial conditions + continuity). | $\theta$ continuous in $t$, $\sigma_{\min}$ continuous. | ✓ |
| S2 | On $[0, T^*]$, P2 $\Rightarrow$ [[17_cor_exp_decay]] applies, giving (loss_decay) and (error_integral). | [[17_cor_exp_decay]]. | ✓ — *conditional on the scope mismatch noted in [[17_cor_exp_decay]] Flag-ED1*. |
| S3 | P1 at $T^*$: $\|A(T^*) - A(0)\|_F \leq R_A/2 < R_A$. | [[14_lem_drift]] + (C1a). | ✓ (Verified in [[15_cond_C1]] audit table.) |
| S4 | Similarly for $B, U, \mathbf u$. | Analogous. | ✓ |
| S5 | P2 at $T^*$ — substitute drift bounds into [[13_lem_Zstar_perturb]] **and verify (C1b) implies $\|Z^*(T^*) - Z^*(0)\|_F < \lambda_0/4$**. | (C1b). | **See Flag-MC3 below: the paper skips the substitution.** Reconstructing: from [[13_lem_Zstar_perturb]],<br>$\|Z^*(T^*)-Z^*(0)\|_F \leq \frac{\|X\|_F}{m}\|U(T^*)-U(0)\|_F + \frac{\|X\|_F\|U(0)\|_2}{m^2}\|W(T^*) - W(0)\|_2$.<br>(a) $\|U(T^*)-U(0)\|_F \leq \frac{\bar\lambda_\mathbf u\|X\|_F}{m}\cdot \frac{4\sqrt{2\mathcal L(0)}}{\lambda_0^2}$.<br>(b) $\|W(T^*)-W(0)\|_2 \leq 2\bar\lambda_A\|A(T^*)-A(0)\|_2 + 2\|B(T^*)-B(0)\|_2 \leq 2\bar\lambda_A\|A(T^*)-A(0)\|_F + 2\|B(T^*)-B(0)\|_F$, and substituting [[14_lem_drift]]:<br>$\leq 2\bar\lambda_A \cdot \frac{2\bar\lambda_{Uu A}\|X\|_F}{m^2}\cdot \frac{4\sqrt{2\mathcal L(0)}}{\lambda_0^2} + 2\cdot \frac{2\bar\lambda_{Uu}\|X\|_F}{m^2}\cdot\frac{4\sqrt{2\mathcal L(0)}}{\lambda_0^2}$<br>$= \frac{16\|X\|_F\sqrt{2\mathcal L(0)}}{m^2\lambda_0^2}(\bar\lambda_A \bar\lambda_{Uu A} + \bar\lambda_{Uu}) = \frac{16\bar\lambda_{Uu}\|X\|_F\sqrt{2\mathcal L(0)}}{m^2\lambda_0^2}(\bar\lambda_A^2 + 1)$<br>where we used $\bar\lambda_{Uu A} = \bar\lambda_U \bar\lambda_\mathbf u\bar\lambda_A$.<br>Putting it together with $\|U(0)\|_2 \leq \bar\lambda_U$:<br>$\|Z^*(T^*)-Z^*(0)\|_F \leq \frac{4\bar\lambda_\mathbf u\|X\|_F^2\sqrt{2\mathcal L(0)}}{m^2\lambda_0^2} + \frac{16\bar\lambda_U\bar\lambda_{Uu}\|X\|_F^2\sqrt{2\mathcal L(0)}(\bar\lambda_A^2+1)}{m^4\lambda_0^2}$.<br>Demand this be $\leq \lambda_0/4$:<br>$\lambda_0^3 \geq \frac{16\bar\lambda_\mathbf u\|X\|_F^2\sqrt{2\mathcal L(0)}}{m^2} + \frac{64\bar\lambda_U\bar\lambda_{Uu}\|X\|_F^2\sqrt{2\mathcal L(0)}(\bar\lambda_A^2+1)}{m^4}$.<br>Factor: $\lambda_0^3 \geq \frac{16\bar\lambda_\mathbf u\|X\|_F^2\sqrt{2\mathcal L(0)}}{m^2}\left[1 + \frac{4\bar\lambda_U^2(\bar\lambda_A^2 + 1)}{m^2}\right]$<br>(using $\bar\lambda_U\bar\lambda_{Uu} = \bar\lambda_U^2\bar\lambda_\mathbf u$).<br>Compare to (C1b) as stated:<br>$\lambda_0^3 \geq 16\|X\|_F^2\sqrt{2\mathcal L(0)}\cdot\frac{\bar\lambda_\mathbf u}{m^2}\left(\frac{4(\bar\lambda_{UA}^2 + \bar\lambda_U^2)}{m^2} + 1\right) = \frac{16\bar\lambda_\mathbf u\|X\|_F^2\sqrt{2\mathcal L(0)}}{m^2}\left(\frac{4(\bar\lambda_{UA}^2+\bar\lambda_U^2)}{m^2} + 1\right)$.<br>Inside the brackets, the paper has $4(\bar\lambda_{UA}^2 + \bar\lambda_U^2)/m^2 + 1 = 4\bar\lambda_U^2(\bar\lambda_A^2 + 1)/m^2 + 1$ (since $\bar\lambda_{UA} = \bar\lambda_U\bar\lambda_A$). **Matches my reconstruction exactly!** ✓ |
| S6 | $\sigma_{\min}(Z(T^*)) \geq \lambda_0 - \lambda_0/4 = 3\lambda_0/4 > \lambda_0/2$, strict. | Weyl's inequality + S5. | ✓ |
| S7 | All three properties hold strictly at $T^*$; by continuity they extend to $[0, T^* + \varepsilon)$. Contradiction. | Continuity argument. | ✓ |

### Flags on this proof

- **Flag-MC1 (typo in conclusion).** L877 writes "$\sigma_{\min}(Z^*(T^*)) \geq \lambda_0 - \lambda_0/4 = \lambda_0\, 3/4 > \lambda_0/2$". Math is right ($\lambda_0 - \lambda_0/4 = 3\lambda_0/4$), but the formatting "$\lambda_0\,3/4$" is awkward. **Severity: Minor.**
- **Flag-MC2 (dropped quadratic term).** L869–L873 writes
  $$\|W(T^*) - W(0)\|_2 \leq 2\bar\lambda_A\|A(T^*) - A(0)\|_2 + 2\|B(T^*) - B(0)\|_2$$
  **dropping the quadratic term** $\|A_1 - A_0\|_2^2$ "for small drift". This is **not rigorous**: dropping a quadratic term is fine only if you check that the drop is dominated by the linear part. With $\|A(T^*) - A(0)\|_F \leq R_A/2$, the quadratic term is $\leq R_A^2/4$, while the linear term is $\leq 2\bar\lambda_A \cdot R_A/2 = \bar\lambda_A R_A$. So the quadratic is dominated iff $R_A/4 \leq \bar\lambda_A$, i.e., $R_A \leq 4\bar\lambda_A$ — a constraint on $R_A$ that the paper does **not** state. A more rigorous bound would be $\|W - W'\|_2 \leq (2\bar\lambda_A + R_A)\|A - A'\|_2 + 2\|B - B'\|_2 \leq 3\bar\lambda_A\|A - A'\|_2 + 2\|B-B'\|_2$ if $R_A \leq \bar\lambda_A$, etc. The constants in (C1b) would shift by absolute factors but the structure would survive. **Severity: Major (rigor gap; fixable with a constant tweak).**
- **Flag-MC3 (computation hidden in (C1b)).** L875 says "Substituting the drift bounds and applying Condition (C1b) gives $\|Z^*(T^*) - Z^*(0)\|_F < \lambda_0/4$" without doing the computation. The computation is reproducible (above, S5) and the constants in (C1b) **work out exactly to give $< \lambda_0/4$** (modulo the quadratic-term drop, Flag-MC2). The paper expects the reader to fill in 6 lines of algebra. **Severity: Major (rigor; not erroneous but unconvincing).**
- **Flag-MC4 (depends on broken lemma chain).** The whole argument inherits the unproven [[11_lem_upperbound_VQ]] (used by [[12_lem_grad_bounds]] → [[14_lem_drift]]) and the gap in [[13_lem_Zstar_perturb]]. The conclusions are correct *if* one supplies clean proofs for those lemmas, but the proof chain as written has gaps. **Severity: Critical (inherits).**
- **Flag-MC5 (over-parameterization claim).** L728 says "the width of the hidden layer is $O(N)$". This is misleading: the proof requires $n \geq N$ for $\sigma_{\min}(Z(0)) = \lambda_0 > 0$ to hold (otherwise $Z(0)$ has nullity $\geq N - n > 0$ generically). The actual sufficient width is **$n \geq N$**, not $n = O(N)$ in the asymptotic sense. Other DEQ/NTK papers require $n = \tilde\Omega(\text{poly}(N))$ for $\lambda_0$ to concentrate; this paper assumes $\lambda_0 > 0$ as an explicit hypothesis instead. **Severity: Minor (phrasing).**
- **Flag-MC6 (no random initialization analysis).** The paper does **not** show that under any specific random init, $\lambda_0$ is bounded below by a useful quantity, nor that (C1a)/(C1b) are achievable for any standard initialization scheme. The conditions are existential — there is no constructive verification. The paper's intro promises "global linear convergence under gradient descent by leveraging over-parameterization", but the version proven here is conditional on a quantitative initialization condition with no concrete instantiation. **Severity: Major (gap between claim and proof in introduction vs the formal theorem).**
- **Flag-MC7 (gradient flow vs gradient descent).** The introduction (L75) says "the gradient descent converges to the global optimum at a linear rate". The theorem actually proves convergence under **gradient flow** (continuous time, infinitesimal step). Discrete-time GD with finite step size is **not** analyzed — Section 3.4 mentions PL and a discrete result, but the theorem proved is purely continuous. **Severity: Major (claim mismatch between intro/abstract and result).**

## Notes / Errors

The main theorem **as stated** has a correct argument *modulo*:
- The unproven inner lemma ([[11_lem_upperbound_VQ]]).
- The proof gap in perturbation lemma ([[13_lem_Zstar_perturb]]).
- The hand-waved quadratic-term drop (Flag-MC2).
- The hidden computation justifying (C1b) (Flag-MC3).
- The mismatch between abstract claim (discrete GD) and theorem (gradient flow) (Flag-MC7).
