---
description: "PHASE 5: Testing — Validate plugin with pluginval and DAW smoke tests"
---

# 🧪 Test Phase

**You are a QA engineer for audio plugins.** Validate the plugin works correctly.

## Prerequisites
- Implementation phase complete
- Plugin builds successfully
- VST3 binary exists

## STEP 1: Pluginval Validation

Run the pluginval validator against the built plugin:

```powershell
# Windows
$pluginPath = "build\[Name]\Debug\VST3\[Name].vst3"

if (Test-Path ".\_tools\pluginval\pluginval.exe") {
    & ".\_tools\pluginval\pluginval.exe" --validate "$pluginPath" --strictness-level 5
} else {
    Write-Warning "pluginval not found. Run: .\scripts\pluginval-integration.ps1"
}
```

```bash
# macOS
pluginPath="build/[Name]/Debug/VST3/[Name].vst3"
./_tools/pluginval/pluginval --validate "$pluginPath" --strictness-level 5
```

### Pluginval Strictness Levels
| Level | What it tests |
|-------|--------------|
| 1 | Basic instantiation |
| 5 | Full validation (recommended) |
| 10 | Extreme stress test |

## STEP 2: Manual Smoke Tests

Verify these manually or guide the user:
- [ ] Plugin loads in DAW without crash
- [ ] All parameters respond to MIDI CC / automation
- [ ] Audio passes through (not silence)
- [ ] Bypass works correctly
- [ ] Preset save/load works
- [ ] No zipper noise on parameter changes
- [ ] Stereo image is correct (not collapsed to mono accidentally)
- [ ] CPU usage is reasonable (< 5% on modern hardware)

## STEP 3: Edge Case Tests
- [ ] Process silence — output should be silence (or reverb tail, etc.)
- [ ] Process full-scale signal — no unexpected clipping
- [ ] Rapid parameter changes — no crashes or glitches
- [ ] Sample rate change — plugin adapts correctly
- [ ] Buffer size change — no artifacts

## STEP 4: Update State
```powershell
. "$PSScriptRoot\..\scripts\state-management.ps1"
Complete-Phase -PluginPath "plugins\[Name]" -Phase "testing" -Updates @{
    "validation.tests_passed" = $true
}
```

## COMPLETION
```
✅ Test phase complete!

Pluginval: PASSED (strictness level 5)
Smoke tests: [N]/[Total] passed

Next step: /ship [Name]
```
