# SKILL: ARCHITECTURE & PLANNING
**Goal:** Define DSP architecture, complexity assessment, and implementation strategy
**Trigger:** `/plan [Name]`
**Input:** Reads `plugins/[Name]/.ideas/creative-brief.md` and `parameter-spec.md`
**Output Location:** `plugins/[Name]/.ideas/`

---

## 🎯 PHASE 2: PLAN (Architecture & Strategy)

**Prerequisites:**
- `plugins/[Name]/.ideas/creative-brief.md` exists
- `plugins/[Name]/.ideas/parameter-spec.md` exists
- Phase 1 (DREAM) complete

**Output Files:**
- `plugins/[Name]/.ideas/architecture.md` - DSP component design
- `plugins/[Name]/.ideas/plan.md` - Implementation strategy and complexity assessment

---

## 📋 STEP 1: ARCHITECTURE DEFINITION

### 1.1 Read Input Contracts
Read these files in parallel:
- `plugins/[Name]/.ideas/creative-brief.md` - Plugin concept and behavior
- `plugins/[Name]/.ideas/parameter-spec.md` - Parameter definitions and ranges

### 1.2 Define DSP Architecture
Create `plugins/[Name]/.ideas/architecture.md` with this structure:

```markdown
# DSP Architecture Specification

## Core Components
List the main DSP building blocks needed:

**Example for a compressor:**
- Input gain stage
- Sidechain detection (RMS/Peak)
- Gain computer (log/linear)
- Lookahead delay buffer
- Output gain stage

**Example for a delay:**
- Input buffer management
- Delay line (circular buffer)
- Feedback processing
- Mix control
- Output processing

## Processing Chain
Define the signal flow:

```
Input → [Component 1] → [Component 2] → ... → Output
```

**Example:**
```
Input → Input Gain → Sidechain → Detector → Gain Computer → Lookahead → Output Gain → Output
```

## Parameter Mapping
Map each parameter to DSP components:

| Parameter | Component | Function | Range |
|-----------|-----------|----------|-------|
| Threshold | Detector | Sets detection level | -60dB to 0dB |
| Ratio | Gain Computer | Sets compression ratio | 1:1 to 20:1 |
| Attack | Detector | Sets attack time | 0.1ms to 100ms |

## Plugin Format Support
Determine which formats to build:

| Format | Platform | Notes |
|--------|----------|-------|
| VST3 | Win/Mac/Linux | Default, always included |
| AU | macOS only | Required for Logic Pro |
| CLAP | Win/Mac/Linux | Modern standard, better parameter handling |
| Standalone | All | For testing, no DAW needed |
| LV2 | Linux | Open-source DAWs |

**CLAP Advantages:**
- Per-note modulation (polyphonic modulation)
- Better parameter automation handling
- Thread-safe parameter access
- Faster scanning by DAWs (no COM/ARA overhead)
- Growing ecosystem (Bitwig, Reaper, FL Studio, etc.)

## Preset System Design
Design the preset architecture:

- **Preset format:** XML (via JUCE ValueTree) or JSON
- **Factory preset categories:** [Based on creative brief sonic character]
- **Estimated factory preset count:** [Based on parameter count and complexity]
- **User preset location:** Platform-specific AppData directory
- **DAW integration:** Programs/getNumPrograms() support
- **Preset browser:** In-plugin UI component

See `skill_presets/SKILL.md` for implementation details.

## Collaboration Support (Optional)
If real-time collaboration is desired:
- OSC-based parameter sync
- JUCE `juce_osc` module required
- Session management (host/join)

See `skill_collaboration/SKILL.md` for implementation details.

## Complexity Assessment
Rate the plugin complexity (1-5):

**Level 1 (Simple):** Basic gain, simple filter, single parameter
**Level 2 (Moderate):** Multi-parameter, basic dynamics, simple modulation
**Level 3 (Advanced):** Multi-band processing, complex algorithms, state management
**Level 4 (Expert):** Synthesis engines, complex feedback, real-time analysis
**Level 5 (Research):** Machine learning, complex modeling, advanced DSP

**Score: [1-5]**
**Rationale: [Explain the complexity factors]**
```

---

## 📊 STEP 2: IMPLEMENTATION PLANNING

### 2.1 Create Implementation Plan
Create `plugins/[Name]/.ideas/plan.md` with this structure:

```markdown
# Implementation Plan

## Complexity Score: [1-5]

## Implementation Strategy

### Single-Pass Implementation (Score ≤2)
Execute all DSP components in one implementation session:
- Core processing logic
- Parameter binding
- Basic optimization

**Example for simple gain plugin:**
- Implement gain calculation
- Connect to parameter
- Add smoothing

### Phased Implementation (Score ≥3)
Break implementation into logical phases:

**Phase 2.1.1: Core Processing**
- [ ] Basic signal path
- [ ] Core DSP algorithm
- [ ] Parameter integration

**Phase 2.1.2: Optimization**
- [ ] Real-time safety
- [ ] Memory management
- [ ] Performance tuning

**Phase 2.1.3: Polish**
- [ ] Edge case handling
- [ ] State management
- [ ] Testing integration

**Phase 2.1.4: Presets & Distribution**
- [ ] Factory preset generation
- [ ] Preset browser UI
- [ ] Marketplace metadata
- [ ] Installer packaging

## Dependencies
List required JUCE modules and external dependencies:

**Required JUCE Modules:**
- juce_audio_basics
- juce_audio_processors
- juce_dsp

**Optional Modules:**
- juce_gui_basics (for custom UI)
- juce_audio_formats (for file I/O)
- juce_osc (for real-time collaboration)

**CLAP SDK (if CLAP format selected):**
- clap-juce-extensions (add as git submodule)

## Risk Assessment
Identify potential implementation challenges:

**High Risk:**
- [List complex algorithms or real-time constraints]

**Medium Risk:**
- [List parameter smoothing or state management issues]

**Low Risk:**
- [List straightforward components]

## Distribution Readiness
Assess marketplace and distribution requirements:

- **Target marketplaces:** Direct download, Gumroad, itch.io, KVR
- **Installer needed:** Windows (InnoSetup), macOS (pkgbuild)
- **Code signing:** Required for macOS Gatekeeper
- **License system:** None / Serial / Online activation

See `skill_marketplace/SKILL.md` for packaging details.
```

---

## 🎨 STEP 3: UI FRAMEWORK SELECTION

### 3.1 Framework Decision
Based on the architecture complexity and plugin requirements:

**Visage Framework (Recommended for):**
- Simple to moderate complexity (Score 1-3)
- Performance-critical plugins
- Minimal UI requirements
- Pure C++ development preference

**WebView Framework (Recommended for):**
- Complex UI requirements
- Rich visualizations
- Interactive controls
- Web-based design workflow

**If user has not explicitly chosen a UI framework, ask this question before deciding:**
"Do you want WebView2 (HTML/JS UI) or Visage (native C++) for this plugin?"

**Decision: [visage/webview]**
**Rationale: [Explain the choice based on plugin requirements]**

### 3.2 Update Project State
Use the state management system to update project state:

**Import state management:**
```powershell
# Import state management module
. "$PSScriptRoot\..\scripts\state-management.ps1"
```

**Validate prerequisites:**
```powershell
# Check that ideation phase is complete
if (-not (Test-PluginState -PluginPath "plugins\[Name]" -RequiredPhase "ideation_complete" -RequiredFiles @(".ideas/creative-brief.md", ".ideas/parameter-spec.md"))) {
    Write-Error "Prerequisites not met. Complete ideation phase first."
    exit 1
}
```

**Update state with framework selection:**
```powershell
# Use centralized framework selection
Set-PluginFramework -PluginPath "plugins\[Name]" -Framework "[visage/webview]" -Rationale "[Explain framework choice]"

# Update state with planning completion
Complete-Phase -PluginPath "plugins\[Name]" -Phase "plan" -Updates @{
  "complexity_score" = [1-5]
  "framework_selection.implementation_strategy" = "[single-pass/phased]"
  "validation.architecture_defined" = $true
  "validation.ui_framework_selected" = $true
}
```

**State schema after planning:**
```json
{
  "plugin_name": "[Name]",
  "version": "v0.0.0",
  "current_phase": "plan_complete",
  "ui_framework": "[visage/webview]",
  "complexity_score": [1-5],
  "created_at": "2026-01-04T20:20:00Z",
  "last_modified": "2026-01-04T20:20:00Z",
  "phase_history": [
    {
      "phase": "ideation_complete",
      "completed_at": "2026-01-04T20:20:00Z",
      "framework_selected": null
    },
    {
      "phase": "plan_complete",
      "completed_at": "2026-01-04T20:20:00Z",
      "framework_selected": "[visage/webview]"
    }
  ],
  "validation": {
    "creative_brief_exists": true,
    "parameter_spec_exists": true,
    "architecture_defined": true,
    "ui_framework_selected": true,
    "design_complete": false,
    "code_complete": false,
    "tests_passed": false,
    "ship_ready": false
  },
  "framework_selection": {
    "decision": "[visage/webview]",
    "rationale": "[Framework choice explanation]",
    "implementation_strategy": "[single-pass/phased]"
  },
  "error_recovery": {
    "last_backup": null,
    "rollback_available": false,
    "error_log": []
  }
}
```

---

## 🔄 STEP 4: VALIDATION & CONTINUATION

### 4.1 Validation Checklist
Verify all required files exist:
- [ ] `plugins/[Name]/.ideas/architecture.md` created
- [ ] `plugins/[Name]/.ideas/plan.md` created
- [ ] `plugins/[Name]/status.json` updated with framework selection
- [ ] Complexity score assigned and justified

### 4.2 Decision Menu
Present user with next steps:

```
✓ Architecture & Planning Complete

Plugin: [Name]
Complexity: [Score]/5
Framework: [visage/webview]

What's next?
1. Start DESIGN phase - Create UI mockups and specifications
2. Review architecture - Examine DSP design and implementation plan
3. Modify plan - Adjust complexity assessment or framework choice
4. Pause here

Choose (1-4): _
```

### 4.3 Routing Logic
- **Option 1:** Proceed to Phase 3 (DESIGN) - invoke `skill_design.md`
- **Option 2:** Read and display architecture.md, plan.md, parameter-spec.md
- **Option 3:** Allow user to modify plan.md and re-run validation
- **Option 4:** Save state and exit skill

---

## 📚 INTEGRATION

**Invoked by:**
- Natural language: "Plan architecture for [Name]", "Research DSP for [Name]"
- After Phase 1 (DREAM) complete
- Manual trigger via `/plan [Name]`

**Creates:**
- `plugins/[Name]/.ideas/architecture.md` - DSP component design
- `plugins/[Name]/.ideas/plan.md` - Implementation strategy
- Updates `plugins/[Name]/status.json` - Framework selection

**Next phase:**
- Phase 3: DESIGN (if user chooses to continue)

---

## ⚠️ CRITICAL RULES

1. **Architecture First:** Always define DSP components before implementation
2. **Complexity Assessment:** Be honest about complexity - affects implementation approach
3. **Framework Selection:** Choose based on plugin requirements, not preference
4. **State Management:** Always update status.json with framework selection
5. **Validation:** Verify all contracts exist before proceeding
6. **User Choice:** Present clear options for continuation

---

## 🛠️ TROUBLESHOOTING

**Missing creative brief:**
- Error: "Creative brief required. Run /dream [Name] first."
- Solution: Ensure Phase 1 is complete

**Missing parameter spec:**
- Error: "Parameter specification required."
- Solution: Create parameter-spec.md from creative brief

**Architecture validation fails:**
- Check all required sections exist
- Verify parameter mapping is complete
- Ensure complexity score is justified

**Framework selection unclear:**
- Re-evaluate plugin requirements
- Consider performance vs. UI complexity trade-offs
- Default to Visage for simpler plugins
