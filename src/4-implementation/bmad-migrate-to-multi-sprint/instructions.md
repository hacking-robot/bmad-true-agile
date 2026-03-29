# Migrate to True Agile (Just-In-Time Story Creation)

<critical>The workflow execution engine is governed by: {project-root}/_bmad/core/tasks/workflow.xml</critical>
<critical>Communicate all responses in {communication_language}</critical>

PURPOSE: Migrate an existing BMAD project from the old "all stories upfront" approach to true just-in-time agile where stories are created during sprint-planning based on capacity.

---

## Overview

This workflow uses a **step-based approach** with sub-steps to handle large epics.md files reliably:

```
step-migrate-01-analyze.md
       ↓
step-migrate-02-generate.md
       ↓ (loops for each epic)
   step-migrate-02b-migrate-epic.md
       ↓
step-migrate-03-archive.md
```

**Why this approach?**
- Large epics.md files are hard to parse and transform in one shot
- Sub-steps allow processing one epic at a time
- Story content is **extracted, not generated** - zero data loss
- Clear boundaries prevent truncation

---

## Key Concepts

**OLD Approach (Model A):**
- create-epics creates ALL stories upfront
- sprint-planning SELECTS from existing stories
- backlog-refinement upgrades stories from placeholder→outlined→ready

**NEW Approach (Model B - True Agile):**
- create-epics creates EPIC CONTAINERS ONLY (no stories)
- sprint-planning CREATES stories based on target capacity
- Stories are created just-in-time, not pre-planned
- backlog-refinement workflow is DEPRECATED (no longer needed)

---

## Migration Strategy

### What We Keep

| Item | Action | Reason |
|------|--------|--------|
| **Done stories in epics.md** | Keep with full content | Historical record, proves epic progress |
| **Done story files** | Keep as-is | Contains completed work, decisions, useful history |
| **Undone stories in epics.md** | Remove | Sprint-planning re-analyzes from FRs, NFRs, Architecture, and epic scope |
| **Undone story files** | Archive to `archive/` | Preserve detailed planning, avoid conflicts with new creations |
| **sprint-status.yaml** | Archive | Replaced by sprint-N.yaml files |

### What We Don't Need

| Item | Reason |
|------|--------|
| **detail_level field** | No longer used - stories are either done or not done |
| **backlog-refinement references** | Workflow is deprecated |
| **Sprint history reconstruction** | Optional - only for velocity tracking |

---

## How Story Extraction Works

### Story Boundary Detection

```
### Story X.Y: Title                    ← START of story
│
│  [EVERYTHING - preserved exactly]
│  - Acceptance Criteria (Given/When/Then)
│  - Technical Notes
│  - FRs addressed
│  - NFRs addressed
│  - Dependencies
│  - Architecture Notes
│  - Infrastructure Context
│  - Testing Acceptance Criteria
│  - Any other sections...
│
▼
### Story X.Z: ...  ← END of story (next story starts)
```

### Done Status Determination

Priority order:
1. Story file exists + `Status: done` in file → **DONE**
2. Story file exists + other status → **NOT DONE** (note)
3. No story file exists → **NOT DONE**
4. Story title contains "ELIMINATED" → **SKIP** (don't migrate)
5. Story has `**BLOCKED**` marker → **NOT DONE** (note)

### New Story Format

```markdown
### Story 1.8: Mastra Public Endpoint Setup
key: 1-8-mastra-public-endpoint-setup
points: 8
status: done
jira_key: null

[ORIGINAL CONTENT PRESERVED EXACTLY AS-IS]

**Acceptance Criteria:**
...

**Technical Notes:**
...

**FRs addressed:** ...
```

---

## Workflow Execution

<workflow>

<step n="1" goal="Analyze Current State">

<action>Load step: ./steps/step-migrate-01-analyze.md</action>
<action>Follow all instructions in that step</action>
<action>Receive: migration analysis with done/not-done status</action>

</step>

<step n="2" goal="Generate New Epics File">

<action>Load step: ./steps/step-migrate-02-generate.md</action>
<action>Follow all instructions in that step</action>
<action>That step will loop through epics using step-migrate-02b-migrate-epic.md</action>
<action>Receive: new epics-migrated.md file</action>

</step>

<step n="3" goal="Archive Undone Files">

<action>Load step: ./steps/step-migrate-03-archive.md</action>
<action>Follow all instructions in that step</action>
<action>Receive: archived undone story files</action>

</step>

<step n="4" goal="Create Historical Sprint (Optional)">

<action>Load step: ./steps/step-migrate-04-sprint-history.md</action>
<action>Follow all instructions in that step</action>
<action>Optional: Create historical sprint from done stories</action>

</step>

<step n="5" goal="Create Velocity Log (Optional)">

<action>Load step: ./steps/step-migrate-05-velocity-log.md</action>
<action>Follow all instructions in that step</action>
<action>Optional: Create velocity log for tracking</action>

</step>

<step n="6" goal="Finalize Migration">

<action>Load step: ./steps/step-migrate-06-finalize.md</action>
<action>Follow all instructions in that step</action>
<action>Receive: final migration summary and next steps</action>

</step>

</workflow>

---

## What Could Go Wrong

| Issue | Solution |
|-------|----------|
| Story content truncated | Re-run migration, check story boundaries |
| Wrong done/not-done classification | Manually update in analysis step |
| Missing FR coverage | Add to FRs Covered section in epic |
| Points not in story file | Estimated during migration, verify with user |
| Large file timeout | Sub-steps process one epic at a time |

---

## Verification

After migration:

1. ✅ Review epics.md - all done stories present with full content
2. ✅ Check archive/ - contains undone story files
3. ✅ Run `/bmad-bmm-sprint-status` - shows epic progress correctly
4. ✅ Run `/bmad-bmm-sprint-planning` - analyzes requirements fresh to create stories
