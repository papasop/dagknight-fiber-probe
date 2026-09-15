# realizability-fiber

A fiber-bundle realizability framework applied to Kaspa DAGKnight DAG data.

This repository is an independent verification sandbox for a layered
consensus-structure program. It does **not** modify DAGKnight source code.
It only constructs a response map from DAG states, computes its differential,
and tests whether the kernel is nontrivial.

---

## 0. Motivation

DAGKnight infers network delay from k-cluster structure. This inference is a
response map

    R_Pi : M -> O_Pi

where M is the DAG state space and O_Pi is the response space. Any such map
has a kernel ker DR_Pi. Directions in this kernel are invisible to the
response: DAG changes along them do not change the inferred delay.

The fiber-bundle realizability framework treats this kernel as the vertical
subbundle of a response bundle. The three modifications below ask, in order:

1. Does the kernel exist and is it nontrivial?
2. Is there a critical scale at which the response structure becomes rank-one?
3. Does that rank-one branch carry a zero-cost evolution?

---

## 1. Modification 1: Kernel Diagnosis

### Goal

Construct the response map R_Pi from DAG states, compute its differential
DR_Pi, and compute ker DR_Pi.

### Object

    ker DR_Pi != {0}

### Logic Layer

Structural identification. This is an algebraic fact, not an independent
assumption.

### Deliverables

- `dag_state(block, dag)` -> state vector x
- `response_map(x, params)` -> response vector o
- `numerical_jacobian(response_map, x, params)` -> DR_Pi
- `kernel_basis(J, tol)` -> basis of ker DR_Pi, kernel dimension
- Report: kernel dimension distribution over >= 100 blocks

### Success Criterion

    dim ker DR_Pi > 0

on a non-negligible fraction of DAG states.

### Current Status

- Simulated run: n=32, m=62, rank=31, kernel_dimension=1
- tests_passed=16
- anchored_kernel_dimension=0
- Result: PASS

---

## 2. Modification 2: Critical Scale Selection

### Goal

Assuming Modification 1 succeeds, construct a Lorentzian quadratic form G on
the kernel direction and select the critical scale d_c.

### Object

    d_c(G) = alpha * sqrt(-1 / det G)
    D = d_c I

### Logic Layer

Independent selection. This is not a consequence of Modification 1.

### Deliverables

- `build_G_from_kernel(kernel_basis)` -> Lorentzian G
- `critical_scale(G)` -> d_c
- `critical_branch(D, d_c)` -> rank-one generator
- Report: existence and uniqueness of d_c on simulated data

### Success Criterion

    D = d_c I
    rank B_c = 1
    Im B_c subset N(G)

### Current Status

- Not implemented. Waiting for Modification 1 stability.

---

## 3. Modification 3: Zero-Cost Evolution

### Goal

Assuming Modifications 1 and 2 succeed, verify the critical G-null flow.

### Object

    B_c = J_G - d_c I = -2 d_c Pi_-
    rank B_c = 1
    Im B_c subset N(G)

### Logic Layer

Conditional dynamical consequence. Requires Law I, Law II, Law III.

### Deliverables

- `critical_generator(J_G, d_c)` -> B_c
- `null_image(B_c, G)` -> Im B_c, check subset N(G)
- `null_flow(B_c, G, z0, u)` -> trajectory z(tau)
- Report: stability, safety, and zero-cost property

### Success Criterion

    z_dot in Im B_c  =>  z_dot^T G z_dot = 0

along a nonconstant trajectory.

### Current Status

- Not implemented. Depends on Modifications 1 and 2.

---

## 4. Logical Dependencies

    Modification 1  ->  ker DR_Pi
    Modification 2  ->  d_c, D = d_c I
    Modification 3  ->  B_c = -2 d_c Pi_-, Im B_c subset N(G)

    Mod 1 is a prerequisite for Mod 2.
    Mod 2 is a prerequisite for Mod 3.
    Mod 3 cannot be tested directly.

---

## 5. What This Repository Does Not Do

- It does not fork DAGKnight source code.
- It does not modify Kaspa consensus.
- It does not assume G, d_c, or Law I/II/III in Modification 1.
- It does not claim mainnet readiness.
- It does not claim a physical clock, time orientation, or global geometry.

---

## 6. Repository Structure

    realizability-fiber/
    ├── README.md
    ├── notebooks/
    │   └── mod1_kernel_probe.ipynb
    ├── src/
    │   ├── dag_state.py
    │   ├── response_map.py
    │   ├── jacobian.py
    │   └── kernel_basis.py
    ├── results/
    │   ├── kernel_dim_distribution.csv
    │   └── kernel_basis_examples.json
    ├── docs/
    │   ├── fiber_bundle_mapping.md
    │   └── dagknight_response_map.md
    └── LICENSE

---

## 7. Roadmap

| Phase | Action | Status |
|---|---|---|
| 1 | Run Modification 1 on simulated DAG | PASS |
| 2 | Expand to 100+ seeds | TODO |
| 3 | Record kernel basis vectors | TODO |
| 4 | Import simpa DAG output | TODO |
| 5 | Implement Modification 2 | TODO |
| 6 | Implement Modification 3 | TODO |
| 7 | Fork rusty-kaspa dk branch | WAITING |

---

## 8. One-Line Summary

    Mod 1: find the kernel.
    Mod 2: choose the critical scale.
    Mod 3: realize the zero-cost flow.

    Mod 1 is a prerequisite for Mod 2.
    Mod 2 is a prerequisite for Mod 3.
    Mod 3 cannot be tested directly.
