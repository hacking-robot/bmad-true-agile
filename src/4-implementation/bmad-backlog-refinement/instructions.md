# Backlog Refinement - Story Detail Upgrade

<critical>The workflow execution engine is governed by: {project-root}/_bmad/core/tasks/workflow.xml</critical>
<critical>You MUST have already loaded and processed: {installed_path}/workflow.yaml</critical>
<critical>Communicate all responses in {communication_language} and generate all documents in {document_output_language}</critical>

<critical>
PURPOSE: Upgrade stories from 'placeholder' or 'outlined' to 'ready' level so they can be selected for sprint planning.
TARGET FILE: Stories are updated IN PLACE in their epic files (epic-1.md, epic-2.md, or epics.md). No new files are created.
</critical>

<workflow>

<step n="1" goal="Discovery - Find stories needing refinement">
  <action>Load {project_context} for project-wide patterns and conventions (if exists)</action>
  <action>Communicate in {communication_language} with {user_name}</action>
  
  <action>Load all epic files matching {epics_pattern}</action>
  <action>Scan for stories with `detail_level: placeholder` or `detail_level: outlined`</action>
  <action>Group stories by epic for display</action>
  
  <check if="no stories need refinement">
    <output>
All stories are already at 'ready' level!

You can proceed directly to sprint planning.
    </output>
    <action>End workflow - suggest user run sprint-planning</action>
  </check>
  
  <check if="stories need refinement">
    <output>
═══════════════════════════════════════════════════════════════
📝 Backlog Refinement
═══════════════════════════════════════════════════════════════

Stories needing refinement:

┌─ Epic 1: [Epic Title] ────────────────────────────────────────┐
│  ✓ All stories ready                                             │
└─────────────────────────────────────────────────────────────────┘

┌─ Epic 2: [Epic Title] ────────────────────────────────────────┐
│  ○ 2.2 Edit Content (outlined)                                  │
│  ○ 2.3 Delete Content (placeholder)                             │
└─────────────────────────────────────────────────────────────────┘

┌─ Epic 3: [Epic Title] ────────────────────────────────────────┐
│  ○ 3.1 Basic Search (placeholder)                               │
│  ○ 3.2 Advanced Filters (placeholder)                           │
└─────────────────────────────────────────────────────────────────┘

Which story would you like to refine? (e.g., "2.2" or "3.1")
    </output>
    
    <action>WAIT for user selection</action>
    <action>Parse selection to extract epic number and story number</action>
    <action>Set {{target_epic_num}} and {{target_story_num}}</action>
  </check>
</step>

<step n="2" goal="Gather context for selected story">
  <action>Locate the story in the epic file</action>
  <action>Read current story content (title, brief, current points estimate)</action>
  
  <check if="PRD exists">
    <action>Load PRD and identify requirements related to this story</action>
  </check>
  
  <check if="Architecture exists">
    <action>Load Architecture and identify technical constraints/patterns for this story</action>
  </check>
  
  <check if="UX Design exists and story has UI components">
    <action>Load UX Design and identify UX requirements for this story</action>
  </check>
  
  <action>Load previously completed stories to identify patterns to follow</action>
  
  <output>
──────────────────────────────────────────────────────────────
Refining: Story {{target_epic_num}}.{{target_story_num}} - {{story_title}}

Current detail level: {{current_detail_level}}
Current point estimate: {{current_points}}

Related Requirements:
{{related_requirements}}

Technical Context:
{{technical_context}}

Similar Completed Stories:
{{similar_stories}}
──────────────────────────────────────────────────────────────
  </output>
</step>

<step n="3" goal="Write full story details">
  <output>
Let's write the full story for {{story_title}}:

**Story {{target_epic_num}}.{{target_story_num}}: {{story_title}}**
  </output>
  
  <action>Ask user to confirm or adjust the point estimate</action>
  <action>Set {{confirmed_points}}</action>
  
  <output>
**Detail Level:** ready
**Points:** {{confirmed_points}}
**Jira:** (optional - leave blank or enter Jira key)

As a {{user_type}},
I want {{capability}},
So that {{benefit}}.
  </output>
  
  <action>Collaborate with user to write the user story</action>
  <action>WAIT for user input on user story</action>
  
  <output>
**Acceptance Criteria:**
  </output>
  
  <action>Guide user through writing acceptance criteria</action>
  
  <output>
For each acceptance criterion, use Given/When/Then format:

**AC 1:**
**Given** {{precondition}}
**When** {{action}}
**Then** {{expected_result}}

Add another AC? (Y/N)
  </output>
  
  <action>Continue until all acceptance criteria are defined</action>
  <action>Display complete story for review</action>
  
  <output>
──────────────────────────────────────────────────────────────
Complete Story:

### Story {{target_epic_num}}.{{target_story_num}}: {{story_title}}

**Detail Level:** ready
**Points:** {{confirmed_points}}
**Jira:** {{jira_key}}

As a {{user_type}},
I want {{capability}},
So that {{benefit}}.

**Acceptance Criteria:**

{{all_acceptance_criteria}}

──────────────────────────────────────────────────────────────
Does this look correct? (Y/N/Edit)
  </output>
  
  <action>WAIT for user confirmation</action>
  
  <check if="user says Edit">
    <action>Ask what needs to be changed and make edits</action>
    <action>Redisplay for confirmation</action>
  </check>
</step>

<step n="4" goal="Update epic file">
  <action>Locate the story in the epic file</action>
  <action>Replace the story content with the new ready-level content</action>
  <action>Update `detail_level: outlined` or `detail_level: placeholder` to `detail_level: ready`</action>
  <action>Update points if changed</action>
  <action>Add full user story and acceptance criteria</action>
  
  <output>
✅ Story {{target_epic_num}}.{{target_story_num}} updated to 'ready' status!

Updated: {{epic_file_path}}
  </output>
  
  <action>Ask if user wants to refine another story</action>
  
  <check if="user wants to refine another">
    <action>Loop back to Step 1</action>
  </check>
  
  <check if="user is done refining">
    <output>
✅ Backlog refinement session complete!

Stories refined this session: {{stories_refined_count}}
All refined stories are now ready for sprint planning.

Run `/bmad-bmm-sprint-planning` to select stories for your next sprint.
    </output>
  </check>
</step>

</workflow>

## Story Point Estimation Guide

Use Fibonacci scale for story points:
- **1** - Trivial, less than 1 hour
- **2** - Simple, few hours
- **3** - Moderate, half day
- **5** - Complex, full day
- **8** - Very complex, 2 days
- **13** - Epic-sized, consider splitting
- **21** - Too large, MUST split

## Definition of Ready Checklist

A story is ready for sprint when:
- [ ] Has clear user story (As a/I want/So that)
- [ ] Has detailed acceptance criteria (Given/When/Then)
- [ ] Has point estimate
- [ ] No dependencies on future stories
- [ ] Technical approach is understood
- [ ] Can be completed by single developer
