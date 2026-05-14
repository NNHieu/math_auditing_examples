# 03. Proposition — Lipschitz constant of $T_\alpha$ (upper_LT)

**Source:** L317–L322, `\begin{proposition}[\citep{monotonedeq}]` label `lem:upper_LT`.
**Item Status:** 🟡 Warning — proposition correct, one downstream wording slip.
**Phase-3 verification:** E03-INT 🚨 confirmed: L329 writes "$\alpha \in (0, 2m/L^2)$ then $L[T_\alpha] < 1$" (open, strict contraction) and L330 writes "contractive for any $\alpha \in (0, 2m/L^2]$" (closed). At the closed endpoint $\alpha = 2m/L^2$, $1 - 2\alpha m + \alpha^2 L^2 = 1 - (2m/L^2)\cdot 2m + (4m^2/L^4)L^2 = 1 - 4m^2/L^2 + 4m^2/L^2 = 1$, so $L[T_\alpha] \leq 1$, non-expansive, not strictly contractive.

---

## Statement

Let $I - W$ be $m$-strongly monotone with Lipschitz constant $L = \|I - W\|_2$. Then the Lipschitz constant of $T_\alpha = I - \alpha(I-W)$ satisfies
$$
L[T_\alpha] \leq \sqrt{1 - 2\alpha m + \alpha^2 L^2}.
$$

## Original Proof

Cited to [[monotonedeq]] — proof omitted in this paper.

## Notes / Errors

- This proposition is the standard contraction estimate for the forward step in FBS when $I-W$ is $m$-strongly monotone and $L$-Lipschitz: $\|x - \alpha(I-W)x\|^2 = \|x\|^2 - 2\alpha\langle x, (I-W)x\rangle + \alpha^2\|(I-W)x\|^2 \leq (1 - 2\alpha m + \alpha^2 L^2)\|x\|^2$.
- The inequality is valid whenever $1 - 2\alpha m + \alpha^2 L^2 \geq 0$, i.e. always — but the bound is $<1$ (i.e., contraction) only for $\alpha \in (0, 2m/L^2)$.
- L329 makes the claim "$\alpha \in (0, 2m/L^2)$ implies $L[T_\alpha] < 1$". Solving $1 - 2\alpha m + \alpha^2 L^2 < 1 \Leftrightarrow \alpha(\alpha L^2 - 2m) < 0 \Leftrightarrow \alpha \in (0, 2m/L^2)$ — correct.
- L330 writes "$\alpha \in (0, 2m/L^2]$" with the right endpoint **closed**, then immediately says "contractive". At $\alpha = 2m/L^2$ we get $L[T_\alpha] \leq 1$ (i.e., non-expansive but **not strictly** contractive). Minor inconsistency.
  - **Flag:** Minor presentation inconsistency, not load-bearing for any later result (downstream uses always pick a strictly interior $\alpha$).
