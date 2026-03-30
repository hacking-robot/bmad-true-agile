# Step Migrate-2b: Migrate Single Epic (Sub-Step)

## STEP GOAL:

To migrate one epic and its done stories to the new format. This step is called by step-migrate-02-generate.md for each epic.

## INPUTS (provided by parent step):

- `epic_number`: The epic number (e.g., 1, 2, 3)
- `epic_title`: The epic title
- `epic_metadata`: All content between `## Epic X:` and first `### Story`
- `done_stories`: Array of done stories with their full content

## OUTPUTS (returned to parent step):

- `migrated_epic_content`: The formatted epic block ready to append

---

## EXECUTION PROTOCOL:

<workflow>

<step n="1" goal="Generate Epic Header">

<action>Determine epic status:
  - IF all stories in epic are done → status: `done`
  - IF some stories done → status: `in-progress`
  - IF no stories done → status: `not-started`
</action>

<action>Calculate total points from done stories:
  - IF story file has `Points: N` → use that
  - ELSE estimate based on complexity (ask user if unsure)
</action>

<output>
📝 **Processing Epic {{epic_number}}**

- Status: {{epic_status}}
- Done Stories: {{done_stories_count}}
- Total Points: {{total_points}}
</output>

<action>Proceed to Step 2</action>

</step>

<step n="2" goal="Build Epic Section">

<action>Start building epic content:

```markdown
---

## Epic {{epic_number}}: {{epic_title}}

**Status:** {{epic_status}}
**Goal:** {{extracted_goal_from_metadata}}

{{epic_metadata_content - PRESERVE AS-IS}}

```
</action>

<action>Proceed to Step 3</action>

</step>

<step n="3" goal="Migrate Done Stories (Loop)">

<action>Set {{story_index}} = 0</action>

<check if="{{story_index}} < {{done_stories_count}}">
  <action>Set {{current_story}} = done_stories[{{story_index}}]</action>

  <output>
   ├── Migrating Story {{current_story.number}}: {{current_story.title}}
  </output>

  <action>Extract story slug from title or file:
    - Convert to lowercase
    - Replace spaces/special chars with hyphens
    - Result: "1-8-mastra-public-endpoint-setup"
  </action>

  <action>Determine points:
    - IF story file has `Points: N` → use that
    - IF story file has `Estimate: N` → use that
    - ELSE estimate and note for user review
  </action>

  <action>Build story block:

```markdown
### Story {{current_story.number}}: {{current_story.title}}
key: {{current_story.key}}
points: {{current_story.points}}
status: done
jira_key: null

{{current_story.full_content - PRESERVE EXACTLY AS EXTRACTED}}

---
```
</action>

  <action>Add story to Story Summary table (for later)</action>

  <action>Increment {{story_index}} += 1</action>

  <action>Loop back</action>
</check>

<check if="{{story_index}} >= {{done_stories_count}}">
  <action>All done stories migrated for this epic</action>
  <action>Proceed to Step 4</action>
</check>

</step>

<step n="4" goal="Build Story Summary Table">

<action>Generate summary table for all migrated stories:

```markdown
**Story Summary:**

| Key | Title | Points | Status |
|-----|-------|--------|--------|
{{for each done story}}
| {{story.key}} | {{story.title}} | {{story.points}} | done |
{{end}}
| **Total** | | **{{total_points}}** | |
```
</action>

<action>Append to epic content</action>

<action>Proceed to Step 5</action>

</step>

<step n="5" goal="Return Migrated Content">

<action>Combine all parts into {{migrated_epic_content}}</action>

<output>
   └── Epic {{epic_number}} migrated: {{done_stories_count}} stories, {{total_points}} points
</output>

<action>Return {{migrated_epic_content}} to parent step</action>

</step>

</workflow>

---

## STORY CONTENT EXTRACTION RULES

### What to Extract (FULL CONTENT):

```
### Story X.Y: Title
│
│  **As a** ...                    ← INCLUDE
│  **I want** ...                  ← INCLUDE
│  **So that** ...                 ← INCLUDE
│
│  **Acceptance Criteria:**        ← INCLUDE
│  **Given** ...                   ← INCLUDE
│  **When** ...                    ← INCLUDE
│  **Then** ...                    ← INCLUDE
│
│  **Technical Notes:**            ← INCLUDE
│  - ...                           ← INCLUDE
│
│  **FRs addressed:** ...          ← INCLUDE
│  **NFRs addressed:** ...         ← INCLUDE (if present)
│  **Additional requirements addressed:** ... ← INCLUDE (if present)
│  **Dependencies:** ...           ← INCLUDE (if present)
│  **Architecture Note:** ...      ← INCLUDE (if present)
│  **Infrastructure Context:** ... ← INCLUDE (if present)
│  **Testing Acceptance Criteria:** ← INCLUDE (if present)
│  **Any other section:** ...      ← INCLUDE (if present)
│
▼
### Story X.Z: ...  ← STOP HERE (next story)
```

### What to Prepend (NEW ONLY):

```markdown
### Story X.Y: [original title]
key: X-Y-slug
points: N
status: done
jira_key: null

[ORIGINAL CONTENT STARTS HERE - UNTOUCHED]
```

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- All done stories included
- Story content preserved exactly
- New metadata block prepended correctly
- Points estimated if not present

### ❌ SYSTEM FAILURE:

- Truncating story content
- Reformatting story internals
- Missing stories
- Wrong story boundaries

**Master Rule:** Story content is sacred. Prepend metadata only.
