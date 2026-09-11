---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round, with a recommended answer on every question. Then wait for the user's answers before the next round.

Grill in plain language. Prefer simple terms over jargon. Be concise, but keep every detail needed for a correct decision: options, constraints, and tradeoffs stay explicit.

When a round needs technical terms or concepts, define them first in a short block in chat, above the questions: one short clause per term, only the terms that round actually uses. Skip the block when every term in the round is already familiar or settled. Then ask the questions in plain language that can rely on those definitions.

**Closed questions** (fixed choices) live in the agent's **question tool** when it has one. One call per round, every closed frontier question in that call. Put the recommended option first and end its label with `(Recommended)`. Put the rationale in the question text, one clause. Chat holds the definitions block and any open-ended question.

If that tool is missing, or only accepts one question at a time, ask the whole frontier in chat:

```
**<Term>**: <one-clause definition>

**Q1. <question> <one-clause rationale>**
a. <answer> (recommended)
b. <answer>
```

Open-ended questions stay in chat and omit labeled options. Plain wording must still name a sharp decision surface, never a vague restatement. Use a concrete example or analogy only when a direct phrasing remains abstract. Do not restate every user reply. Restate settled decisions plainly in branch summaries and the final shared-understanding check. Those are not questions: they stay in chat as prose.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a later round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it. Don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report. Ask the rest of the frontier now. The _decisions_ are the user's. Put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
