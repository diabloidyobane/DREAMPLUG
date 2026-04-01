# SKILL: IDEATION & DREAMING

**Goal:** Define the plugin concept and initialize the project state.
**Trigger:** `/dream [Name]`
**Output Location:** `plugins/[Name]/`

## ⛔ OUTPUT RESTRICTIONS (MANDATORY)
- **NO C++ Code.**
- **NO Build Scripts.**
- **NO CMake configurations.**

## STEP 1: THE INTERVIEW

**Do NOT generate files yet.**

If the user prompt is vague, ask these clarifying questions one at a time:

### 1. Plugin Category & Type
Determine which DSP category the plugin falls into:

| Category | Subcategories | Key DSP Concepts |
|----------|--------------|-------------------|
| **Synthesizer** | Subtractive, FM, Wavetable, Granular, Additive, Physical Modeling | Oscillators, filters, envelopes, LFOs, voice mgmt |
| **Effect — Dynamics** | Compressor, Limiter, Gate, Expander, De-esser, Transient Shaper | Envelope detection, gain computer, lookahead |
| **Effect — Time** | Delay, Reverb, Chorus, Flanger, Phaser, Echo | Delay lines, diffusion, allpass networks, modulation |
| **Effect — Spectral** | EQ, Filter, Vocoder, Pitch Shifter, Harmonizer | FFT, biquad cascade, phase vocoder |
| **Effect — Distortion** | Overdrive, Saturation, Bitcrush, Wavefolder, Amp Sim | Waveshaping, sample rate reduction, convolution |
| **Effect — Modulation** | Tremolo, Auto-pan, Ring Mod, Lo-Fi, Vibrato | LFO, sample & hold, envelope follower |
| **Utility** | Gain, Stereo Imager, Mid-Side, Analyzer, Meter | Level detection, correlation, FFT analysis |
| **Sampler** | Drum Machine, Sample Player, Granular Sampler | Sample playback, slicing, pitch shifting |

### 2. Sonic Character
"What is the sonic character? (e.g., 'Warm tape delay with flutter', 'Aggressive digital distortion', 'Lush analog chorus')"

### 3. Core Parameters
"What are the 3-5 most important parameters? (e.g., 'Delay time, feedback, wow/flutter, saturation, mix')"

### 4. Visual Vibe
"What should it look like? (e.g., 'Dark vintage hardware', 'Clean modern minimal', 'Retro neon', 'Brushed metal rack unit')"

### 5. Target Format
"Which plugin formats do you need?"
- **VST3** (Windows/macOS/Linux) — Default
- **AU** (macOS only)
- **CLAP** (Cross-platform, modern standard)
- **Standalone** (No DAW required)

**STOP and WAIT for the user's response.**

## STEP 2: CONCEPT GENERATION
*Only after the user answers, generate these files:*

### 1. `plugins/[Name]/.ideas/creative-brief.md`
The vision statement:
- **Hook:** One-line marketing pitch
- **Category:** From taxonomy above
- **Sonic Character:** Detailed behavioral description
- **Target User:** Who is this for?
- **Reference Plugins:** 2-3 similar commercial plugins
- **Target Formats:** VST3, AU, CLAP, Standalone (as chosen)
- **Preset Philosophy:** What kind of presets should ship with this plugin?
  - Factory preset categories (e.g., "Subtle", "Extreme", "Vocal", "Drums")
  - Estimated preset count (e.g., 20-50 factory presets)

### 2. `plugins/[Name]/.ideas/parameter-spec.md`
The definitive list of controls:

| ID | Name | Type | Range | Default | Unit | Smoothing | MIDI CC |
|:---|:-----|:-----|:------|:--------|:-----|:----------|:--------|
| `gain` | Gain | Float | 0.0–1.0 | 0.5 | dB | 20ms | CC7 |

Include:
- Smoothing time recommendations
- Suggested MIDI CC mappings
- Parameter groups (for organizing in DAW)

### 3. `plugins/[Name]/status.json` (ROOT)
Initialize using state management:

```powershell
. "$PSScriptRoot\..\scripts\state-management.ps1"
New-PluginState -PluginName "[Name]" -PluginPath "plugins\[Name]"
```

**Enhanced schema:**
```json
{
  "plugin_name": "[Name]",
  "version": "v0.0.0",
  "current_phase": "ideation",
  "ui_framework": "pending",
  "target_formats": ["VST3"],
  "clap_support": false,
  "preset_system": {
    "enabled": false,
    "factory_preset_count": 0,
    "categories": []
  },
  "complexity_score": 0,
  "created_at": "2026-01-01T00:00:00Z",
  "last_modified": "2026-01-01T00:00:00Z",
  "phase_history": [],
  "validation": {
    "creative_brief_exists": true,
    "parameter_spec_exists": true,
    "architecture_defined": false,
    "ui_framework_selected": false,
    "design_complete": false,
    "code_complete": false,
    "tests_passed": false,
    "ship_ready": false
  },
  "framework_selection": {
    "decision": "pending",
    "rationale": "",
    "implementation_strategy": "pending"
  },
  "error_recovery": {
    "last_backup": null,
    "rollback_available": false,
    "error_log": []
  }
}
```

## STEP 3: TERMINATION
1. Confirm files are created.
2. STOP.
3. Say: "Concept defined. Type `/plan [Name]` to architect the DSP and UI."