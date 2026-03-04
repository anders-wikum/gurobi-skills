<p align="center">
  <a href="https://gurobi.com" target="_blank" rel="noopener noreferrer">
    <img src="https://www.gurobi.com/wp-content/uploads/2020/12/gurobi-logo.png" height="48">
  </a>
  <br />
</p>
<div align="center">
  <h1>
    Gurobi Skills
  </h1>
  <a href="https://docs.gurobi.com/projects/optimizer/en/current/">
    <img alt="Documentation" src="https://img.shields.io/badge/documentation-gurobi-red.svg" />
  </a>
  <br />
  <br />
  <p>
    <strong>
      Skills to help AI coding agents work more accurately with the Gurobi Optimizer.
    </strong>
  </p>
</div>

---

Skills follow the [Agent Skills](https://agentskills.io/) format.

## Install

```bash
npx add-skill henryrobbins/gurobi-skills
```

### Alternative Installation

```bash
# Manual (Claude Code)
git clone https://github.com/henryrobbins/gurobi-skills ~/.claude/skills/gurobi
```

## Skills

| Skill              | Purpose                                    | When to Use                                               |
| ------------------ | ------------------------------------------ | --------------------------------------------------------- |
| `gurobi-docs`      | Navigate the Gurobi Optimizer docs         | Any question about gurobipy API, parameters, attributes, features, or solver behavior |
| `gurobi-cut-config` | Configure cutting plane separators         | Enabling, disabling, or tuning Gurobi cut parameters for specific problem types |

## Quick Start

### 1. Install Gurobi and gurobipy

Get a license from the [Gurobi website](https://www.gurobi.com/downloads/) and install the Python package:

```bash
pip install gurobipy
```

### 2. Ask Your Agent

| You Say                                                          | What Happens                               |
| ---------------------------------------------------------------- | ------------------------------------------ |
| "How do I add a constraint in gurobipy?"                        | Routes to `reference/python/model.html`    |
| "What does the MIPGap parameter control?"                        | Routes to `reference/parameters.html`      |
| "How do I read the dual values of my LP constraints?"            | Routes to `reference/attributes.html`      |
| "My model status is GRB.INFEASIBLE, how do I find which constraints are causing it?" | Routes to `features/infeasibility.html` |
| "How do I use the solution pool to get the top 5 solutions?"    | Routes to `features/solutionpool.html`     |
| "What changed in the Gurobi 11 Python API?"                      | Routes to `/en/11.0/reference/python.html` |
| "Which cuts should I enable for my knapsack problem?"            | Matches problem type to relevant cutting plane families |
| "Tune cutting plane settings across my dataset of VRP instances" | Writes a script to generate and test cutting plane configurations |

## Repository Structure

```
gurobi-skills/
├── gurobi-docs/
│   ├── SKILL.md              # Navigation decision tree + routing rules
│   └── references/
│       └── url-map.md        # Complete URL index for all doc sections
├── gurobi-cut-config/
│   ├── SKILL.md              # Cut parameter decision tree + quick reference
│   └── references/
│       └── separators.md     # Detailed descriptions of all 17 Gurobi separators
├── evals/
│   └── evals.json            # Test cases for the skill
└── README.md
```

## How the Skills Work

### gurobi-docs

Teaches AI agents to navigate `docs.gurobi.com` accurately instead of relying on training data that may be outdated. Uses a decision tree to route questions to the right section:

- **API methods** (e.g., `addConstr`, `addMVar`, `optimize`) → `reference/python/model.html`
- **Parameters** (e.g., `TimeLimit`, `MIPGap`, `Threads`) → `reference/parameters.html`
- **Attributes** (e.g., `ObjVal`, `X`, `Pi`, `Status`) → `reference/attributes.html`
- **Named features** (callbacks, infeasibility, solution pool, multiple objectives) → `features/`
- **Version-specific questions** → swaps `current` for the version number (e.g., `/en/11.0/`)

### gurobi-cut-config

Provides a quick-reference table and detailed descriptions for all 17 Gurobi cutting plane separators (plus global cut controls). Helps agents recommend which cuts to enable, disable, or tune based on problem structure — e.g., knapsack constraints → Cover/MIR cuts, network flow → Network/FlowCover cuts. Includes common recipes for cut configuration.

## Resources

- [Gurobi Optimizer Docs](https://docs.gurobi.com/projects/optimizer/en/current/)
- [gurobipy on PyPI](https://pypi.org/project/gurobipy/)
- [Gurobi Examples](https://github.com/Gurobi/modeling-examples)
- [Gurobi Community Forum](https://support.gurobi.com/hc/en-us/community/topics)

## License

MIT
