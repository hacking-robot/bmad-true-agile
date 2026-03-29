# Step Migrate-4: Create Sprint History (Optional)

## STEP GOAL:

To create historical sprint file(s) from completed work, proposing logical splits based on story dates, epic progression, or custom grouping.

## MANDATORY EXECUTION RULES (READ FIRST):

### Universal Rules:

- 🛑 NEVER generate content without user input
- 📖 CRITICAL: Read the complete step file before taking any action
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

### Optional Step Rules:

- This step is OPTIONAL - skip if user doesn't want velocity tracking
- Historical sprints provide baseline velocity for future planning
- Propose splits based on available data (dates, epic progression, etc.)
- User has final say on how stories are distributed

---

## EXECUTION PROTOCOLS:

<workflow>

<step n="1" goal="Ask User About Historical Sprint">

<output>
📜 **Historical Sprint Creation (Optional)**

Creating historical sprint records provides:
- Baseline velocity for future planning
- Historical reference of completed work
- Accurate sprint-planning recommendations
- Better trend analysis

Do you want to create historical sprint record(s)?
</output>

<ask>Create historical sprints? (Y/n)</ask>

<check if="user says no">
  <output>⏭️ Skipping historical sprint creation.</output>
  <action>Proceed to step-migrate-05-velocity-log.md</action>
</check>

<check if="user says yes">
  <action>Proceed to Step 2</action>
</check>

</step>

<step n="2" goal="Analyze Done Stories for Splitting">

<action>Load done stories from migration analysis</action>

<action>For each done story, gather:
  - Story key and title
  - Epic number
  - Points
  - File modification date (if available from story file)
  - Completion order (based on file dates or epic/story numbering)
</action>

<output>
📊 **Analyzing Done Stories for Sprint Distribution**

Found: {{done_stories_count}} done stories
Total points: {{total_points}}
</output>

<action>Group stories by available signals:
  1. **By file dates** (if story files have different mod dates)
  2. **By epic progression** (Epic 0 → Epic 1 → Epic 2)
  3. **By story numbering** (stories 0.x, 1.x, 2.x)
</action>

<action>Proceed to Step 3</action>

</step>

<step n="3" goal="Propose Sprint Splits">

<action>Generate split proposals based on analysis</action>

<check if="file dates show clear groupings">
  <action>Propose date-based splits:
    - Group stories completed around the same time
    - Suggest sprint boundaries at date gaps
  </action>
</check>

<check if="epic progression is clear">
  <action>Propose epic-based splits:
    - Sprint 1: Epic 0 stories (Testing Infrastructure)
    - Sprint 2: Epic 1 early stories
    - Sprint 3: Epic 1 remaining + Epic 2 stories
    - etc.
  </action>
</check>

<check if="no clear signals">
  <action>Propose simple splits:
    - Single sprint: All done stories
    - Two-sprint split: First half / Second half
    - Custom: Let user specify
  </action>
</check>

<output>
──────────────────────────────────────────────────────────────
📋 **Proposed Sprint Distribution**
──────────────────────────────────────────────────────────────

Based on analysis, here are proposed splits:

**Option A: Epic-Based Splits (Recommended)**
{{for each proposed sprint}}
📦 **Sprint {{sprint_number}}** ({{sprint_points}} pts)
{{for each story}}
   ├── {{story.key}}: {{story.title}} ({{story.points}} pts)
{{end}}

{{end}}

**Option B: Single Sprint**
📦 **Sprint 1** ({{total_points}} pts) - All {{done_stories_count}} stories

**Option C: Custom**
You specify how to split the stories.

──────────────────────────────────────────────────────────────
</output>

<ask>Which option do you prefer? (A/B/C or describe custom split)</ask>

<check if="user selects A">
  <action>Use epic-based splits</action>
  <action>Set {{sprint_splits}} = proposed epic-based grouping</action>
</check>

<check if="user selects B">
  <action>Use single sprint</action>
  <action>Set {{sprint_splits}} = [{ all stories }]</action>
</check>

<check if="user selects C or custom">
  <ask>Describe how you want to split the stories (e.g., "Sprint 1: Epic 0, Sprint 2: Epic 1.1-1.8, Sprint 3: rest")</ask>
  <action>Parse user input into {{sprint_splits}}</action>
</check>

<action>Proceed to Step 4</action>

</step>

<step n="4" goal="Gather Sprint Dates">

<action>For each proposed sprint:</action>

<ask>What date was Sprint {{sprint_number}} completed? (e.g., "2026-02-15" or press Enter to skip)</ask>

<action>Store {{end_date}} for each sprint (null if skipped)</action>

<action>Proceed to Step 5</action>

</step>

<step n="5" goal="Create Sprint Files">

<action>For each sprint in {{sprint_splits}}:</action>

<action>Create {sprints_dir}/sprint-{{sprint_number}}.yaml</action>

<action>Write sprint file:
```yaml
sprint_number: {{sprint_number}}
status: completed
start_date: null  # Unknown for historical
target_end: {{end_date}}
actual_end: {{end_date}}
team_members: []  # Unknown for historical

planned_stories:
{{for each story in sprint}}
  - key: {{story.key}}
    epic: {{story.epic_number}}
    title: "{{story.title}}"
    points: {{story.points}}
    status: done
{{end}}

metrics:
  total_points: {{sprint_points}}
  completed_points: {{sprint_points}}
  story_count: {{sprint_story_count}}
  stories_completed: {{sprint_story_count}}
  stories_carried: 0

notes: |
  Historical sprint created during migration to True Agile.
  {{sprint_description}}
```
</action>

<output>
✅ **Sprint {{sprint_number}} Created**

File: {sprints_dir}/sprint-{{sprint_number}}.yaml
- Stories: {{sprint_story_count}}
- Points: {{sprint_points}}
- Status: completed
</output>

<action>Repeat for all sprints in {{sprint_splits}}</action>

<action>Proceed to step-migrate-05-velocity-log.md</action>

</step>

</workflow>

---

## Example Split Proposals

### Epic-Based Splits (Common Pattern)

```
📦 Sprint 1 (14 pts) - Testing Infrastructure
   ├── 0-1-install-configure-vitest (3 pts)
   ├── 0-2-create-test-database-fixtures (5 pts)
   ├── 0-3-configure-cicd-testing-pipeline (3 pts)
   └── 0-4-document-testing-patterns (3 pts)

📦 Sprint 2 (36 pts) - Core Chat Foundation
   ├── 1-f0-schema-first-foundation (5 pts)
   ├── 1-1-basic-chat-endpoint (5 pts)
   ├── 1-2-streaming-responses (3 pts)
   ├── ... (more Epic 1 stories)

📦 Sprint 3 (30 pts) - Gateway Integration
   ├── 2-1-gateway-client-foundation (8 pts)
   ├── 2-2-read-workflow-definitions (5 pts)
   ├── ... (more Epic 2 stories)
```

### Date-Based Splits (If Available)

```
📦 Sprint 1 (20 pts) - Completed 2026-01-28
   [Stories with file dates around Jan 28]

📦 Sprint 2 (35 pts) - Completed 2026-02-15
   [Stories with file dates around Feb 15]

📦 Sprint 3 (25 pts) - Completed 2026-03-12
   [Stories with file dates around Mar 12]
```

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- User selects split strategy
- Sprint files created with correct format
- All done stories distributed across sprints
- Points totaled correctly per sprint

### ❌ SYSTEM FAILURE:

- Skipping user input on splits
- Missing stories from sprint files
- Incorrect point totals
- Invalid YAML format

**Master Rule:** Propose logical splits, let user decide, create one file per sprint.
