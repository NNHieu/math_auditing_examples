# 08. Lemma — Partial derivatives of $g_\theta$ (deri_g)

**Source:** L461–L473 (statement); appendix proof L1038–L1106 (`proof:deri_g`).
**Item Status:** 🟡 Warning — sign equation OK; batch lift omitted.
**Phase-3 verification:**
- E08-SIGN 🟢 **False alarm.** Re-verified L1102: $-[\mathbf z^\top \otimes J]P_A = J[\mathbf z^\top \otimes I_n](I+K)(I\otimes A^\top)$. Since $P_A = -(I+K)(I\otimes A^\top)$, the leading minus combined with $-P_A = +(I+K)(I\otimes A^\top)$ produces the displayed RHS. ✓
- E08-BATCH 🚨 confirmed: appendix at L1041 explicitly proves "single input case i.e $g_\theta(\mathbf z, \mathbf x)$"; the batch version L461–L473 (with $D \in \mathbb{R}^{Nn \times Nn}$, $I_N \otimes W$, etc.) is asserted, not proven.

---

## Statement

Let $D = \operatorname{diag}(\operatorname{vec}(\sigma'(WZ^* + UX))) \in \mathbb{R}^{Nn\times Nn}$,
$Q = I_{Nn} - D(I_N \otimes W)$,
$P_A = -(I_{n^2} + K^{(n)})(I_n \otimes A^\top)$, and
$P_B = I_{n^2} - K^{(n)}$.
Then
$$
\begin{aligned}
\frac{\partial g_\theta}{\partial \operatorname{vec}[Z]} &= Q, \\
\frac{\partial g_\theta}{\partial \operatorname{vec}[A]} &= -D\,[Z^\top \otimes I_n]\,P_A, \\
\frac{\partial g_\theta}{\partial \operatorname{vec}[B]} &= -D\,[Z^\top \otimes I_n]\,P_B, \\
\frac{\partial g_\theta}{\partial \operatorname{vec}[U]} &= -D\,[X^\top \otimes I_n].
\end{aligned}
$$

## Original Proof

Single-sample version proved in Appendix L1038–L1106. Steps in the appendix:
1. $d g_\theta = (I - JW)\,d\mathbf{z} - J(dW)\mathbf{z} - J(dU)\mathbf{x}$ where $J = \operatorname{diag}(\sigma'(W\mathbf{z}+U\mathbf{x}))$.
2. Vectorize via K1: $\operatorname{vec}[d g_\theta] = (I-JW)\,d\mathbf{z} - (\mathbf{z}^\top \otimes J)\operatorname{vec}[dW] - (\mathbf{x}^\top \otimes J)\operatorname{vec}[dU]$.
3. Express $\operatorname{vec}[dW] = P_A\operatorname{vec}[dA] + P_B\operatorname{vec}[dB]$ using $W = (1-m)I - A^\top A + B - B^\top$.
4. Substitute.

In the **batch** case, $g_\theta(Z;X) = Z - \sigma(WZ + UX)$ is a matrix-valued residual. Then $D = \operatorname{diag}(\operatorname{vec}(\sigma'(WZ^* + UX)))$ and $\partial g/\partial \operatorname{vec}[Z] = I_{Nn} - D(I_N \otimes W)$.

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | $g_\theta(Z;X) = Z - \sigma(WZ + UX)$. | [[07_def_residual]]. | Definition. |
| S2 | The differential (matrix form) is $dg = dZ - D'\circ(dW \cdot Z + W\cdot dZ + dU\cdot X)$, where $D'$ is the entrywise ReLU derivative. | Chain rule on element-wise composition. | $\sigma$ is element-wise; gradient w.r.t. its argument is diagonal scaling. |
| S3 | Vectorize: $\operatorname{vec}[dg] = \operatorname{vec}[dZ] - D\bigl(\operatorname{vec}[dW \cdot Z] + (I_N \otimes W)\operatorname{vec}[dZ] + \operatorname{vec}[dU \cdot X]\bigr)$. | K1: $\operatorname{vec}[ABC] = (C^\top \otimes A)\operatorname{vec}[B]$. | $\operatorname{vec}[W\cdot dZ] = (I_N \otimes W)\operatorname{vec}[dZ]$, $\operatorname{vec}[dW\cdot Z] = (Z^\top \otimes I_n)\operatorname{vec}[dW]$, $\operatorname{vec}[dU\cdot X] = (X^\top \otimes I_n)\operatorname{vec}[dU]$. |
| S4 | $\partial g/\partial \operatorname{vec}[Z] = I_{Nn} - D(I_N \otimes W) = Q$. | S3, isolating $\operatorname{vec}[dZ]$. | ✓ |
| S5 | $\partial g/\partial \operatorname{vec}[U] = -D(X^\top \otimes I_n)$. | S3. | ✓ |
| S6 | $\partial g/\partial \operatorname{vec}[W] = -D(Z^\top \otimes I_n)$. | S3. | ✓ |
| S7 | $\operatorname{vec}[dW] = P_A\operatorname{vec}[dA] + P_B\operatorname{vec}[dB]$ where $P_A = -(I_{n^2}+K^{(n)})(I_n \otimes A^\top)$ and $P_B = I_{n^2} - K^{(n)}$. | $W = (1-m)I - A^\top A + B - B^\top$; K1 and commutation matrix identity $K^{(n)}\operatorname{vec}[M] = \operatorname{vec}[M^\top]$. | See Flag-G1 below — appendix derivation works out. |
| S8 | $\partial g/\partial \operatorname{vec}[A] = -D(Z^\top\otimes I_n)P_A$, $\partial g/\partial \operatorname{vec}[B] = -D(Z^\top\otimes I_n)P_B$. | S6, S7, chain rule. | ✓ |

### Flags on this proof

- **Flag-G1 (sign/formula consistency in appendix).** The appendix at L1102–L1103 writes
  $$\frac{\partial g_\theta}{\partial A} = -[\mathbf{z}^\top \otimes J] P_A = J[\mathbf{z}^\top \otimes I_n][(I+K^{(n)})(I\otimes A^\top)].$$
  This equality uses $P_A = -(I+K^{(n)})(I\otimes A^\top)$, so $-P_A = +(I+K^{(n)})(I\otimes A^\top)$. Combining with the explicit factor $-1$: $-[\mathbf z^\top\otimes J]\cdot P_A = -J[\mathbf z^\top \otimes I_n]\cdot P_A = +J[\mathbf z^\top \otimes I_n](I+K^{(n)})(I\otimes A^\top)$. ✓ The right-hand side **omits the leading minus sign** — should be $+J[\dots]$. As written, the equation is correct only if the implicit pull-out makes the signs match; readers must trace carefully. **Severity: Minor (presentation).**
- **Flag-G2 (factoring $J$ across Kronecker).** L1071–L1072 writes $(\mathbf{z}^\top \otimes J) = J(\mathbf{z}^\top \otimes I_n)$. Verify: $(\mathbf{z}^\top \otimes J)$ acts on $\operatorname{vec}[M]$ as $\operatorname{vec}[JM\mathbf{z}]$ (since $\operatorname{vec}[JMZ] = (Z^\top \otimes J)\operatorname{vec}[M]$, with $Z = \mathbf{z}$ a column). On the other hand $J(\mathbf{z}^\top \otimes I_n)\operatorname{vec}[M] = J\operatorname{vec}[M\mathbf{z}\cdot I_n^\top]$... wait, that's vec of $I_n M \mathbf{z} = M\mathbf{z}$, then left-multiplied by $J$, giving $JM\mathbf{z}$ — but $J(\mathbf{z}^\top \otimes I_n)$ where $\mathbf{z}^\top \otimes I_n \in \mathbb{R}^{n\times n^2}$ and $J \in \mathbb{R}^{n\times n}$: $J(\mathbf{z}^\top \otimes I_n) = \mathbf{z}^\top \otimes J$ (using $(A\otimes B)(C\otimes D) = AC \otimes BD$ — yes, $I_1 \cdot \mathbf{z}^\top \otimes J\cdot I_n = \mathbf{z}^\top \otimes J$). ✓ Identity is correct. **Severity: None.**
- **Flag-G3 (batch lift).** The appendix proves the single-sample case ($g_\theta(\mathbf{z};\mathbf{x})$). The batch case L461–L473 is stated without a separate proof. The lift requires: replace $\mathbf{z}$ by columns of $Z$, $\mathbf{x}$ by columns of $X$, $J$ becomes $D = \operatorname{diag}(\operatorname{vec}(\sigma'(WZ+UX)))$ (with entries indexed by both column and row), and identities $\operatorname{vec}[WZ] = (I_N \otimes W)\operatorname{vec}[Z]$, $\operatorname{vec}[dW \cdot Z] = (Z^\top \otimes I_n)\operatorname{vec}[dW]$ replace the single-sample versions. Done correctly in S3 above. The paper does not write this out. **Severity: Minor (gap, fillable).**

## Notes / Errors

Final formulas correct; appendix presentation is sloppy with signs and skips the batch lift.
