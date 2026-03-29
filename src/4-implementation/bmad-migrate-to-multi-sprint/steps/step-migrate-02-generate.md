---
outputFile: '{planning_artifacts}/epics-migrated.md'
---

# Step Migrate-2: Generate New Epics File

## STEP GOAL:

To generate a fresh epics.md file in the new format, iterating through each epic and its done stories.

## MANDATORY EXECUTION RULES (READ FIRST):

### Universal Rules:

- 🛑 NEVER generate content without user input
- 📖 CRITICAL: Read the complete step file before taking any action
- 📋 PRESERVE ALL STORY CONTENT - do not modify, truncate, or reformat
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

### Critical Content Rules:

- 📦 Story content is EXTRACTED, not generated
- 🔒 ALL content between `### Story X.Y:` and next story/epic boundary is preserved
- 🚫 DO NOT parse, interpret, or modify story internals (AC, Tech Notes, FRs, etc.)
- ✅ ONLY prepend the new metadata block

## EXECUTION PROTOCOLS:

<workflow>

<step n="1" goal="Initialize New Epics File">

<action>Load migration analysis from previous step</action>

<output>
📝 **Generating New Epics File**
</output>

<action>Create empty {outputFile}</action>

<action>Write file header:
```markdown
---
stepsCompleted: ['migration-analysis', 'migration-generate']
workflowStatus: migrated
migratedAt: {{date}}
previousFile: epics.md → archive/epics-pre-migration.md
---

# {{project_name}} - Epic Breakdown

> **Migration Note:** This file was migrated from the legacy format.
> Done stories preserved with full content. Undone stories removed.
> See archive/epics-pre-migration.md for original.

## Overview

This document provides the complete epic breakdown with done stories.
New stories will be created during sprint-planning based on capacity.

```
</action>

<action>Call sub-step: step-migrate-02a-extract-requirements.md to extract and build Requirements Inventory:
  - Scan old epics.md for all FRs, NFRs, ARCH requirements
  - Scan done stories for "FRs addressed:", "NFRs addressed:", "Architecture requirements:" sections
  - Build consolidated Requirements Inventory section
  - Build Requirements Coverage Map showing which done stories cover which requirements
</action>

<action>Append Requirements Inventory to {outputFile}</action>

<action>Set {{current_epic_index}} = 0</action>

<action>Proceed to Step 2 (Epic Loop)</action>

</step>

<step n="2" goal="Migrate Each Epic (Loop)">

<check if="{{current_epic_index}} < {{total_epics}}">
  <action>Set {{current_epic}} = epics[{{current_epic_index}}]</action>

  <output>
──────────────────────────────────────────────────────────────
📦 **Migrating Epic {{current_epic.number}}: {{current_epic.title}}**
──────────────────────────────────────────────────────────────
  </output>

  <action>Call sub-step: step-migrate-02b-migrate-epic.md with:
    - epic_number: {{current_epic.number}}
    - epic_title: {{current_epic.title}}
    - epic_metadata: {{current_epic.metadata}}
    - done_stories: {{current_epic.done_stories}}
  </action>

  <action>Receive {{migrated_epic_content}} from sub-step</action>

  <action>Append {{migrated_epic_content}} to {outputFile}</action>

  <action>Increment {{current_epic_index}} += 1</action>

  <action>Loop back to start of Step 2</action>
</check>

<check if="{{current_epic_index}} >= {{total_epics}}">
  <action>All epics migrated</action>
  <action>Proceed to Step 3</action>
</check>

</step>

<step n="3" goal="Finalize Migration">

<action>Append file footer (if needed)</action>

<output>
═══════════════════════════════════════════════════════════════
✅ **New Epics File Generated**
═══════════════════════════════════════════════════════════════

**Output:** {outputFile}

**Epics Migrated:** {{total_epics}}
**Stories Migrated:** {{total_done_stories}}
**Stories Skipped:** {{total_not_done_stories}}
</output>

<action>Proceed to step-migrate-03-archive.md</action>

</step>

</workflow>

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- All epics processed
- All done stories migrated with full content
- New file valid markdown
- Loop completed without truncation

### ❌ SYSTEM FAILURE:

- Skipping epics
- Modifying story content
- Incomplete loop (stopping early)
- Invalid output file

**Master Rule:** Loop through ALL epics, extract ALL done story content untouched.
