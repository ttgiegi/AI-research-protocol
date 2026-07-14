---
name: mx-plasma-debug
description: Scientific paper reproduction protocol for the MX-Plasma MATLAB plasma solver. Use when explicitly requested to reproduce, diagnose, or continue work on matching solver behavior to a source paper, including cases where the issue may be code, numerical method, model interpretation, boundary conditions, normalization, experiment design, validation target selection, or paper-reading ambiguity.
---

# MX-Plasma Paper Reproduction

## Core Principle

Treat this as a scientific reproduction protocol for AI agents, not as a generic coding prompt.

The objective is to reproduce the paper, not merely to fix code. Do not optimize for changing code quickly. Optimize for eliminating wrong hypotheses quickly.

Maximize validated scientific knowledge per unit of time, not the number of experiments performed.

A failed experiment that rules out a plausible explanation is progress. Prefer small falsifiable experiments that shrink the search space over broad rewrites that appear productive but leave the causal story unclear.

## Research Loop

Use this loop as the default operating cycle:

1. `Observe`: identify the mismatch using named observables, not vague impressions.
2. `Hypothesis`: state one suspected cause and label its evidence.
3. `Paper Trace`: connect the hypothesis or proposed change to the paper.
4. `Experiment`: design the smallest test that can support or falsify the hypothesis.
5. `Evidence`: collect paper, implementation, or runtime evidence.
6. `Decision`: continue, stop, revert, escalate to paper archaeology, or revise the plan.
7. `Log`: update the experiment log so future agents do not repeat the same work.
8. `Next`: choose the next highest-value unknown.

Change one scientific or numerical assumption at a time unless the user explicitly requests a larger intervention.

Experiment is not the default action. It must earn its cost by reducing a specific uncertainty.

## Entry Protocol

Do not hard-code project-specific filenames as the general workflow. First locate and read the project's available equivalents of:

1. Project Overview
2. Current Experiment Status
3. Experiment History
4. Paper or Theory Source
5. Equation or Model Reference
6. Implementation Plan
7. Knowledge State
8. Question Queue
9. Experiment Index, if the experiment log is long

If the project provides explicit configuration for these roles, follow it. Otherwise infer the mapping from filenames, headings, and repository context, then state the mapping briefly before doing substantive work.

For MX-Plasma, the current known mapping includes:

- Project Overview: `README.md`
- Current Experiment Status: `Experiment Log.md` / `Current Status`
- Experiment History: `Experiment Log.md`
- Paper or Theory Source: `Paper or Theory Source.md` and the exported paper images when text is ambiguous
- Equation or Model Reference: `Implementation Plan.md`
- Implementation Plan: `README.md` and current experiment-log next candidates
- Knowledge State: `Knowledge.md`
- Question Queue: `Questions.md`
- Experiment Index: `Experiment Index.md`, when present

## Protocol Ecosystem

Keep the core protocol stable. Do not keep adding rules when the better move is to add project artifacts around the protocol.

Prefer this ecosystem:

- `Core`: the stable protocol in this skill.
- `Templates`: reusable skeletons for experiments, observables, failures, current status, and implementation plans.
- `Config`: project-specific mapping from protocol roles to filenames.
- `Examples`: concrete reproduction examples that teach the pattern.
- `Project`: living project documents such as paper source, implementation plan, knowledge state, current status, and experiment log.

The skill defines how to think. The project artifacts define what is currently known.

Use the memory architecture as context compression:

- Paper or Theory Source: facts from the source.
- Implementation Plan: current implementation/model plan.
- Experiment Log: long-term memory of what was tried.
- Experiment Index: navigation layer for the long experiment log.
- Knowledge State: compressed working memory of what is known now.
- Question Queue: unresolved questions and how to answer them.
- Skill: reasoning protocol.

When an `Experiment Index.md` exists, use it before scanning the full experiment log. Search the index by topic, EXP range, tag, or question ID; then open only the relevant entries in `Experiment Log.md`.

## Knowledge State

Maintain a `Knowledge.md` file when the project has accumulated enough experiments.

The experiment log records what was done. Knowledge State records what is currently known.

Use `Knowledge.md` as the first high-signal read after project overview and before scanning the full experiment log. Keep it concise and evidence-linked:

- Known Facts: claims supported by paper, implementation, or experiment evidence.
- Current Unknowns: unresolved questions that still affect reproduction.
- Unavailable Information: details the original paper does not provide after paper archaeology.
- Rejected Hypotheses: plausible explanations ruled out by experiments.
- Active Hypotheses: current suspects with confidence and evidence links.
- Next Best Questions: the smallest unknowns most worth resolving next.

Every knowledge item should link to supporting experiment IDs, paper locations, or implementation files. Keep `[RECONSTRUCTION]` choices explicitly provisional even after testing: experiments can validate their behavior, but cannot convert them into paper facts. If a claim cannot be linked, label it `[ASSUMPTION]` or leave it out.

Do not let `Knowledge.md` become a second experiment log. Keep process details in `Experiment Log.md`. Update `Knowledge.md` only when a conclusion changes, when about five new experiments have accumulated, or when a successful experiment changes the current best explanation.

## Experiment Index

Maintain an `Experiment Index.md` once `Experiment Log.md` becomes long enough that full-log reading is inefficient.

The index is a navigation layer, not a replacement for the log. It should contain:

- Phase ranges, such as `EXP-0050--EXP-0067`.
- Main topic and affected question IDs.
- Current verdict or whether the phase is superseded.
- Best lookup keywords and important EXP IDs.
- The next place to read in the full log when detail is needed.

Use this lookup order:

1. Read `Knowledge.md` for current conclusions.
2. Read `Questions.md` for open gates and priority.
3. Search `Experiment Index.md` for the relevant phase or EXP IDs.
4. Open only the needed entries in `Experiment Log.md`.

Do not duplicate full experiment narratives in the index. Keep it short enough to read quickly.

## Question Queue

Maintain `Questions.md` for unresolved questions that should not immediately become code changes.

Use it for paper ambiguities, parameter-source gaps, notation questions, validation-target uncertainty, boundary-condition ambiguity, and blocked implementation decisions.

Distinguish `Unknown` from `Unavailable`:

- `Unknown`: the answer is not currently known, but the project has not exhausted the relevant paper/source search path.
- `Unavailable`: the original paper or provided paper package does not supply enough information after paper archaeology. Record the missing item, the searched source locations, why it matters, and the next external-reference or assumption-test action.

Do not turn `Paper Missing` into `Implementation Bug`. If a parameter, closure, normalization, partition function, or validation observable is unavailable in the paper, do not silently invent a default and then debug the resulting behavior as if the code were wrong. Either search the cited references, add a clearly labeled `[ASSUMPTION]` diagnostic, block solver/model changes, or use `[RECONSTRUCTION]` only through the separately authorized Reconstruction Gate below.

Each question should record:

- Question ID
- Status
- Blocking Level
- Tags
- Question
- Information Availability
- Unavailable Reason
- Current Evidence
- Why It Matters
- Priority: `Critical`, `High`, `Medium`, or `Low`
- Next Verification Step
- Final Answer

Move a resolved question into `Knowledge.md` only as a concise known fact, with a link back to the question ID and evidence. Do not duplicate the full investigation narrative.

## Evidence Protocol

Label every non-trivial conclusion with its source:

- `[PAPER]` means the claim comes from the source paper or its extracted text/images.
- `[IMPLEMENTATION]` means the claim comes from repository code or project documents.
- `[EXPERIMENT]` means the claim comes from a command, script run, diagnostic output, plot, or numerical result.
- `[RECONSTRUCTION]` means a minimal, explicit, testable operational completion of a procedure that the paper requires but does not fully specify. It is anchored to paper evidence but is not itself a paper claim.
- `[ASSUMPTION]` means the claim is a hypothesis, inference, or guess that has not yet been verified.

Never treat `[ASSUMPTION]` as fact. Do not make code or model changes from an assumption alone. First convert the assumption into either `[PAPER]`, `[IMPLEMENTATION]`, or `[EXPERIMENT]` evidence, or explicitly state the smallest experiment that would test it.

Never cite `[RECONSTRUCTION]` as `[PAPER]`. A successful run can add `[EXPERIMENT]` evidence about the reconstructed procedure, but cannot prove that the author used that completion. Always preserve the chain `[PAPER] required structure -> Unavailable detail -> [RECONSTRUCTION] completion -> [EXPERIMENT] sensitivity result`.

When proposing or applying a change, state the evidence chain that justifies it. If the evidence chain is incomplete, pause the change and gather the missing evidence first.

## Evidence Weight

When evidence conflicts, prefer higher-weight evidence unless there is a clear reason not to:

1. Paper equation: strongest.
2. Paper figure or table: strongest for target behavior and parameters.
3. Paper caption, notation, or surrounding explanatory text: strong.
4. Reproducible experiment result: strong, but only for the tested configuration.
5. Implementation inspection: medium; it shows what the code does, not whether it matches the paper.
6. Project notes or previous logs: medium; useful but may be superseded.
7. Reconstruction: provisional; constrained by paper evidence and sensitivity testing, but not evidence of author intent.
8. AI reasoning or analogy: weak.
9. Assumption: weakest; must be tested or traced.

Do not resolve a conflict by intuition when stronger evidence can be gathered.

## Reconstruction Gate

Before proposing or executing any `[RECONSTRUCTION]`, read and follow [references/reconstruction.md](references/reconstruction.md). The gate requires an explicit paper anchor, an `Unavailable` operational detail, user authorization, minimality, preregistration, isolation, and sensitivity analysis. A faithful-implementation or no-guessing goal must stop instead.

Use reconstruction primarily for missing operational numerical details when the paper already fixes the surrounding method. Never use it to disguise a new governing equation, physical closure, source term, boundary condition, material constant, acceptance philosophy, or target-fitting heuristic. Never tune a reconstruction after seeing which choice best matches the target.

## Paper Trace

Before making any implementation, model, numerical, parameter, or validation-target change, answer:

- Which paper equation does this correspond to?
- Which paper section or paragraph motivates it?
- Which paper figure is it intended to reproduce or explain?
- Which paper table or parameter listing supports it?

If none of these can be answered, do not make the change. Gather paper evidence first, or run a diagnostic that does not alter solver behavior.

For each accepted change, record the paper trace in the working notes or final response using the clearest available locator, such as equation number, section name, figure number, table number, caption, or quoted phrase from the extracted paper text.

## Decision Gate

Use these gates before and after each experiment:

- Evidence missing: do not modify solver behavior. Gather paper, implementation, or experiment evidence first.
- Paper Trace missing: do not modify solver behavior. Run diagnostics only.
- Paper information unavailable: do not recast the gap as a code bug. Search cited references, mark the item `Unavailable`, design an explicitly labeled assumption diagnostic, or enter the Reconstruction Gate only when the goal authorizes it.
- Necessary implementation detail unavailable: stop the current implementation/debugging goal unless the user has explicitly authorized a reconstruction experiment. Record the missing detail, searched locations, why it blocks faithful implementation, and the next evidence path in `Questions.md` and `Experiment Log.md` before any reconstruction.
- Reconstruction gate incomplete: do not implement. Missing paper anchor, unavailable-item record, minimality rationale, preregistration, isolation, or sensitivity plan makes the proposed completion an `[ASSUMPTION]`, not a `[RECONSTRUCTION]`.
- Hypothesis unclear: do not modify. Reformulate into one falsifiable claim.
- Experiment supports hypothesis: continue with the next cheapest validation or a narrower follow-up.
- Experiment partially supports hypothesis: keep only the supported part, lower confidence, and design a discriminating experiment.
- Experiment falsifies hypothesis: revert or mark the change as failed/probe-only, then log what was ruled out.
- Same problem fails to improve after two consecutive implementation/numerical changes: enter Paper Archaeology.
- Five consecutive experiments produce no observable improvement: stop the current long run and produce a revised plan.

## Expected Information Gain Gate

Before starting a new experiment, estimate its research ROI:

1. What specific uncertainty will this reduce?
2. What evidence could realistically be obtained?
3. Would either likely result change the implementation, validation, or next-question decision?
4. Is this question higher priority than the other open questions?

Do not run hope-driven experiments. If the experiment is only "maybe this will improve" and cannot say what result A or result B would mean, do not run it.

If expected information gain is negligible, stop the experiment path and instead:

- summarize the current evidence,
- update `Questions.md` or `Knowledge.md`,
- propose the next highest-value investigation.

Question priority controls attention. Resolve `Critical` and `High` questions before spending long effort on `Medium` or `Low` questions, unless the lower-priority question is cheap and directly unblocks a higher-priority one.

## Observable Registry

Define reproduction progress through named observables. Do not write only "better" or "worse".

For each active observable, track:

- Observable Name
- Definition
- Paper Target or Paper Source
- Current Value
- History or Related Experiments
- Interpretation

Examples: `|R|_inf`, residual group maximum, boundary residual, output current, electron temperature range, electron density peak, voltage drop, figure-shape agreement, monotonic trend under a paper parameter sweep.

An experiment counts as improved only if at least one named observable improves without unacceptable regression in a higher-priority observable.

## Regression Evolution Protocol

Treat regression gates as scientific claims, not immutable contracts.

When new paper evidence or higher-weight evidence contradicts an existing regression standard, audit the gate before changing the implementation. Do not force a paper-correct model to satisfy a stale benchmark.

Distinguish:

- `Implementation regression`: the code diverges from the current paper-traced gate.
- `Gate evolution`: the benchmark, comparator, tolerance, or validation role was based on incomplete or superseded evidence.

Use this workflow:

1. Identify the old gate: observable, comparator, tolerance, validation role (`HardGate`, `PaperCalibration`, `BranchAnchor`, or `DevelopmentCheck`), and evidence source.
2. Identify the new evidence and its Evidence Weight.
3. Decide whether the gate is confirmed, narrowed, reclassified, superseded, or retired.
4. Update `Knowledge.md`, `Questions.md`, `Experiment Log.md`, `Experiment Index.md`, and regression scripts or metadata when the gate changes.
5. Preserve the old result as historical or superseded evidence, not as an implementation failure.
6. Evaluate implementation behavior only after the revised gate is explicit.

If a paper-traced model worsens an older fixed-state or no-worse metric, first ask whether the metric was legacy-shaped. A `Regression Gate Updated` verdict is valid progress when evidence warrants it.

Do not downgrade paper evidence to satisfy automated tests. Evolve the tests and gates to match the evidence hierarchy.

Example: if Eq. 4-61 paper evidence makes a DR perturbation expected, revise the Figure 4.24-like fixed-state gate instead of tuning DR to reproduce the legacy branch.

## Numerical Isolation Protocol

When paper-traced physics is frozen, isolate numerical mechanisms.

After the relevant equations, source terms, boundary conditions, closures, constants, and validation gates are paper-traced or explicitly frozen, a Goal must not modify more than one numerical mechanism at a time.

Examples of separate numerical mechanisms include:

- Candidate selection
- Merit function
- Damping schedule
- Line search
- Trust region
- Residual weighting
- Continuation schedule
- Scaling or normalization of solver variables

Do not combine changes such as damping plus line search plus trust region in a single experiment. Pick one mechanism, define the observable it should affect, and test it before touching the next mechanism.

If an experiment appears to require multiple numerical changes at once, first run diagnostic enumeration or logging to identify which mechanism is actually selecting, rejecting, or distorting the relevant candidate. Multi-mechanism changes are allowed only after a prior isolated experiment shows the mechanisms cannot be separated, and the coupling must be recorded explicitly in `Experiment Log.md`.

## Figure Calibration Protocol

Treat a quantitative figure as an observable, not as decoration. A paper figure may be the only available source for a validation target, branch shape, convergence trend, or calibration scale.

If a figure represents a quantitative observable and numerical values are not otherwise provided:

1. Identify the observable shown by the figure, including axis meaning, units, scale type, series identities, and the relevant caption or surrounding text.
2. Digitize the figure before using it for numerical comparison. Do not rely on a visual impression or multimodal reading alone as numeric data.
3. Estimate uncertainty from image quality, axis readability, curve overlap, annotations, markers, compression, scan distortion, and whether the axis is linear or logarithmic.
4. Produce a reproducible artifact when possible: script, CSV, MAT/JSON, plot reconstruction, and a QC overlay showing extracted points against the original image.
5. Require human QC before the digitized data can guide regression decisions. Automated or multimodal digitization remains provisional until a human has checked axis calibration, series identity, endpoints, curve crossings, and obvious extraction errors.
6. Record the resulting comparison level:
   - `Qualitative`: trend, ordering, existence, or sign only.
   - `Semi-quantitative`: approximate scale, shape, branch anchor, or convergence trend; useful for diagnostics but not for tight tolerances.
   - `Quantitative`: enough axis/series/QC confidence to support a numeric regression benchmark with stated uncertainty.
   - `HardGate`: allowed only when the paper provides explicit numerical values or the digitization has independently verified uncertainty and human approval strong enough for hard pass/fail use.
7. Record the data confidence and validation role in the project knowledge state or observable registry. A digitized figure should default to `PaperCalibration` or `BranchAnchor`, not `HardGate`.

The evidence chain is:

```text
Figure -> Digitize -> QC / Uncertainty -> Benchmark -> Regression Test
```

Multimodal ability can help locate curves, read captions, and propose an extraction strategy. It does not by itself make numeric data reliable. If human QC has not confirmed a digitization, use it only for exploration and label it provisional.

When figure extraction conflicts with paper equations or explicit table values, prefer the equation/table unless the conflict is the specific subject of a calibration investigation.

## Failure Taxonomy

Do not record all failures as `Failed`. Classify the failure:

- Paper-understanding failure
- Parameter-mapping failure
- Unit, scaling, or normalization failure
- Boundary or sheath interpretation failure
- Discretization failure
- Solver strategy failure
- Implementation bug
- Experiment design failure
- Validation or diagnostic failure
- Paper information insufficient
- Paper information unavailable
- Unknown

Use tags so failures can be counted later. The goal is to learn where the reproduction search space is actually collapsing.

## Scientific Debug

Treat debugging as scientific reproduction, not as generic fixing.

For every attempt, use this structure:

1. `Hypothesis`: state the specific suspected cause of mismatch and label its source using the Evidence Protocol.
2. `Experiment`: state the smallest code, parameter, diagnostic, or paper-reading action that tests the hypothesis.
3. `Evidence`: report the observed result from paper trace, implementation inspection, or numerical run.
4. `Conclusion`: decide whether to keep investigating, revert, validate further, or update the reproduction interpretation.

Example:

- `Hypothesis`: `[ASSUMPTION]` The time step may be too large for the observed residual behavior.
- `Experiment`: halve `dt` and rerun the cheapest relevant residual diagnostic.
- `Evidence`: `[EXPERIMENT]` Residual decreases monotonically after the change.
- `Conclusion`: continue validation against paper observables before changing default solver settings.

## Experiment Log Protocol

The skill decides how the agent thinks. The experiment log decides what the agent can think about.

Treat `Experiment Log.md` as an Experiment Log, not as a diary. It should be structured enough that an agent can search it like a database.

Before starting reproduction work, read the `Current Status` section first. Use it as the one-page summary of the current best path, strongest evidence, excluded directions, and next candidates.

After every meaningful experiment, update `Experiment Log.md` with a new `EXP-####` entry or update the existing entry if the experiment is still active. Do not leave important results only in chat.

Each experiment entry should use this schema:

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

### Paper Trace
- Equation:
- Section:
- Figure:
- Table:

### Modification
- Files:
- Change:
- Why Worth Testing:

### Validation
- Command:
- Runtime:
- Result:
- Residual:
- Convergence:

### Paper Comparison
- Observable:
- Match:
- Mismatch:

### Verdict
- Conclusion:
- Keep/Revert:
- What This Rules Out:

### Next Candidate
- Next:
```

Record why a modification is worth testing, not only what changed. A useful entry says what should happen if the hypothesis is true and what can be ruled out if it is false.

Use `Tags` for searchability, such as `Path4`, `Boundary`, `Residual`, `Equation18`, `Normalization`, `Seed`, `Current`, or `Sheath`. Use `Related` to link experiments that belong to the same investigation chain.

Use `Confidence` to prioritize future work. Confidence is about the hypothesis, not about the agent's certainty in its own reasoning.

## Return To Paper Triggers

Immediately stop modifying code and reread the paper or paper-derived artifacts when any of these conditions occur:

1. The same problem has failed to improve after two consecutive implementation or numerical changes.
2. A proposed change would alter a core formula, coefficient, sign, unit, scaling, or normalization.
3. A proposed change would alter a boundary condition or sheath/interface condition.
4. Solver output has a trend that is qualitatively different from the paper, not just numerically offset.
5. A parameter, coefficient, normalization choice, or validation target cannot be traced to a clear source.

Assume a missing sentence, table note, figure caption, normalization convention, or boundary-condition detail may explain the mismatch. Resume code changes only after the relevant paper evidence has been located, the remaining uncertainty has been explicitly labeled `[ASSUMPTION]` and converted into a testable diagnostic, or a separately authorized proposal passes the Reconstruction Gate.

## Paper Archaeology

Enter Paper Archaeology mode when any of these appear:

- A parameter source is unknown.
- A variable meaning is ambiguous.
- A derivation skips steps that affect implementation.
- A formula, coefficient, sign, unit, or normalization is unclear.
- Boundary or sheath implementation details are missing.
- The implementation needs a detail that the current notes cannot trace to the paper.

In Paper Archaeology mode, do this before further solver changes:

1. Reread the section before and after the relevant passage.
2. Reread the formula in its surrounding text, not as an isolated equation.
3. Check notation, definitions, subscripts, superscripts, normalization, and units.
4. Check figure captions, table notes, and parameter descriptions.
5. Check cited references only if the source paper appears to delegate the missing detail.
6. If the original paper does not provide the needed detail, mark it `Unavailable` in the question queue with searched locations and the next external-reference path.
7. Return to the code only after the paper evidence is located, the unavailable item is resolved by a cited/reference source, the remaining uncertainty is explicitly recorded as `[ASSUMPTION]` with a testable diagnostic, or an explicitly authorized reconstruction passes every Reconstruction Gate requirement.

## Goal Stop Protocol

Stop the current goal or long debugging run when five consecutive experiments produce no observable improvement, when a necessary implementation detail is unavailable after the relevant paper/source search and reconstruction is not explicitly authorized, or when several consecutive low-value experiments have negligible expected information gain compared with their cost.

Observable improvement must be tied to a reproduction target, such as convergence behavior, residual trend, stability, boundary behavior, conserved/current quantities, profile shape, figure agreement, or a clearer explanation of a paper-to-code mismatch.

Stopping for unavailable information is not failure. It is a valid scientific result when it prevents converting a missing paper detail into a fake implementation bug.

When stopping, do not keep spending tokens on speculative edits. Instead output:

1. The current evidence table: `[PAPER]`, `[IMPLEMENTATION]`, `[EXPERIMENT]`, `[RECONSTRUCTION]`, and `[ASSUMPTION]`.
2. The failed experiments or paper/source searches and what each ruled out.
3. The blocking missing detail, with `Unknown`, `Partially Available`, or `Unavailable` status.
4. The marginal information gain trend: what new knowledge was gained recently and why continuing this path has low research ROI.
5. Why the current goal should stop now rather than continue with assumptions or hope-driven experiments.
6. A revised plan that starts from the highest-value unknown, cited reference, or isolated assumption diagnostic, not from the latest failed edit.

## Suspicion Order

When reproduction fails, investigate possible causes in this order unless evidence points elsewhere:

1. Own understanding of the paper or reproduction target.
2. Parameter values or parameter mapping.
3. Units, scaling, or normalization.
4. Boundary conditions, sheath conditions, or interface interpretation.
5. Discretization or discrete operator mismatch.
6. Solver strategy: coupling, damping, stepping, tolerances, convergence criteria.
7. Code implementation bug.
8. Paper error.

Treat the paper, especially a published or classic paper, as provisionally reliable. Paper errors are possible, but they should be the last explanation, reached only after the reproduction setup, interpretation, numerical method, and implementation have been checked with evidence.

When the paper omits required information, classify the omission as `Unavailable` before suspecting an implementation bug. Missing paper data is a research-input gap, not a defect in the solver.
