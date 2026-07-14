# Reconstruction Evidence Protocol

Use this protocol whenever proposing, implementing, evaluating, or reporting `[RECONSTRUCTION]` evidence.

## Entry Gate

Require all conditions:

1. `[PAPER]` explicitly requires the surrounding procedure, observable, or algorithmic structure.
2. One exact operational detail needed to execute it remains `Unavailable` after Paper Archaeology, including any reasonably accessible delegated source.
3. The current goal explicitly authorizes reconstruction. A faithful-implementation, no-guessing, or missing-specification-stop goal must stop instead.
4. The completion introduces the fewest new degrees of freedom, does not fit a paper target, and changes no unrelated choice.
5. The change is isolated, reversible, opt-in when code is involved, and leaves defaults unchanged unless promotion is separately authorized.
6. The value or rule and rationale are preregistered before observing the target result.
7. Sensitivity is preregistered using at least three bracketing numeric values or the smallest justified set of discrete alternatives.

If any condition is absent, classify the proposal as `[ASSUMPTION]`, not `[RECONSTRUCTION]`.

## Required Record

- `Paper Anchor`: equation, section, figure, table, or quoted requirement.
- `Unavailable Detail`: exact omission and searched locations.
- `Minimal Completion`: selected rule or value.
- `Why Minimal`: unsupported degrees of freedom avoided.
- `Alternatives Rejected`: plausible choices and rejection reasons.
- `Isolation`: opt-in switch, affected files, and unchanged-default proof.
- `Sensitivity Plan`: preregistered alternatives and named observables.
- `Robustness Result`: invariant, quantitatively sensitive, qualitatively sensitive, or unresolved.
- `Promotion Status`: reconstruction-only, experimentally supported reconstruction, or separately authorized implementation convention.

## Result Classification

- `Reconstruction-robust`: the scientific conclusion is unchanged across the preregistered set.
- `Reconstruction-sensitive`: quantitative results change materially but the qualitative conclusion remains stable.
- `Reconstruction-underdetermined`: the scientific conclusion changes across reasonable completions. Stop; the omission remains decision-relevant.

An experiment validates behavior only. It does not prove that the author used the reconstructed completion.

## Example

- `[PAPER]` Section 4.7 requires split relaxation but does not print an inner stopping tolerance.
- `[RECONSTRUCTION]` Use relative subsystem residual reduction `1e-6` centrally, with preregistered sensitivity values `1e-4`, `1e-6`, and `1e-8`.
- `[EXPERIMENT]` Report whether convergence classification and named physical observables are robust across all three values.

The central value is not a paper value. Never choose or revise it after seeing which value best matches the target curve.
