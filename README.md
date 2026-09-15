# dagknight-fiber-probe

## 0. What This Is

`dagknight-fiber-probe` is a diagnostic repository for studying conditional
kernel directions in DAGKnight-style consensus-response computations.

The mathematical object is a response map

```text
R_Pi: M -> O_Pi
ker DR_Pi(x) subset T_x M
```

A kernel direction is therefore a tangent direction in the input state space.
The condition `DR_Pi(x) v = 0` means first-order response invariance. Whether a
finite displacement along such a direction preserves the measured response is a
separate check, and is reported separately below.

The current research claim is narrow: local experiments found a conditional
response kernel related to UMC voting that is not just the timestamp-origin
freedom. This is not yet a mainnet result, a valid-block transformation, or a
full-consensus proof.

## 1. Reproducibility Status

The experiments described here are locally complete, including native Rust
recoloring, but the repository reproducibility package has not yet been
published.

Before the status should be read as externally reproducible, the repository
needs at least:

- runnable scripts, dependencies, and one launch command,
- the frozen response protocol, input data, and data provenance,
- `summary.json`, Jacobian artifacts, kernel bases, and finite-displacement
  results,
- pinned upstream commits, file SHA-256 hashes, and runtime environment notes,
- license files and license notes for any inherited code or data.

Until those materials are committed, the correct status is:

```text
Local experiment complete, including native recoloring; repository reproduction
package pending.
```

## 2. Motivation

DAGKnight computes consensus behavior from structured DAG data, coloring,
parent selection, ordering metadata, blue work, root work, and voting margins.
The diagnostic question is whether a selected numerical response map has
nontrivial tangent directions that leave the response unchanged to first order,
and whether any of those directions also pass finite-displacement preservation
checks under the same fixed conditions.

This repository tests that question in stages:

- first under a simple time-difference response where timestamp-origin freedom
  is expected,
- then under a fixed-coloring UMC conditional response,
- later, under exact `bits` transfer and native conflict-zone recoloring,
- finally, under valid-block constraints and fuller consensus checks.

## 3. Tested Coordinates and Response Map

The currently reported UMC conditional test uses:

- `x`: 92 positive real-valued relaxed work coordinates for participating
  voting blocks,
- unit convention: each coordinate is divided by the frozen original conflict
  root work,
- `R_16`: all recorded per-vote decision margins for six subgroups at `k=16`,
  final scores, total work, and root work,
- fixed inputs: topology, historical parent selection, coloring, ordering
  metadata, and archived context values,
- local validity region: the branch where vote signs and discrete ordering
  choices remain unchanged.

The test is therefore about preservation of numerical margins and related work
coordinates. It is not merely detecting a small flat region in Boolean vote
outputs.

## 4. Stage A: Time-Difference Response and 100-Seed Check

Status: local experiment complete; repository reproduction package pending.

Results:

- `n=32`, `m=62`, `rank=31`, `kernel_dimension=1`
- Kernel direction: common timestamp translation
- `100/100` seeds consistent
- After anchoring the timestamp origin, all `100/100` samples had kernel
  dimension `0`

This stage identifies and then removes the expected timestamp-origin freedom.

## 5. Stage B: Fixed-Coloring UMC Conditional Kernel

Status: local experiment complete; repository reproduction package pending.

Results:

- Python integer replay is consistent with the archived native results:
  `246/246`
- Single baseline context at fixed `k=16`:
  `x=92`, `response=271`, `rank=45`, `kernel=47`
- Three numerical tolerance levels give the same kernel dimension: `47`
- Positive and negative finite displacements along main kernel directions:
  `94/94` passed
- Cross-`k=0..40` margin-preserving sufficient linear-constraint subkernel:
  `6` dimensions
- Positive and negative finite displacements in that subkernel: `12/12` passed
- Independently constructible two-block work-exchange directions inside the
  main kernel: `44`

The precise meaning of the headline numbers is:

| Number | Meaning |
| --- | --- |
| `47` | Dimension of the kernel of the `271 x 92` numerical Jacobian at fixed `k=16` in one baseline context |
| `6` | Dimension of a sufficient linear-constraint subkernel that preserves recorded margins across `k=0..40` colorings |
| `44` | Independent two-block work-exchange directions constructible from equal-coefficient columns inside the main kernel |

The `6`-dimensional subkernel is not a derivative with respect to the integer
parameter `k`. These numbers should not be added. The full `47`-dimensional
main kernel is also not fully explained by two-block exchanges; some directions
involve more general multiblock linear cancellations.

## 6. Stage C: Exact `bits` Transfer and Native Recoloring

Status: complete locally; repository reproduction package pending.

This stage tests exact discrete work transfer and native conflict-zone
recoloring. It should not be described as removing all conditioning: historical
header metadata remains fixed.

Results:

- Native Rust compiled against a pinned rusty-kaspa commit.
- Native replay consistency: `246/246`.
- Kernel candidates preserving the full decision-margin and score response:
  `22/24`.
- Kernel candidates preserving `k=0..40` subgroup decisions: `24/24`.
- Native rank remained `14`: `24/24`.
- Final selected parent remained unchanged: `24/24`.
- Non-kernel controls changed the full response: `2/2`.
- Failure cases: `2/24`, both from negative perturbations along the `p3`
  direction.
  - Perturbation magnitudes: approximately `0.1%` and `1%`.
  - Boundary location: one `k=3` subgroup.
  - Observed score changed from `-42,893,983` to `-43,988,761`.
  - The coloring structure changed.
- Full raw-ordering consistency was not established: baseline reruns also
  showed ordering changes.

Conclusion: the conditional kernel mostly survives native recoloring, but it
has a localized failure boundary in the `p3` direction.

## 6.1 Stage C+: `p3` Boundary Audit

Status: complete locally; repository reproduction package pending.

Results:

- `p3` positive direction: 22 magnitudes, two repeats each, `44/44` preserved
  the full response.
- `p3` negative direction: the same magnitudes, two repeats each, `0/44`
  preserved the full response.
- Non-kernel controls: `0/4` preserved the full response.
- Tested integer-work magnitudes: `1..16`, `32`, `64`, `128`, `256`, `527`,
  and `5274`.
- A negative transfer of only `1` unit of work already triggered a structural
  change.
- In the same `k=3` subgroup, internal parent selection and blue/red membership
  changed, and the score moved from `-42,893,983` to `-43,988,761`.
- Final decision, native rank, and selected parent remained unchanged in all
  cases.

Conclusion: `p3` has a one-sided boundary. Positive perturbations preserve the
full response, while negative perturbations fail immediately. The failure is
triggered by discrete tie-breaking at the reference point, not by a large
magnitude threshold.

## 7. Stage D: Valid-Block Constraints and Full Consensus Check

Status: not started.

This stage would test whether any response-preserving direction corresponds to
a valid block-level transformation under fuller consensus constraints.

## 8. Audit Completion vs Positive Finding

These are separate outcomes.

An audit is complete when:

- the data are frozen and complete,
- scripts and dependencies are runnable by an external reader,
- rank, kernel, and displacement computations are reproducible,
- assumptions and fixed inputs are documented,
- negative results are recorded rather than hidden.

A positive finding occurs only when, under the declared conditions:

- a nonzero kernel is found,
- finite-displacement preservation checks pass,
- the kernel direction has an interpretable coordinate-level explanation.

A zero kernel, or a kernel that fails after recoloring, can still be a
successful diagnostic audit.

## 9. Work-Coordinate Interpretation

In the fixed-coloring UMC conditional kernel, some kernel directions can be
interpreted as transferring work contribution between two blocks while
preserving recorded margins, final scores, total work, and root work.

Under native recoloring, the positive `p3` direction still preserves this
work-coordinate interpretation. The negative `p3` direction triggers a coloring
structure switch and no longer preserves the full response.

Other directions are more general multiblock linear cancellations. The current
evidence establishes a conditional cancellation relation in work coordinates;
it does not establish a physical process, a valid block transformation, or an
effective chain transition.

## 10. Exploration Directions Not Yet Implemented

Critical-scale selection and zero-cost evolution remain exploration directions.
They are not implemented by the current kernel diagnostics, and the existence
of the kernel does not by itself construct `G`, `d_c`, or any downstream
dynamical structure.

## 11. Not Proven

This repository does not currently prove that:

- the result holds on mainnet data,
- the perturbations correspond to valid on-chain blocks,
- the kernel survives after historical header metadata assumptions are fully
  removed; Stage C tested native recoloring, but historical header metadata
  remained fixed,
- the result implies a consensus optimization,
- `G` or `d_c` has been constructed,
- a fiber-bundle feasibility layer is justified,
- the `47`-dimensional kernel remains a `47`-dimensional kernel under the full
  recolored map; that dimension was not recomputed in this round.

These are open boundaries, not hidden assumptions.

## 12. What This Repository Does Not Do

This repository does not:

- replace DAGKnight consensus logic,
- modify rusty-kaspa consensus behavior,
- claim a production optimization,
- claim a live network exploit,
- claim that kernel directions are automatically valid block operations,
- claim that all kernel directions are two-block exchanges.

## 13. Expected Repository Structure

The repository still needs the reproduction package. The expected structure is:

```text
dagknight-fiber-probe/
  README.md
  LICENSE
  requirements.txt
  scripts/
  data/
    frozen_protocol/
    inputs/
    provenance/
  results/
    summary.json
    jacobian/
    kernel_basis/
    finite_displacements/
  docs/
    assumptions.md
    license_notes.md
    reproduction.md
```

## 14. Roadmap

| Stage | Status | Description |
| --- | --- | --- |
| A | Local complete; repo package pending | Time-difference response and 100-seed check |
| B | Local complete; repo package pending | Fixed-coloring UMC conditional kernel |
| C | Local complete; repo package pending | Exact `bits` transfer and native conflict-zone recoloring |
| C+ | Local complete; repo package pending | `p3` boundary audit: positive preserved, negative failed |
| D | Not started | Valid-block constraints and full consensus check |

## 15. One-Sentence Summary

Local diagnostics have found a conditional UMC response kernel beyond
timestamp-origin freedom: in one fixed-coloring baseline context, the `271 x 92`
Jacobian has a `47`-dimensional kernel stable across three numerical
tolerances, including a `6`-dimensional sufficient margin-preserving subkernel
and `44` constructible two-block exchange directions. Under native recoloring,
`22/24` kernel candidates preserved the full response. The `p3` direction has a
one-sided structure: positive perturbations preserved the full response in
`44/44` trials, while negative perturbations preserved it in `0/44` trials, and
a negative transfer of only `1` unit of work already triggered a coloring
structure switch in the `k=3` subgroup. The external reproduction package is
still pending.
