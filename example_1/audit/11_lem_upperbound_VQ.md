# 11. Lemma — $\|(\mathbf{u}^\top \otimes I_N) Q^{-1}\| \leq \|\mathbf{u}\|/m$ (upperbound_VQ)

**Source:** L519–L523 (statement); appendix proof L1122–L1196 (`proof:upperbound_VQ`).
**Item Status:** 🔴 Contains Fatal Error — proof of the auxiliary inner lemma is invalid.
**Phase-3 verification:**
- E11-PI 🚨 **confirmed (Critical) at L1167:** "Take the limit $\mu \to 0^+$, we have $\|\Pi\| \le 1$." This is false. $\Pi = [J + \mu(I - J)]^{-1}$ has $\Pi_{ii} = 1/[J_{ii} + \mu(1 - J_{ii})]$. For ReLU's $J_{ii} = 0$, $\Pi_{ii} = 1/\mu \to \infty$ as $\mu \to 0^+$. The norm of $\Pi$ diverges along inactive coords, so "$\|\Pi\| \leq 1$" in the limit is mathematically wrong.
- E11-JLT1 🚨 confirmed at L1137: "Since $J_{ii} < 1\;\forall i \in [n]$, we have $\Pi \in D^+$." For ReLU $J_{ii} \in \{0, 1\}$, so the active case has $J_{ii} = 1$, not $<1$. The conclusion "$\Pi \in D^+$" is independently true regardless (positivity of $1/[J_{ii} + \mu(1-J_{ii})]$ for $\mu > 0$), so the gloss is harmless logically, but the asserted inequality is false.
- E11-RG 🚨 confirmed at L1155–L1157: "$R_G = (I + \gamma G)^{-1}$, $\gamma = 1$, $R_G = \Pi^{-1}[I + \mu W^\top(I-J)]^{-1}$." No verification that $G$ is maximally monotone, hence no firm-non-expansiveness of $R_G$.
- E11-TYPO 🚨 confirmed at L1183: `V \in \mathbb{R}^{N \time n}` should be `\times`.

---

## Statement

$$
\bigl\|(\mathbf{u}^\top \otimes I_N)\,Q^{-1}\bigr\|_2 \;\leq\; \frac{\|\mathbf{u}\|}{m}.
$$

## Original Proof

Two-step proof in appendix:

**Inner lemma (L1131–L1133, L1134–L1176).** Claim: $\|(I - W^\top J)^{-1}\mathbf{u}\| \leq \|\mathbf{u}\|/m$ for $J$ a $0/1$ diagonal.

Sketch (paper): Introduce $\Pi = [J + \mu(I - J)]^{-1}$, $\mu > 0$. Notes $\Pi \in D^+$ (positive diagonal). Let $\mathbf{v}^* = (I - W^\top J)^{-1}\mathbf{u}$, so $\mathbf{v}^* = W^\top J \mathbf{v}^* + \mathbf{u}$; write $\mathbf{v}^* = \Pi \bar{\mathbf{v}}^*$, plug in, rearrange to
$$\Pi \bar{\mathbf{v}}' + \mu W^\top(I-J)\Pi \bar{\mathbf{v}}' = W^\top \bar{\mathbf{v}}' + \mathbf{u},$$
i.e. $\{[I + \mu W^\top(I-J)]\Pi - I\}\bar{\mathbf{v}}' + (I - W^\top)\bar{\mathbf{v}}' = \mathbf{u}$.

Set $G = \{[I + \mu W^\top(I-J)]\Pi - I\}$, $F(\bar{\mathbf v}') = (I - W^\top)\bar{\mathbf v}' - \mathbf{u}$. Resolvent $R_G = (I + \gamma G)^{-1}$ with $\gamma = 1$ gives $R_G = \Pi^{-1}[I + \mu W^\top(I-J)]^{-1}$. The FBS update
$$\bar{\mathbf v}^{k+1} = R_G(\bar{\mathbf v}^k - \alpha F(\bar{\mathbf v}^k)) = \Pi^{-1}[I + \mu W^\top(I-J)]^{-1}\{(I - \alpha(I-W^\top))\bar{\mathbf v}^k + \alpha \mathbf u\}.$$
"Take limit $\mu \to 0^+$, $\|\Pi\| \leq 1$ and $\|R_G\| \leq 1$. Same argument as [[04_lem_Zstar_norm]]: $\|\bar{\mathbf v}^*\| \leq \|\mathbf{u}\|/m$." Concludes $\|\mathbf v^*\| \leq \|\mathbf v^* / \Pi\| = \|\bar{\mathbf v}^*\| \leq \|\mathbf u\|/m$.

**Outer lemma (L1182–L1196).** Writes $(\mathbf{u}^\top \otimes I_N)Q^{-1} = V \ast I_N$ (row-wise Khatri-Rao product) where $V_{i:} = \mathbf{u}^\top (I - J_i W)^{-1}$. Uses inner lemma to bound row norms by $\|\mathbf{u}\|/m$, then computes $\|(V\ast I_N)\mathbf{v}\|^2 \leq \sum_i \|V_{i:}\|^2\|\mathbf v_i\|^2 \leq (\|\mathbf u\|/m)^2 \|\mathbf v\|^2$.

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | $Q = I_{Nn} - D(I_N \otimes W)$ is block-diagonal across the $N$ samples, with block $i$ equal to $I_n - D_i W$ where $D_i = \operatorname{diag}(\sigma'(W z_i^* + U x_i))$, $i \in [N]$. | Structure of $D$. | $D$ is diagonal, so it commutes with the block structure of $I_N \otimes W$. |
| S2 | Therefore $(\mathbf{u}^\top \otimes I_N) Q^{-1}$ has shape $N \times Nn$, with row $i$ given by $\mathbf{u}^\top (I_n - D_i W)^{-1}$ embedded in slot $i$. | S1 plus Kronecker algebra. | $(\mathbf{u}^\top \otimes I_N)$ acts on each block independently. See Flag-VQ1: in fact, the paper writes "$V_{i:} = \mathbf{u}^\top [I - J_i W]^{-1}$" — note this is $I - J_i W$, **not** $I - W^\top J_i$. |
| S3 | Need: $\|\mathbf{u}^\top (I - D_i W)^{-1}\| \leq \|\mathbf u\|/m$ for each $i$. Equivalently, $\|(I - W^\top D_i)^{-1}\mathbf u\| \leq \|\mathbf u\|/m$ (transpose). | Reduce outer to inner lemma. | $\mathbf{u}^\top A^{-1}$ has the same norm as $A^{-\top}\mathbf u = (A^\top)^{-1}\mathbf u$. The paper conflates these two but the **norm bound** is symmetric. ✓ |
| S4 | Verify $\|(I - W^\top D)^{-1}\mathbf u\| \leq \|\mathbf u\|/m$ where $D$ is $0/1$ diagonal. | **This is the inner lemma — and the paper's proof has issues.** | See Flag-VQ2 — the FBS argument is incomplete; the cleaner argument uses strong monotonicity of $I - W^\top D$ on the "active" subspace. |
| S5 | (Aggregating rows via Khatri-Rao.) $\|(V \ast I_N) \mathbf v\|^2 = \sum_i \langle V_{i:}^\top, \mathbf v_i\rangle^2 \leq \sum_i \|V_{i:}\|^2 \|\mathbf v_i\|^2 \leq (\|\mathbf u\|/m)^2 \sum_i \|\mathbf v_i\|^2 = (\|\mathbf u\|/m)^2 \|\mathbf v\|^2$. | Cauchy–Schwarz, S4. | ✓ assuming S4 holds. |

### Flags on this proof

- **Flag-VQ1 (matrix discrepancy).** Outer lemma uses $V_{i:} = \mathbf{u}^\top[I - J_i W]^{-1}$ (with $J_i W$, NOT $W^\top J_i$), but the inner lemma is stated/proven for $(I - W^\top J)^{-1}\mathbf{u}$. These two are related by transposition: $\mathbf{u}^\top (I - J_i W)^{-1} = [((I - J_i W)^{-\top})\mathbf u]^\top = [(I - W^\top J_i)^{-1}\mathbf u]^\top$. The vector norms are equal, so the bound transfers. ✓ **Severity: None (presentation: doesn't note transpose).**
- **Flag-VQ2 (inner-lemma proof is invalid as written).** The proof of $\|(I-W^\top J)^{-1}\mathbf u\| \leq \|\mathbf u\|/m$ attempts to reuse the FBS contraction from [[04_lem_Zstar_norm]] via a regularization $\Pi = [J + \mu(I-J)]^{-1}$ and a limit $\mu \to 0^+$. There are multiple problems:
  1. **$\Pi$ is singular in the limit.** As $\mu \to 0^+$, $\Pi_{ii} \to 1/J_{ii}$. If $J_{ii} = 0$ then $\Pi_{ii} \to +\infty$, so $\Pi$ is **not bounded** in the limit. The claim "take limit $\mu \to 0^+$, $\|\Pi\| \leq 1$" (L1167) is **wrong**: in that limit, $\Pi$ has $+\infty$ entries on coordinates where $J$ vanishes.
  2. **Reduction to FBS uses operator $I - W^\top$ but Lipschitz/strong-monotonicity must be on $W^\top$.** The paper invokes [[03_prop_upper_LT]] applied to $I - W^\top$ in passing, which is fine since $I - W$ strongly monotone implies $I - W^\top$ strongly monotone (same symmetrized condition). OK.
  3. **The FBS step combines $\Pi^{-1}[I+\mu W^\top(I-J)]^{-1}$ as a resolvent of $G$, but $G$ is not maximal monotone for generic $\mu, W, J$.** Maximal monotonicity is needed for the resolvent to be firmly non-expansive.
  4. The "same argument as [[04_lem_Zstar_norm]]" requires a contractive forward operator. The paper writes the forward operator as $I - \alpha(I - W^\top)$ — this is fine for some range of $\alpha$ — but the **backward** operator is $R_G$, which the paper claims has norm $\leq 1$ only in the limit $\mu \to 0^+$. The limit is exactly where $\Pi^{-1}$ becomes singular along inactive coordinates, so $R_G$ does **not** have a clean bound in the limit.
  5. The intended conclusion ($\|\mathbf v^*\| \leq \|\mathbf u\|/m$) **is in fact true** for the correct reason (see "Correct argument" below) — but the chain of reasoning in the appendix is invalid.
  
  **Correct argument (sketch).** Strong monotonicity gives $\langle x, (I-W)x\rangle \geq m\|x\|^2$ for all $x$. Equivalently, for any $0/1$ diagonal $J$, the restricted operator $(I - W^\top J)$ on the *active subspace* $S = \{i : J_{ii} = 1\}$ inherits the strong-monotonicity-on-$S$ property. Off-$S$, the equation $(I - W^\top J)\mathbf v = \mathbf u$ projects trivially: the $i$-th equation for $i \notin S$ reads $v_i - 0 = u_i$, so $v_i = u_i$. On-$S$, restricting and using $J_S = I$ gives $(I_S - W_{SS}^\top)\mathbf v_S = \mathbf u_S - (\text{cross terms})$; the strong monotonicity of $I-W$ implies $\|(I - W^\top)_S^{-1}\| \leq 1/m$ for the restricted operator. Combining, $\|\mathbf v\| \leq \|\mathbf u\|/m$. This argument exists in the monDEQ literature (Winston & Kolter, NeurIPS 2020 supplement), but **the paper does not give it; the FBS-with-$\Pi$ argument is broken**.
  
  **Severity: Critical (proof invalid; conclusion correct but unsupported by the given derivation).**
- **Flag-VQ3 (typo).** L1183: `$V \in \mathbb{R}^{N \time n}$` — should be `\times`. **Severity: Minor.**
- **Flag-VQ4 (asserted ReLU subgradient set).** L1136 says "$J_{ii} < 1\;\forall i$"; this is wrong — for ReLU the subgradient at strictly positive arguments is $1$ (not $<1$). The paper apparently means $J_{ii} \in \{0,1\}$. The condition $J_{ii} < 1$ would require $\sigma'$ values strictly less than $1$, which excludes the active ReLU case. **Severity: Major (claim used to argue $\Pi \in D^+$ at L1136 fails).**

## Notes / Errors

The inner lemma's conclusion is correct, but the proof has fundamental gaps. Logging multiple flags:
- VQ-fatal: invalid limit argument for $\|\Pi\| \leq 1$.
- VQ-typo: wrong relation $J_{ii} < 1$.
- VQ-typo2: `\time` typo.
