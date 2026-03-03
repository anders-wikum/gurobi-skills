# Gurobi Docs URL Map

Base: `https://docs.gurobi.com/projects/optimizer/en/current/`

---

## Concepts

| Topic | URL |
|-------|-----|
| Concepts overview | `concepts/` |
| Modeling Components | `concepts/modeling.html` |
| — Variables | `concepts/modeling/variables.html` |
| — Constraints | `concepts/modeling/constraints.html` |
| — Objectives | `concepts/modeling/objectives.html` |
| — Tolerances & Ill Conditioning | `concepts/modeling/tolerances.html` |
| API Usage | `concepts/apiusage.html` |
| Environments | `concepts/environments.html` |
| Attributes (conceptual) | `concepts/attributes.html` |
| Parameters (conceptual) | `concepts/parameters.html` |
| Logging | `concepts/logging.html` |
| Numerical Issues Guide | `concepts/numericguide.html` |

---

## Features

| Topic | URL |
|-------|-----|
| Features overview | `features/` |
| Batch Optimization | `features/batchoptimization.html` |
| Callbacks | `features/callbacks.html` |
| Concurrent Optimization | `features/concurrent.html` |
| Distributed Optimization | `features/distributed.html` |
| Gurobi Instant Cloud | `features/cloud.html` |
| Infeasibility Analysis (IIS) | `features/infeasibility.html` |
| Multiple Objectives | `features/multiobjective.html` |
| Multiple Scenarios | `features/multiscenario.html` |
| Nonlinear Constraints | `features/nonlinear.html` |
| Parameter Tuning Tool | `features/tuning.html` |
| Solution Pool | `features/solutionpool.html` |

---

## Reference — Language APIs

| Language | URL |
|----------|-----|
| Python (gurobipy) | `reference/python.html` |
| C API | `reference/c.html` |
| C++ API | `reference/cpp.html` |
| Java API | `reference/java.html` |
| .NET API | `reference/dotnet.html` |
| MATLAB API | `reference/matlab.html` |
| R API | `reference/r.html` |

### Python API Classes

All relative to `reference/python/` (e.g., full URL: `https://docs.gurobi.com/projects/optimizer/en/current/reference/python/model.html`):

| Class | URL path | Key methods |
|-------|----------|-------------|
| Model | `model.html` | addVar, addConstr, addMVar, addMConstr, optimize, setParam, getAttr, setAttr, computeIIS, addGenConstr* |
| Var | `var.html` | getAttr, setAttr (X, Obj, LB, UB, VType) |
| MVar | `mvar.html` | Matrix variable API |
| Constr | `constr.html` | getAttr (Pi, Slack, RHS, Sense) |
| MConstr | `mconstr.html` | Matrix constraint |
| QConstr | `qconstr.html` | Quadratic constraint |
| GenConstr | `genconstr.html` | General constraint |
| LinExpr | `linexpr.html` | Linear expression builder |
| QuadExpr | `quadexpr.html` | Quadratic expression |
| NLExpr | `nlexpr.html` | Nonlinear expression (Gurobi 11+) |
| MLinExpr | `mlinexpr.html` | Matrix linear expr |
| MQuadExpr | `mquadexpr.html` | Matrix quadratic expr |
| Env | `env.html` | Environment management |
| Batch | `batch.html` | Batch optimization |
| GRB | `grb.html` | Constants (GRB.OPTIMAL, GRB.BINARY, etc.) |
| tuplelist | `tuplelist.html` | Helper for index sets |
| tupledict | `tupledict.html` | Helper for sparse data |
| Column | `column.html` | Column-wise model building |

---

## Reference — Attributes

Page: `reference/attributes.html`

Link to individual attribute: `reference/attributes.html#{category}{lowercasename}`
(e.g., `#variablex` for the `X` attribute, `#modelobjval` for `ObjVal`)

### Common Attributes by Category

**Model attributes** (read after `model.optimize()`):
- `ObjVal` — objective value of best solution
- `Status` — optimization status (use with `GRB.Status.*`)
- `ObjBound` — best bound (MIP)
- `MIPGap` — relative MIP gap
- `Runtime` — solve time in seconds
- `IterCount` — simplex iteration count
- `NodeCount` — B&B nodes explored
- `SolCount` — number of solutions in pool

**Variable attributes** (read from `Var` object):
- `X` — solution value
- `Obj` — objective coefficient
- `LB`, `UB` — lower/upper bound
- `VType` — variable type (GRB.CONTINUOUS, GRB.BINARY, GRB.INTEGER)
- `RC` — reduced cost
- `Start` — MIP start value

**Constraint attributes** (read from `Constr` object):
- `Pi` — dual value (shadow price)
- `Slack` — constraint slack
- `RHS` — right-hand side
- `Sense` — `<`, `>`, or `=`
- `ConstrName` — name

**Other categories:**
- SOS attributes — `reference/attributes.html` (SOS section)
- Quadratic constraint attributes — QConstr Pi, Slack
- Quality attributes — KappaExact, BoundVio, etc.
- Multi-objective attributes — ObjNVal, ObjNBound, ObjNPriority
- Batch attributes — BatchStatus, BatchID

---

## Reference — Parameters

Page: `reference/parameters.html`

Link to individual parameter: `reference/parameters.html#parameter{lowercasename}`
(e.g., `#parametermipgap` for `MIPGap`, `#parametertimelimit` for `TimeLimit`)

### Common Parameters by Category

**Termination:**
- `TimeLimit` — max solve time (seconds), default Inf
- `BestObjStop` — stop when obj ≤ this value
- `BestBdStop` — stop when bound ≥ this value
- `Cutoff` — discard solutions worse than this
- `SolutionLimit` — stop after N MIP solutions

**MIP quality:**
- `MIPGap` — relative gap tolerance, default 1e-4
- `MIPGapAbs` — absolute gap tolerance
- `IntFeasTol` — integer feasibility tolerance, default 1e-5
- `FeasibilityTol` — primal feasibility tolerance, default 1e-6
- `OptimalityTol` — dual feasibility tolerance, default 1e-6

**Performance:**
- `Threads` — number of threads (0 = auto)
- `Presolve` — presolve level (-1 auto, 0 off, 1 conservative, 2 aggressive)
- `Method` — algorithm (-1 auto, 0 primal simplex, 1 dual simplex, 2 barrier)
- `Heuristics` — fraction of time on MIP heuristics, default 0.05
- `NodeLimit` — max B&B nodes

**Output:**
- `OutputFlag` — 0=silent, 1=verbose (default 1)
- `LogFile` — path to log file
- `LogToConsole` — 0 or 1

**Solution pool:**
- `PoolSolutions` — max solutions to store
- `PoolSearchMode` — 0=keep found, 1=find more, 2=find best N
- `PoolGap` — solutions within this gap of optimal

---

## Reference — Other

| Topic | URL |
|-------|-----|
| Numeric/Status Codes | `reference/numericcodes.html` |
| File Formats | `reference/fileformats.html` |
| Release Notes | `reference/releasenotes.html` |

### Status Codes (GRB.Status)

Defined at `reference/numericcodes.html`:
- `1` = LOADED
- `2` = OPTIMAL
- `3` = INFEASIBLE
- `4` = INF_OR_UNBD`
- `5` = UNBOUNDED
- `6` = CUTOFF
- `7` = ITERATION_LIMIT
- `8` = NODE_LIMIT
- `9` = TIME_LIMIT
- `10` = SOLUTION_LIMIT
- `11` = INTERRUPTED
- `12` = NUMERIC
- `13` = SUBOPTIMAL
- `14` = INPROGRESS
- `15` = USER_OBJ_LIMIT

---

## Version Archive URLs

Replace `current` with version number:
- Gurobi 11.0: `https://docs.gurobi.com/projects/optimizer/en/11.0/`
- Gurobi 12.0: `https://docs.gurobi.com/projects/optimizer/en/12.0/`
- Gurobi 13.0: `https://docs.gurobi.com/projects/optimizer/en/13.0/` (= current)
