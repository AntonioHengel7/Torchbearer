# Development Log – The Torchbearer

**Student Name:** Antonio Hengel
**Student ID:** 828052622

> Instructions: Write at least four dated entries. Required entry types are marked below.
> Two to five sentences per entry is sufficient. Write entries as you go, not all in one
> sitting. Graders check that entries reflect genuine work across multiple sessions.
> Delete all blockquotes before submitting.

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

## Entry 3 – [Date]: [Short description]

_Your entry here._

---

## Entry 4 – [Date]: Post-Implementation Reflection

> Required. Written after your implementation is complete. Describe what you would
> change or improve given more time.

_Your entry here._

---

## Final Entry – [Date]: Time Estimate

> Required. Estimate minutes spent per part. Honesty is expected; accuracy is not graded.

| Part | Estimated Hours |
|---|---|
| Part 1: Problem Analysis | |
| Part 2: Precomputation Design | |
| Part 3: Algorithm Correctness | |
| Part 4: Search Design | |
| Part 5: State and Search Space | |
| Part 6: Pruning | |
| Part 7: Implementation | |
| README and DEVLOG writing | |
| **Total** | |
