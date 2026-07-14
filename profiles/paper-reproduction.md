# Paper Reproduction Profile

Activate this profile when `Research Mode` is `Paper Reproduction` or when a scientific paper is authoritative for the active decision. Apply the core protocol first; this profile adds paper-specific source tracing, figure handling, and reconstruction safeguards.

## Paper Trace

For every material implementation, model, numerical, parameter, or validation-target change, record the paper locator that supports it:

- Equation number
- Section or paragraph
- Figure or table
- Caption or quoted phrase from extracted text
- Cited reference, when the paper delegates the detail

Do not make a behavior-changing edit when the Paper Trace is absent. Gather paper evidence first or run a diagnostic that preserves behavior.

## Paper Archaeology

Use the core Source Archaeology procedure, then additionally:

1. Read formulas in their surrounding text, not in isolation.
2. Check notation, subscripts, superscripts, units, normalization, figure captions, table notes, and parameter descriptions.
3. Search cited references only when the paper delegates the missing detail.
4. Mark the detail `Unavailable` when the paper package and delegated sources do not provide it.

Do not turn `Paper Missing` into `Implementation Bug`.

## Figure Calibration

When a paper figure is the only source of a quantitative target:

1. Identify the observable, axes, units, scale type, series, caption, and surrounding context.
2. Digitize it before numerical comparison; do not use visual impression as data.
3. Estimate uncertainty from image quality, axis readability, overlap, annotations, markers, scan distortion, and scale type.
4. Produce reproducible extraction artifacts when possible: script, CSV, data file, reconstructed plot, and QC overlay.
5. Require human QC before the digitized values guide strict regression decisions.
6. Classify the comparison as `Qualitative`, `Semi-quantitative`, `Quantitative`, or `HardGate`.

Default a digitized figure to `PaperCalibration` or `BranchAnchor`, not `HardGate`. Prefer explicit equations or tables when they conflict with a figure, unless the conflict itself is the subject of the investigation.

## Reconstruction Evidence Protocol

Use `[RECONSTRUCTION]` only for a minimal, explicit, testable operational completion of a procedure required by a paper but not fully specified by it. It is not `[SOURCE]` evidence.

Require all conditions:

1. `[SOURCE]` identifies the paper requirement, observable, or algorithmic structure.
2. One operational detail remains `Unavailable` after Paper Archaeology and reasonably accessible delegated-source search.
3. The current goal explicitly authorizes reconstruction; a faithful-implementation or no-guessing goal must stop instead.
4. The completion introduces the fewest degrees of freedom, does not fit a paper target, and changes no unrelated choice.
5. The change is isolated, reversible, opt-in when code is involved, and leaves defaults unchanged unless promotion is separately authorized.
6. The rationale and value or rule are preregistered before observing the target result.
7. Sensitivity is preregistered using at least three bracketing numeric values or the smallest justified set of discrete alternatives.

Record the paper anchor, unavailable detail, minimal completion, rejected alternatives, isolation, sensitivity plan, robustness result, and promotion status. A successful experiment can validate behavior, but cannot prove that the paper author used the reconstructed completion.

Use this evidence chain:

```text
[SOURCE] paper requirement
  -> Unavailable detail
  -> [RECONSTRUCTION] minimal completion
  -> [EXPERIMENT] sensitivity result
```
