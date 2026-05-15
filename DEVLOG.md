# Development Log – The Torchbearer

**Student Name:** Antonio Hengel
**Student ID:** 828052622


---

## Entry 1 – [5/12/2026]: Initial Plan

> Required. Write this before writing any code. Describe your plan: what you will
> implement first, what parts you expect to be difficult, and how you plan to test.

- My plan is to fully understand the requirements then outline what tasks I'll have to do in what order. My first implementation will probably try Dijkstra's algorithm since its an easy to implement greedy approach. I know that I will have to eventually tackle pruning and expect that to be the hardest part. For testing I will for now use the provided tests.

---

## Entry 2 – [5/13/2026]: Testing blocked by missing pipeline

> Required. At least one entry must describe a bug, wrong assumption, or design change
> you encountered. Describe what went wrong and how you resolved it.

- I tried running the provided tests but hit a TypeError because solve() still returns None. I realized the tests depend on the full pipeline (precompute + route search), so I need to finish those functions before tests will run. For now I am focusing on filling in the remaining functions and then will re-run the tests.

---

## Entry 3 – [5/14/2026]: Implemented search and pruning

- I wrote Parts 4 through 6 in the README to document the search approach and pruning idea. I implemented the recursive _explore() search and wired solve() to the full pipeline. I ran the provided tests and confirmed they all passed. I also compared the search behavior against the spec examples to make sure the ordering logic made sense.

---

## Entry 4 – [5/14/2026]: Post-Implementation Reflection

> Required. Written after your implementation is complete. Describe what you would
> change or improve given more time.

- With more time I would add clearer comments in the search code so the pruning rationale is easier to follow. I would also write extra tests beyond the provided ones and handle more edge cases explicitly, like disconnected relic subsets or missing exit paths.

---

## Final Entry – [Date]: Time Estimate

> Required. Estimate minutes spent per part. Honesty is expected; accuracy is not graded.

| Part | Estimated Hours |
|---|---|
| Part 1: Problem Analysis | 30 mins |
| Part 2: Precomputation Design | 1 hour |
| Part 3: Algorithm Correctness | 45 mins |
| Part 4: Search Design | 45 mins |
| Part 5: State and Search Space | 45 mins |
| Part 6: Pruning | 30 mins |
| Part 7: Implementation | 2 hours |
| README and DEVLOG writing | 1 hour |
| **Total** | about 7 hours |
