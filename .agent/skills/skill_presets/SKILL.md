# SKILL: PRESET MANAGEMENT

**Goal:** Implement a robust preset management system for the plugin.
**Trigger:** Called during `/impl` phase or standalone via `/presets [Name]`
**Output Location:** `plugins/[Name]/Source/Presets/`

---

## Overview

Every professional plugin needs presets. This skill guides implementation of:
1. **Factory presets** — Ship with the plugin
2. **User presets** — Save/load/organize
3. **Preset browser** — In-plugin preset selection UI
4. **DAW integration** — Preset list visible in host

---

## STEP 1: Preset Architecture

### File Format
Use **XML-based presets** via JUCE's `ValueTree`:

```cpp
// Save preset
void savePreset(const juce::String& name, const juce::File& file)
{
    auto state = apvts.copyState();
    auto xml = state.createXml();
    xml->setAttribute("presetName", name);
    xml->setAttribute("pluginVersion", JucePlugin_VersionString);
    xml->writeTo(file);
}

// Load preset
void loadPreset(const juce::File& file)
{
    auto xml = juce::XmlDocument::parse(file);
    if (xml && xml->hasTagName(apvts.state.getType()))
    {
        apvts.replaceState(juce::ValueTree::fromXml(*xml));
    }
}
```

### Preset Directory Structure
```
[Plugin Name] Presets/
├── Factory/
│   ├── Subtle/
│   │   ├── Warm Glow.xml
│   │   └── Gentle Touch.xml
│   ├── Extreme/
│   │   ├── Full Crush.xml
│   │   └── Mayhem.xml
│   └── _manifest.json          # Preset metadata index
├── User/
│   ├── My Presets/
│   └── _manifest.json
└── Favorites/
```

### Platform Preset Locations
```cpp
// Windows: %APPDATA%/[PluginName]/Presets/
// macOS:   ~/Library/Application Support/[PluginName]/Presets/
// Linux:   ~/.config/[PluginName]/Presets/

juce::File getPresetDirectory()
{
    auto appData = juce::File::getSpecialLocation(
        juce::File::userApplicationDataDirectory);
    return appData.getChildFile(JucePlugin_Name).getChildFile("Presets");
}
```

## STEP 2: Preset Browser UI

### WebView Preset Browser
```html
<div class="preset-browser">
    <div class="preset-header">
        <button class="prev-preset">◀</button>
        <span class="preset-name">Init</span>
        <button class="next-preset">▶</button>
        <button class="save-preset">💾</button>
    </div>
    <div class="preset-categories">
        <!-- Category buttons -->
    </div>
    <div class="preset-list">
        <!-- Scrollable preset list -->
    </div>
</div>
```

### Visage Preset Browser
```cpp
class PresetBrowserComponent : public juce::Component
{
public:
    void paint(juce::Graphics& g) override;
    void resized() override;

private:
    juce::ComboBox categorySelector;
    juce::ListBox presetList;
    juce::TextButton prevButton { "<" };
    juce::TextButton nextButton { ">" };
    juce::TextButton saveButton { "Save" };
    juce::Label presetNameLabel;
};
```

## STEP 3: DAW Preset Integration

### JUCE Program Change Support
```cpp
// In PluginProcessor.h
int getNumPrograms() override { return presetManager.getNumPresets(); }
int getCurrentProgram() override { return presetManager.getCurrentPresetIndex(); }
void setCurrentProgram(int index) override { presetManager.loadPreset(index); }
const juce::String getProgramName(int index) override { return presetManager.getPresetName(index); }
```

## STEP 4: Factory Preset Generation

Create a preset generation helper that produces well-tuned factory presets:

```cpp
struct PresetData {
    juce::String name;
    juce::String category;
    std::map<juce::String, float> parameters;
};

void generateFactoryPresets(const std::vector<PresetData>& presets)
{
    auto factoryDir = getPresetDirectory().getChildFile("Factory");
    for (auto& preset : presets)
    {
        auto categoryDir = factoryDir.getChildFile(preset.category);
        categoryDir.createDirectory();
        // ... save each preset
    }
}
```

## ⚠️ CRITICAL RULES
1. **Never overwrite user presets** without confirmation
2. **Version tag all presets** for forward compatibility
3. **Validate preset data** before loading (check param ranges, version)
4. **Thread-safe loading** — Load presets on message thread, not audio thread
