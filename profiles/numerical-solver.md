# Numerical Solver Profile

Activate this profile when the active question changes or diagnoses numerical-solver behavior. Apply the core protocol first, then isolate numerical mechanisms.

## Numerical Isolation

After the relevant model, source constraints, boundary conditions, constants, and validation gates are traced or explicitly frozen, change no more than one numerical mechanism in a single experiment.

Examples include candidate selection, merit function, damping schedule, line search, trust region, residual weighting, continuation schedule, and variable scaling or normalization.

If an experiment appears to require coupled numerical changes, first add diagnostics that identify which mechanism selects, rejects, or distorts the candidate. Allow a coupled intervention only after an isolated experiment shows the mechanisms cannot be separated, and record the coupling in the experiment log.

## Numerical Observables

Name the relevant observables, such as residual norms, boundary residuals, convergence classification, conservation error, stability margin, profile shape, or monotonicity under a controlled parameter sweep.

Do not call a numerical change an improvement unless it improves a named observable without an unacceptable regression in a higher-priority observable.
