---
outputFile: '{planning_artifacts}/migration-analysis-{{date}}.md'
---

# Step Migrate-1: Analyze Current State

## STEP GOAL:

To analyze the existing project structure and identify all epics, stories, and their completion status.

## MANDATORY EXECUTION RULES (READ FIRST):

### Universal Rules:

- 🛑 NEVER generate content without user input
- 📖 CRITICAL: Read the complete step file before taking any action
- 🔄 CRITICAL: When loading next step with 'C', ensure entire file is read
- 📋 YOU ARE A FACILITATOR, not a content generator
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

### Role Reinforcement:

- ✅ You are an expert Scrum Master performing migration
- ✅ Your goal is clean, accurate data migration
- ✅ You preserve all valuable planning information
- ✅ Success is measured in zero data loss and correct status

## EXECUTION PROTOCOLS:

<workflow>

<step n="1" goal="Discover Project Files">

<action>Load {project_context} for project-wide patterns (if exists)</action>
<action>Communicate in {communication_language} with {user_name}</action>

<output>
🔍 **Analyzing Project Structure for Migration**
</output>

<action>Scan for epic files:
  - Check {planning_artifacts}/epics.md (whole file)
  - OR check {planning_artifacts}/epic*/*.md (sharded)
</action>

<action>Scan for story files in {implementation_artifacts}/*.md</action>

<action>Build story file index:
  - For each .md file, extract story key from filename pattern: {epic}-{story}-{slug}.md
  - Extract Status: field from file content (first 20 lines)
</action>

<check if="no epic files found">
  <output>❌ **No Epic Files Found**

Cannot proceed without epics.md or sharded epic files.
  </output>
  <action>End workflow</action>
</check>

<action>Proceed to Step 2</action>

</step>

<step n="2" goal="Parse Epic Structure">

<action>Load the epics file(s) completely</action>

<action>Identify all EPIC blocks using pattern:
  - Start: `## Epic X:` (H2 heading)
  - End: Next `## Epic` OR end of file
</action>

<action>For each epic block, extract:
  - Epic number
  - Epic title
  - Epic metadata (Goal, Success Criteria, etc.) - everything BEFORE first ### Story
</action>

<action>Proceed to Step 3</action>

</step>

<step n="3" goal="Parse Stories Within Each Epic">

<action>For each epic block identified in Step 2:</action>

<action>Identify all STORY blocks using pattern:
  - Start: `### Story X.Y:` (H3 heading)
  - End: Next `### Story` OR `## Epic` OR `---` OR end of epic block
</action>

<action>For each story block, extract:
  - Story number (X.Y)
  - Story title
  - FULL CONTENT (everything between start and end markers - DO NOT PARSE INTERNALS)
</action>

<action>Build story data structure:
  ```
  {
    epic_number: X,
    story_number: Y,
    full_key: "X-Y-slug",
    title: "...",
    full_content: "...(raw markdown)...",
    story_file_exists: boolean (check against file index from Step 1),
    story_file_status: "done" | "not-done" | null
  }
  ```
</action>

<action>Proceed to Step 4</action>

</step>

<step n="4" goal="Determine Story Done Status">

<output>
📊 **Determining Story Completion Status**
</output>

<action>For each story, determine if DONE using this priority:
  1. IF story file exists AND has `Status: done` → DONE
  2. IF story file exists AND has `Status: partial` or other → NOT DONE (with note)
  3. IF story file does NOT exist → NOT DONE
  4. IF story title contains "ELIMINATED" → SKIP (don't migrate)
  5. IF story has `**BLOCKED**` marker → NOT DONE (with note)
</action>

<action>Build migration status matrix</action>

<action>Proceed to Step 5</action>

</step>

<step n="5" goal="Display Migration Summary">

<action>Generate summary showing:</action>

<output>
═══════════════════════════════════════════════════════════════
📊 **Migration Analysis Complete**
═══════════════════════════════════════════════════════════════

**Epics Found:** {{epic_count}}

{{for each epic}}
📦 **Epic {{epic_number}}: {{epic_title}}**
   ├── Done stories: {{done_count}}
   ├── Not done stories: {{not_done_count}}
   └── Eliminated stories: {{eliminated_count}}

   Stories:
{{for each story}}
   {{status_icon}} Story {{story_number}}: {{story_title}}
       └── Status: {{migration_action}}
{{end}}

{{end}}

──────────────────────────────────────────────────────────────
**Summary:**
- Total stories: {{total_stories}}
- Will MIGRATE (done): {{migrate_count}}
- Will SKIP (not done): {{skip_count}}
- Will IGNORE (eliminated): {{eliminated_count}}

**Files to archive:** {{not_done_count}} story files
</output>

<action>Save analysis to {outputFile}</action>

<ask>Does this analysis look correct? (Y/n/adjust)</ask>

<check if="user says adjust">
  <ask>Which story status needs adjustment? (e.g., "1.5 should be done")</ask>
  <action>Update migration status matrix</action>
  <action>Re-display summary</action>
</check>

<action>Proceed to step-migrate-02-generate.md</action>

</step>

</workflow>

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- All epic blocks identified correctly
- All story blocks extracted with full content
- Done status determined accurately via file existence
- Migration matrix complete and user-verified

### ❌ SYSTEM FAILURE:

- Missing epic blocks
- Truncated story content
- Wrong done/not-done classification
- Proceeding without user verification

**Master Rule:** Story file existence + Status: done = migrate. Everything else = skip or archive.
