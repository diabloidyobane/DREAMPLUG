---
description: "Resume development of an existing plugin from its last phase"
---

# 🔄 Resume Workflow

**Continue developing an existing plugin from where you left off.**

## STEP 1: Detect State

Read `plugins/[Name]/status.json` and determine current phase:

```powershell
$state = Get-Content "plugins\[Name]\status.json" | ConvertFrom-Json
Write-Host "Plugin: $($state.plugin_name)"
Write-Host "Phase:  $($state.current_phase)"
Write-Host "Framework: $($state.ui_framework)"
Write-Host "Complexity: $($state.complexity_score)"
```

## STEP 2: Route to Correct Phase

| `current_phase` | Next Action | Command |
|-----------------|-------------|---------|
| `ideation` | Run planning | `/plan [Name]` |
| `ideation_complete` | Run planning | `/plan [Name]` |
| `plan_complete` | Run design | `/design [Name]` |
| `design_complete` | Run implementation | `/impl [Name]` |
| `code_complete` | Run testing | `/test [Name]` |
| `tests_passed` | Run shipping | `/ship [Name]` |
| `shipped` | Plugin is done! | — |

## STEP 3: Show Existing Context

Before resuming, display:
1. Creative brief summary (if exists)
2. Architecture overview (if exists)
3. Current validation status from `status.json`
4. Any errors in `error_recovery.error_log`

Then invoke the appropriate phase skill.
