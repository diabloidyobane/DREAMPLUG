---
description: "PHASE 4: Implementation — Build DSP engine, integrate UI, compile plugin"
---

# 💻 Implementation Phase

**You are an expert JUCE 8 audio plugin developer.** Build the plugin from the approved architecture and design.

## Prerequisites
- Design phase complete
- `plugins/[Name]/.ideas/architecture.md` exists
- `plugins/[Name]/Design/` contains approved design files
- `ui_framework` is set in `status.json`

## STEP 1: Backup State
```powershell
. "$PSScriptRoot\..\scripts\state-management.ps1"
Backup-PluginState -PluginPath "plugins\[Name]"
```

## STEP 2: Scaffold Plugin

### Create CMakeLists.txt
Use `templates/CMakeLists.txt.template` (WebView) or `templates/CMakeLists.visage.template` (Visage).

### Create Source Files
Generate in `plugins/[Name]/Source/`:

**PluginProcessor.h/.cpp** — Audio processing engine
- `prepareToPlay()`: Initialize DSP at correct sample rate/buffer size
- `processBlock()`: Main audio callback — MUST be real-time safe
- `getStateInformation()` / `setStateInformation()`: Preset save/load
- APVTS (AudioProcessorValueTreeState) for all parameters

**PluginEditor.h/.cpp** — GUI
- Framework-specific (WebView HTML or Visage frames)
- Connect UI controls to APVTS parameters via attachments

### JUCE 8 Critical Rules
1. **APVTS is mandatory** — All parameters go through `AudioProcessorValueTreeState`
2. **No allocations in processBlock** — No `new`, no `std::vector::push_back`, no string ops
3. **Parameter smoothing** — Use `juce::SmoothedValue` for all continuous parameters
4. **Thread safety** — GUI thread and audio thread NEVER share raw data
5. **WebView member order** — Relays → WebView → Attachments (prevents crash on unload)

## STEP 3: DSP Implementation Patterns

### Filter (Biquad)
```cpp
juce::dsp::ProcessorChain<juce::dsp::IIR::Filter<float>> filter;
// In prepareToPlay:
auto coeffs = juce::dsp::IIR::Coefficients<float>::makeLowPass(sampleRate, frequency, Q);
filter.get<0>().coefficients = coeffs;
```

### Delay Line
```cpp
juce::dsp::DelayLine<float, juce::dsp::DelayLineInterpolationTypes::Lagrange3rd> delayLine;
// In prepareToPlay:
delayLine.setMaximumDelayInSamples(maxDelaySamples);
```

### Gain with Smoothing
```cpp
juce::SmoothedValue<float> gainSmoothed;
// In prepareToPlay:
gainSmoothed.reset(sampleRate, 0.02); // 20ms smoothing
// In processBlock:
gainSmoothed.setTargetValue(gainParam->load());
for (int s = 0; s < numSamples; ++s) {
    float g = gainSmoothed.getNextValue();
    // apply g to all channels
}
```

### Distortion
```cpp
// Soft clip (tanh saturation)
float process(float x, float drive) {
    return std::tanh(x * drive) / std::tanh(drive);
}
```

## STEP 4: Build & Test
```powershell
.\scripts\build-and-install.ps1 -PluginName [Name]
```

### WebView Pre-Build Validation
```powershell
if (Test-Path ".\scripts\validate-webview-member-order.ps1") {
    & ".\scripts\validate-webview-member-order.ps1" -PluginName $PluginName
}
```

## STEP 5: Error Recovery
If build fails:
```powershell
Restore-PluginState -PluginPath "plugins\[Name]"
```

Check known issues:
```powershell
# Search troubleshooting database
Get-Content .agent\troubleshooting\known-issues.yaml
```

## STEP 6: Update State
```powershell
Complete-Phase -PluginPath "plugins\[Name]" -Phase "implementation" -Updates @{
    "validation.code_complete" = $true
}
```

## COMPLETION
```
✅ Implementation phase complete!

Plugin built successfully!
Location: build/[Name]/Debug/VST3/[Name].vst3

Next step: /test [Name] or /ship [Name]
```
