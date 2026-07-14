---
name: ai-research-protocol
description: Evidence-driven protocol for long-running scientific and technical research with AI collaborators. Use when planning, diagnosing, reproducing, validating, or continuing research that needs authoritative-source tracing, evidence tracking, hypothesis-driven experiments, decision records, and durable project context.
---

# Evidence-Driven Research Protocol

## Core Principle

Treat this as a research protocol for AI agents, not as a generic coding prompt.

Assume every research project has one or more **Authoritative Sources** that define the expected behavior, constraints, terminology, or acceptance criteria of the system. A source may be a scientific paper, standard, specification, dataset, experimental observation, official documentation, physical law, requirement, or other project-approved record.

Do not optimize for changing code quickly. Optimize for eliminating wrong hypotheses quickly. Maximize validated knowledge per unit of time, not the number of experiments performed.

A failed experiment that rules out a plausible explanation is progress. Prefer small falsifiable experiments that shrink the search space over broad rewrites that leave the causal story unclear.

## Project Metadata

Before substantive work, identify or create project metadata with:

- `Research Mode`: `Paper Reproduction`, `Specification Driven`, `Dataset Driven`, `Observation Driven`, or `Mixed`.
- `Authoritative Sources`: source ID, type, location, owner where applicable, scope, and authority level.
- `Primary Observables`: the measurements, outputs, or acceptance criteria that define progress.
- `Constraints`: requirements that must not be changed without explicit authorization.
- `Active Profiles`: any applicable profile under `profiles/`.

Use `Source Trace` as the common term in the core protocol. Apply the active profile to use a more specific label such as Paper Trace, Specification Trace, Dataset Trace, or Observation Trace.

Read [profiles/paper-reproduction.md](profiles/paper-reproduction.md) whenever `Research Mode` is `Paper Reproduction` or a paper is authoritative for the active decision. Read [profiles/numerical-solver.md](profiles/numerical-solver.md) whenever the work changes or diagnoses numerical-solver behavior.

## Research Loop

Use this loop as the default operating cycle:

1. `Observe`: identify the mismatch, question, or opportunity using named observables rather than vague impressions.
2. `Hypothesis`: state one suspected cause and label its current evidence.
3. `Source Trace`: connect the hypothesis or proposed change to an authoritative source, or explicitly mark the source gap.
4. `Experiment`: design the smallest test that can support or falsify the hypothesis.
5. `Evidence`: collect source, implementation, experiment, or observation evidence.
6. `Decision`: continue, stop, revert, enter Source Archaeology, or revise the plan.
7. `Log`: update the experiment log so future agents do not repeat the same work.
8. `Next`: choose the highest-value unresolved question.

Change one material assumption at a time unless an explicitly recorded experiment requires a coupled intervention. An experiment must earn its cost by reducing a specific uncertainty.

## Entry Protocol

Do not hard-code project-specific filenames. Locate the project's available equivalents of:

1. Project Overview
2. Current Status
3. Experiment History
4. Authoritative Source Register
5. Model, Data, or Requirement Reference
6. Implementation Plan
7. Knowledge State
8. Question Queue
9. Experiment Index, when the log is long

If the project provides a mapping, follow it. Otherwise infer the mapping from filenames, headings, and repository context, then state it before acting.

A typical mapping may be:

- Project Overview: `README.md`
- Current Status and Experiment History: `Experiment Log.md`
- Authoritative Source Register: `Sources.md` or `Project Metadata.md`
- Model, Data, or Requirement Reference: source-linked notes or project documents
- Implementation Plan: `Implementation Plan.md`
- Knowledge State: `Knowledge.md`
- Question Queue: `Questions.md`
- Experiment Index: `Experiment Index.md`

## Protocol Ecosystem

Keep the core protocol stable. Extend it through project artifacts and focused profiles rather than adding project-specific rules to the core.

- `Core`: the workflow in this file.
- `Profiles`: mode-specific rules, such as paper reproduction or numerical solvers.
- `Project Metadata`: research mode, source register, and profile selection.
- `Knowledge`: current evidence-backed conclusions.
- `Experiment Log`: chronological record of tests and decisions.
- `Question Queue`: unresolved, decision-relevant uncertainties.
- `Implementation Plan`: proposed next actions.

The protocol defines how to reason. The project artifacts define what is known now.

## Knowledge State

Maintain `Knowledge.md` when the project has accumulated enough work that rereading all history is inefficient.

The experiment log records what was done. Knowledge State records what is currently known. Keep it concise and evidence-linked:

- Known Facts: claims supported by source, implementation, experiment, or observation evidence.
- Current Unknowns: unresolved questions that affect the project goal.
- Unavailable Information: decision-relevant source details that remain unavailable after Source Archaeology.
- Rejected Hypotheses: plausible explanations ruled out by experiments.
- Active Hypotheses: current suspects with confidence and evidence links.
- Next Best Questions: the smallest unknowns most worth resolving next.

Link every item to experiment IDs, source locators, observations, or implementation files. If a claim lacks support, label it `[ASSUMPTION]` or leave it out. Do not let `Knowledge.md` become a second experiment log.

## Experiment Index

Maintain `Experiment Index.md` once `Experiment Log.md` becomes too long to scan efficiently. Use it as a navigation layer, not as a replacement for the log.

Include phase ranges, main topics, affected question IDs, current verdicts, lookup keywords, important experiment IDs, and the next detailed entry to read. Read `Knowledge.md`, then `Questions.md`, then the relevant index entries before opening only the needed log records.

## Question Queue

Maintain `Questions.md` for uncertainties that should not immediately become code changes. Use it for source ambiguity, requirement gaps, data provenance, notation, parameter meaning, validation uncertainty, and blocked decisions.

Distinguish:

- `Unknown`: the answer is not known and the relevant source path has not been exhausted.
- `Unavailable`: the answer remains absent after appropriate Source Archaeology. Record the missing detail, searched locations, why it matters, and the next external-reference or assumption-test action.

Do not turn a missing source detail into an implementation bug. Do not silently invent a default. Search the source path, record `Unavailable`, run a clearly labeled assumption diagnostic, or stop the affected work.

Each question should record ID, status, blocking level, tags, question, information availability, current evidence, impact, priority, next verification step, and final answer.

## Evidence Protocol

Label every non-trivial conclusion with its source:

- `[SOURCE]`: an authoritative source, identified by source ID and locator. The source may be a paper, specification, dataset, documentation, standard, requirement, or approved observation record.
- `[IMPLEMENTATION]`: repository code, configuration, or project documents show the system currently behaves this way.
- `[EXPERIMENT]`: a reproducible test, command, diagnostic, plot, or numerical result supports the claim for its tested conditions.
- `[OBSERVATION]`: a measurement or recorded real-world observation supports the claim, with method and conditions stated.
- `[ASSUMPTION]`: a hypothesis, inference, or guess that has not been verified.

Never treat `[ASSUMPTION]` as fact. Do not make a material code, model, configuration, or acceptance-criteria change from an assumption alone. Convert it to evidence or state the smallest test that could do so.

When proposing or applying a change, state its evidence chain. Pause the change when the chain is incomplete and a relevant source or experiment can resolve it.

## Evidence Weight

When evidence conflicts, prefer the evidence that is authoritative for the question and strongest for the stated conditions:

1. Authoritative source directly governing the requirement, model, or acceptance criterion.
2. Direct, well-characterized observation for the real system and conditions at issue.
3. Reproducible experiment or validated dataset result for the tested configuration.
4. Implementation inspection, which shows what the current system does rather than what it should do.
5. Project notes or prior logs, which may be superseded.
6. AI reasoning, analogy, or untested inference.
7. Assumption.

Do not resolve a conflict by intuition when stronger evidence can be gathered. Record why the selected source is authoritative when multiple sources disagree.

## Source Trace

Before making a material change, answer:

- Which authoritative source, requirement, observation, or approved project decision supports it?
- What exact locator supports it: section, requirement ID, dataset field, figure, table, equation, observation record, or commit?
- Which observable or acceptance criterion should this change affect?

If no source trace exists, do not make a behavior-changing implementation edit. Gather evidence first or run a diagnostic that leaves behavior unchanged.

## Decision Gate

Use these gates before and after each experiment:

- Evidence missing: gather source, implementation, experiment, or observation evidence first.
- Source Trace missing: run diagnostics only; do not change system behavior.
- Authoritative source unavailable: record `Unavailable`, search delegated or governing sources, and stop the affected goal unless an explicitly authorized assumption experiment is appropriate.
- Hypothesis unclear: reformulate it into one falsifiable claim.
- Experiment supports the hypothesis: run the cheapest necessary validation or a narrower follow-up.
- Experiment partially supports it: retain only the supported part, lower confidence, and design a discriminating experiment.
- Experiment falsifies it: revert or mark it probe-only, then log what was ruled out.
- Repeated interventions fail to add information: enter Source Archaeology or stop and revise the plan.

## Expected Information Gain Gate

Before starting an experiment, ask:

1. What uncertainty will this reduce?
2. What evidence could realistically be obtained?
3. Would either likely result change the next decision?
4. Is this more important than the other open questions?

Do not run hope-driven experiments. If the experiment cannot say what result A or result B would mean, redesign it or choose a higher-value question.

## Observable Registry

Define progress through named observables. Do not write only "better" or "worse." For every active observable, record its name, definition, authoritative target or source, current value, history, and interpretation.

An experiment is progress only when it improves a named observable, rules out a plausible cause, improves the evidence chain, or resolves a decision-relevant uncertainty without unacceptable regression elsewhere.

## Regression Evolution Protocol

Treat regression gates as evidence-backed claims, not immutable contracts. When stronger source or observation evidence contradicts a benchmark, audit the benchmark before changing the implementation.

Record the old gate, its comparator and tolerance, its validation role, the new evidence, the decision to confirm or revise it, and the affected project documents or scripts. Preserve superseded results as history rather than recasting them as implementation failures.

## Reference Calibration Protocol

Treat a quantitative reference as an observable, not decoration. A reference may be a figure, dataset, golden output, official result, simulation, or measured observation.

Before using a reference for numerical comparison, identify its quantity, units, scale, provenance, uncertainty, and comparison role. Produce reproducible extraction artifacts when possible, require human QC before strict pass/fail use, and label the result `Qualitative`, `Semi-quantitative`, `Quantitative`, or `HardGate` according to its confidence.

## Failure Taxonomy

Classify failures rather than recording all of them as `Failed`:

- Source-understanding failure
- Requirement or model-mapping failure
- Data provenance or measurement failure
- Configuration failure
- Execution or environment failure
- Implementation bug
- Experiment-design failure
- Validation or diagnostic failure
- Source information insufficient or unavailable
- Unknown

Use tags so the project can learn where the search space is actually collapsing.

## Scientific Debug

Treat debugging as evidence-driven research, not generic fixing. For every attempt, record:

1. `Hypothesis`: the specific suspected cause and its evidence.
2. `Experiment`: the smallest code, configuration, observation, or source-reading action that tests it.
3. `Evidence`: the result from source trace, implementation inspection, experiment, or observation.
4. `Conclusion`: whether to continue, revert, validate further, update the interpretation, or stop.

## Experiment Log Protocol

Treat `Experiment Log.md` as a structured, searchable decision record rather than a diary. Before starting work, read its current-status summary. After every meaningful experiment, add or update an `EXP-####` entry. Do not leave material results only in chat.

```markdown
## EXP-####: short descriptive title

- Status:
- Date:
- Goal:
- Tags:
- Related:
- Confidence:
- Failure Type:

### Hypothesis
- Claim:
- Evidence:

### Source Trace
- Source ID and Type:
- Locator:
- Constraint or Expected Behavior:

### Modification
- Files or System:
- Change:
- Why Worth Testing:

### Validation
- Method or Command:
- Conditions:
- Result:
- Named Observables:

### Reference Comparison
- Reference:
- Match or Mismatch:
- Uncertainty:

### Verdict
- Conclusion:
- Keep/Revert:
- What This Rules Out:

### Next Candidate
- Next:
```

Record what should happen if the hypothesis is true and what can be ruled out if it is false. Use tags, related IDs, and confidence to preserve the investigation chain.

## Return To Source Triggers

Immediately pause behavior-changing edits and return to authoritative sources when:

1. Two consecutive interventions fail to improve a named observable or add decision-relevant information.
2. A proposed change would alter a core model, requirement, invariant, acceptance criterion, unit, scaling, or configuration constraint.
3. The observed behavior conflicts qualitatively with an authoritative reference.
4. A parameter, target, requirement, or validation rule cannot be traced to a clear source.

Resume changes only after locating the relevant evidence, converting the remaining uncertainty into a testable assumption diagnostic, or explicitly stopping the affected goal.

## Source Archaeology

Enter Source Archaeology when a source is ambiguous, incomplete, contradictory, or not traceable to its governing record. Before changing behavior:

1. Read the relevant source in context rather than isolated fragments.
2. Check definitions, units, versions, ownership, provenance, and delegated references.
3. Compare the source with related requirements, datasets, observation records, or approved decisions.
4. Record `Unavailable` when an authoritative detail remains missing, including the searched locations and why it blocks the decision.
5. Return to implementation only after the issue is resolved, isolated as a testable assumption, or explicitly accepted as a project constraint.

## Goal Stop Protocol

Stop a long debugging or research run when repeated experiments produce no observable improvement or information gain, when a necessary authoritative detail is unavailable, or when continued work would depend on unrecorded assumptions.

Stopping is a valid research result when it prevents speculative changes. Summarize the evidence table, failed experiments or source searches, blocking uncertainty, information-gain trend, reason to stop, and the highest-value next investigation.

## Suspicion Order

When the project behavior conflicts with its target, investigate in this order unless evidence points elsewhere:

1. Source interpretation or target definition.
2. Model, requirement, or data mapping.
3. Implementation.
4. Configuration, parameters, units, or environment.
5. Execution path or runtime behavior.
6. Observation, measurement, or validation method.
7. Source error or inconsistency.

Treat an authoritative source as provisionally reliable, but never infallible. Reach a source-error conclusion only after the interpretation, mapping, implementation, configuration, execution, and observation paths have been checked with evidence.
