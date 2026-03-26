# Design & documentation philosophy

**Scope:** Project-agnostic principles for **repository layout**, **technical documentation**, and **authoring tone**. Use as a checklist when starting a repo, restructuring, or reviewing docs PRs.

---

## 1. Repository & layout

### 1.1 Defaults

| Principle | Practice |
| :--- | :--- |
| **Boundaries over blobs** | Prefer **directories and packages** that reflect **ownership** (e.g. API vs core domain vs delivery vs infra) over dumping modules at root. |
| **One obvious entry** | Expose a **single primary entry** (CLI, `main`, service bootstrap) documented in the thin plan; helpers live **beside** it, not competing with it. |
| **Committed vs local** | **Shared truth** in tracked paths (e.g. `docs/`); **machine-specific** notes, scratch, and personal path lists in **gitignored** local areas — never assume others have them. |
| **Infra beside app** | Charts, IaC snippets, or deploy manifests live **with the repo they deploy**, unless org policy mandates a mono-repo. |

### 1.2 Edge cases — decision prompts

When unsure, answer these **in writing** (even one line in a PR). That becomes the precedent.

| Situation | Questions to decide |
| :--- | :--- |
| **New top-level folder?** | Does this create a **new lifecycle** (release, ownership, or deployable unit)? If it’s just a few files, keep it under an existing boundary until it grows. |
| **Split vs merge packages** | Is the split driven by **import/cycle** needs, **team ownership**, or **test isolation**? If only “cleanliness,” defer until pain is real. |
| **Optional / experimental code** | Flag with **explicit module path or feature flag**, not ambiguous `util` sprawl; document **off** as default if risky. |
| **Generated artifacts** | Output dirs **documented** and ideally **gitignored**; inputs and generators **committed**; regenerate instructions live next to the generator. |
| **Shared libraries** | If copy-paste crosses **three** uses with drift risk, extract — not before. |

---

## 2. Documentation system

### 2.1 One job per artifact

| Artifact type | Typical role | Avoid |
| :--- | :--- | :--- |
| **Thin plan** | Scope, delivery shape, runtime model, v1-ish checklist — readable in **one sitting**. | Duplicating full technical spec or labeled commitments. |
| **Commitments / decisions** | Normative agreements, **status labels** (`SETTLED`, `CHOOSE LATER`, …), callouts. | Long implementation how-to; replace with links. |
| **Roadmap** | Deferred product, sequencing, **non-binding** exploration. | Restating contract already in plan/commitments. |
| **Reference / handbook** | Stack, examples, long procedures, full tree, API lists. | Executive summary; that belongs in the plan. |
| **Root README** | What it is, how to run/dev/deploy **minimally**, where deeper docs live. | Entire architecture treatise. |
| **CHANGELOG** | Notable **user-visible** or **operator-visible** change, release hygiene. | Internal refactor noise unless risky. |

**Conflict handling (template):** define **precedence** (e.g. commitments → plan → roadmap, or your org’s order) and **require updating** the lower-precedence doc when facts drift.

### 2.2 Naming

| Element | Convention |
| :--- | :--- |
| **Filenames** | `SCREAMING_SNAKE` only when industry-standard (e.g. `README`, `LICENSE`); otherwise **`kebab-case.md`** or **`PascalCase`** per language norms for code. **Meaningful names** over `notes.md`. |
| **Doc titles** | Match **purpose**, not process (“`DESIGN_COMMITMENTS`” not `CLARIFICATIONS_FROM_MARCH`”). |
| **Headings** | Sentence case or title case — **pick one** per repo and stay consistent. |
| **Stable anchors** | Prefer short, stable `##` headings for links; avoid punctuation-heavy slugs if tools differ. |

### 2.3 Layout & organization

| Principle | Practice |
| :--- | :--- |
| **Spine vs depth** | **Few** top-level sections in the primary plan; push detail to **linked** files. Many shallow `##` blocks **hurt skim speed**. |
| **Subfolders in `docs/`** | Use when: **audience** splits (ops vs dev), **subsystem** docs would exceed ~3 related files, or **release** packaging needs isolation. Don’t nest purely for aesthetics. |
| **Parity with code** | When code has clear bounded contexts, **mirror loosely** in docs (links or subfolders) so readers don’t hunt. |
| **Single scroll limit** | If a doc routinely exceeds **~2–3 screens** without a structural payoff, split or demote to reference. |

### 2.4 Visual structure & formatting

| Tool | When to use |
| :--- | :--- |
| **Tables** | Comparisons, routing (“if you need X → open Y”), mode matrices, responsibilities. |
| **Horizontal rules (`---`)** | Between **major** sections only — not after every paragraph. |
| **Callouts (`> [!NOTE]`, `[!TIP]`, …)** | **High-signal** exceptions, safety, or “this wins in a dispute” — not every note. |
| **Code fences** | Real commands, config, API examples — **copy-pasteable**, language tag when useful. |
| **Lists** | Parallel items; **avoid** deep nesting in prose docs (prefer tables or subheadings). |

### 2.5 Flow & reading order

| Principle | Practice |
| :--- | :--- |
| **Lead with substance** | Open with **what / why / boundaries**, not meta-instructions about how to read the doc. |
| **Questions & feedback last** | Collect **review prompts**, open questions, and “how to respond” **at the end** (meeting-style closure). |
| **Forward links** | Early **compact** pointer to where depth lives; repeat only in a **routing** table if helpful. |

### 2.6 Root vs `docs/`

| Location | Contents |
| :--- | :--- |
| **Repository root** | `README.md`, `CHANGELOG.md`, license, dependency manifests — **first impression** and **release** artifacts. |
| **`docs/`** (or org-standard equivalent) | Plans, ADRs, handbooks, diagrams source — **versioned** with code. |

---

## 3. Tone & verbiage

### 3.1 Audience assumption

Write for **competent technical readers** (engineers, PMs, infra leads). **Clarity ≠ simplification labels** — do not announce “plain English,” “simple,” or “for everyone.” Let structure carry accessibility.

### 3.2 Prefer

- **Direct, declarative** sentences; **active voice** where natural.
- **Defined terms once**, then consistent vocabulary (inventory, control plane, service boundary — pick and reuse).
- **Explicit scope**: what the system **does not** do is as important as what it does.
- **Concrete artifacts**: endpoints, paths, commands — **when** that reduces ambiguity.

### 3.3 Avoid

- **Patronizing** frames (“easy,” “just,” “simply,” “anyone can”).
- **Process theater** in the opener (“this document will…,” “we humbly present…”).
- **Duplicated metaphor** (saying the same thing in three registers for “accessibility”).
- **Unbounded jargon** — introduce acronyms **once** or link to a glossary file if density is high.

### 3.4 Honesty about readership

Most readers open deep docs **on demand** (failure, audit, onboarding crunch). Optimize for **fast routing** and **searchable reference**; don’t rely on linear reading of 50-page guides.

---

## 4. Categorical directives (quick scan)

| Category | Directive |
| :--- | :--- |
| **Structure** | Boundaries first; grow folders when **ownership or lifecycle** demands it. |
| **Documentation** | One **role** per file; thin plan + deep reference + labeled commitments + roadmap (adapt names to project). |
| **Navigation** | Short primary doc; **tables** route to detail; **few** top-level headings. |
| **Presentation** | Tables, sparing rules, callouts for **signal** only. |
| **Tone** | Professional, peer-level; no condescension; no “dumbing down” preamble. |
| **Flow** | Substance first; **review / questions** last. |
| **Truth** | **Precedence rules** when docs can disagree; **gitignore** local-only narrative. |
| **Reality** | Assume **selective reading**; make titles and first lines **search-friendly**. |

---

## 5. Adoption

- For a **new repo**: define **`docs/`** layout and the **four roles** (plan, commitments, roadmap, reference — drop any that don’t apply) **before** bulk authoring.
- For **existing repos**: migrate incrementally — extract depth first, **shorten** the plan last, so links stay valid.
- **Markdown in `docs/`:** Prefer **`.md`** (or org-standard plain text) as the **canonical** format — diffable in PRs, reviewable like code. No separate “indexable” format is required for normal repository workflows.
- **PDFs and similar binaries:** Treat them as **supplementary** or archival, not the maintained spec. If engineering must work from PDF content, **convert to Markdown** (or approved text) with **org-approved** tooling, then commit under `docs/` (or your records process). PDFs are awkward for line-by-line review and drift from the truth in practice.

This file is **normative for how we work**, not a product spec. Project-specific decisions still live in that project’s plan and commitments.
