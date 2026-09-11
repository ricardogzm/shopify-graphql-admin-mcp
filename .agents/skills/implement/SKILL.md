---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

Before editing, record the exact pre-work commit as the review fixed point unless the caller supplied one.

Use /tdd where possible, at pre-agreed seams.

Run typechecking and focused test files regularly.

Create a candidate commit with `/commit`, then use `/code-review` against the fixed point. Resolve every finding by fixing it or recording concrete repository or spec evidence that it does not apply. After fixes, run affected checks, update the candidate with `/commit`, and repeat `/code-review`.

**Done when:** `/code-review` reports no unresolved Standards or Spec findings against the candidate.

Run the full test suite against the reviewed candidate. If it requires a repair, verify the repair, update the candidate with `/commit`, repeat `/code-review`, then rerun the full suite.

**Done when:** the full suite passes against the final reviewed candidate.
