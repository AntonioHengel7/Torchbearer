# The Torchbearer

**Student Name:** Antonio Hengel
**Student ID:** 828052622
**Course:** CS 460 – Algorithms | Spring 2026

> This README is your project documentation. Write it the way a developer would document
> their design decisions , bullet points, brief justifications, and concrete examples where
> required. You are not writing an essay. You are explaining what you built and why you built
> it that way. Delete all blockquotes like this one before submitting.

---

## Part 1: Problem Analysis

> Document why this problem is not just a shortest-path problem. Three bullet points, one
> per question. Each bullet should be 1-2 sentences max.

- **Why a single shortest-path run from S is not enough:**
  - A single shortest-path run from S only gives distances from S to each node which doesnt help when finding the best order to visit each relic chamber needed and we need to end at T.

- **What decision remains after all inter-location costs are known:**
  - After all inter-locations are found the decision remaining is what order to visit all of the required relics using the least amount of fuel from S to T.

- **Why this requires a search over orders (one sentence):**
  - This requires a search over orders because each different relic visitation order gives a different total cost, even when the same relics must all be visited.

---

## Part 2: Precomputation Design

### Part 2a: Source Selection

> List the source node types as a bullet list. For each, one-line reason.

| Source Node Type | Why it is a source |
|---|---|
| Start | This is a source because we always start from this node. |
| Relic | This is a source because we will want to go from one relic to the next/exit. |

### Part 2b: Distance Storage

> Fill in the table. No prose required.

| Property | Your answer |
|---|---|
| Data structure name | hash map |
| What the keys represent | outer key = source node, inner key = destination node. |
| What the values represent | minimum fuel cost from source to destination. |
| Lookup time complexity | O(1) |
| Why O(1) lookup is possible | hash table access for both levels is constant because scanning is not required. |

### Part 2c: Precomputation Complexity

> State the total complexity and show the arithmetic. Two to three lines max.

- **Number of Dijkstra runs:** k + 1 (Start plus each of the k relics)
- **Cost per run:** O(mlogn)
- **Total complexity:** O((k + 1)mlogn)
- **Justification (one line):** We have to run Dijkstra once per source and then multiply by the per-run cost each time.

---

## Part 3: Algorithm Correctness

> Document your understanding of why Dijkstra produces correct distances.
> Bullet points and short sentences throughout. No paragraphs.

### Part 3a: What the Invariant Means

> Two bullets: one for finalized nodes, one for non-finalized nodes.
> Do not copy the invariant text from the spec.

- **For nodes already finalized (in S):**
  - The recorded distance for any node in S is final and cannot be improved by any other path.

- **For nodes not yet finalized (not in S):**
  - The recorded distance for any node outside S is the best known path that only passes through nodes in S.

### Part 3b: Why Each Phase Holds

> One to two bullets per phase. Maintenance must mention nonnegative edge weights.

- **Initialization : why the invariant holds before iteration 1:**
  - Initially, S is empty, the source has distance 0, and all other nodes are infinity, so the best known paths use only S.

- **Maintenance : why finalizing the min-dist node is always correct:**
  - The node with smallest tentative distance cannot be improved later because all edge weights are nonnegative, so any alternate path would be at least as large.

- **Termination : what the invariant guarantees when the algorithm ends:**
  - When the algorithm ends, every reachable node has its true shortest distance and unreachable nodes remain at infinity.

### Part 3c: Why This Matters for the Route Planner

> One sentence connecting correct distances to correct routing decisions.

Dijkstra must return correct shortest-path costs so the route planner compares relic orders using real fuel costs, not underestimates.

---

## Part 4: Search Design

### Why Greedy Fails

> State the failure mode. Then give a concrete counter-example using specific node names
> or costs (you may use the illustration example from the spec). Three to five bullets.

- **The failure mode:** Greedy picks the next relic with the smallest immediate cost, which can trap the route into expensive later moves.
- **Counter-example setup:** Using the spec example, from S the costs are S->B=1, S->C=2, S->D=2, and some relic-to-relic edges cost 100 while others cost 1.
- **What greedy picks:** Greedy picks B first because it is the cheapest immediate move from S.
- **What optimal picks:** The optimal order is S->B->D->C->T with total cost 4, while other greedy-like orders cost more.
- **Why greedy loses:** The locally cheapest step can force a later 100-cost edge, so the overall sum is worse than a different order.

### What the Algorithm Must Explore

> One bullet. Must use the word "order."

- _Your answer here._
- The algorithm must explore different relic visitation order choices, not just the cheapest next step.

---

## Part 5: State and Search Space

### Part 5a: State Representation

> Document the three components of your search state as a table.
> Variable names here must match exactly what you use in torchbearer.py.

| Component | Variable name in code | Data type | Description |
|---|---|---|---|
| Current location | current_loc | node | The node where the search is currently located. |
| Relics already collected | relics_visited_order | list[node] | The ordered list of relics collected so far. |
| Fuel cost so far | cost_so_far | float | The total fuel cost accumulated up to this state. |

### Part 5b: Data Structure for Visited Relics

> Fill in the table.

| Property | Your answer |
|---|---|
| Data structure chosen | set |
| Operation: check if relic already collected | Time complexity: O(1) |
| Operation: mark a relic as collected | Time complexity: O(1) |
| Operation: unmark a relic (backtrack) | Time complexity: O(1) |
| Why this structure fits | Fast membership checks and add/remove during backtracking. |

### Part 5c: Worst-Case Search Space

> Two bullets.

- **Worst-case number of orders considered:** k!
- **Why:** Every relic order is a permutation when no pruning is possible.

---

## Part 6: Pruning

### Part 6a: Best-So-Far Tracking

> Three bullets.

- **What is tracked:** The best (lowest) total cost found so far and its relic order.
- **When it is used:** Before expanding a branch, compare its bound or current cost against the best cost.
- **What it allows the algorithm to skip:** Any partial route that cannot beat the best known solution.

### Part 6b: Lower Bound Estimation

> Three bullets.

- **What information is available at the current state:** Current location, remaining relics, and precomputed distances.
- **What the lower bound accounts for:** The cheapest move from the current node to any remaining relic plus the cheapest remaining-to-exit cost.
- **Why it never overestimates:** Any valid route must include a path to some remaining relic and a final path to the exit, so this sum is optimistic.

### Part 6c: Pruning Correctness

> One to two bullets. Explain why pruning is safe.

- If the lower bound is already at least the best cost, no completion of this branch can beat the best solution.
- Therefore pruning cannot remove the optimal route.

---

## References

> Bullet list. If none beyond lecture notes, write that.

- _Your references here._
