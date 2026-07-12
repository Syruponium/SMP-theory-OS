# THEORY OS - Fundamental Architecture

**Version**: 1.0  
**Date**: 2026-07-12  
**Status**: Core philosophy (not yet implemented)

---

## The Four Pillars

Everything in Theory OS is built from the repetition of a single pattern, applied at different scales with different context-access.

### Pillar 1: STATE

**Definition**: A tagged representation of what currently exists and what needs resolution.

```yaml
state:
  type: "gravity"
  value: "thermodynamics"
  status: "ACTIVE"
  triggered_schemas: [5, 12, 47]  # Which knowledge nodes activated
  open_questions: 
    - "What is gravity?"
    - "How does gravity work?"
  next_layer: "REFINE"
```

**Purpose**: 
- Not storage. Not a database entry.
- A **scheme trigger**. The state tells downstream AI which mental models to activate.
- Always includes: what we know + what we don't + what activated

**Equivalence in research**:
- Lab notes saying "Tested X, found Y, need to verify Z"
- The status of a hypothesis: ACTIVE, REFINED, LOCKED, or PARKED

**Transparency in AI thinking**:
- It is always required that human can look into the thinking mode in the back, where human can see
what schemes where activated, so that it can also see what potential schemes where maybe missing, or if there where
wrong assumptions made from for example the Standard Model where the values or assumptions are not compatible with the SMP model.

---

### Pillar 2: CONTEXT

**Definition**: The boundary of what a particular AI can see, determined by its role and the current task.

Three standard contexts / AI models:

### LAYER CONTEXT 1: FULL_BRAINSTORM 
- **FLOW/STORE**: [INPUT > OPTIMAL FLOW]
- **Access TheoryOS**: Everything in STORE + all FLOW/TEMP-STORE
- **Access Internet**: Full access
- **Access layer**: Every STORE layer
- **Role**: Generalist researcher (optimal FLOW)
- **Cost**: High token burn (all schemas visible + all knowledge outside Theory.OS)
- **Purpose**: Pattern-finding, cross-layer discovery: **Final Descionmaker in Review-mode**
- **Limitation**: Can't parallelize (one brain processing everything)

### LAYER CONTEXT 2: FILTERED
- **FLOW/STORE**: [INPUT > LIMITED FLOW + LIMITED STORE > GO TO OUTPUT REVIEW]
- **Access TheoryOS**: LOCKED layers (+ option to select more layers if needed manually) + dependencies of current state
- **Access Internet**: limited access
- **Access layer**: Only selected layer
- **Role**: Specialist AI (refinement, consistency check, validation)
- **Cost**: Low token burn (only relevant schemas activated)
- **Purpose**: Deep work on specific question, fast iteration
- **Strength**: Can run in parallel (many specialists on different sub-problems)

### LAYER CONTEXT 3: FULL_REVIEW
- **FLOW/STORE**: [INPUT FROM LAYER 2 > REVIEW: LIMITED FLOW = TEMP OUTPUT - REVIEW]
- **Access TheoryOS**: Everything in STORE + Only selected REVIEW layer + option for FULL CONTEXT
- **Access Internet**: No access (only by request)
- **Access layer**: Only selected layer + sublayer in [REVIEW]
- **Role**: Generalist researcher (and architect human); all factual knowledge:lead AI)
- **Cost**: High token burn (all schemas visible)
- **Purpose**: Pattern-finding, cross-layer discovery: **Final Descionmaker in Review-mode**
- **Limitation**: Can't parallelize (one brain processing everything)

### LAYER CONTEXT 4: GATED OUTPUT
- **FLOW/STORE**: [INPUT FROM REVIEW > STORE > COMPRESSION > OUTPUT - PAPER]
- **Access TheoryOS**: LOCKED layers + specific selected content
- **Access Internet**: Full
- **Access layer**: Only STORE layers
- **Role**: Output AI (LaTeX generator, visualization, external publication)
- **Cost**: Minimal token burn (only what's needed)
- **Purpose**: Convert theory into artifact
- **Constraint**: Read-only, no modification of STORE

**Permission Model** (future, for teams):
```yaml
researcher:
  name: "Alice"
  role: "biology_specialist"
  can_see: 
    - L5_biologie_* (full)
    - L0_canon_* (full)
    - L4_chemistry_* (dependencies only)
  can_modify:
    - L5_biologie_* (only in REVIEW)
    - L0_canon_* (never - LOCKED)
  can_access_ai: 
    - FULL context (own layers)
    - FILTERED context (dependencies)
```

---

### Pillar 3: SCHEMA-SET

**Definition**: The actual knowledge that can activate. The content.

**At every level:**
- Markdown files (L0, L1, L2, ... Ln)
- Formulas (LaTeX)
- Derived values (with audit trails)
- Open questions (marked as HYP or with open_issues tag)
- Dependencies (links between layers)

**But here's the key**: The schema-set is **identical in structure** at every scale.

```
GLOBAL SCHEMA-SET (all layers)
├─ L0: Constants + Axioms
├─ L1: Substrate + Medium
├─ L2: Particles
└─ ... (all the way up)

FOCUSED SCHEMA-SET (for REFINE task)
├─ L1_medium_01_substrate (main)
├─ L1_medium_02_fases (dependency)
├─ L0_canon_01_constants (anchor)
└─ [everything else is noise to this AI]

SUB-SCHEMA-SET (within a section)
├─ Title
├─ Definition (with status)
├─ Derivation (with formula refs)
├─ Open question (if HYP)
└─ Links (to adjacent concepts)
```

**The pattern scales infinitely.**

---

### Pillar 4: OUTPUT

**Definition**: The compressed artifact that represents the theory in externally-consumable form.

**Formats:**
- **LATEX** (academic papers, dissertations)
- **MARKDOWN** (blog, notes, references)
- **STRUCTURED JSON** (machine-readable, for citation or integration)
- **VISUAL** (diagrams, charts—generated via external tools)

**Key constraint**: Output is **derived from LOCKED + explicitly-selected content**.

```
User selects: [L1_medium_01, L1_medium_02, L2_elementary_01]
              + "write methods section"

OUTPUT AI:
├─ Access: Only selected + their dependencies (GATED context)
├─ Read: Status metadata + content
├─ Generate: .tex file with citations
└─ Output: Theory_OS_Methods_2026_07_12.tex
```

**The output format is a **template**—you customize once, apply everywhere.**

```yaml
template: "smp_theoretical_foundation"
sections:
  - name: "Axioms"
    format: "list_with_derivations"
    cite_status: "LOCKED_only"
  - name: "Derivations"
    format: "step_by_step_math"
    cite_status: "DERIVED_with_audit"
  - name: "Open Questions"
    format: "highlighted_subsection"
    cite_status: "HYP_with_flags"
```

You write the template once. The AI applies it to any subset of your theory.

---

## The Repetition (The Secret)

Every interaction in Theory OS follows the same four-step cycle, regardless of scale:

```
INPUT STATE
    ↓
SELECT CONTEXT
    ↓
ACTIVATE SCHEMAS (from schema-set)
    ↓
GENERATE OUTPUT
    ↓
UPDATE STATE
```

**At SCALE 1** (you asking Claude):
- State: "I want to consolidate substrate and phonons"
- Context: FULL (you see everything)
- Schemas: All layers accessible
- Output: Brainstorm + hypothesis

**At SCALE 2** (Review AI refining):
- State: "Is this consolidation consistent?"
- Context: FILTERED (substrate + phonons + constants only)
- Schemas: Relevant dependencies only
- Output: Validation result + required changes

**At SCALE 3** (Output AI generating paper):
- State: "Convert section 3.2 to LaTeX"
- Context: GATED (selected content only)
- Schemas: LOCKED layers only
- Output: .tex file

**The code is the same. The data-flow is the same. The interface is identical.**

Only the context-gate changes.

---

## Status Metadata (The Context-Gate Trigger)

Every file in STORE has metadata that tells downstream layers what they need to know:

```markdown
---
name: Kritical Phase Boundary < Tc
status: **LOCKED** | DERIVED | EMPIRICAL_FIT | HYP | VERV
status_date: 2026-07-10
open_issues: 
  - "None"
dependencies:
  - L0_canon_01_constants (reason: uses Ttp, Ptp)
  - L1_medium_02_fases (reason: two-phase interaction)
access_level: public | restricted | private
important values: & T_c & 13,8033 K (H$_2$-tripelpunt, NIST)
version: 1.2
---

# Content
```

**This metadata is not a comment. It's executable instruction for the system.**

- Review AI reads this and decides: "Should I activate this?"
- Output AI reads this and decides: "Can I cite this?"
- Specialist AI reads this and decides: "Is this stable enough for my task?"

---

## Symbiosis: Generalist + Specialists

### The Generalist AI (You + FULL context system)
- **Role**: Discovery, pattern-finding, cross-layer synthesis
- **Context**: Everything
- **Limitation**: One person/AI can't parallelize
- **Strength**: Sees the whole system
- **Bottleneck**: Your time + token burn

### The Specialist AIs (FILTERED context)
- **Role**: Deep validation, consistency checking, format conversion
- **Context**: Only dependencies of current focus
- **Limitation**: Doesn't see the whole system
- **Strength**: Fast, parallel, focused
- **Benefit**: Multiplies your bandwidth

### The Integration
**They only communicate through STATE + STATUS METADATA.**

Generalist doesn't say: "Check this section"
Generalist creates STATE: "Section A is HYP, needs validation"

Specialist doesn't report back in prose.
Specialist updates METADATA: "Status upgraded to DERIVED, audit trail: [...]"

Generalist scans metadata, decides next action.

**No chaining. No prompt-piping. Just state-updates and context-gating.**

---

## Why This Works (The Neuroscience Part)

In your brain (Goleman's model):
- **Spreading activation** = all relevant schemas light up
- **Context-gating** = irrelevant schemas stay dark
- **No token burn** = only active schemas consume energy

In Theory OS:
- **Spreading activation** = State metadata triggers schemas
- **Context-gating** = AI only sees its assigned context
- **No token burn** = each AI only processes relevant data

The isomorphism is exact.

---

## Implementation Roadmap (Rough)

### Phase 1: Single-User Generalist
- [ ] Interface: Center element + chat + right sidebar
- [ ] STORE: Layer files with metadata headers
- [ ] FLOW: Brainstorm chat (FULL context)
- [ ] TEMP-STORE: Parking for HYP (Status: to-be-decided)
- [ ] REVIEW: Fractal chat (same interface, deeper)

### Phase 2: Specialist Integration
- [ ] REVIEW AI: Stateless, FILTERED context only
- [ ] Consistency Engine: Metadata classifier
- [ ] Validation Framework: Checks against L0
- [ ] Status updater: Automatic metadata versioning

### Phase 3: Output Pipeline
- [ ] LaTeX generator: Template-based, GATED context
- [ ] Section composer: Multi-layer selection
- [ ] Citation builder: Automatic audit trails
- [ ] Version control: Track what went into each paper

### Phase 4: Team Scale (Future)
- [ ] Permission model: Role-based context access
- [ ] Researcher dashboard: Who's working on what
- [ ] Conflict detection: Parallel edits, resolution
- [ ] Knowledge synthesis: Merge findings from sub-researchers

---

## The Core Insight

You've just described something profound:

> "Context-gates are not for token-saving. They're for **conceptual clarity**. An AI working with only relevant context thinks differently—more deeply, more accurately—than one drowning in noise."

This is true in neuroscience. It's true in research teams. It's true in software.

**Theory OS is the first tool that makes this architectural principle executable.**

---

## One Final Note

This document describes **an interface philosophy**, not an implementation.

The code can change. The interface can evolve. The servers can migrate.

But if you preserve these four pillars—**State, Context, Schema-Set, Output**—and their repetition at every scale, you've built something that will scale from you alone, to a team, to an organization, without fundamental redesign.

That's the elegance.
