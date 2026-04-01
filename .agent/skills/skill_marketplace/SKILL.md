# SKILL: PLUGIN MARKETPLACE INTEGRATION

**Goal:** Prepare the plugin for distribution via marketplaces and direct sales.
**Trigger:** Called during `/ship` phase or standalone via `/marketplace [Name]`
**Output Location:** `plugins/[Name]/dist/`

---

## Overview

This skill handles packaging and distribution of finished plugins to reach end users:
1. **Installer creation** — Professional installers for Windows/macOS
2. **Marketplace metadata** — Screenshots, descriptions, categories
3. **License system** — Serial number validation (optional)
4. **Update system** — Version checking and auto-update support

---

## STEP 1: Distribution Formats

### Windows Installer (InnoSetup)
Generate `scripts/installer/[Name]-setup.iss`:

```inno
[Setup]
AppName={#PluginName}
AppVersion={#PluginVersion}
DefaultDirName={autopf}\{#PluginName}
OutputDir=..\..\dist
OutputBaseFilename={#PluginName}-{#PluginVersion}-Setup

[Files]
; VST3
Source: "..\..\build\{#PluginName}\Release\VST3\{#PluginName}.vst3\*"; DestDir: "{commoncf}\VST3\{#PluginName}.vst3"; Flags: recursesubdirs
; CLAP (if enabled)
Source: "..\..\build\{#PluginName}\Release\CLAP\{#PluginName}.clap"; DestDir: "{commoncf}\CLAP"; Flags: ignoreversion
; Presets
Source: "..\..\plugins\{#PluginName}\Presets\Factory\*"; DestDir: "{userappdata}\{#PluginName}\Presets\Factory"; Flags: recursesubdirs
```

### macOS Installer (pkgbuild)
```bash
pkgbuild \
    --root "build/${PLUGIN_NAME}/Release/VST3" \
    --install-location "/Library/Audio/Plug-Ins/VST3" \
    --identifier "com.dreamplug.${PLUGIN_NAME}" \
    --version "${VERSION}" \
    "dist/${PLUGIN_NAME}-${VERSION}.pkg"
```

## STEP 2: Marketplace Metadata

### Product Page Template
Create `plugins/[Name]/dist/marketplace-listing.md`:

```markdown
# [Plugin Name]

## Tagline
[One-line hook from creative brief]

## Description
[2-3 paragraph description]

## Key Features
- Feature 1
- Feature 2
- Feature 3

## System Requirements
- Windows 10/11 or macOS 10.13+
- VST3 / AU / CLAP compatible DAW
- 4GB RAM minimum

## Screenshots
- Main UI screenshot (1200x800)
- Detail shot of key controls
- In-DAW screenshot

## Tags / Categories
[e.g., Effect, Delay, Creative, Vintage]

## Price
[Free / $XX.XX]
```

### Screenshot Generation Guide
1. **Main screenshot:** Full plugin UI, dark background, 1200x800px
2. **Detail shot:** Close-up of unique controls or features
3. **In-DAW:** Plugin loaded in a popular DAW (Ableton, FL Studio, Logic)

## STEP 3: License System (Optional)

### Simple Serial Validation
```cpp
class LicenseManager
{
public:
    bool validateSerial(const juce::String& serial)
    {
        // Simple checksum validation
        // For production: use server-side validation
        auto hash = juce::SHA256(serial.toUTF8()).toHexString();
        return hash.startsWith("dreamplug");
    }

    void saveLicense(const juce::String& serial)
    {
        auto licenseFile = getLicenseFile();
        licenseFile.replaceWithText(serial);
    }

    juce::File getLicenseFile()
    {
        return juce::File::getSpecialLocation(
            juce::File::userApplicationDataDirectory)
            .getChildFile(JucePlugin_Name)
            .getChildFile("license.key");
    }
};
```

## STEP 4: Version Update Check

```cpp
// Simple version check against a JSON endpoint
void checkForUpdates()
{
    juce::URL updateUrl("https://your-domain.com/api/plugins/[Name]/version");
    auto response = updateUrl.readEntireTextStream();
    auto json = juce::JSON::parse(response);
    auto latestVersion = json.getProperty("version", "").toString();
    // Compare with JucePlugin_VersionString
}
```

## ⚠️ CRITICAL RULES
1. **Code-sign** all binaries for macOS (required by Gatekeeper)
2. **Notarize** macOS builds (required for distribution outside App Store)
3. **Test installers** on clean machines before distribution
4. **Include uninstaller** for Windows builds
