# Reasoning Inspectability Implementation

## Core Flow

User Question
→ Reasoning Mode Selected
→ Context Package Built
→ AI Response Generated
→ Reasoning Audit Created
→ User Reviews
→ Extracted Objects Enter Inbox
→ Human Accepts or Rejects
→ Theory Graph Updated

## UI Pattern

Every AI message should contain a compact audit control.

Example:

AI Response
[Show Reasoning Audit]

When clicked, open either:

- inline dropdown
- side panel
- full audit view

## Reasoning Audit Panel

The audit panel should display:

### 1. Context Used
Shows which objects were included in the response.

Examples:

- Layer Summary
- Active Claims
- Formulas
- Values
- Sources
- Open Questions
- Previous Session Summary

### 2. Reasoning Mode
Shows the selected reasoning mode.

Examples:

- Clarification
- Compression
- Consistency Check
- Formula Review
- Evidence Review
- Cross-Theory Comparison

### 3. Assumptions
Lists assumptions made during reasoning.

Each assumption can be converted into:

- Claim
- Open Question
- Dependency
- Review Item

### 4. Formulas and Values
Lists all formulas and values used.

Each item should link back to its Theory Object.

Example:

Formula:
N_ICI = 3^{c_eff}

Values:
c_eff = 5
S_c = 1.7772

### 5. Calculation Trace
Shows visible calculations, code snippets, numerical steps or external tool outputs when available.

This is especially important for Python-generated calculations.

### 6. Sources and Citations
Shows all external sources used.

Each source should include:

- title
- author if available
- link
- citation
- relevance
- confidence

### 7. Output Claims
Lists the claims implied by the response.

Each can be sent to Inbox.

### 8. Uncertainty
Shows what is uncertain, estimated, inferred or unsupported.

### 9. Impact Preview
Shows which objects may be affected if the response is accepted.

Examples:

- 3 claims updated
- 1 formula affected
- 2 open questions created
- 1 dependency added

## Data Model

ReasoningAudit {
  id
  session_id
  message_id
  reasoning_mode
  context_package_id
  assumptions
  formulas_used
  values_used
  sources_used
  calculation_trace
  output_claims
  uncertainty_notes
  affected_objects
  created_at
}

## Design Principle

The audit is not optional metadata.

It is part of the reasoning object.

Every important AI response should be inspectable, traceable and reviewable.

## Important Constraint

Theory OS should not depend on hidden model chain-of-thought.

Instead, it should store explicit reasoning artifacts created for human verification.

The goal is not to expose the AI's private inner process.

The goal is to make the scientific reasoning process reproducible, inspectable and accountable.
