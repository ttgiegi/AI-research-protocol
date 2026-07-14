# AI Research Protocol

## Why This Skill Exists

As modern large language models (LLMs) become increasingly capable of generating ideas, they are most commonly used to answer questions and write code. Yet long-running, open-ended scientific projects often fail or progress less efficiently with LLM assistance than straightforward coding tasks. The limiting factor is usually not programming ability. It is protocol drift, loss of context, and decisions that were never recorded.

This repository provides a structured workflow that treats an AI as a research collaborator rather than an answer generator. Its central principle is simple: **evidence first, experiments driven by hypotheses, and protocols recorded before code is changed.**

Every project has one or more authoritative sources that define expected behavior, constraints, or acceptance criteria. A source may be a scientific paper, standard, specification, dataset, experimental observation, official documentation, physical law, or approved requirement. The core protocol works from those sources; focused profiles add domain-specific rules such as paper reproduction and numerical-solver analysis.

## Protocol Architecture

```text
Core Protocol
  |- Evidence
  |- Experiments
  |- Knowledge
  |- Question Queue
  |- Git and Experiment History
  |- Decision Gates
  |- Goal Stops
  |
  +-- Profiles
       |- Paper Reproduction
       |- Numerical Solver
       |- Dataset Analysis (future)
       |- Specification Validation (future)
```

The core protocol is source-driven and applies to every mode. Profiles add strict domain-specific safeguards only when their assumptions apply.

## Why This Exists

An AI agent started modifying a converged solver because the latest code structure looked cleaner than the historical experiment path.

Git and the experiment log showed that the new execution path had never existed historically.

The protocol prevented weeks of unnecessary debugging.

## Common Failure Modes of AI Research

Long-running AI-assisted research does not usually fail because an agent cannot write code. It fails when the agent loses the scientific thread of the project.

| Failure mode | Control mechanism |
| --- | --- |
| ❌ The AI modifies a module that has already been validated. | **Experiment Log (EXP Log):** preserve what was tested, under which conditions, and what was concluded. |
| ❌ The AI reinterprets the paper without tracing the source. | **Evidence Tags:** label claims by their source, such as paper evidence, implementation evidence, runtime evidence, or assumption. |
| ❌ The AI begins optimizing the wrong problem. | **Goal Stop:** state the current scientific objective and define conditions that make an intervention out of scope. |
| ❌ The AI forgets historical experiments. | **Git:** use history to recover what changed, when it changed, and whether an execution path ever existed. |
| ❌ The AI invents a new protocol during the investigation. | **Protocol Freeze:** keep the operating procedure stable unless its revision is explicitly decided and recorded. |
| ❌ The AI treats a refactor as scientific progress. | **Decision Gate:** require evidence that a proposed change reduces a named uncertainty before it is implemented. |

This is not a prompt for generating answers. It is a workflow for managing AI as a research collaborator: preserve the context, make evidence visible, constrain interventions, and record decisions so that progress remains scientifically interpretable.

## Best Practices for AI-Assisted Scientific Reproduction

### 1. Convert PDFs into AI-Friendly Text First

Most scientific papers are written for humans, not for language models. A conventional PDF often contains a two-column layout, headers and footers, mathematical equations, figure references, or scanned pages. AI systems generally interpret these formats less reliably than structured text, especially when a detail must be traced precisely across a long investigation.

Use a conversion pipeline that turns the paper into an inspectable research artifact:

```text
PDF
  |
  v
OCR / Vision Model
  |
  v
Markdown
  |
  v
LaTeX Equations
  |
  v
Version-Controlled Text
```

Markdown is much easier for both humans and AI to search, diff, annotate, and reference. Keeping the converted text under version control also makes corrections to OCR, equations, and source interpretation visible rather than silently replacing the evidence base.

### 2. Preserve Equations as LaTeX

Mathematical notation is often central to scientific reproduction, yet plain-text OCR can turn it into an ambiguous string. For example:

```text
Te = ni2/(k*e)
```

may be interpreted in several incompatible ways, while the LaTeX representation preserves the intended structure:

```latex
T_e = \frac{n_i^2}{ke}
```

LaTeX acts as a lossless interface between humans and AI. It keeps subscripts, superscripts, fractions, operators, and grouping explicit, allowing an agent to reason about the same mathematical object that a researcher sees in the paper. This matters throughout scientific work, where equations must be extracted, checked against implementations, transformed into code, and traced back to their source.

### 3. Do Not Ask AI to Search the Paper Repeatedly

Repeatedly asking an AI agent to search a raw PDF is a common and expensive failure mode. Prefer a knowledge pipeline that extracts the relevant evidence once and makes it available in a stable, searchable form:

```text
Paper
  |
  v
Structured Notes
  |
  v
Knowledge.md
  |
  v
AI
```

Do not use this as the default loop:

```text
Paper
  |
  v
AI searches the PDF every time
```

Searching the PDF afresh wastes context, can miss relevant passages, produces inconsistent citations, and consumes substantial tokens. A maintained knowledge document gives each new research session the same reviewed evidence base.

### 4. Separate Immutable Knowledge from Experiment Logs

Keep different kinds of research state in different documents. This distinction lets an AI agent tell facts, hypotheses, and plans apart before it takes action.

| Document | Record here |
| --- | --- |
| `Knowledge.md` | Paper content, confirmed facts, and validated theory. |
| `Experiment Log` | What was tested, what happened, and why the experiment was stopped or continued. |
| `Implementation Plan` | The next proposed actions and the rationale for taking them. |

Treat `Knowledge.md` as an evidence-backed record, not a place for untested interpretations. Treat the experiment log as a chronological decision record, and the implementation plan as a revisable statement of intent. With this separation, the AI can distinguish what is known, what is hypothesized, and what is merely planned.

### 5. Let Git Remember History, Not the AI

AI should not be treated as long-term memory. Instead, make the project memory a combination of:

```text
Git
  +
Experiment Log
  +
Structured Documents
```

Structured documents record the reasoning and decisions behind the work. Git records the change history, which is particularly important in code-based research projects. Together, they preserve the evidence outside the AI chat window.

Agent context is limited, so a single conversation should not be expected to carry a scientific project indefinitely. When the project state is managed through structured documents and Git, a new session can recover the current knowledge, past decisions, experiments, and code history without relying on the memory of a previous agent. This makes the research process both auditable and restartable.

### 6. Ask Narrower Questions

Do not ask an AI agent a broad question such as:

```text
Why doesn't my solver converge?
```

Ask a question that identifies a mechanism and can guide a discriminating experiment:

```text
Does the local spectral radius indicate contraction?
```

or:

```text
Is the plateau caused by a fixed-point property or an implementation bug?
```

Questions of this form let the AI propose specific measurements, predictions, and experiments. The goal is to design the direction of exploration from an evidence chain, rather than repeatedly giving an agent an open-ended problem with no supporting context. Broad questions can produce plausible explanations, but they rarely provide a concrete next step that materially advances the investigation.

### 7. Treat AI as a Collaborator, Not an Oracle

AI should:

- Generate hypotheses.
- Design experiments.
- Interpret evidence.

AI should not:

- Replace evidence.
- Replace validation.
- Replace scientific judgment.

The agent is most useful when it expands the researcher's ability to reason, inspect alternatives, and design tests. Its output remains a proposal until it is connected to evidence and validated through the research process.

### 8. Build Evidence Before Optimizing

When a numerical calculation shows a large residual, the tempting response is often:

```text
Large residual
  |
  v
Repeatedly modify the solver
```

Use an evidence-first path instead:

```text
Large residual
  |
  v
Design an experiment
  |
  v
Establish the cause
  |
  v
Modify the implementation only when justified
```

This is the experiment protocol in practice. Do not optimize before identifying what the observation means. A large residual may arise from a model property, a boundary condition, a normalization choice, an expected numerical limit, or an implementation defect. Evidence determines which intervention is warranted.

### 9. Separate Protocol Changes from Scientific Discoveries

Not every important finding is a scientific discovery. For example, an entry such as `EXP-0205` may identify **protocol drift**: the investigation has stopped following the agreed workflow or has lost the distinction between validated results and new assumptions. That is a process finding, not evidence about the scientific system.

Record protocol changes separately from scientific discoveries. Otherwise, an AI agent may try to repair the protocol by changing a solver that has already been validated, creating a new scientific regression while addressing an organizational problem.

When an investigation suddenly appears to be wrong at a fundamental level, look for evidence first. Trace the source, the experiments, the assumptions, and the historical execution path before changing a proven implementation.

### 10. Prefer Reproducibility Over Cleverness

A reproducible experiment that clearly explains why a decision was made is usually more valuable than a sophisticated implementation whose reasoning is lost.

Optimize for work that another researcher, a future agent, or a new session can inspect and repeat. Cleverness that cannot be traced, tested, or reconstructed does not create durable scientific progress.

## Lessons Learned

- AI works best with structured context.
- Git is more important than longer prompts.
- Experiment protocols prevent protocol drift.
- Scientific evidence should drive code changes.
- Markdown and LaTeX are significantly more AI-friendly than raw PDFs.
- Narrow questions usually produce much better answers than broad ones.
- Long-running AI projects need persistent memory outside the chat window.

## Adapting This Workflow

To use this workflow well, organize the Skill around the work you are actually doing. The authoritative sources, observables, documents, profiles, and experiment structure should reflect your project rather than copying this repository mechanically.

The problem-decomposition and research-management approach can still be useful in difficult situations: reproducing an old paper with missing information, or pursuing a complex topic beyond your current expertise when external help is unavailable and AI is the only practical collaborator. In particular, the use of structured Markdown documents as AI input, image recognition to recover information from figures or scans, and LaTeX to preserve mathematical expressions are among the most valuable parts of this Skill.

This Skill does not guarantee that a research task will succeed. Its purpose is to provide a way of thinking that may help make the work more traceable, evidence-driven, and manageable.

Questions and workflow-improvement suggestions are welcome at [wsh14250@qq.com](mailto:wsh14250@qq.com). I will probably not understand your research domain, but I will be glad to help improve the workflow. I built it while reproducing a decades-old numerical code for a highly nonlinear, coupled physical system from an incomplete source paper.
