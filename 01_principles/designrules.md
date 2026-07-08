# Design Rules

## Purpose

This document defines the immutable architectural principles of Theory OS.

These rules are independent of implementation, programming language, database or AI model.

Every future feature, UI component, workflow or AI integration must satisfy these principles.

If a proposed feature violates one or more of these rules, the architecture should be reconsidered before implementation.

Theory OS is not designed around documents.

It is designed around structured reasoning.

## Rule 1 — STORE and FLOW are fundamentally different

Knowledge and reasoning are different kinds of information.

STORE contains persistent knowledge.

FLOW contains temporary reasoning.

FLOW produces knowledge.

STORE never reasons. But it stores facts and values (ancors) that FLOW uses to reason.

```
FLOW
↓
Review
↓
STORE
```

Reasoning Sessions are temporary.

Theory Objects are permanent.

---

## Rule 2 — The Theory Graph is the long-term memory

Chats are never memory.

Reasoning Sessions are never memory.

The Theory Graph is the only persistent representation of accepted knowledge.

Everything stored in the Theory Graph must be intentionally accepted.

Nothing enters automatically.

---

## Rule 3 — Knowledge is composed of objects

Theory OS never stores knowledge as one large document.

Knowledge is composed of independent objects.

Examples:

- Claim
- Formula
- Value
- Question
- Concept
- Evidence
- Source

Every object has:

- identity
- relationships
- dependencies
- history
- context

---

## Rule 4 — Navigation determines context

The current navigation defines the reasoning context.

```
Workspace

↓

Theory

↓

Layer

↓

Branch

↓

Focus
```

Changing navigation changes context.

Navigation never changes knowledge.

---

## Rule 5 — Context is constructed, never accumulated

A large context windows are not the solution, neither is more information.

Only relevant information is what makes the proper context.

Every AI interaction can move from a clean slate, or receives a generated Context Package.

A Context Package is built from:

- navigation
- selected object
- dependencies
- related claims
- formulas
- values
- evidence
- open questions
- session goal

Context is selected.

Never dumped.

---

## Rule 6 — Every reasoning process has an intent - or prompted by the user, or selected from a list.

A list of the specific reasoning intents:

- A brainstorm where creating and exploring ideas and concept is the main intent
- A clarification session
- A rapid-fire-session (question/answer) for optimal FLOW of Ideas, Questions answered, clarification, etc
- A compression of information (also what a rapid-fire-session can do)
- Formula Review
- A consistency Check of internal logic and formulas
- Evidence Review
- Cross Theory Comparison
- Deep Research

Intent determines:

- context
- output
- review process

---

## Rule 7 — Reasoning is disposable

Reasoning Sessions are temporary computations.

They are not knowledge, but they are for the intent to create new knowledge, insights or ideas.

They may contain:

- incorrect ideas
- contradictions
- dead ends
- experiments

This is expected. Only accepted results survive.

---

## Rule 8 — Review precedes acceptance

Nothing becomes knowledge automatically.

Every extracted insight passes through Review.

Review produces:

- contradictions
- missing evidence
- weak dependencies
- missing assumptions
- possible compression
- proposed patches

Human approval is required before acceptance. AI can never - without premission of a human - write new knowledge to STORE. It can however READ what is in the STORE and use STORE for reason.

---

## Rule 9 — The Inbox separates FLOW from STORE

Every extracted object first enters the Inbox.

```
Reasoning (FLOW)

↓

Extraction (FLOW)

↓

Inbox (FLOW)

↓

Review (FLOW <-> STORE)

↓

Accept (FLOW <-> STORE)

↓

Theory Graph (STORE)
```

The Inbox protects the integrity of the Theory.

---

## Rule 10 — AI proposes, humans decide

AI is the reasoning enginem prompted by a question, or other types of input.

Humans remain responsible for acceptance.

AI may:

- propose
- summarize
- review
- compare
- challenge
- compress

AI never silently changes the Theory Graph. Theory graph is a One-way street Writing. AI can only read. Human can Write (accept) to STORE new knowledge > Theory graph.

---

## Rule 11 — Dependencies are first-class objects

Every object may depend on other objects.

Examples:

Claim → Formula

Formula → Value

Question → Claim

Evidence → Claim

Layer → Layer

Dependencies are explicit.

Never implicit.

---

## Rule 12 — Every change must be traceable

Every accepted modification creates history.

Nothing is overwritten.

Theory evolution must remain inspectable.

Future versions should support impact analysis.

Example:

Changing one Formula reveals every affected:

- Formula
- Claim
- Question
- Summary
- Context Package

---

## Rule 13 — Compression creates leverage

Compression is not deletion.

Compression creates higher-level representations.

Examples:

Reasoning Session

↓

Summary

↓

Claim

↓

Theory

Every compression step must preserve meaning while reducing cognitive complexity.

---

## Rule 14 — Interfaces exist to reduce cognitive load

Every interface should reduce the amount of information a human must actively maintain.

The purpose of Theory OS is not to display more information.

The purpose is to display only the information required for the current reasoning task.

---

## Rule 15 — Everything exists to improve context selection

Theory OS is fundamentally a Context Operating System.

The central problem is not storing information.

The central problem is selecting the correct information at the correct moment to better compress and output structureed information for papers, blogs, or videos. Information without output and integration, is useless.

Every feature should improve one or more of:

- context quality
- reasoning quality
- navigation
- consistency
- review
- compression

If it does not, it likely does not belong in Theory OS.

---

## Rule 16 — The architecture must remain modular

Every subsystem should be replaceable.

Examples:

AI Provider

Database

Graph Engine

Search Engine

Embedding Engine

Review Engine

The architecture should survive changes in technology.

---

## Rule 17 - easoning must be inspectable

Theory OS must make reasoning inspectable without requiring the user to trust the AI blindly.

Every AI output should provide an accessible reasoning audit.

This does not require exposing private model internals or hidden chain-of-thought. Instead, the system must expose the reasoning artifacts necessary for human verification.

A reasoning audit should include:

- selected context
- reasoning mode
- assumptions
- intermediate steps
- python code and formulas used
- values used
- source citations
- calculation traces
- uncertainty notes
- proposed conclusions
- affected objects

The user should always be able to inspect why an answer was produced, what information was used, and which parts of the Theory Graph would be affected if the output is accepted.

## Rule 18 — The Theory evolves through feedback loops

Theory OS mirrors the same feedback principles it is designed to support.

```
Question

↓

Reasoning

↓

Insight

↓

Review

↓

Acceptance

↓

Theory

↓

New Context

↓

New Question
```

Knowledge grows through iterative refinement.

Not accumulation.

---

## Rule 18 — The human is the navigator

Humans should never become data-entry operators.

AI performs reasoning.

Theory OS manages structure.

The human decides direction.

The role of the human is navigation.
