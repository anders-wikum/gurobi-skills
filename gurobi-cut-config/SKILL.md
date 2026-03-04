---
name: gurobi-cut-config
description: Use when the user asks about configuring, enabling, disabling, or tuning Gurobi cutting planes (also called cuts or separators). Covers all Gurobi cut parameters (CliqueCuts, CoverCuts, MIRCuts, etc.), the global Cuts parameter, cut pass controls (CutPasses, CutAggPasses, GomoryPasses), and advice on which cuts to enable or disable for specific problem types. Always use this skill instead of answering from memory — cut parameter behavior is version-specific.
---

# Gurobi Cutting Plane Configuration (Gurobi 13)

## Global Cut Parameters

| Parameter | Values | Default | Purpose |
|-----------|--------|---------|---------|
| `Cuts` | -1, 0, 1, 2, 3 | -1 (auto) | Global cut aggressiveness. 0 = off, 1 = moderate, 2 = aggressive, 3 = very aggressive. Overridden by individual cut params when set. |
| `CutPasses` | -1 to MAXINT | -1 (auto) | Max number of cutting plane passes at the root node. |
| `CutAggPasses` | -1 to MAXINT | -1 (auto) | Max constraint aggregation passes during cut generation. |
| `GomoryPasses` | -1 to MAXINT | -1 (auto) | Max Gomory cut passes. |

## Individual Cut Parameters

Each controls a specific separator. Values: **-1** (auto, default), **0** (disabled), **1** (moderate), **2** (aggressive). Read `references/separators.md` for detailed descriptions, theory, or application-specific advice.

| Cut Name | Parameter | Best For | Comp. Cost |
|----------|-----------|----------|------------|
| Clique | `CliqueCuts` | Set packing, conflict graphs, binary variable conflicts | High |
| Cover | `CoverCuts` | Knapsack constraints, capacity limits, set covering | Low |
| Flow Cover | `FlowCoverCuts` | Fixed-charge networks, single-node flow sets | Low-Moderate |
| Flow Path | `FlowPathCuts` | Fixed-charge path flows, lot-sizing | Low |
| GUB Cover | `GUBCoverCuts` | Generalized upper bound constraints, facility location | Low |
| Implied Bound | `ImpliedCuts` | Binary-continuous implications, on/off decisions | Low |
| Infeasibility Proof | `InfProofCuts` | Large-scale MIP, feasibility-driven models | Moderate-High |
| Lift-and-Project | `LiftProjectCuts` | TSP, set cover/packing, knapsack, binary disjunctions | **High** |
| MIR | `MIRCuts` | General MIP, mixed-integer constraints, knapsack | Low |
| Mixing | `MixingCuts` | Lot-sizing, capacitated facility location, multi-row aggregation | Moderate |
| Mod-k | `ModKCuts` | Scheduling, integer divisibility constraints | Moderate |
| Network | `NetworkCuts` | Supply chain, transportation, network flow problems | Moderate |
| Relax-and-Lift | `RelaxLiftCuts` | Combinatorial optimization, knapsack, VRP | Moderate-High |
| Sub-MIP | `SubMIPCuts` | Large/decomposable MIPs, facility location, scheduling | High |
| Strong CG | `StrongCGCuts` | General MIP, set partitioning, weak LP relaxations | **High** |
| Zero-Half | `ZeroHalfCuts` | Clique partitioning, ATSP, plant location, network matrices | Moderate |
| Proj. Implied Bound | `ProjImpliedCuts` | Network design, production planning, binary-continuous interactions | Low |

## Decision Tree

```
User question about cutting planes
|
+-- "What cuts should I enable/disable for [problem type]?"
|   -> Use the Quick Reference Table above to match problem type to cuts.
|   -> For detailed rationale, read references/separators.md.
|
+-- "What does [CutName] do?" / "Explain [CutName] cuts"
|   -> Read references/separators.md for the full description.
|
+-- "What's the difference between [CutA] and [CutB]?"
    -> Read references/separators.md for both and compare.
```

## Tuning Workflow

When the user has a dataset of instances and wants to find the best cut configuration, first clarify the goal: **faster time to optimality**, or **better gap within a fixed time limit**. Then, write a Python script that:

1. **Defines candidate configs** as a list of dicts mapping parameter names to values. Use the Quick Reference Table to select cuts relevant to the problem type. Always include the default (all -1) as a baseline.
2. **Loops over configs, instances, and seeds.** For each config, solve every instance across multiple random seeds (`model.setParam("Seed", s)`). Record both `model.Runtime` and `model.MIPGap` for every run. Average across seeds per instance.
3. **Outputs the full distribution.** Tabulate per-instance results for each config (a config x instance table). Present the spread, not just a single summary statistic — a config that helps most instances but blows up on one may not be the right choice.

## Docs Reference

For full parameter documentation:
`https://docs.gurobi.com/projects/optimizer/en/current/reference/parameters.html`
