# Automated Assembly-Level Mitigation of Transient Execution Attacks using Diversified Code Variants and Genetic Search

**Gabriele Giordanelli** — M.Sc. Thesis, Computer Science and Engineering  
Politecnico di Milano, A.Y. 2025–2026

> A paper fully based on this thesis has been submitted to the 39th IEEE International Symposium on Defect and Fault Tolerance in VLSI and Nanotechnology Systems (DTFS 2026) and is currently under review.

---

## Overview

Transient Execution Attacks (TEAs) such as Spectre and Meltdown exploit speculative and out-of-order execution to leak architecturally inaccessible data through microarchitectural side channels. Existing defenses tend to be either too coarse (global barriers with high overhead) or too narrow (manual, attack-specific patches that don't scale).

This thesis presents an **end-to-end compile-time mitigation framework** that performs assembly-to-assembly rewriting of compiler-emitted x86-64 code. The framework selectively detects and rewrites only vulnerable transient windows, replacing each with a set of diversified hardened variants, and optionally uses **genetic search** to automatically identify a single global configuration that balances protection and performance overhead across multiple TEA classes simultaneously.

The framework requires no source-code annotations, no manual assembly patching, and integrates into standard toolchains as a drop-in hardening stage.

---

## Targeted Attack Classes

- **Spectre V1** — bounds-check bypass via control-flow speculation
- **Spectre V4** — speculative store bypass via memory disambiguation
- **Meltdown-like** — faulting-load patterns that transiently forward data before fault handling

---

## Key Contributions

- **Assembly rewriting pipeline** — detects candidate transient windows and rewrites only affected regions, preserving correctness and full toolchain compatibility
- **Variant-based mitigation** — each vulnerable window is replaced by a lightweight runtime selector and *N* semantically equivalent hardened variants, enabling both diversification and transformation composition
- **Composable transformation catalog** — fences and ordering barriers, index masking and pointer sandboxing, dependency injection, and lightweight structural perturbations, each applied conservatively with applicability checks
- **Surrogate-assisted genetic optimizer** — trains lightweight regressors on empirical data to predict the leakage/overhead trade-off of configurations, then searches the mixed discrete/continuous configuration space to output a single global configuration under a user-specified overhead budget or protection target

---

## Selected Experimental Results

All results are reported relative to an unprotected baseline. The table below summarizes the genetic optimizer's global configuration against the best per-attack hand-tuned baselines, under a ~24% average overhead budget.

| Attack | Strategy | Protection (%) | Overhead (%) |
|---|---|---|---|
| Spectre V1 | GA global | 95.74 | 24.02 |
| Spectre V1 | Best hand-picked | 97.68 | 22.63 |
| Spectre V4 | GA global | **95.93** | 24.02 |
| Spectre V4 | Best hand-picked | 87.57 | 29.21 |
| Meltdown-like | GA global | 96.86 | 24.02 |
| Meltdown-like | Best hand-picked | 98.66 | 24.57 |

The GA-selected configuration is **competitive with per-attack hand-tuned baselines on V1 and Meltdown-like**, and **substantially outperforms manual tuning on V4** (the budget-limiting case), while remaining within the overhead budget across all three attack classes simultaneously — something manual per-attack tuning cannot achieve by design.

Key empirical findings:
- Diversification alone is insufficient; robust leakage suppression requires at least one direct mechanism (ordering barriers or data sanitization)
- Under overhead budgets, mixed policies allocate overhead more efficiently than fence-only strategies
- Large *N* with small *k* consistently yields better budgeted trade-offs than aggressive per-variant rewriting

---

## Repository Contents

```
📄 ExecutiveSummary.pdf — 6-page summary covering motivation, method, and results
📄 MasterThesis.pdf     — ≈130-page full master thesis covering the complete transformation catalog, detector design,
                          surrogate model architecture, genetic algorithm implementation details, and extended experimental analysis
```

---

## Future Work

- LLVM integration as a compiler pass
- Broader TEA coverage (BTI/V2-heavy patterns)
- Portability to additional ISAs and assembly dialects
- Validation on realistic workloads and diverse microarchitectures

---

## Author

**Gabriele Giordanelli**  
M.Sc. Computer Science and Engineering — Politecnico di Milano  
[LinkedIn](https://linkedin.com/in/gabrigio30) · [GitHub](https://github.com/gabrigio30)
