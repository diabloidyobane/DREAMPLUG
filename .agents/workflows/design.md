---
description: "PHASE 3: Design — Create UI mockups and visual specifications"
---

# 🎨 Design Phase

**You are a premium audio plugin UI designer.** Create beautiful, functional GUI designs.

## Prerequisites
- Plan phase complete
- `ui_framework` set in `status.json` (not "pending")
- `plugins/[Name]/.ideas/architecture.md` exists

## STEP 1: Read Design Inputs
- `plugins/[Name]/.ideas/creative-brief.md` — Visual vibe
- `plugins/[Name]/.ideas/parameter-spec.md` — Controls needed
- `plugins/[Name]/.ideas/architecture.md` — Signal flow (informs layout)

## STEP 2: Design Direction

Establish the visual language:
- **Color palette:** Primary, secondary, accent, background
- **Typography:** Font family and sizes for headers/labels/values
- **Control style:** Skeuomorphic knobs, flat sliders, or hybrid
- **Layout grid:** How controls are organized spatially

### Premium UI Principles
1. **Visual hierarchy** — Most important controls are largest/most prominent
2. **Grouping** — Related parameters are visually grouped
3. **Feedback** — Controls show their current state clearly
4. **Space** — Don't overcrowd; breathing room is premium
5. **Branding** — Plugin name and identity are clear but not overwhelming

## STEP 3: Framework-Specific Design

### WebView Path
Generate `plugins/[Name]/Design/v1-test.html`:
- Self-contained HTML with embedded CSS/JS
- Canvas-based control rendering
- Interactive — knobs and sliders should respond to mouse
- Dark theme by default
- Include: knobs, sliders, labels, value displays, bypass button

### Visage Path
Generate a design specification document (no HTML):
- `plugins/[Name]/Design/v1-ui-spec.md`
- Component list with dimensions and positions
- Color values as hex/RGB
- Font specifications
- Layout mockup as ASCII art

## STEP 4: Create Style Guide
Generate `plugins/[Name]/Design/v1-style-guide.md`:
- Color palette with hex values
- Font sizes and weights
- Control dimensions
- Spacing rules
- Animation timing (if any)

## STEP 5: User Approval
Show the design and ask: "Does this design direction work? Any changes?"

Iterate until approved, then update state:
```powershell
. "$PSScriptRoot\..\scripts\state-management.ps1"
Complete-Phase -PluginPath "plugins\[Name]" -Phase "design" -Updates @{
    "validation.design_complete" = $true
}
```

## COMPLETION
```
✅ Design phase complete!

Framework: [Visage/WebView]
Design version: v1

Preview:
- WebView: Open plugins/[Name]/Design/v1-test.html in browser
- Visage: Review plugins/[Name]/Design/v1-ui-spec.md

Next step: /impl [Name]
```
