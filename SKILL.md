---
name: proof-audit
description: Systematically audit a mathematical paper's proofs. Creates per-item audit files, tracks errors with status, and validates each error against the LaTeX source. Use when the user wants to audit a paper, verify proofs rigorously, check for mathematical errors, or reproduce corrected proofs for a blog or notes.
user-invocable: true
allowed-tools: Read, Write, Edit, Bash, Glob
---

# /proof-audit — Mathematical Paper Proof Auditor

Arguments: `$ARGUMENTS`

Audit the proofs in a LaTeX paper by creating a structured `audit/` folder.
Run in three phases; which phases to run depends on the state of the repo and any argument the user passed.

---

## Dispatch on arguments and state

| Condition | Action |
|-----------|--------|
| No `audit/` folder yet | Run **Phase 1**, then **Phase 2** |
| `audit/` exists but errors are "⬜ To validate" | Run **Phase 3** (Validation) |
| `audit/` exists and all errors confirmed | Report final summary to user |
| Argument is a `.tex` path | Use that file as the paper source |
| Argument is `validate` | Skip to Phase 3 only |
| No `.tex` file found | Ask the user which file to audit |

*Tool Hint:* To find the paper, use `Bash`: `find . -name "*.tex" -not -path "*/node_modules/*"`. If multiple `.tex` files exist, ask the user to pick one.

---

## Phase 1 — Building the Audit Backbone

Scan the document to reconstruct the paper’s structure. The objective is to list all definitions, assumptions, lemmas, theorems, and corollaries, and establish their dependencies. 

### 1a. Error Index
Create a file named `audit/error_table.md` containing a Markdown table:
`File | Error ID | One-line description | Status | Severity | Confidence`
*(Initially empty; to be populated in later steps).*

*   **Status:** `⬜ To validate` | `🚨 Confirmed Error` | `🟢 False Alarm`
*   **Severity:** `Critical` | `Major` | `Minor`
*   **Confidence:** 
    *   `High` (explicit contradiction, failed algebra, dimension mismatch)
    *   `Medium` (likely unstated assumption, suspicious leap, citation mismatch)
    *   `Low` (stylistic rigor concerns, ambiguous notation)

### 1b. Mathematical Item Files 
Create one file per mathematical item (definition | assumption | lemma | theorem) in the order they appear. Number sequentially: `01_def_fbs.md`, `02_lem_grad_loss.md`, etc. 

**Format for each file:**
```markdown
# NN. Type — Human Name (short_identifier)

**Source:** Paper section / label / approximate line numbers
**Item Status:** 🟢 Valid | 🟡 Warning (Needs Review) | 🔴 Contains Fatal Error

---
## Statement
(Full mathematical statement with MathJax: $$...$$)

## Original Proof
(Copy the original proof verbatim. Annotate references to other files using their short identifiers. Include contextual explanations.)

## Notes / Errors
(Initially empty. Record suspected gaps or errors here.)

```

### 1c. Proof Dependency Graph

Write `audit/structure.md` containing:

1. **Graphviz Output:** A ````dot` code block representing a dependency graph (Node A -> Node B if B depends on A).
2. **Prose Summary:** A layered explanation of how foundational elements build up to the main result.
3. **Structural Issues:** List missing assumptions, circular dependencies, or unproven regularity conditions.

*Actionable:* Record any structural issues found here in `audit/error_table.md` with status `⬜ To validate`.

---

## Phase 2 — Line-by-Line Audit

Process each item file iteratively, starting from foundational items and following the dependency graph. Insert the following section after `## Original Proof`:

```markdown
## Audit Proof

(Rewrite the proof into a sequence of **atomic, verifiable steps**. Every step must be minimal, logically justified, and explicitly reference assumptions.)

| Step ID | Claim | Premises | Justification |
|---------|-------|----------|---------------|
| S1      | ...   | ...      | ...           |

```

**Audit Imperatives:**

* **Deconstruct:** Expand the proof. If multiple inferences happen in one sentence, split them into multiple `S`-steps.
* **Verify Variables & Conditions:** Highlight the introduction of new objects. If a theorem is invoked, explicitly verify its conditions in a dedicated step.
* **Flag Aggressively:** Actively try to break the proof. Look for hidden assumption drift, boundary case failures, or silent quantifier changes.
* **Record Flags:** If a step cannot be justified, add a note below the table. Log every missing justification or questionable logic in `audit/error_table.md` as `⬜ To validate`.

---

## Phase 3 — Validation

For each `⬜ To validate` error in `audit/error_table.md`, find the exact line in the LaTeX source.

1. **Locate:** Use `Read` or `grep -B 3 -A 3` (to account for LaTeX line breaks) to inspect the source.
2. **Validate:** Does the source actually contain the error? Or was it a misinterpretation?
3. **Update Item File:** Change item status. Add a one-sentence verification note with the exact line number. Provide mathematical proof of the error (e.g., dimension check, L'Hôpital computation).
4. **Update Index:** Change status in `error_table.md` to `🚨 Confirmed Error` or `🟢 False Alarm`.

---

## Global Style & Tool Rules

* **MathJax:** Use `$$...$$` for block, `$...$` for inline. Never use standard markdown for math.
* **Notation:** Use standard macros (`\operatorname{vec}`, `\otimes`, `\|\cdot\|_2`). End proofs with $\square$.
* **Rigor:** Verify intermediate steps explicitly. Do NOT accept "by standard arguments."
* **Tool Usage:** Save files iteratively using `Write` or `Edit`. Do not attempt to process the entire paper in a single context window without saving intermediate states to the `audit/` folder.
