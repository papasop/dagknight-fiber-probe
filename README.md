# dagknight-fiber-probe

## 0. What This Is

dagknight-fiber-probe is a diagnostic repository for studying conditional kernel
directions in DAGKnight-style consensus-response computations.

The mathematical object is a response map

    R_Pi: M -> O_Pi
    ker DR_Pi(x) subset T_x M

This response map can be read as a fiber-bundle projection: M is the base,
O_Pi is the response space, and ker DR_Pi is the vertical subbundle. A kernel
direction is therefore a tangent direction in the input state space.

The condition DR_Pi(x) v = 0 means first-order response invariance. Whether a
finite displacement along such a direction preserves the measured response is a
separate check, and is reported separately below.

The current research claim is narrow: local experiments found a conditional
response kernel related to UMC voting that is not just the timestamp-origin
freedom. This is not yet a mainnet result, a valid-block transformation, or a
full-consensus proof.

## 1. Current Status

The conditional UMC kernel exists locally, is directional under native
recoloring, is eliminated by cross-k constraints in new DAG samples, and has a
parent-selection boundary that can be predicted by a frozen work inequality;
G and d_c have not been constructed.

## 2. How to Read This Repository

This README is organized by question rather than only by time order.

### 2.1 If you want to know whether the kernel exists

Read §4 and §5.

### 2.2 If you want to know whether the kernel survives native recoloring

Read §6 and §6.1.

### 2.3 If you want to know whether parent selection can be predicted

Read §6.2.

### 2.4 If you want to know whether the kernel survives across k

Read §6.3, §6.4, §6.5, and §6.6.

### 2.5 If you want to know what is not proven

Read §11.1 and §11.2.

## 3. Reproducibility Status

The experiments described here are locally complete, including native Rust
recoloring, but the repository reproduction package has not yet been published.

Before the status should be read as externally reproducible, the repository
needs at least:

- runnable scripts, dependencies, and one launch command,
- the frozen response protocol, input data, and data provenance,
- summary.json, Jacobian artifacts, kernel bases, and finite-displacement
  results,
- pinned upstream commits, file SHA-256 hashes, and runtime environment notes,
- license files and license notes for any inherited code or data.

Until those materials are committed, the correct status is:

    Local experiment complete, including native recoloring;
    repository reproduction package pending.

## 4. Stage A: Time-Difference Response and 100-Seed Check

Status: local experiment complete; repository reproduction package pending.

Results:

- n=32, m=62, rank=31, kernel_dimension=1
- Kernel direction: common timestamp translation
- 100/100 seeds consistent
- After anchoring the timestamp origin, all 100/100 samples had kernel
  dimension 0

This stage identifies and then removes the expected timestamp-origin freedom.

## 5. Stage B: Fixed-Coloring UMC Conditional Kernel

Status: local experiment complete; repository reproduction package pending.

Tested coordinates and response map:

- x: 92 positive real-valued relaxed work coordinates for participating voting
  blocks,
- unit convention: each coordinate is divided by the frozen original conflict
  root work,
- R_16: all recorded per-vote decision margins for six subgroups at k=16, final
  scores, total work, and root work,
- fixed inputs: topology, historical parent selection, coloring, ordering
  metadata, and archived context values,
- local validity region: the branch where vote signs and discrete ordering
  choices remain unchanged.

The test is therefore about preservation of numerical margins and related work
coordinates. It is not merely detecting a small flat region in Boolean vote
outputs.

Results:

- Python integer replay is consistent with the archived native results:
  246/246
- Single baseline context at fixed k=16: x=92, response=271, rank=45,
  kernel=47
- Three numerical tolerance levels give the same kernel dimension: 47
- Positive and negative finite displacements along main kernel directions:
  94/94 passed
- Cross-k=0..40 margin-preserving sufficient linear-constraint subkernel:
  6 dimensions
- Positive and negative finite displacements in that subkernel: 12/12 passed
- Independently constructible two-block work-exchange directions inside the
  main kernel: 44

The precise meaning of the headline numbers is:

| Number | Meaning |
|--------|---------|
| 47 | Dimension of the kernel of the 271 x 92 numerical Jacobian at fixed k=16 in one baseline context |
| 6 | Dimension of a sufficient linear-constraint subkernel that preserves recorded margins across k=0..40 colorings |
| 44 | Independent two-block work-exchange directions constructible from equal-coefficient columns inside the main kernel |

The 6-dimensional subkernel is not a derivative with respect to the integer
parameter k. These numbers should not be added. The full 47-dimensional main
kernel is also not fully explained by two-block exchanges; some directions
involve more general multiblock linear cancellations.

## 6. Stage C: Exact Bits Transfer and Native Recoloring

Status: complete locally; repository reproduction package pending.

This stage tests exact discrete work transfer and native conflict-zone
recoloring. It should not be described as removing all conditioning: historical
header metadata remains fixed.

Results:

- Native Rust compiled against a pinned rusty-kaspa commit.
- Native replay consistency: 246/246.
- Kernel candidates preserving the full decision-margin and score response:
  22/24.
- Kernel candidates preserving k=0..40 subgroup decisions: 24/24.
- Native rank remained 14: 24/24.
- Final selected parent remained unchanged: 24/24.
- Non-kernel controls changed the full response: 2/2.
- Failure cases: 2/24, both from negative perturbations along the p3 direction.
- Perturbation magnitudes: approximately 0.1% and 1%.
- Boundary location: one k=3 subgroup.
- Observed score changed from -42,893,983 to -43,988,761.
- The coloring structure changed.
- Full raw-ordering consistency was not established: baseline reruns also
  showed ordering changes.

Conclusion: the conditional kernel mostly survives native recoloring, but it
has a localized failure boundary in the p3 direction. See §6.1 for the boundary
audit and §6.2 for the branch-preservation certificate.

## 6.1 Stage C+: p3 Boundary Audit

Status: complete locally; repository reproduction package pending.

Results:

- p3 positive direction: 22 magnitudes, two repeats each, 44/44 preserved the
  full response.
- p3 negative direction: the same magnitudes, two repeats each, 0/44 preserved
  the full response.
- Non-kernel controls: 0/4 preserved the full response.
- Tested integer-work magnitudes: 1..16, 32, 64, 128, 256, 527, and 5274.
- A negative transfer of only 1 unit of work already triggered a structural
  change.
- In the same k=3 subgroup, internal parent selection and blue/red membership
  changed, and the score moved from -42,893,983 to -43,988,761.
- Final decision, native rank, and selected parent remained unchanged in all
  cases.

Conclusion: p3 has a one-sided boundary. Positive perturbations preserve the
full response, while negative perturbations fail immediately. The one-sidedness
comes from the reference point lying exactly on a tie boundary: positive
perturbations keep the original winner group ahead, while negative
perturbations let the other group win by hash order. The failure is triggered
by discrete tie-breaking at the reference point, not by a large magnitude
threshold.

## 6.2 Stage V6-r1: Branch-Preservation Certificate

Status: complete locally; repository reproduction package pending.

Results:

- 11 inputs and 33 native runs.
- Work formulas for 4 candidate parents: all matched exactly.
- Parent-selection predictions for the following 10 inputs: all correct.
- No observed response or coloring effect from diagnostic switches.
- False positives where "branch condition + frozen UMC response preservation"
  incorrectly predicted preservation: 0.

V5 mechanism result: the reference point has four tied candidate parents. The
perturbation splits them into two groups, and the winner inside each group is
then determined by hash order.

Core V6-r1 certificate: when the current candidate set, work formulas, and hash
ordering are preserved, the condition for the original parent ecc1... to keep
winning reduces to the single inequality

    w_7a9c... >= w_9aa8...

In the original state, both work values are 527,474, exactly on the equality
boundary.

| Input          | Work difference | Branch prediction          | Native full response |
|----------------|-----------------|----------------------------|----------------------|
| Original state | 0               | original parent preserved  | baseline             |
| p3 +31         | +31             | original parent preserved  | preserved            |
| p3 -31         | -31             | parent switches            | changed              |

Conclusion: finite preservation along the kernel direction can be predicted in
advance by the parent-selection inequality. With the candidate set, work
formulas, and hash order fixed, the remaining degree of freedom is the work
difference between two candidates; hence the branch condition reduces to a
single inequality. The formula comes from native work accumulation relations
and was then frozen for prediction; it was not fitted to the later outcomes.

Boundaries:

- This is a conditional local parent-selection certificate.
- Full-response preservation after combining the certificate with the UMC
  kernel has only been validated on these samples so far.
- The formula's applicability is still checked by native recomputation, so it
  has not yet saved computation.
- The kernel dimension of the full consensus map has not been established.

## 6.3 Stage V7: Cross-k Rank Growth Audit

Status: complete locally; repository reproduction package pending.

Results on three new DAGs:

| New DAG  | State dim | Fixed k=16 kernel dim | Cross-k=0..40 sufficient-constraint kernel dim |
|----------|-----------|------------------------|------------------------------------------------|
| 66 blocks| 15        | 5                      | 0                                              |
| 64 blocks| 35        | 6                      | 0                                              |
| 67 blocks| 20        | 6                      | 0                                              |

Conclusion: fixed-k conditional kernels still appear in all three new DAGs, but
cross-k shared nonzero directions did not reproduce. The sufficient-constraint
matrices are full column rank, with minimum singular values well above
tolerance. This negative result is reported as `CROSS_DAG_INCONCLUSIVE`.

## 6.4 Stage V8: Which k Constraints Eliminate the Kernel

Status: complete locally; repository reproduction package pending.

Using exact rational elimination, not floating-point tolerance.

| Seed       | k=16 kernel dim | Kernel becomes 0 after adding k in |
|------------|-----------------|-------------------------------------|
| 2026091601 | 5               | {1, 16}                             |
| 2026091602 | 6               | {1, 2, 3, 16}                       |
| 2026091603 | 6               | {1, 2, 16}                          |

Interpretation: changes invisible at a single k can be distinguished by the
response constraints of other k. The eliminating sets are not guaranteed
minimal, and the conclusion is limited to the declared coordinates and frozen
branch constraints.

## 6.5 Stage V8+: Which k Is Not Replaceable

Correction to V8: k=1 is not non-replaceable in all three samples.

| Seed       | After removing k=1 from all k=0..40 constraints | Non-replaceable? |
|------------|--------------------------------------------------|------------------|
| 2026091601 | kernel dimension still 0                         | No               |
| 2026091602 | kernel dimension becomes 1                       | Yes              |
| 2026091603 | kernel dimension still 0                         | No               |

Second sample mechanism: blocks `6c62a0...` (A) and `787bc1...` (B) are both
blue at k=1, but their vote signs are +1 and -1 respectively. The two-block
transfer

    delta w_A = -eps,  delta w_B = +eps

keeps total work unchanged, satisfies all other frozen-k constraints, but
changes w_A - w_B at k=1, eliminating the last kernel dimension. At k=2 and
k=16, both blocks vote in the same direction, so the transfer cancels in the
relevant sums. Different k values have different vote-discrimination power;
this does not give k=1 universal critical meaning or establish a link to the
paper's K=1.

## 6.6 Stage V9: Native k=1 Mechanism Verification

Status: complete locally; repository reproduction package pending.

Results:

| Case              | Changed k values |
|-------------------|------------------|
| target_plus1      | [1]              |
| target_minus1     | [1]              |
| target_plus31     | [1]              |
| target_minus31    | [1]              |
| target_plus527    | [1]              |
| target_minus527   | [1]              |
| control_plus31    | [0..40]          |
| control_minus31   | [0..40]          |
| sham_plus0        | []               |

STATUS: NATIVE_K1_MECHANISM_SUPPORTED_TESTED_CASES

Conclusion: the target transfer changes only k=1, not other k. The control
transfer changes all k. The sham changes nothing. The k=1 mechanism from V8+ is
supported under native recoloring on the tested cases.

## 7. Stage D: Valid-Block Constraints and Full Consensus Check

Status: not started.

This stage would test whether any response-preserving direction corresponds to a
valid block-level transformation under fuller consensus constraints.

Stage D is intentionally not started. It requires the reproduction package and
a stable branch-preservation certificate on multiple DAG samples.

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

Under native recoloring, the positive p3 direction still preserves this
work-coordinate interpretation. The negative p3 direction triggers a coloring
structure switch and no longer preserves the full response.

Other directions are more general multiblock linear cancellations. The current
evidence establishes a conditional cancellation relation in work coordinates;
it does not establish a physical process, a valid block transformation, or an
effective chain transition.

## 10. Exploration Directions Not Yet Implemented

Critical-scale selection and zero-cost evolution remain exploration directions.
They are not implemented by the current kernel diagnostics, and the existence
of the kernel does not by itself construct G, d_c, or any downstream dynamical
structure.

## 11. Boundaries

### 11.1 Not Proven

This repository does not currently prove that:

- the result holds on mainnet data,
- the perturbations correspond to valid on-chain blocks,
- the kernel survives after historical header metadata assumptions are fully
  removed; Stage C tested native recoloring, but historical header metadata
  remained fixed; this is a known boundary, not a hidden assumption,
- the result implies a consensus optimization,
- G or d_c has been constructed,
- a fiber-bundle realizability layer is justified,
- the 47-dimensional kernel remains a 47-dimensional kernel under the full
  recolored map; that dimension was not recomputed in this round,
- the number k=1 has universal critical meaning; V8+ and V9 show it is sample-
  specific.

These are open boundaries, not hidden assumptions.

### 11.2 What This Repository Does Not Do

This repository does not:

- replace DAGKnight consensus logic,
- modify rusty-kaspa consensus behavior,
- claim a production optimization,
- claim a live network exploit,
- claim that kernel directions are automatically valid block operations,
- claim that all kernel directions are two-block exchanges.

## 12. Expected Repository Structure

The repository still needs the reproduction package. The expected structure is:

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

`docs/assumptions.md` should list all fixed inputs and conditional assumptions,
including topology, historical parent selection, coloring, and ordering
metadata.

## 13. Roadmap

| Stage | Status                            | Description |
|-------|-----------------------------------|-------------|
| A     | Local complete; repo package pending | Time-difference response and 100-seed check |
| B     | Local complete; repo package pending | Fixed-coloring UMC conditional kernel |
| C     | Local complete; repo package pending | Exact bits transfer and native conflict-zone recoloring |
| C+    | Local complete; repo package pending | p3 boundary audit: positive preserved, negative failed |
| V5    | Local complete; repo package pending | p3 one-sided mechanism: tie plus hash tie-break |
| V6-r1 | Local complete; repo package pending | Branch-preservation certificate: one inequality predicts parent selection |
| V7    | Local complete; repo package pending | Cross-k rank growth audit: cross-k kernel did not reproduce |
| V8    | Local complete; repo package pending | Which k constraints eliminate the kernel |
| V8+   | Local complete; repo package pending | k=1 not non-replaceable in all samples; vote-sign mechanism |
| V9    | Local complete; repo package pending | Native k=1 mechanism verification: target changes only k=1 |
| D     | Not started                       | Valid-block constraints and full consensus check |

## 14. Summary

### 14.1 One-Paragraph Summary

Local diagnostics have found a conditional UMC response kernel beyond
timestamp-origin freedom: in one fixed-coloring baseline context, the 271 x 92
Jacobian has a 47-dimensional kernel stable across three numerical tolerances,
including a 6-dimensional sufficient margin-preserving subkernel and 44
constructible two-block exchange directions. Under native recoloring, 22/24
kernel candidates preserved the full response, while the p3 direction has a
one-sided structure: positive perturbations preserved the full response in
44/44 trials, negative perturbations preserved it in 0/44 trials, and a
negative transfer of only 1 unit of work already triggered a coloring structure
switch in the k=3 subgroup. Cross-k shared directions did not reproduce on
three new DAGs, and the k=1 mechanism is sample-specific rather than universal.
The external reproduction package is still pending.

### 14.2 Detailed Summary

V5 identified the p3 mechanism: four candidate parents are tied at the
reference point, the perturbation splits them into two groups, and hash order
decides the winner inside each group. V6-r1 reduces the preserved branch
condition to one checkable inequality,

    w_7a9c... >= w_9aa8...

where the original state has both values tied at 527,474, p3 +31 preserves the
parent, and p3 -31 switches it. The next 10 parent-selection predictions were
all correct, with 0 false preservation predictions. V7 showed that the cross-k
kernel does not reproduce on three new DAGs; V8 located which k constraints
eliminate the kernel; V8+ showed that k=1 is not non-replaceable in all samples,
but has a vote-sign mechanism in the second sample; V9 verified the k=1
mechanism under native recoloring, with target transfers changing only k=1,
controls changing all k, and sham changing nothing. The external reproduction
package is still pending.
