---
description: "PHASE 1: Ideation — Define plugin concept, sonic character, and parameters"
---

# 💭 Dream Phase

**You are an expert audio plugin designer.** Guide the user through ideation of a new plugin concept.

## Prerequisites
- None — this is the entry point for new plugins.

## State Check
```powershell
if (Test-Path "plugins\$PluginName") {
    Write-Warning "Plugin '$PluginName' already exists. Use /resume to continue."
    exit 1
}
```

## THE INTERVIEW (Do NOT generate files yet)

Ask these questions one at a time. Wait for user answers.

### 1. Plugin Category
Determine which DSP category the plugin falls into:

| Category | Examples | Typical Parameters |
|----------|----------|-------------------|
| **Synthesizer** | Subtractive, FM, wavetable, granular | Oscillators, filters, envelopes, LFOs |
| **Effect — Dynamics** | Compressor, limiter, gate, expander | Threshold, ratio, attack, release, knee |
| **Effect — Time** | Delay, reverb, chorus, flanger, phaser | Time, feedback, mix, modulation rate/depth |
| **Effect — Spectral** | EQ, filter, vocoder, pitch shifter | Frequency, resonance, gain, bands |
| **Effect — Distortion** | Overdrive, saturation, bitcrush, wavefold | Drive, tone, mix, algorithm type |
| **Effect — Modulation** | Tremolo, auto-pan, ring mod, lo-fi | Rate, depth, shape, sync |
| **Utility** | Gain, stereo imager, mid-side, analyzer | Level, width, balance, metering |
| **Sampler** | Drum machine, sample player, granular | Sample select, pitch, envelope, slice |

### 2. Sonic Character
"What is the sonic character? (e.g., 'Warm tape delay with flutter', 'Aggressive digital distortion')"

### 3. Core Parameters
"What are the 3-5 most important parameters?"

### 4. Visual Vibe
"What should it look like? (e.g., 'Dark vintage hardware', 'Clean modern minimal', 'Retro neon')"

## CONCEPT GENERATION (After user answers)

Create these files:

### `plugins/[Name]/.ideas/creative-brief.md`
- **Hook:** One-line marketing pitch
- **Category:** DSP category from taxonomy above
- **Sonic Character:** Detailed description of the sound
- **Target User:** Who is this for?
- **Reference Plugins:** Similar commercial plugins for inspiration

### `plugins/[Name]/.ideas/parameter-spec.md`
Full parameter table:
| ID | Name | Type | Range | Default | Unit | Smoothing |
|:---|:-----|:-----|:------|:--------|:-----|:----------|
| `gain` | Gain | Float | 0.0–1.0 | 0.5 | dB | 20ms |

Include smoothing time recommendations for each parameter.

### `plugins/[Name]/status.json`
Initialize using state management:
```powershell
. "$PSScriptRoot\..\scripts\state-management.ps1"
New-PluginState -PluginName "[Name]" -PluginPath "plugins\[Name]"
```

## COMPLETION
```
✅ Dream phase complete!

Files created:
- plugins/[Name]/.ideas/creative-brief.md
- plugins/[Name]/.ideas/parameter-spec.md
- plugins/[Name]/status.json

Next step: /plan [Name]
```
