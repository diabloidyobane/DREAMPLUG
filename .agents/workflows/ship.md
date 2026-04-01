---
description: "PHASE 6: Packaging — Build release, create installers, distribute"
---

# 🚀 Ship Phase

**You are a release engineer for audio plugins.** Package the plugin for distribution.

## Prerequisites
- Implementation complete (or testing complete)
- Plugin builds successfully

## STEP 1: Release Build
```powershell
# Build in Release mode
.\scripts\build-and-install.ps1 -PluginName [Name] -Configuration Release
```

## STEP 2: Package

### Windows
Create installer using InnoSetup or ZIP:
```powershell
# ZIP package
$version = (Get-Content "plugins\[Name]\status.json" | ConvertFrom-Json).version
$outDir = "dist"
New-Item -ItemType Directory -Force -Path $outDir
Compress-Archive -Path "build\[Name]\Release\VST3\[Name].vst3" -DestinationPath "$outDir\[Name]-$version-win.zip"
```

### macOS
```bash
version=$(jq -r '.version' plugins/[Name]/status.json)
mkdir -p dist
zip -r "dist/[Name]-$version-mac.zip" "build/[Name]/Release/VST3/[Name].vst3"
```

## STEP 3: Create Release Notes
Auto-generate from status.json and creative brief.

## STEP 4: Update State
```powershell
. "$PSScriptRoot\..\scripts\state-management.ps1"
Complete-Phase -PluginPath "plugins\[Name]" -Phase "shipped" -Updates @{
    "validation.ship_ready" = $true
}
```

## COMPLETION
```
🎉 Plugin shipped!

Format: VST3
Package: dist/[Name]-v[version]-win.zip

Plugin development complete!
```
