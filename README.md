# The Torchbearer

**Student Name:** Antonio Hengel
**Student ID:** 828052622
**Course:** CS 460 – Algorithms | Spring 2026

---

## Part 1: Problem Analysis

- **Why a single shortest-path run from S is not enough:**
  - A single shortest-path run from S only gives distances from S to each node which doesnt help when finding the best order to visit each relic chamber needed and we need to end at T.

- **What decision remains after all inter-location costs are known:**
  - After all inter-locations are found the decision remaining is what order to visit all of the required relics using the least amount of fuel from S to T.

- **Why this requires a search over orders (one sentence):**
  - This requires a search over orders because each different relic visitation order gives a different total cost, even when the same relics must all be visited.

---

## Part 2: Precomputation Design

### Part 2a: Source Selection

| Source Node Type | Why it is a source |
|---|---|
| Start | This is a source because we always start from this node. |
| Relic | This is a source because we will want to go from one relic to the next/exit. |

### Part 2b: Distance Storage

| Property | Your answer |
|---|---|
| Data structure name | hash map |
| What the keys represent | outer key = source node, inner key = destination node. |
| What the values represent | minimum fuel cost from source to destination. |
| Lookup time complexity | O(1) |
| Why O(1) lookup is possible | hash table access for both levels is constant because scanning is not required. |

### Part 2c: Precomputation Complexity

- **Number of Dijkstra runs:** k + 1 (Start plus each of the k relics)
- **Cost per run:** O(mlogn)
- **Total complexity:** O((k + 1)mlogn)
- **Justification (one line):** We have to run Dijkstra once per source and then multiply by the per-run cost each time.

---

## Part 3: Algorithm Correctness

### Part 3a: What the Invariant Means

- **For nodes already finalized (in S):**
  - The recorded distance for any node in S is final and cannot be improved by any other path.

- **For nodes not yet finalized (not in S):**
  - The recorded distance for any node outside S is the best known path that only passes through nodes in S.

### Part 3b: Why Each Phase Holds

- **Initialization : why the invariant holds before iteration 1:**
  - Initially, S is empty, the source has distance 0, and all other nodes are infinity, so the best known paths use only S.

- **Maintenance : why finalizing the min-dist node is always correct:**
  - The node with smallest tentative distance cannot be improved later because all edge weights are nonnegative, so any alternate path would be at least as large.

- **Termination : what the invariant guarantees when the algorithm ends:**
  - When the algorithm ends, every reachable node has its true shortest distance and unreachable nodes remain at infinity.

### Part 3c: Why This Matters for the Route Planner

Dijkstra must return correct shortest-path costs so the route planner compares relic orders using real fuel costs, not underestimates.

---

## Part 4: Search Design

### Why Greedy Fails

- **The failure mode:** Greedy picks the next relic with the smallest immediate cost, which can trap the route into expensive later moves.
- **Counter-example setup:** Using the spec example, from S the costs are S->B=1, S->C=2, S->D=2, and some relic-to-relic edges cost 100 while others cost 1.
- **What greedy picks:** Greedy picks B first because it is the cheapest immediate move from S.
- **What optimal picks:** The optimal order is S->B->D->C->T with total cost 4, while other greedy-like orders cost more.
- **Why greedy loses:** The locally cheapest step can force a later 100-cost edge, so the overall sum is worse than a different order.

### What the Algorithm Must Explore

- The algorithm must explore different relic visitation order choices, not just the cheapest next step.

---

## Part 5: State and Search Space

### Part 5a: State Representation

| Component | Variable name in code | Data type | Description |
|---|---|---|---|
| Current location | current_loc | node | The node where the search is currently located. |
| Relics already collected | relics_visited_order | list[node] | The ordered list of relics collected so far. |
| Fuel cost so far | cost_so_far | float | The total fuel cost accumulated up to this state. |

### Part 5b: Data Structure for Visited Relics

| Property | Your answer |
|---|---|
| Data structure chosen | set |
| Operation: check if relic already collected | Time complexity: O(1) |
| Operation: mark a relic as collected | Time complexity: O(1) |
| Operation: unmark a relic (backtrack) | Time complexity: O(1) |
| Why this structure fits | Fast membership checks and add/remove during backtracking. |

### Part 5c: Worst-Case Search Space

- **Worst-case number of orders considered:** k!
- **Why:** Every relic order is a permutation when no pruning is possible.

---

## Part 6: Pruning

### Part 6a: Best-So-Far Tracking

- **What is tracked:** The best (lowest) total cost found so far and its relic order.
- **When it is used:** Before expanding a branch, compare its bound or current cost against the best cost.
- **What it allows the algorithm to skip:** Any partial route that cannot beat the best known solution.

### Part 6b: Lower Bound Estimation

- **What information is available at the current state:** Current location, remaining relics, and precomputed distances.
- **What the lower bound accounts for:** The cheapest move from the current node to any remaining relic plus the cheapest remaining-to-exit cost.
- **Why it never overestimates:** Any valid route must include a path to some remaining relic and a final path to the exit, so this sum is optimistic.

### Part 6c: Pruning Correctness

- If the lower bound is already at least the best cost, no completion of this branch can beat the best solution.
- Therefore pruning cannot remove the optimal route.

---

## References

- None beyond lecture notes
