---
description: "PHASE 2: Architecture — Define DSP design, select UI framework, plan implementation"
---

# 📋 Plan Phase

**You are a senior audio DSP architect.** Design the plugin's signal processing architecture.

## Prerequisites
- `plugins/[Name]/.ideas/creative-brief.md` exists
- `plugins/[Name]/.ideas/parameter-spec.md` exists
- Dream phase complete

## STEP 1: Read Input
Read these files:
- `plugins/[Name]/.ideas/creative-brief.md`
- `plugins/[Name]/.ideas/parameter-spec.md`

## STEP 2: DSP Architecture

Create `plugins/[Name]/.ideas/architecture.md` with:

### Signal Flow Diagram
Use ASCII art to show the processing chain:
```
Input → [Gain Stage] → [Core DSP] → [Output Mix] → Output
                            ↑
                    [Modulation Source]
```

### Architecture Patterns Reference
Choose the appropriate pattern:

**Serial Chain** (Most effects):
```
Input → Stage1 → Stage2 → ... → Mix → Output
```

**Parallel Processing** (Multi-band, parallel compression):
```
Input → Split → [Band1] → Sum → Output
              → [Band2] →
```

**Feedback Loop** (Delays, reverbs):
```
Input → [+ Sum] → Processing → Output
           ↑                      |
           └── [Feedback] ←──────┘
```

**Modulated** (Chorus, flanger, phaser):
```
Input → [Delay Line] → Mix → Output
             ↑
        [LFO/Modulator]
```

### Core Components
List each DSP building block with:
- Name and purpose
- Key algorithm (e.g., "Biquad filter", "Circular buffer delay")
- JUCE class to use (e.g., `juce::dsp::IIR::Filter`, `juce::dsp::DelayLine`)
- Real-time safety notes

### Parameter Mapping
| Parameter | Component | Function | Smoothing |
|-----------|-----------|----------|-----------|
| Each param | Which DSP block | What it controls | Time in ms |

## STEP 3: UI Framework Decision

**Ask the user if they haven't chosen:**
"Do you want **WebView** (HTML/JS — rich visuals, fast iteration) or **Visage** (native C++ — maximum performance)?"

| Factor | WebView | Visage |
|--------|---------|--------|
| Performance | Good | Best |
| Visual richness | Excellent | Good |
| Dev speed | Fast | Moderate |
| Hot reload | Yes | No |

## STEP 4: Complexity Score

Rate 1-5:
- **1 Simple:** Single-parameter utility (gain, pan)
- **2 Moderate:** Multi-parameter single-stage (EQ band, simple filter)
- **3 Advanced:** Multi-stage with modulation (chorus, delay with feedback)
- **4 Expert:** Complex algorithms (reverb, vocoder, multi-band dynamics)
- **5 Research:** ML-based, physical modeling, spectral processing

## STEP 5: Update State
```powershell
. "$PSScriptRoot\..\scripts\state-management.ps1"
Set-PluginFramework -PluginPath "plugins\[Name]" -Framework "[visage/webview]" -Rationale "[reason]"
Complete-Phase -PluginPath "plugins\[Name]" -Phase "plan" -Updates @{
    "complexity_score" = [1-5]
    "validation.architecture_defined" = $true
    "validation.ui_framework_selected" = $true
}
```

## COMPLETION
```
✅ Plan phase complete!

Framework: [Visage/WebView]
Complexity: [N]/5
Architecture: [Pattern name]

Files created:
- plugins/[Name]/.ideas/architecture.md
- plugins/[Name]/.ideas/plan.md

Next step: /design [Name]
```
