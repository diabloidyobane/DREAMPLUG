# 🌙 DREAM PLUG

> **AI-powered framework for vibe-coding professional audio plugins — from concept to shipped product.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![JUCE](https://img.shields.io/badge/JUCE-8.0-blue.svg)](https://juce.com/)
[![Platform](https://img.shields.io/badge/Platform-Windows%2011%20%7C%20macOS-0078D4.svg)](#)

---

## What is DREAM PLUG?

**DREAM PLUG** is a structured, AI-driven workflow system that guides LLM agents through the entire audio plugin development lifecycle. It's a fork of [Audio Plugin Coder](https://github.com/Noizefield/audio-plugin-coder), enhanced with deeper DSP domain knowledge, richer workflow prompts, premium UI patterns, and native Antigravity integration.

Describe your plugin in plain language. The AI builds it for you — DSP, GUI, build system, packaging, and all.

## ✨ Key Features

- 🤖 **Agent Agnostic** — Works with Antigravity, Kilo, Claude Code, Cursor, or any coding agent
- 🎯 **6-Phase Workflow** — Dream → Plan → Design → Implement → Test → Ship
- 🧠 **Deep DSP Knowledge** — Pre-built architecture patterns for synths, effects, dynamics, analyzers
- 🎨 **Dual UI Frameworks** — Visage (pure C++) or WebView (HTML5 Canvas)
- 📊 **State Management** — Automatic progress tracking, validation, and rollback
- 🔧 **Self-Improving** — Auto-captures troubleshooting knowledge; gets smarter over time
- 🏗️ **Production Ready** — JUCE 8 + CMake + C++20
- 🧪 **Built-in Testing** — Pluginval integration and DAW smoke testing workflows
- 🔌 **CLAP Format** — Modern plugin format support via clap-juce-extensions
- 🎛️ **Preset Management** — XML/ValueTree-based preset architecture with factory presets
- 🛒 **Plugin Marketplace** — Distribution-ready with installer scripts, licensing, and metadata
- 🤝 **Real-time Collaboration** — OSC-based parameter sync between plugin instances

## 🚀 Quick Start

### Option 1: Claude Code (npx install)

```bash
npx -y degit diabloidyobane/DREAMPLUG my-plugin-workspace
cd my-plugin-workspace
git init && git submodule update --init --recursive
```

Then open the folder in Claude Code and run `/dream MyPlugin`.

### Option 2: Git Clone

**Windows (PowerShell)**

```powershell
git clone --recurse-submodules https://github.com/diabloidyobane/DREAMPLUG.git
cd DREAMPLUG
```

**macOS / Linux (Bash)**

```bash
git clone --recurse-submodules https://github.com/diabloidyobane/DREAMPLUG.git
cd DREAMPLUG
```

### Prerequisites

- **Windows:** Windows 11, PowerShell 7+, Visual Studio 2022 (C++ tools), CMake 3.22+, Git
- **macOS:** macOS 10.13+, Xcode + CLI tools, CMake 3.22+, Git
- **All platforms:** An LLM coding agent (Antigravity, Claude Code, Kilo, Cursor)

### Create Your First Plugin

```
/dream MyReverb
```

The AI walks you through every phase from concept to compiled VST3.

## 🔄 What's New vs Original [Audio Plugin Coder](https://github.com/Noizefield/audio-plugin-coder)

> Forked from [Noizefield/audio-plugin-coder](https://github.com/Noizefield/audio-plugin-coder). Here's everything DREAM PLUG adds:

### New Features

| Feature | Description |
|---------|-------------|
| 🔌 **CLAP Format** | Full CLAP plugin format via `clap-juce-extensions` — CMake template ready |
| 🎛️ **Preset System** | Complete XML/ValueTree preset architecture — save, load, browse, factory presets |
| 🛒 **Marketplace Kit** | Installer scripts (InnoSetup/pkgbuild), licensing, metadata, update checks |
| 🤝 **Real-time Collab** | OSC-based parameter sync between plugin instances via `juce_osc` |
| 🧪 **Test Workflow** | New `/test` command — pluginval integration + DAW smoke testing |
| 🔄 **Smart Resume** | New `/resume` command — auto-detects last phase from `status.json` |
| 📊 **Status Check** | New `/status` command — pretty-print project state at a glance |

### Enhanced Skills (deeper domain knowledge)

| Skill | What Changed |
|-------|-------------|
| **Ideation** | Added DSP category taxonomy (synth, effect, utility, sampler, analyzer), CLAP support, preset philosophy, MIDI CC mapping |
| **Planning** | Added architecture pattern library, CLAP format considerations, distribution readiness assessment |
| **Design** | Added preset browser UI patterns (Top Bar, Sidebar, Dropdown), collaboration UI, CLAP-specific design notes |
| **Implementation** | Added production DSP code patterns, CLAP compliance, preset/collaboration integration hooks |

### 3 New Skill Modules

| Skill | Purpose |
|-------|---------|
| `skill_presets` | Preset save/load, browser UI, DAW integration, factory preset generation |
| `skill_marketplace` | Installer builds, licensing, metadata templates, update mechanisms |
| `skill_collaboration` | OSC parameter sync, session management, conflict resolution |

### Infrastructure

- **Antigravity-native workflows** — Full `.agents/workflows/` directory (8 workflow files)
- **CMake template upgraded** — CLAP support, `juce_osc` module, C++20, better defaults
- **Dual agent support** — Works with both `.agents/` (Antigravity) and `.agent/` (Kilo/Claude)
- **Rebranded** — All references updated from "Audio Plugin Coder" → "DREAM PLUG"

## 📖 How It Works

### The Six-Phase Workflow
```
💭 DREAM  → Define concept, parameters, sonic character
📋 PLAN   → Design DSP architecture, select UI framework
🎨 DESIGN → Create UI mockups, iterate visuals
💻 IMPL   → Build DSP engine, integrate UI, compile
🧪 TEST   → Validate with pluginval, DAW smoke test
🚀 SHIP   → Build installers, package for distribution
```

### Slash Commands
| Command | Description |
|---------|-------------|
| `/dream [Name]` | Start new plugin ideation |
| `/plan [Name]` | Define architecture and select UI framework |
| `/design [Name]` | Create GUI mockups and visual design |
| `/impl [Name]` | Implement DSP and UI code |
| `/test [Name]` | Run pluginval + DAW smoke tests |
| `/ship [Name]` | Package and distribute plugin |
| `/status [Name]` | Check current progress and state |
| `/resume [Name]` | Continue from last phase |

## 🏗️ Directory Structure
```
DREAMPLUG/
├── .agents/                     # Antigravity agent config
│   └── workflows/               # Slash command workflows
├── .agent/                      # Kilo/Claude agent config
│   ├── workflows/               # Slash command workflows
│   ├── skills/                  # Domain knowledge modules
│   │   ├── skill_ideation/      # Concept & parameter design
│   │   ├── skill_planning/      # DSP architecture patterns
│   │   ├── skill_design/        # UI design & component library
│   │   ├── skill_implementation/# Production code patterns
│   │   ├── skill_testing/       # Validation & QA
│   │   ├── skill_packaging/     # Build & distribute
│   │   ├── skill_presets/       # Preset management system
│   │   ├── skill_marketplace/   # Plugin distribution & licensing
│   │   ├── skill_collaboration/ # Real-time collaboration (OSC)
│   │   ├── skill_debug/         # Debugging strategies
│   │   └── skill_troubleshooting/ # Self-improving issue DB
│   ├── guides/                  # Reference documentation
│   ├── rules/                   # System constraints
│   └── troubleshooting/         # Auto-captured known issues
├── templates/                   # Plugin scaffolding templates
│   ├── visage/                  # Visage (C++) UI templates
│   ├── webview/                 # WebView (HTML5) UI templates
│   ├── ffgl/                    # FFGL visual plugin templates
│   └── max-external/            # Max/MSP external templates
├── plugins/                     # Your generated plugins
├── scripts/                     # Build automation & utilities
├── _tools/                      # JUCE 8, pluginval, visage
└── docs/                        # Documentation
```

## 🛠️ Technology Stack

- **JUCE 8** — Industry-standard audio plugin framework
- **CMake** — Cross-platform build system (C++20)
- **WebView2 / WKWebView** — Web-based plugin UIs
- **Visage** — Native C++ UI framework
- **PowerShell / Bash** — Automation scripting

## 📋 Supported Plugin Formats

| Format | Windows | macOS |
|--------|---------|-------|
| VST3 | ✅ | ✅ |
| CLAP | ✅ | ✅ |
| Standalone | ✅ | ✅ |
| AU | ❌ | ✅ |

## 🙏 Credits

- **[Noizefield](https://github.com/Noizefield)** — Original Audio Plugin Coder framework
- **JUCE Team** — Industry-standard audio framework
- **Matt Tytel** — Visage UI library
- **[TÂCHES](https://github.com/glittercowboy)** — Context engineering inspiration

## 📄 License

MIT License — see [LICENCE.md](LICENCE.md)

> **Note:** JUCE 8 has its own licensing terms. Review the [JUCE license](https://juce.com/legal/juce-8-licence/) for commercial use.
