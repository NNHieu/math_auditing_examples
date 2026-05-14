# 04. Lemma — Bound on $\|Z^*\|_F$ (Zstar_norm)

**Source:** L332–L373, `\begin{lemma}[Bound on $\|Z^*\|_F$]` label `lem:Zstar_norm`.
**Item Status:** 🟡 Warning — final inequality correct; proof has minor slips (E04-LHOP, E04-MONO, E04-LIM).
**Phase-3 verification:**
- E04-LHOP 🚨 confirmed at L363–L364: the displayed L'Hôpital step writes $\lim_{\alpha\to 0^+} 1/(d/d\alpha)[\dots]$ and then substitutes $\alpha = 0$ into the (correctly simplified) derivative ratio. End answer $1/m$ is correct; the intermediate displayed form is sloppy.
- E04-MONO 🚨 confirmed at L368: the parenthetical "using $h'(\alpha) = -2m + 2\alpha L^2$ and $\sqrt h < 1$" does not imply $f'(\alpha) > 0$. The actual sufficient condition is $\alpha^2(L^2 - m^2) > 0$ (worked out in the audit table S11).
- E04-LIM 🚨 confirmed: L344–L352 bound the FBS iterates; L370 then writes "$\|Z^*\|_F \leq f(\alpha)\|U\|_2\|X\|_F$" identifying $Z^*$ with the FBS limit without explicit justification.

---

## Statement

$$
\|Z^*\|_F \;\leq\; \frac{\|U\|_2 \, \|X\|_F}{m}.
$$

## Original Proof

Run FBS from $Z^{[0]} = 0$ with any $\alpha \in (0, 2m/L^2)$. By non-expansiveness of ReLU:
$$
\|Z^{[\ell]}\|_F \;\leq\; \|T_\alpha Z^{[\ell-1]} + \alpha U X\|_F \;\leq\; L[T_\alpha]\,\|Z^{[\ell-1]}\|_F + \alpha\|U\|_2\|X\|_F.
$$
With $c = L[T_\alpha] \in [0,1)$, $r = \alpha\|U\|_2\|X\|_F$ and $Z^{[0]} = 0$:
$$
\|Z^{[\ell]}\|_F \;\leq\; r\sum_{i=0}^{\ell-1} c^i \;\leq\; \frac{r}{1-c} \;=\; \frac{\alpha\|U\|_2\|X\|_F}{1-\sqrt{1-2\alpha m + \alpha^2 L^2}}.
$$
Define $f(\alpha) = \alpha / (1 - \sqrt{1 - 2\alpha m + \alpha^2 L^2})$. **Step 1** (L'Hôpital):
$$
\lim_{\alpha\to 0^+} f(\alpha) = \frac{1}{(d/d\alpha)\!\bigl[1 - \sqrt{1 - 2\alpha m + \alpha^2 L^2}\bigr]\big|_{\alpha=0}} = \frac{1}{m}.
$$
**Step 2** ($f$ is increasing): writes $h(\alpha) = 1 - 2\alpha m + \alpha^2 L^2$, $h'(\alpha) = -2m + 2\alpha L^2$, and asserts $f'(\alpha) > 0$ on $(0, 2m/L^2)$ via "differentiating and simplifying". Conclude $\inf f = 1/m$.

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | $Z^{[\ell+1]} = \sigma(T_\alpha Z^{[\ell]} + \alpha U X)$ | [[02_def_fbs]] | Definition of FBS. |
| S2 | ReLU $\sigma$ is 1-Lipschitz w.r.t. Frobenius norm, fixing $\sigma(0)=0$. | Properties of ReLU. | Element-wise: $|\sigma(a)-\sigma(b)| \leq |a-b|$, so $\|\sigma(M)\|_F \leq \|M\|_F$. |
| S3 | $\|Z^{[\ell+1]}\|_F \leq \|T_\alpha Z^{[\ell]} + \alpha U X\|_F$ | S1, S2 with $b=0$. | Apply S2 entrywise. |
| S4 | $\|T_\alpha Z^{[\ell]} + \alpha U X\|_F \leq \|T_\alpha Z^{[\ell]}\|_F + \alpha\|U X\|_F$ | Triangle ineq. | Triangle inequality for Frobenius. |
| S5 | $\|T_\alpha Z^{[\ell]}\|_F \leq L[T_\alpha]\,\|Z^{[\ell]}\|_F$ | [[03_prop_upper_LT]] | $T_\alpha$ is $L[T_\alpha]$-Lipschitz as a linear map; submultiplicative on $\|\cdot\|_F$ via $\|AZ\|_F \leq \|A\|_2\|Z\|_F$ and $\|T_\alpha\|_2 = L[T_\alpha]$. |
| S6 | $\|UX\|_F \leq \|U\|_2 \|X\|_F$ | Standard. | Operator-Frobenius submultiplicativity. |
| S7 | $\|Z^{[\ell+1]}\|_F \leq c\,\|Z^{[\ell]}\|_F + r$ with $c = L[T_\alpha]$, $r = \alpha\|U\|_2\|X\|_F$. | S3–S6. | Substitution. |
| S8 | $\|Z^{[\ell]}\|_F \leq r\,(1-c^{\ell})/(1-c) \leq r/(1-c)$ | S7, $Z^{[0]} = 0$, $c \in [0,1)$. | Geometric series, induction on $\ell$. |
| S9 | $\|Z^*\|_F \leq r/(1-c) = \alpha\|U\|_2\|X\|_F / (1 - \sqrt{1-2\alpha m + \alpha^2 L^2})$ | S8, FBS convergence to $Z^*$. | Existence of $Z^*$ as limit of $Z^{[\ell]}$ (well-posedness from [[01_def_mondeq]] appendix). |
| S10 | $\lim_{\alpha\to 0^+}\alpha / (1 - \sqrt{1-2\alpha m + \alpha^2 L^2}) = 1/m$. | L'Hôpital. | See **Flag-Z1** below — the paper's L'Hôpital computation is sloppy; corrected version: numerator derivative is $1$; denominator derivative is $-\frac{1}{2}(1-2\alpha m + \alpha^2 L^2)^{-1/2}(-2m + 2\alpha L^2) = \frac{m - \alpha L^2}{\sqrt{1-2\alpha m + \alpha^2 L^2}}$. At $\alpha=0^+$ the denominator's derivative $= m/1 = m$. So limit $= 1/m$. ✓ |
| S11 | $f(\alpha) = \alpha/(1-\sqrt{h(\alpha)})$ is strictly increasing on $(0, 2m/L^2)$. | Calculus. | See **Flag-Z2** — the paper waves hands ("differentiating and simplifying"). A clean argument: write $g(\alpha) = 1 - \sqrt{h(\alpha)}$, so $f = \alpha/g$, $f' = (g - \alpha g')/g^2$. We need $g - \alpha g' > 0$. Compute $g'(\alpha) = -h'/(2\sqrt{h}) = (m - \alpha L^2)/\sqrt{h}$. Then $g - \alpha g' = 1 - \sqrt{h} - \alpha(m - \alpha L^2)/\sqrt{h} = \bigl[\sqrt{h} - h - \alpha(m - \alpha L^2)\bigr]/\sqrt{h} = \bigl[\sqrt{h} - (1 - 2\alpha m + \alpha^2 L^2) - \alpha m + \alpha^2 L^2\bigr]/\sqrt{h} = \bigl[\sqrt{h} - 1 + \alpha m\bigr]/\sqrt{h}$. Need $\sqrt{h} > 1 - \alpha m$. If $1 - \alpha m \leq 0$ (i.e. $\alpha \geq 1/m$), trivial since $\sqrt{h} \geq 0$. Else $1-\alpha m > 0$; squaring is monotone, need $h > (1-\alpha m)^2$, i.e. $1 - 2\alpha m + \alpha^2 L^2 > 1 - 2\alpha m + \alpha^2 m^2$, i.e. $\alpha^2(L^2 - m^2) > 0$, true since $L \geq m$ and $\alpha > 0$. (Note: strict only when $L > m$; when $L = m$ the difference is $0$, so $f$ is *non-decreasing*, but the **infimum at $0^+$ is still $1/m$**, so the conclusion holds either way.) ✓ |
| S12 | $\inf_{\alpha \in (0, 2m/L^2)} f(\alpha) = 1/m$. | S10, S11. | Strictly increasing (or non-decreasing) function on $(0, 2m/L^2)$ with right limit $1/m$ at $0^+$ has infimum $1/m$. ✓ |
| S13 | $\|Z^*\|_F \leq \|U\|_2\|X\|_F/m$. | S9, S12. | The bound $\|Z^*\|_F \leq f(\alpha)\|U\|_2\|X\|_F$ holds for every $\alpha$ in the open interval; take infimum. ✓ |

### Flags on this proof

- **Flag-Z1 (L'Hôpital algebra).** L362–L366 writes the L'Hôpital step as
  $$\lim_{\alpha\to 0^+} f(\alpha) = \lim_{\alpha\to 0^+}\frac{1}{(d/d\alpha)[1-\sqrt{1-2\alpha m+\alpha^2 L^2}]}.$$
  The numerator's derivative is $1$ (correctly used), but the denominator's derivative should be written as a *function* of $\alpha$ then evaluated, not as the limit-inside-the-fraction. The displayed expression simplifies fine, but the L'Hôpital step as written technically requires the limit of the derivative ratio, not a substitution at $\alpha = 0$ into the unsimplified ratio that itself was $0/0$ at $\alpha = 0$. The end value is still $1/m$. **Severity: Minor (presentational).**
- **Flag-Z2 (monotonicity hand-wave).** L368 says "differentiating and simplifying ... shows $f'(\alpha) > 0$". The actual derivation (above, S11) requires the algebraic identity $\sqrt{h} > 1 - \alpha m \Leftrightarrow \alpha^2(L^2 - m^2) > 0$. The "$\sqrt h < 1$" hint quoted in the paper is **not what makes the derivative positive** — that fact alone does not imply monotonicity. **Severity: Minor (gap in rigor, fillable).**
- **Flag-Z3 (existence of $Z^*$ as a limit).** Implicitly the proof identifies $\lim Z^{[\ell]}$ with $Z^*$ via FBS convergence ([[03_prop_upper_LT]], $c < 1$). Convergence is geometric, so the bound carries over to the limit. The paper does not state this explicitly. **Severity: Minor.**
- **Flag-Z4 (cited but uses operator inequality).** The proof uses $\|UX\|_F \leq \|U\|_2 \|X\|_F$ — fine. It does **not** require $\|Z^*\|_F \leq \|U\|_2 \|X\|_F / m$ in the symmetric/PSD sense; only the strong-monotonicity-in-operator-sense (downstream use in [[12_lem_grad_bounds]] also wants $\|(I-W)^{-1}\|_2 \leq 1/m$, which is the operator-norm form). Cross-reference with [[01_def_mondeq]] convention drift.

## Notes / Errors

Conclusion of the lemma is **correct** but the proof has two minor presentation gaps to log in [[error_table]].
