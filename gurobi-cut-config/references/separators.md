# Gurobi Cutting Plane Separators — Full Reference

Each separator below corresponds to a Gurobi parameter that controls cut generation. All parameters accept values: **-1** (auto, default), **0** (off), **1** (moderate), **2** (aggressive).

Set via `model.setParam("<ParamName>", value)` in gurobipy.

---

## Clique Cuts

**Parameter:** `CliqueCuts`

**What They Are:**
Clique cutting planes are valid inequalities derived from the set-packing formulation, particularly useful in problems involving binary variables. The inequalities are based on identifying cliques in a conflict graph representation of the problem. A clique is a subset of mutually adjacent vertices in a graph, representing a set of constraints that cannot be simultaneously satisfied. The corresponding cutting planes exclude infeasible solutions by enforcing that only one element from each clique can be selected.

**Constraints/Structures They Operate On:**
Applicable in problems where the constraints can be modeled using a conflict graph. This is common in set-packing problems, where the constraints require selecting a subset of non-overlapping items. The structure needs to allow for the identification of cliques in the graph, making the method particularly suitable for 0-1 integer programming problems where constraints are naturally binary.

**Applications:**
1. **Set-Packing Problems** — selecting a subset of items such that no pair of selected items conflict.
2. **Graph-based Problems** — such as the maximum independent set problem, finding the largest set of non-adjacent vertices.
3. **General MIP** — any scenario where constraints can be represented through a conflict graph.

**Computational Demand:**
Can be demanding because finding cliques in a graph is combinatorial. While small cliques are feasible, larger or dense graphs make generation expensive. Often incorporated into branch-and-cut algorithms where their strength in tightening the LP relaxation justifies the cost.

---

## Cover Cuts

**Parameter:** `CoverCuts`

**What They Are:**
Cover cutting planes are valid inequalities derived from knapsack problems, specifically when the feasible set includes a cover. A cover is a subset of variables whose total exceeds the knapsack's capacity, meaning not all variables in this subset can be selected simultaneously. The cover inequality tightens the linear relaxation by forbidding these infeasible combinations.

**Constraints/Structures They Operate On:**
Apply primarily to 0-1 knapsack constraints. The constraints involve binary variables representing decisions (e.g., whether to include an item) and a capacity constraint. The cut identifies a subset of items (a cover) that collectively violate the constraint and formulates an inequality to exclude this infeasible combination.

**Applications:**
1. **Knapsack Problems** — resource allocation, logistics, and combinatorial optimization.
2. **Set Covering Problems** — covering all elements of a set with the fewest subsets.
3. **General MIP** — especially when the problem has knapsack-like constraints.

**Computational Demand:**
Relatively efficient. The process typically involves identifying covers using greedy algorithms or heuristics. Lifting procedures to strengthen cuts can increase complexity, but basic generation is not computationally intensive.

---

## Flow Cover Cuts

**Parameter:** `FlowCoverCuts`

**What They Are:**
Flow cover cuts are valid inequalities used to tighten the linear relaxations of MIP problems, particularly for binary single-node flow sets. They are useful in network design and fixed charge problems, where variables represent flows subject to upper bounds and binary decisions.

**Constraints/Structures They Operate On:**
Operate on constraints involving continuous flow variables and binary variables. Applied to problems like the fixed-charge network design problem and mixed-integer flow problems.

**Applications:**
1. **Fixed-charge network design** — where opening a route incurs a fixed cost.
2. **Mixed-integer flow problems** — combining binary activation decisions with continuous flows.
3. **Single-node flow sets** — problems reducible to flow through a single node with binary controls.

**Computational Demand:**
Finding the most violated flow cover cut can be done through heuristic algorithms such as solving a knapsack problem, which is relatively efficient but requires careful implementation for scalability in large MIP problems.

---

## Flow Path Cuts

**Parameter:** `FlowPathCuts`

**What They Are:**
Flow path cuts are valid inequalities that strengthen the linear relaxation of MIPs for problems involving fixed charge networks. They help model flow through a sequence of nodes where fixed charges are incurred if any flow occurs along a path.

**Constraints/Structures They Operate On:**
Operate on constraints related to fixed charge paths, where binary and continuous variables govern flow through a network.

**Applications:**
1. **Fixed charge network design** — path-based flow with activation costs.
2. **Lot-sizing problems** — production planning with setup costs across time periods.
3. **Multi-echelon supply chains** — flow through sequential stages with fixed costs.

**Computational Demand:**
The structure exploited by these cuts is very specific, meaning they are only applicable to certain problem types. When applicable, they are efficient to generate.

---

## GUB Cover Cuts

**Parameter:** `GUBCoverCuts`

**What They Are:**
Generalized Upper Bound (GUB) cover cutting planes are derived from GUB constraints. A GUB constraint limits the sum of a specific subset of variables to an upper bound. The GUB cover cut identifies subsets of variables (covers) that, if all selected, would exceed the GUB limit, then excludes such infeasible combinations.

**Constraints/Structures They Operate On:**
Operate on constraints involving a generalized upper bound on a group of variables. These appear in problems where there is a limit on the total number of items selected from a group — e.g., facility location or resource allocation problems.

**Applications:**
1. **Facility Location Problems** — choosing a subset of locations within a capacity limit.
2. **Resource Allocation Problems** — distributing resources among competing demands under an upper bound.
3. **General MIP** — wherever GUB constraints appear.

**Computational Demand:**
Relatively efficient. The process involves identifying covers and creating inequalities. Complexity can increase with lifting procedures, but basic GUB cover cuts are computationally manageable.

---

## Implied Bound Cuts

**Parameter:** `ImpliedCuts`

**What They Are:**
Implied bound cuts leverage implications between binary and continuous variables to restrict the feasible region. They enforce tighter constraints by exploiting logical relationships, such as when a binary variable limits the upper or lower bound of a continuous variable.

**Constraints/Structures They Operate On:**
Used in problems where there are logical implications between binary and continuous variables. For example, if a binary variable is 0, it may impose an upper bound on a continuous variable, and if it is 1, it may relax that bound.

**Applications:**
1. **Network design** — binary on/off decisions affecting flow capacities.
2. **Facility location** — open/close decisions implying bounds on demand or capacity.
3. **Production planning** — machine on/off decisions affecting production levels.

**Computational Demand:**
Relatively lightweight. They exploit simple logical conditions, making them computationally efficient. While not as impactful as more complex cuts, they contribute significantly in models with clear binary-continuous variable interactions.

---

## Infeasibility Proof Cuts

**Parameter:** `InfProofCuts`

**What They Are:**
InfProof cutting planes improve the solver's ability to handle infeasibility by generating cuts that provide proof of infeasibility for certain fractional solutions. They tighten the feasible region by eliminating solutions proven to be infeasible.

**Constraints/Structures They Operate On:**
Based on generating valid inequalities that demonstrate infeasibility in specific fractional solutions. These proofs result from analyzing the problem structure and identifying regions where no feasible integer solution can exist, even though the relaxed solution may suggest otherwise.

**Applications:**
1. **Large-scale MIP** — where infeasibility arises in large, complex solution spaces.
2. **Feasibility-driven models** — scheduling or network flow problems where proving infeasibility saves significant computation time.
3. **Complex combinatorial problems** — where infeasibility is common in fractional solutions.

**Computational Demand:**
Requires identifying infeasible fractional solutions and proving they are not part of the integer feasible region. The additional cost can be offset by drastically reduced search space and faster convergence for difficult MIP problems.

---

## Lift-and-Project Cuts

**Parameter:** `LiftProjectCuts`

**What They Are:**
Lift-and-project cuts operate by lifting the problem into a higher-dimensional space, creating stronger constraints, and then projecting them back onto the original space, generating cuts that eliminate fractional solutions.

**Constraints/Structures They Operate On:**
Work on disjunctions, particularly involving binary (0-1) variables. By lifting the constraints formed by binary variables and projecting them, the method eliminates fractional solutions in the LP relaxation. Applies to polyhedral representations and mixed-integer sets.

**Applications:**
1. **Traveling salesperson problems** — eliminating fractional subtour solutions.
2. **Set covering and packing** — enforcing integrality in selection problems.
3. **Knapsack problems** — strengthening LP relaxations for capacity constraints.

**Computational Demand:**
**Expensive.** Requires solving an additional LP about double the size of the original LP relaxation. Although they significantly tighten relaxations, the high cost means they are typically applied selectively.

---

## Mixed Integer Rounding (MIR) Cuts

**Parameter:** `MIRCuts`

**What They Are:**
MIR cuts are derived from mixed-integer sets, particularly constraints with both continuous and integer variables. They generate valid inequalities by rounding coefficients to tighten the LP relaxation, using a disjunctive argument to separate fractional solutions from the feasible region.

**Constraints/Structures They Operate On:**
Operate on mixed-integer linear constraints of the form a^T x + y <= b, where x represents integer variables and y continuous variables. Particularly effective for problems with a mix of integer and continuous variables. Can be adapted to knapsack sets.

**Applications:**
1. **Knapsack Problems** — tighter constraints for 0-1 and mixed knapsack problems.
2. **General MIP** — improving the linear relaxation across a wide range of formulations.
3. **Logistics and Supply Chain** — mixed constraints on resources, capacities, and logistics decisions.

**Computational Demand:**
Computationally efficient. Provides a good balance between cut strength and generation effort. Often generated from basic properties of mixed-integer constraints without needing complex separation algorithms.

---

## Mixing Cuts

**Parameter:** `MixingCuts`

**What They Are:**
Mixing cutting planes originate from the mixing set, a generalization of the MIR procedure. They consider multiple constraint rows simultaneously to produce stronger cuts than single-row methods.

**Constraints/Structures They Operate On:**
Applied when multiple constraints can be aggregated to form a stronger cut. They generalize MIR cuts by considering several rows at once, producing inequalities of high MIR rank.

**Applications:**
1. **Lot sizing problems** — inventory or production decisions over time.
2. **Capacitated facility location** — optimizing facility locations to minimize costs.
3. **Capacitated network design** — optimizing capacity and flow in networks.

**Computational Demand:**
More complex than single-row MIR cuts due to multi-row aggregation, but yields stronger cuts. Computational studies show moderate impact — disabling mixing cuts increases solve time by ~1.7% for hard models.

---

## Mod-k Cuts

**Parameter:** `ModKCuts`

**What They Are:**
Mod-k cuts are derived from modular arithmetic principles, leveraging divisibility and congruence properties to eliminate infeasible solutions.

**Constraints/Structures They Operate On:**
Focus on integer variables and apply congruence relations (mod k) that must be satisfied for integer feasibility. They eliminate non-integral solutions that violate specific divisibility rules, tightening the integer solution space.

**Applications:**
1. **Scheduling problems** — where time slots or resources must be allocated in integer units.
2. **Knapsack problems** — ensuring items fit within capacity without fractional quantities.
3. **Integer-based models** — where divisibility conditions are essential, such as production lot sizes or team allocation.

**Computational Demand:**
Requires identifying divisibility patterns in integer variables, which can add overhead. Generally moderate impact, most beneficial in highly integer-constrained models where divisibility rules are frequently violated in relaxed solutions.

---

## Network Cuts

**Parameter:** `NetworkCuts`

**What They Are:**
Network cuts leverage network flow structures within MIP models to improve convergence toward integer solutions.

**Constraints/Structures They Operate On:**
Identify flow-like structures where variables represent flows between nodes (e.g., supply chain, logistics). Use properties such as conservation of flow, demand fulfillment, and capacity constraints.

**Applications:**
1. **Supply chain optimization** — ensuring feasible flows meeting production, transport, and demand constraints.
2. **Transportation and logistics** — managing routing and capacity in a network.
3. **Capacitated network design** — optimizing flows in infrastructure networks like telecommunications or electricity grids.

**Computational Demand:**
Gurobi automatically detects flow-based substructures. While detection adds complexity, it often reduces solve time significantly by removing infeasible solutions early. Particularly effective in problems with prominent flow structures.

---

## Relax-and-Lift Cuts

**Parameter:** `RelaxLiftCuts`

**What They Are:**
Relax-and-lift cuts are based on relaxing certain constraints to make the problem easier, then "lifting" the relaxed solution back into stronger inequalities that tighten the feasible region.

**Constraints/Structures They Operate On:**
Generated by relaxing integer variables to produce a continuous relaxation, then lifting the problem back into the integer space to create stronger cuts. Effective when the relaxation provides useful structural information.

**Applications:**
1. **Combinatorial optimization** — Knapsack, Vehicle Routing, Set Partitioning.
2. **Facility location** — binary open/close decisions with continuous capacity variables.
3. **Quadratic or nonlinear MIP** — problems with nonlinearities requiring stronger cuts.

**Computational Demand:**
Requires solving a relaxed version followed by a lifting process. Can add overhead in large models, but results in stronger cuts that significantly reduce branch-and-bound nodes. Less impactful in simpler problems where standard cuts suffice.

---

## Sub-MIP Cuts

**Parameter:** `SubMIPCuts`

**What They Are:**
Sub-MIP cuts are derived from solving smaller, relaxed versions of the original MIP within a subproblem framework. After solving the subproblem, the solver generates cuts valid for the larger original problem.

**Constraints/Structures They Operate On:**
The subproblems involve a subset of variables or constraints from the full problem. Cuts generated exclude fractional solutions found in the subproblem from the main problem's solution space.

**Applications:**
1. **Large-scale MIP** — where solving directly is expensive and decomposition helps.
2. **Branch-and-bound trees** — pruning the tree by cutting off infeasible subtrees.
3. **Decomposable problems** — facility location, scheduling, or network design with natural decomposition structure.

**Computational Demand:**
Involves solving additional subproblems, introducing overhead. However, cuts can significantly reduce the search space. Effectiveness increases when there is clear structure allowing meaningful decomposition.

---

## Strong Chvatal-Gomory Cuts

**Parameter:** `StrongCGCuts`

**What They Are:**
Strong CG cuts are an extension of classical Chvatal-Gomory cuts. They iteratively tighten the LP relaxation by adding inequalities that exclude fractional solutions. The "strong" variant refers to cuts that are particularly effective in reducing the feasible region.

**Constraints/Structures They Operate On:**
Operate on general MIP problems with linear constraints. Work well on integer polyhedra where the constraints are linear and variables are mixed continuous and integer. Particularly powerful when the LP relaxation provides a weak approximation of the integer hull.

**Applications:**
1. **General MIP** — combinatorial optimization, scheduling, resource allocation.
2. **Set Partitioning** — crew scheduling, vehicle routing.
3. **Knapsack Problems** — efficiently eliminating fractional solutions.

**Computational Demand:**
**Demanding.** Requires solving additional LPs and possibly multiple rounds of cut generation. The separation problem (finding the most violated CG cut) can be NP-hard. The strength of the cuts often justifies the effort.

---

## Zero-Half Cuts

**Parameter:** `ZeroHalfCuts`

**What They Are:**
Zero-half cuts are a specific type of CG cuts derived using coefficients in {0, 1/2} instead of integer coefficients. They tighten the relaxation of integer programming problems, bringing it closer to the convex hull of feasible integer solutions.

**Constraints/Structures They Operate On:**
Useful where constraints can be weakened (e.g., using LU-weakening). Applied to systems of inequalities with integer coefficients, often in cases where the matrix has special structures like tree-based systems or network matrices.

**Applications:**
1. **Clique Partitioning** — optimal clustering where a graph is partitioned into cliques.
2. **Asymmetric Traveling Salesman Problem (ATSP)** — improving branch-and-cut efficiency.
3. **Uncapacitated Plant Location** — optimal facility locations with connection constraints.
4. **Acyclic Subgraph and Linear Ordering** — optimizing ordering in directed graphs.

**Computational Demand:**
Moderate. The {0, 1/2} coefficient restriction simplifies generation compared to general CG cuts while still producing effective inequalities.

---

## Projected Implied Bound Cuts

**Parameter:** `ProjImpliedCuts`

**What They Are:**
Projected Implied Bound (PIB) cuts extend basic implied bound cuts by partitioning variables into sets with different implications and projecting these onto the LP relaxation's feasible space.

**Constraints/Structures They Operate On:**
Given a binary variable z and a continuous variable x with bounds, implications like z = 0 -> x <= b are used to restrict the solution space. PIBs extend this by considering partitions: if z = 0, some variables are forced small; if z = 1, others are relaxed. Projecting these implications generates stronger cuts.

**Applications:**
1. **Network design** — binary variables indicating whether a network element is active, impacting flow or capacity bounds.
2. **Production planning** — on/off decisions affecting capacity or output.
3. **Facility location** — open/close decisions implying bounds on demand or capacity variables.

**Computational Demand:**
Generally lightweight. Disabling these cuts results in ~1.3% increase in solution time for medium-difficulty models. The separation process involves checking implications across constraints, which can be expensive for many variables but is often worthwhile for models with significant binary-continuous interactions.
