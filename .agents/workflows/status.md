---
description: "Check the current development status of a plugin"
---

# 📊 Status Workflow

**Display the current state of a plugin project.**

## STEP 1: Read State
```powershell
$state = Get-Content "plugins\[Name]\status.json" | ConvertFrom-Json
```

## STEP 2: Pretty Print

Display a formatted status report:

```
╔══════════════════════════════════════╗
║         DREAM PLUG — Status         ║
╠══════════════════════════════════════╣
║ Plugin:     [Name]                  ║
║ Phase:      [current_phase]         ║
║ Framework:  [ui_framework]          ║
║ Complexity: [score]/5               ║
║ Version:    [version]               ║
╠══════════════════════════════════════╣
║ ✅ Creative Brief                   ║
║ ✅ Parameter Spec                   ║
║ [?] Architecture                    ║
║ [?] UI Framework Selected           ║
║ [?] Design Complete                 ║
║ [?] Code Complete                   ║
║ [?] Tests Passed                    ║
║ [?] Ship Ready                      ║
╚══════════════════════════════════════╝
```

Replace `[?]` with ✅ or ❌ based on `validation` fields.

## STEP 3: Suggest Next Action
Based on current phase, suggest what to do next.
