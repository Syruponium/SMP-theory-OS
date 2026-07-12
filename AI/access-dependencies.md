# Theory OS: Formula Dependencies & Cross-Layer Linkages

**Version**: 1.0  
**Date**: 2026-07-12  
**Status**: Architecture for interconnected formulas

---

## The Problem You're Describing

Two layers exist:
- `L2_elementary_03_leptons.md` (Tau properties)
- `L2_elementary_02_neutrinos.md` (Muon properties)

Originally treated as **separate**.

Then you discover: **They form a coupled system.**

```
Tau: High-energy particle that decays/radiates
Muon: Lower-energy particle that absorbs/receives

System: Energy flow between them (coupled oscillator model)
```

When you update **Tau's formula**, **Muon's behavior changes automatically** (in the model).

**How do you encode this dependency so both files "know" about each other?**

---

## Solution: Formula Linkage Graph

Instead of embedding formulas in markdown and hoping, create an **explicit dependency layer**.

### Part 1: The Formula Registry (TEMPLATE - Template is the same, formula is different)

Create a new file (or section in L0):

```yaml
# L0_formula_registry.yml
# Central registry of all formulas and their dependencies

formulas:
  
  tau_energy_emission:
    id: "F_tau_E_emit"
    layer: "L2_elementary_03_leptons"
    section: "Tau Energy Dynamics"
    formula: |
      dE_tau/dt = -α_decay * E_tau - β_couple * (E_tau - E_muon)
    parameters:
      α_decay: "decay constant (LOCKED from L0)"
      β_couple: "coupling strength to muon (DERIVED)"
      E_tau: "tau energy state"
      E_muon: "muon energy state (reference)"
    
    depends_on:
      - "F_muon_E_absorb" (bidirectional coupling)
    
    reverse_dependencies:
      - "F_tau_mass" (energy affects mass calculation)
      - "L4_chemistry_01_ICI" (affects interaction rates)
    
    status: DERIVED
    derivation_chain: "L0_canon (coupling principle) → L1_medium (energy flow) → this"
    version: 1.2
    last_modified: 2026-07-12

  muon_energy_absorb:
    id: "F_muon_E_absorb"
    layer: "L2_elementary_02_neutrinos"  # Actually: Muon layer
    section: "Muon Energy Dynamics"
    formula: |
      dE_muon/dt = +β_couple * (E_tau - E_muon) - γ_decay * E_muon
    parameters:
      β_couple: "coupling strength to tau (SAME as F_tau_E_emit)"
      γ_decay: "muon decay constant (LOCKED from L0)"
      E_tau: "tau energy state (reference)"
      E_muon: "muon energy state"
    
    depends_on:
      - "F_tau_E_emit" (bidirectional coupling)
    
    reverse_dependencies:
      - "F_muon_mass"
    
    status: DERIVED
    derivation_chain: "L0_canon → L1_medium → this"
    version: 1.1
    last_modified: 2026-07-10

  # More formulas...
```

**This registry is the "nervous system" of Theory OS.**

---

## Part 2: Embedding Formulas in Layer Files

In your actual layer files, reference the registry:

### File: L2_elementary_03_leptons.md

```markdown
---
status: CLOSED
dependencies:
  - L0_canon_01_constants
  - L0_formula_registry
  - L2_elementary_02_neutrinos (bidirectional via F_tau_E_emit ↔ F_muon_E_absorb)
---

# L2: Elementary Particles - Tau

## Tau Energy Dynamics

### Formula: Tau Energy Emission

**Reference**: [F_tau_E_emit]

```latex
\frac{dE_\tau}{dt} = -\alpha_{decay} \cdot E_\tau - \beta_{couple} \cdot (E_\tau - E_\mu)
```

**Parameters**:
- $\alpha_{decay}$ = decay constant (from L0_canon_01, value = 2.27e-12 s^-1)
- $\beta_{couple}$ = coupling strength = DERIVED BELOW
- $E_\tau$ = tau energy state (dynamical)
- $E_\mu$ = muon energy state (reference to L2_elementary_02_neutrinos)

**Derivation**:
This equation emerges from L1_medium energy-flow principles + the discovered tau-muon coupling.

The coupling term $-\beta_{couple} \cdot (E_\tau - E_\mu)$ represents:
- Energy loss from Tau when E_tau > E_muon
- Energy gain to Tau when E_tau < E_muon
- Forms coupled oscillator system with Muon

**Coupling Derivation**:

From L1_medium_01_substrate (acoustic pressure equilibrium):

$$\beta_{couple} = \frac{K_{interaction}}{2m_{eff}}$$

Where:
- $K_{interaction}$ = interaction strength (LOCKED in L0: value = 1.45e-20)
- $m_{eff}$ = effective mass (DERIVED from Tau + Muon masses)

Status: **DERIVED** (cascades from L0 → L1 → here)

### Cross-Layer Link: Muon Coupling

This formula is **bidirectionally coupled** with:
- File: `L2_elementary_02_neutrinos.md`
- Formula: `F_muon_E_absorb`
- Link type: **Bidirectional Energy Conservation**

When this formula updates, the Muon formula must be rechecked (automatically flagged).

**Status Check**: If either formula's status changes from CLOSED, both are flagged for NEEDS-RECONSIDER.

---

## The Coupled System

Together, Tau + Muon form a system:

```latex
\begin{aligned}
\frac{dE_\tau}{dt} &= -\alpha_{decay} E_\tau - \beta_{couple}(E_\tau - E_\mu) \\
\frac{dE_\mu}{dt} &= +\beta_{couple}(E_\tau - E_\mu) - \gamma_{decay} E_\mu
\end{aligned}
```

**Invariant**: $\frac{d}{dt}(E_\tau + E_\mu) = -\alpha_{decay} E_\tau - \gamma_{decay} E_\mu$ (total energy dissipates, no creation)

**This invariant should be checked automatically** when either formula changes.
```

### File: L2_elementary_02_neutrinos.md

```markdown
---
status: CLOSED
dependencies:
  - L0_canon_01_constants
  - L0_formula_registry
  - L2_elementary_03_leptons (bidirectional via F_muon_E_absorb ↔ F_tau_E_emit)
---

# L2: Elementary Particles - Muon

## Muon Energy Absorption

### Formula: Muon Energy Absorption

**Reference**: [F_muon_E_absorb]

```latex
\frac{dE_\mu}{dt} = +\beta_{couple} \cdot (E_\tau - E_\mu) - \gamma_{decay} \cdot E_\mu
```

[... same structure, but for Muon side ...]

### Cross-Layer Link: Tau Coupling

This formula is **bidirectionally coupled** with:
- File: `L2_elementary_03_leptons.md`
- Formula: `F_tau_E_emit`
- Link type: **Bidirectional Energy Conservation**

[Same bidirectional flagging system]
```

---

## Part 3: The Bidirectional Update Mechanism

When you change **one** formula, what happens to the **other**?

### Scenario: You Update $\beta_{couple}$ in Tau File

**Step 1: Edit Detection**
```
File: L2_elementary_03_leptons.md
Change: β_couple = 1.45e-20 → 1.50e-20
```

**Step 2: Dependency Scan**
```
System reads metadata:
  "depends_on: ['F_muon_E_absorb']"
  
Finds: L2_elementary_02_neutrinos.md also uses β_couple
```

**Step 3: Flag Both**
```
L2_elementary_03_leptons.md:
  status: CLOSED → NEEDS-RECONSIDER
  reason: "F_tau_E_emit was modified (β_couple changed)"
  
L2_elementary_02_neutrinos.md:
  status: CLOSED → NEEDS-RECONSIDER
  reason: "Upstream dependency F_tau_E_emit changed"
  flag: "Check: Energy conservation invariant still holds?"
```

**Step 4: Consistency Check (Automatic)**
```
System runs: Invariant check
  d/dt(E_τ + E_μ) = ?
  
If invariant violated:
  Both files flagged as INCONSISTENT
  AI suggests: "Check if new β_couple violates conservation"
  
If invariant OK:
  Both files: status → CLOSED (if all other checks pass)
```

**Step 5: Notification to Human**
```
Interface shows:
  L2_elementary_03_leptons: "Modified, status OK ✓"
  L2_elementary_02_neutrinos: "Updated due to upstream change, check invariants ✓"
  
Suggestion: "Run FILTERED review on coupled system?"
```

---

## Part 4: The Metadata for Bidirectional Links

Every formula with dependencies needs this metadata:

```yaml
formulas:
  
  tau_energy_emission:
    id: "F_tau_E_emit"
    
    # Standard metadata
    layer: "L2_elementary_03_leptons"
    status: DERIVED
    
    # THE KEY: Bidirectional Links
    depends_on:
      - formula_id: "F_muon_E_absorb"
        layer: "L2_elementary_02_neutrinos"
        link_type: "bidirectional_coupling"
        shared_parameter: "β_couple"
        invariant: "Energy conservation: d/dt(E_τ + E_μ) = dissipation terms only"
        validation_rule: "Run invariant check when either formula changes"
    
    reverse_dependencies:
      - formula_id: "F_tau_mass"
        layer: "L2_elementary_03_leptons"
        link_type: "input_output"
        description: "Tau mass depends on energy state via relativistic relation"
    
    # If ANY dependent changes
    cascading_action: "flag_for_reconsider"
    
    # Validation rules specific to this formula
    validation_rules:
      - "Check: α_decay > 0 (physical)"
      - "Check: β_couple > 0 (physical)"
      - "Check: Invariant holds with coupled muon formula"
      - "Check: Dimensions consistent [1/time]"
```

---

## Part 5: Practical Implementation (How It Runs)

### The Formula Dependency Engine (Pseudo-code)

```python
class FormulaDependencyEngine:
    """Manages bidirectional formula updates and cascading checks"""
    
    def update_formula(self, formula_id, new_content):
        """
        When a formula changes, automatically check all coupled formulas
        """
        formula = self.registry.get(formula_id)
        
        # 1. Update the formula
        formula.content = new_content
        formula.version += 1
        formula.last_modified = now()
        
        # 2. Find all coupled formulas
        coupled = formula.depends_on + formula.reverse_dependencies
        
        # 3. Flag them for reconsideration
        for coupled_formula in coupled:
            coupled_formula.status = "NEEDS-RECONSIDER"
            coupled_formula.flag_reason = f"Upstream change: {formula_id}"
            
            # 4. Run validation rules
            if coupled_formula.validation_rules:
                results = self.run_validations(coupled_formula)
                if not results.all_pass:
                    coupled_formula.status = "INCONSISTENT"
                    self.notify_human(
                        f"Inconsistency in {coupled_formula.id}: {results.failures}"
                    )
        
        # 5. Check invariants
        if formula.invariant:
            invariant_check = self.verify_invariant(
                formula.id, 
                coupled_formulas
            )
            if not invariant_check.holds:
                self.flag_for_review(
                    [formula_id] + [c.id for c in coupled],
                    reason=f"Invariant violated: {invariant_check.error}"
                )
        
        # 6. Update STATUS in both files
        self.update_layer_files(
            [formula.layer, *[c.layer for c in coupled]],
            new_status="NEEDS-RECONSIDER"
        )
```

### When AI (FILTERED context) Validates

```python
class FilteredValidationAI:
    """
    Runs in FILTERED context: only sees coupled formulas + dependencies
    """
    
    def validate_coupled_system(self, formula_id):
        """
        Given one formula changed, check the whole coupled system
        """
        formula = self.registry.get(formula_id)
        coupled = formula.depends_on
        
        # Load only what's needed
        context = {
            "main_formula": load_formula(formula_id),
            "coupled_formulas": [load_formula(c.id) for c in coupled],
            "constants": load_locked_values(),
            "invariants": formula.invariant_checks
        }
        
        # AI operates only on this context (FILTERED)
        # Cannot see: L5_biologie, L6_intelligence, etc.
        
        prompt = f"""
        Check this coupled system:
        
        Main formula: {formula.id}
        New content: {formula.content}
        
        Coupled formulas that depend on this:
        {coupled}
        
        Validation rules:
        {formula.validation_rules}
        
        Question: Does the new formula violate any constraints?
        
        If consistent: respond CONSISTENT
        If inconsistent: specify which rule fails and why
        If unsure: return UNCLEAR + what's missing
        """
        
        result = ai_call(prompt, context=context)
        return result
```

---

## Part 6: How Partial Access Works (Your Second Question)

### The Access Control Model

When you give an AI a context, you don't give it a folder.

You give it an **access specification**.

```yaml
ai_context: "FILTERED_tau_muon_coupling"

access_spec:
  
  # What can this AI READ?
  read:
    - L0_canon_01_constants (FULL - needs all values)
    - L0_formula_registry (PARTIAL - only formulas F_tau_E_emit + F_muon_E_absorb)
    - L2_elementary_03_leptons (PARTIAL - only section "Tau Energy Dynamics")
    - L2_elementary_02_neutrinos (PARTIAL - only section "Muon Energy Absorption")
    
    # These are FORBIDDEN
    - L0_canon_02_axioms (NOT NEEDED for this task)
    - L4_chemistry_* (NOT NEEDED)
    - L5_biologie_* (NOT NEEDED)
    - L6_meercellig_* (NOT NEEDED)
    - L7_intelligence_* (NOT NEEDED)
  
  # What CAN this AI MODIFY?
  write:
    - L0_formula_registry (PARTIAL - can update F_tau_E_emit + F_muon_E_absorb metadata only)
    - L2_elementary_03_leptons (READ-ONLY in this context)
    - L2_elementary_02_neutrinos (READ-ONLY in this context)
  
  # What can it ACCESS from Internet?
  internet:
    - physics_papers (allowed)
    - math_reference (allowed)
    - news_sites (forbidden)
    - social_media (forbidden)

  # Time limits
  token_budget: 8000  # Low, because focused task
  context_budget: "only relevant formulas"
```

### Implementation: Access Gate

```python
class AccessGate:
    """Controls what an AI can see/do based on its context specification"""
    
    def __init__(self, ai_context_spec):
        self.spec = ai_context_spec
    
    def can_read(self, filepath, section=None):
        """Check if this AI can read a file/section"""
        
        # Check whitelist
        if filepath not in self.spec['read']:
            return False, "File not in read whitelist"
        
        # Check partial access
        access_level = self.spec['read'][filepath]
        
        if access_level == "FULL":
            return True, "Full access granted"
        
        elif access_level == "PARTIAL":
            if section is None:
                return False, "This file requires section-level access"
            
            if section in self.spec['read_sections'][filepath]:
                return True, f"Access to section '{section}' granted"
            else:
                return False, f"Section '{section}' not in whitelist"
        
        return False, "Unknown access level"
    
    def can_write(self, filepath, section=None):
        """Check if this AI can modify a file/section"""
        
        # Check if file is writable
        if filepath not in self.spec['write']:
            return False, "File not in write whitelist"
        
        # Check if read-only in this context
        if self.spec['write'][filepath] == "READ_ONLY":
            return False, "This file is read-only for this context"
        
        # Check section-level write
        if section:
            if section in self.spec['write_sections'][filepath]:
                return True, f"Write access to section '{section}' granted"
            else:
                return False, f"Section '{section}' not writable"
        
        return True, "Write access granted"
    
    def load_context(self, ai_model):
        """
        Load ONLY what this AI needs into its context window
        """
        context_items = []
        
        for filepath in self.spec['read']:
            if self.spec['read'][filepath] == "FULL":
                # Load entire file
                context_items.append(load_file(filepath))
            
            elif self.spec['read'][filepath] == "PARTIAL":
                # Load only specified sections
                sections = self.spec['read_sections'].get(filepath, [])
                for section in sections:
                    context_items.append(load_section(filepath, section))
        
        # Compress and deduplicate
        compressed = compress_context(context_items)
        
        # Calculate tokens
        tokens_used = estimate_tokens(compressed)
        
        if tokens_used > self.spec['token_budget']:
            # Recursively reduce context until it fits
            compressed = aggressive_compress(compressed, self.spec['token_budget'])
        
        return compressed
```

---

## Part 7: Practical Example (Step-by-Step)

### Scenario: You Discover Better Coupling Constant

**Step 1: You (FULL_BRAINSTORM) Find Something**
```
"Wait, if Tau and Muon are coupled via acoustic pressure,
 then β_couple should be related to the substrate impedance!"
```

**Step 2: You Create Hypothesis**
```yaml
hypothesis:
  id: "H_beta_couple_impedance_001"
  claim: "β_couple = Z_substrate / 2π × f_coupling"
  derivation: "[math proof]"
  implies: "Both Tau and Muon formulas must update"
  status: "OPEN"
  next_stage: "FILTERED validation"
```

**Step 3: FILTERED AI Validates (Specialized, Low-Token Context)**
```
AccessGate spec: FILTERED_tau_muon_coupling

AI Prompt:
  "Check if β_couple = Z_substrate/(2π·f_coupling) 
   maintains energy conservation in coupled Tau-Muon system"

AI sees ONLY:
  - F_tau_E_emit formula
  - F_muon_E_absorb formula
  - Energy conservation invariant
  - L0 constants (Z_substrate value, f_coupling)

AI cannot see:
  - Anything about leptons beyond these two
  - Chemistry layer
  - Biology layer
  - Intelligence layer

Result: "CONSISTENT ✓"
```

**Step 4: FULL_REVIEW (You) Approve**
```
You see:
  - FILTERED AI says consistent
  - New β_couple value
  - Both formulas updated
  - Both flagged for cascade check

Decision: "APPROVED → STATUS = CLOSED"

Action:
  Update L0_formula_registry:
    β_couple (version 1.3): new value
  
  Update L2_elementary_03_leptons.md:
    status: CLOSED (with new derivation)
  
  Update L2_elementary_02_neutrinos.md:
    status: CLOSED (updated reference to new β_couple)
  
  Cascade check:
    F_tau_mass (depends on β_couple?)
    F_muon_mass (depends on β_couple?)
    
    If yes: Flag them for reconsider
```

**Step 5: Automatic Propagation**
```
Dependency engine runs:
  - Check: Does L4_chemistry use either constant?
  - Check: Does L5_biologie depend on Tau/Muon masses?
  - Check: Invariants in coupled formulas still hold?
  
  Only flag downstream if changes are material
  (not every upstream change cascades)
```

---

## Why This Works

### 1. Formula Registry = Nervous System
All formulas are **visible**, **linked**, **traceable**.

Changes don't hide. They propagate explicitly.

### 2. Bidirectional Links = Physical Reality
In nature, Tau and Muon **do** affect each other.

Your system **models** this, not just documents it.

### 3. Partial Access = Focused Work
FILTERED AI doesn't see noise.

It sees exactly what's relevant.

Token burn: minimal. Accuracy: high.

### 4. Automated Cascading = No Manual Tracking
When β_couple changes, everything downstream **automatically knows**.

No "update this file, oh wait, forgot this other file" errors.

### 5. Bidirectional Updates = No Stale Copies
If you change Tau formula, Muon formula's status updates instantly.

Not "tomorrow when I remember to check."

**Now.**

---

## Next Steps

Would you like me to:

1. **Expand the formula registry structure** to show how multi-layer dependencies work?
2. **Show concrete examples** from your SMP model (e.g., substrate ↔ gravity)?
3. **Code a prototype** of the AccessGate system in Python/pseudocode?
4. **Design the UI** for how this shows in your interface (color coding for coupled formulas, etc.)?

What's most useful right now?
