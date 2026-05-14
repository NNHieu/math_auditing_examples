# 09. Lemma — Partial derivatives of $Z^*$ (deri_Z)

**Source:** L476–L490, label `lem:deri_Z`.
**Item Status:** 🟡 Warning — formula correct, invertibility asserted without proof (E09-INV).
**Phase-3 verification:** E09-INV 🚨 confirmed at L452: the single-sentence claim "Invertibility is guaranteed by the strong monotonicity of $I - W$" provides no derivation, and the load-bearing argument (active-subspace restriction for ReLU dead neurons) is missing.

---

## Statement

$$
\begin{aligned}
\frac{\partial \operatorname{vec}[Z^*]}{\partial \operatorname{vec}[W]} &= Q^{-1}\,D\,[Z^\top \otimes I_n],\\
\frac{\partial \operatorname{vec}[Z^*]}{\partial \operatorname{vec}[A]} &= Q^{-1}\,D\,[Z^\top \otimes I_n]\,P_A,\\
\frac{\partial \operatorname{vec}[Z^*]}{\partial \operatorname{vec}[B]} &= Q^{-1}\,D\,[Z^\top \otimes I_n]\,P_B,\\
\frac{\partial \operatorname{vec}[Z^*]}{\partial \operatorname{vec}[U]} &= Q^{-1}\,D\,[X^\top \otimes I_n],
\end{aligned}
$$
where $Q = I_{Nn} - D(I_N \otimes W)$.

## Original Proof

"Apply (eq:IFT) and substitute Lemma [[08_lem_deri_g]]. The double negation (IFT sign and the minus signs in Lemma [[08_lem_deri_g]]) cancels to give positive signs."

## Audit Proof

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1 | $\frac{d\,\operatorname{vec}[Z^*]}{d\theta} = -\bigl(\partial g/\partial \operatorname{vec}[Z]\bigr)^{-1}\,\partial g/\partial\theta$. | IFT (eq:IFT). | Implicit differentiation of $g_\theta(Z^*;X) = 0$, valid provided $Q$ is invertible. |
| S2 | $Q = I_{Nn} - D(I_N \otimes W)$ is invertible. | Strong monotonicity of $I-W$. | See Flag-DZ1: paper claims this at L452; the precise statement is that **for $D$ with entries in $\{0,1\}$**, $Q = I - D(I_N\otimes W)$ is invertible whenever $I - W$ is non-singular and certain conditions hold. We discuss in Flag-DZ1. |
| S3 | $\partial \operatorname{vec}[Z^*]/\partial \operatorname{vec}[W] = -Q^{-1}\cdot(-D[Z^\top \otimes I_n]) = Q^{-1}D[Z^\top \otimes I_n]$. | S1 with [[08_lem_deri_g]] for $\theta = W$. | Sign cancellation: $-(\text{IFT sign}) \times (-D[Z^\top\otimes I_n]) = +D[Z^\top\otimes I_n]$. ✓ |
| S4 | $\partial \operatorname{vec}[Z^*]/\partial \operatorname{vec}[A] = Q^{-1}D[Z^\top\otimes I_n]P_A$, similarly for $B$. | S3, chain rule via $W = W(A,B)$. | ✓ |
| S5 | $\partial \operatorname{vec}[Z^*]/\partial \operatorname{vec}[U] = Q^{-1}D[X^\top\otimes I_n]$. | S1 with [[08_lem_deri_g]] for $\theta = U$. | ✓ |

### Flags on this proof

- **Flag-DZ1 (invertibility of $Q$).** L452 says invertibility is "guaranteed by strong monotonicity of $I-W$" without further justification. This is a load-bearing claim used downstream in [[11_lem_upperbound_VQ]] to bound $\|(\mathbf{u}^\top\otimes I_N) Q^{-1}\|$ by $\|\mathbf{u}\|/m$. The argument:
  - $Q = I_{Nn} - D(I_N \otimes W)$. With $D$ diagonal with entries in $\{0,1\}$ (since $\sigma' \in \{0,1\}$ for ReLU on the open set of non-zero arguments), each block of $Q$ is $I_n - D_i W$ for the $i$-th sample, where $D_i = \operatorname{diag}(\sigma'(W z_i^* + Ux_i))$.
  - $\|I_n - D_i W\|$ invertibility: write $I_n - D_i W = (I_n - W) + (I_n - D_i)W$. Not immediately invertible just from $I - W$ being $m$-strongly monotone.
  - **Correct argument** (used in monDEQ literature): For $D_i$ a $0/1$ diagonal with active set $S \subseteq [n]$, restricting to coordinates in $S$ gives the linear system $(I - W)_{SS} z = b$, which inherits strong monotonicity. The full operator $I - D_i W$ is then invertible because rows outside $S$ have $1$ on the diagonal and trivial structure. This needs an explicit argument; the paper omits it. **Severity: Major (claim asserted, proof missing).** This is the same gap that [[11_lem_upperbound_VQ]] re-encounters.
- **Flag-DZ2 (sign cancellation).** L487–L489 says "the double negation ... cancels to give positive signs." This is correct for $W, U$ derivatives. For $A, B$: applying chain rule with $W = (1-m)I - A^\top A + B - B^\top$ uses $\partial \operatorname{vec}[W]/\partial \operatorname{vec}[A] = P_A$ (with the $-$ baked into $P_A$). The chain $Q^{-1}D[Z^\top\otimes I_n] P_A$ has the correct sign. ✓ **Severity: None.**
- **Flag-DZ3 (typo in $\frac{\partial \operatorname{vec}[Z^*]}{\partial \operatorname{vec}[W]}$).** A stale comment block (L1110–L1119) shows an *earlier* version of this lemma with **opposite signs and using $I_m \otimes Z$** instead of $Z^\top \otimes I_n$. That's an old/incorrect form (now commented out). Verify the current form: $\operatorname{vec}[WZ] = (Z^\top \otimes I_n)\operatorname{vec}[W]$, so the derivative w.r.t. $\operatorname{vec}[W]$ involves $(Z^\top \otimes I_n)$. ✓ Current form is correct; old commented form was wrong. **Severity: None (already excised).**

## Notes / Errors

The main gap is the **invertibility of $Q$** — load-bearing for the rest of the paper but only asserted.
