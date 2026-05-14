# 07. Definition — Residual function $g_\theta$ (residual)

**Source:** L437–L439.
**Item Status:** 🟡 Warning (smoothness assumption implicit).
**Phase-3 verification:** E07-IFT 🚨 confirmed: L437–L455 introduces $g_\theta$, applies IFT at $Z^*$, and uses $D = \operatorname{diag}(\sigma'(WZ^*+UX))$ — but never addresses that ReLU's $\sigma'$ is undefined at $0$. The IFT is invoked at L443–L450 in the smooth-derivative form; the measure-zero kink set is silently ignored.

---

## Statement

Define $g_\theta(Z; X) = Z - \sigma(WZ + UX)$. The equilibrium satisfies $g_\theta(Z^*; X) = 0$.

## Notes / Errors

- Trivial restatement of the fixed-point equation [[01_def_mondeq]].
- The implicit function theorem (IFT) applied to $g_\theta = 0$ at $Z^*$ requires $\partial g/\partial \operatorname{vec}[Z]$ to be invertible — this is justified at L452 by strong monotonicity of $I-W$. But note ReLU is **only sub-differentiable** at $0$; the formal IFT applies where the ReLU pattern $D = \operatorname{diag}(\sigma'(WZ^* + UX))$ is locally constant. The paper uses the smooth-derivative form everywhere via $D$ and does not discuss the measure-zero set where $D$ jumps.
  - **Flag (smoothness):** This is a known subtlety in DEQ and ReLU NTK literature; the paper inherits the standard implicit-differentiation convention without comment. **Severity: Minor (regularity not addressed).**
