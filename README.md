#### Notation

- Let _A_<sub>m</sub>(d, s, k) be the primary metric (balanced accuracy) of model _m_ on dataset _d_, subject _s_, and random seed _k_.
- Models: **Q** = quantum-hybrid, **C0** = original classical network, **C1** = quantum circuit without entanglement, **C2** = parameter-matched classical layer, **C3** = exact classical simulation of the circuit.
- All models get equal tuning budgets and identical preprocessing, splits, and training code.
- [[Seed-to-seed variation]]: σ<sub>seed</sub>(m) = SD of A<sub>m</sub> across seeds, within a fixed (d, s).
- Contrast: Δ<sub>Q,j</sub> = E[A<sub>Q</sub> − A<sub>j</sub>], the expected accuracy difference between Q and control _j_, for j ∈ {C0, C1, C2, C3}.
- δ = smallest effect size of interest (1.5 percentage points).
- θ = standardized threshold ( Δ / σ<sub>seed</sub> ≥ 0.5), so a difference must exceed run-to-run noise, not just be statistically detectable.

#### Central Question

Does inserting a quantum layer into a standard [[EEG]] decoding network produce a gain in motor imagery classification that (i) exceeds both the [[Glossary/SESOI]] and [[seed-to-seed variation]], (ii) is attributable to the quantum components rather than capacity or classical reproducibility, and (iii) generalizes across subjects and datasets?

#### H1: Existence of the Effect

- **H1<sub>0</sub>:** |Δ<sub>Q,C0</sub>| ≤ δ, or Δ<sub>Q,C0</sub> / σ<sub>seed</sub> < θ.  
    The difference is practically negligible or indistinguishable from seed variation.
- **H1<sub>A</sub>:** Δ<sub>Q,C0</sub> > δ and Δ<sub>Q,C0</sub> / σ<sub>seed</sub> ≥ θ.
- H1<sub>A</sub> is supported if the 95% interval for Δ<sub>Q,C0</sub> lies entirely above δ and the point estimate of Δ<sub>Q,C0</sub> / σ<sub>seed</sub> is at least θ; H1<sub>0</sub> is supported by equivalence if the interval lies entirely within ±δ.

#### H2: Attribution to the Quantum Component

Evaluated only if H1<sub>A</sub> holds. Each sub-hypothesis isolates one alternative explanation.

- **H2a (entanglement):** Δ<sub>Q,C1</sub> > δ.  
    If the gain survives removing entanglement (Δ<sub>Q,C1</sub> ≤ δ), entanglement is not responsible.
- **H2b (capacity):** Δ<sub>Q,C2</sub> > δ.  
    If a parameter-matched classical layer reproduces the gain, the benefit is attributable to capacity, not quantum structure.
- **H2c (classical reproducibility):** |Δ<sub>Q,C3</sub>| ≤ δ (equivalence expected, since C3 computes the same function).  
    Equivalence here means the gain can be reproduced by classical computation of the same function. This does not refute the _functional_ benefit of the circuit, but it rules out any claim that quantum hardware or quantum computation is required.
- **H2 (joint, strong claim):** H2a and H2b hold and Δ<sub>C3,C0</sub> is measured and reported. A claim of _quantum-specific_ benefit requires H2a, H2b, and a stated position on H2c.
- **Multiplicity:** Holm correction across the three contrasts.

##### H3: Consistency

Let _Q_ = 1 for the quantum-hybrid model and 0 for C0. Fit:

A(d, s, k) = β₀ + β₁·Q + u<sub>d</sub> + u<sub>s(d)</sub> + (v<sub>d</sub> + v<sub>s(d)</sub>)·Q + ε

- β₁ is the mean Q − C0 effect.
- u<sub>d</sub>, u<sub>s(d)</sub> are baseline differences in C0 accuracy across datasets and subjects.
- v<sub>d</sub>, v<sub>s(d)</sub> are deviations of the effect across datasets and subjects, with SDs τ<sub>d</sub> and τ<sub>s</sub>.
- **H3<sub>A</sub>:** all of the following hold:
    1. β₁ > δ (95% interval lower bound above δ).
    2. The 95% prediction interval for the effect in a new subject lies above 0.
    3. The effect is positive in at least 70% of subjects and in every dataset.
    4. Leave-one-dataset-out and leave-out-most-extreme-subjects refits still satisfy 1 to 3.
- **H3<sub>0</sub>:** otherwise. The effect may exist on average, but it is not consistent enough to claim it generalizes.
- τ<sub>d</sub> and τ<sub>s</sub> are reported with intervals. The prediction interval is what tests heterogeneity.
- Moderator analyses (baseline accuracy, trial count, channel count) are exploratory unless pre-registered.

#### Decision Rules

| H1 outcome                          | H2                                       | H3             | Conclusion                                                                                                                      |
| ----------------------------------- | ---------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Equivalent to C0                    | not evaluated                            | not evaluated  | No meaningful effect (a true null, not merely absence of evidence)                                                              |
| Q worse than C0 (by more than δ)    | not evaluated                            | not evaluated  | Quantum layer hurts performance                                                                                                 |
| Superior but Δ/σ<sub>seed</sub> < θ | not evaluated                            | not evaluated  | Gain exceeds the SESOI but not run-to-run noise; not established                                                                |
| Inconclusive                        | not evaluated                            | not evaluated  | Insufficient evidence either way                                                                                                |
| H1<sub>A</sub>                      | H2b refuted (H2a any)                    | not evaluated  | Gain reflects added capacity, not quantum structure                                                                             |
| H1<sub>A</sub>                      | H2b holds, H2a refuted                   | not evaluated  | Gain reflects circuit structure, not entanglement                                                                               |
| H1<sub>A</sub>                      | H2a or H2b inconclusive                  | not evaluated  | Source of gain unresolved                                                                                                       |
| H1<sub>A</sub>                      | H2a, H2b hold, H2c holds                 | H3<sub>A</sub> | Consistent, entanglement-dependent benefit, but classically reproducible (no quantum hardware claim)                            |
| H1<sub>A</sub>                      | H2a, H2b hold, H2c holds                 | H3<sub>0</sub> | Conditional, entanglement-dependent benefit, classically reproducible; identify moderators                                      |
| H1<sub>A</sub>                      | H2a, H2b hold, H2c fails or inconclusive | any            | Unexpected, since C3 computes the same function as Q. Audit the implementation (and shot or noise settings) before interpreting |
