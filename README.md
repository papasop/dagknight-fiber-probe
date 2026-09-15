# dagknight-fiber-probe

## 0. What This Is

`dagknight-fiber-probe` is a diagnostic repository for studying whether
DAGKnight consensus-response computations contain nontrivial kernel directions:
directions in input or response space that preserve the measured consensus
response under a fixed set of constraints.

The current focus is not mainnet performance, block validity, or consensus
optimization. The focus is kernel diagnostics: identify, replay, and interpret
response-preserving directions under controlled conditions.

## 1. Motivation

DAGKnight computes consensus behavior from structured DAG data, coloring,
parent selection, ordering metadata, blue work, root work, and voting margins.
If the response map has a nontrivial kernel, then there may be internal
redistribution directions that leave selected consensus responses unchanged.

This repository tests that possibility in stages:

- first under a simple time-translation kernel,
- then under a fixed-coloring UMC conditional response kernel,
- later, if the evidence survives, under less constrained consensus settings.

## 2. Modification 1: Kernel Diagnostics

### Goal

Detect and explain kernel directions in DAGKnight-style response maps.

### Object

The object of study is a conditional response map derived from native replay
data and structured consensus-response features.

### Logical Layer

This is a diagnostic layer over replayed consensus data. It is not a
replacement for consensus logic, and it does not claim that every kernel
direction corresponds to a valid on-chain block perturbation.

### Deliverables

- reproducible replay checks,
- rank and kernel-dimension reports,
- finite-displacement tests in positive and negative kernel directions,
- attribution of kernel basis directions,
- interpretable exchange directions when available.

### Success Criteria

A diagnostic stage is considered successful only when:

- native replay is consistent,
- rank and kernel dimensions are stable across tolerances,
- finite displacements remain response-preserving,
- kernel directions admit a clear interpretation,
- the limitations of the conditioning assumptions are explicitly stated.

### Current Status

#### Stage 1: Time-Translation Kernel (Complete)

- `n=32`, `m=62`, `rank=31`, `kernel_dimension=1`
- Kernel direction: common timestamp translation
- `100/100` seeds consistent
- Kernel dimension after fixing the time anchor: `0/100`

#### Stage 2: UMC Conditional Response Kernel (Complete)

- Native replay: `246/246` consistent
- Fixed `k=16`: `x=92`, `response=271`, `rank=45`, `kernel=47`
- Three tolerance levels: kernel dimension remains `47`
- Positive and negative finite displacements along main kernel directions:
  `94/94` passed
- Cross-`k=0..40` margin-preserving subkernel: `6` dimensions
- Positive and negative finite displacements in the subkernel: `12/12` passed
- Interpretable two-block exchange directions: `44`

#### Stage 3: Remove Fixed Coloring (Todo)

The next target is to test whether the kernel survives beyond the current
conditioning assumptions.

Current conditioning:

- fixed coloring,
- fixed parent selection,
- fixed ordering metadata.

## 3. Modification 2: Critical Scale Selection

This stage is not complete.

The intended question is whether kernel behavior changes at identifiable
critical scales of DAG structure, voting margin, or selected `k` values.

## 4. Modification 3: Zero-Cost Evolution

This stage is not complete.

The intended question is whether a zero-cost property can be defined for
response-preserving evolution directions, without overstating the result as a
valid chain transition or consensus optimization.

## 5. Logical Dependencies

The current evidence depends on the following sequence:

1. Native replay consistency.
2. Construction of the conditional response matrix.
3. Rank and kernel computation.
4. Finite-displacement validation.
5. Cross-`k` margin-preserving subkernel extraction.
6. Kernel-basis attribution.
7. Interpretation of two-block exchange directions.

Later claims require earlier stages to remain valid when conditioning
assumptions are relaxed.

## 6. Physical Interpretation of Kernel Directions

In the UMC conditional kernel, the observed kernel directions correspond to
redistribution directions that:

- increase the contribution of block A,
- decrease the contribution of block B,
- preserve voting margins,
- preserve final scores,
- preserve total work,
- preserve root work.

Under fixed-coloring conditions, this means there are work-redistribution
directions along which the UMC voting response remains unchanged.

This is a conditional diagnostic result. It is not yet a proof that the same
directions survive in the full unconstrained consensus computation.

## 7. Not Proven

This repository does not currently prove that:

- the result holds on mainnet data,
- the Rust native code has been rerun in the current stage,
- the perturbations correspond to valid on-chain blocks,
- the kernel survives after fixed coloring is removed,
- the result implies a consensus optimization,
- `G` or `d_c` has been constructed.

These are open boundaries, not hidden assumptions.

## 8. What This Repository Does Not Do

This repository does not:

- replace DAGKnight consensus logic,
- modify rusty-kaspa consensus behavior,
- claim a production optimization,
- claim a live network exploit,
- claim that kernel directions are automatically valid block operations,
- construct a full fiber bundle model of DAGKnight consensus.

Terms such as rank-one generator, kernel, image, and `Im B_c` are used in their
linear-algebraic sense.

## 9. Repository Structure

The repository is expected to contain:

- replay scripts and native replay summaries,
- response-matrix construction code,
- rank and kernel diagnostics,
- finite-displacement validation scripts,
- kernel-basis attribution artifacts,
- cross-`k` subkernel reports,
- documentation of assumptions and non-claims.

## 10. Roadmap

| Stage | Status | Description |
| --- | --- | --- |
| 1 | Complete | Time-translation kernel |
| 2 | Complete | 100-seed consistency check |
| 3 | Complete | Kernel-basis attribution |
| 4 | Complete | UMC conditional kernel |
| 5 | Todo | Remove fixed coloring |
| 6 | Todo | Test full consensus response |
| 7 | Waiting | Decide whether a fiber-bundle feasibility layer is justified |

## 11. One-Sentence Summary

Under fixed-coloring UMC conditions, DAGKnight response diagnostics exhibit a
stable `47`-dimensional kernel, including a `6`-dimensional margin-preserving
subkernel and `44` interpretable two-block exchange directions; this is a
controlled conditional result, not yet a mainnet or full-consensus proof.
