# Sprint Review - Close Sprint and Calculate Velocity

<critical>The workflow execution engine is governed by: {project-root}/_bmad/core/tasks/workflow.xml</critical>
<critical>You MUST have already loaded and processed: {installed_path}/workflow.yaml</critical>
<critical>Communicate all responses in {communication_language} and generate all documents in {document_output_language}</critical>

<critical>
PURPOSE: Close the current active sprint, demo completed work, handle incomplete stories (carryover), calculate velocity, and update velocity log for future planning.
KEY CONCEPT: Only ONE sprint can be active at a time. Closing a sprint is required before starting the next one.
</critical>

<workflow>

<step n="1" goal="Find active sprint">
  <action>Load {project_context} for project-wide patterns and conventions (if exists)</action>
  <action>Communicate in {communication_language} with {user_name}</action>
  
  <action>Scan {sprints_dir} for sprint files matching pattern sprint-*.yaml</action>
  <action>Find the sprint with status: active</action>
  
  <check if="no active sprint found">
    <output>
🚫 No active sprint found.

This could mean:
1. No sprints have been created yet - run sprint-planning first
2. The previous sprint was already closed - create a new sprint
3. Multiple sprints exist but none are active - check sprint files

Would you like to:
[S] Start sprint-planning to create a new sprint
[V] View all sprint statuses
[Q] Quit
    </output>
    <action>WAIT for user selection</action>
    <action>Handle selection appropriately</action>
  </check>
  
  <check if="active sprint found">
    <action>Load the active sprint file</action>
    <action>Set {{sprint_number}}, {{sprint_file}}, {{sprint_data}}</action>
    
    <output>
═══════════════════════════════════════════════════════════════
📍 Sprint {{sprint_number}} Review
═══════════════════════════════════════════════════════════════

Sprint Dates:
  Start: {{sprint_start_date}}
  Target End: {{sprint_target_end}}
  
Team: {{team_members}}

Proceeding with sprint review...
    </output>
  </check>
</step>

<step n="2" goal="Sync story statuses from sprint-status.yaml into sprint file">
  <action>Load {sprint_status} file (sprint-status.yaml)</action>

  <check if="sprint-status.yaml does not exist">
    <output>No sprint-status.yaml found - story statuses in sprint file will be used as-is.</output>
  </check>

  <check if="sprint-status.yaml exists">
    <action>For each story in planned_stories of the sprint file:</action>
    <action>  Find matching key in sprint-status.yaml development_status</action>
    <action>  Map sprint-status value to sprint file status:</action>
    <action>    ready-for-dev -> planned</action>
    <action>    in-progress -> in-progress</action>
    <action>    review -> review</action>
    <action>    done -> done</action>
    <action>  Update planned_stories[].status with the mapped value</action>

    <action>Recalculate completed_points = sum of points for stories with status = done</action>
    <action>Save the updated sprint file preserving all comments and structure</action>

    <output>Sprint file synced with sprint-status.yaml - story statuses updated.</output>
  </check>
</step>

<step n="3" goal="Review sprint stories by epic">
  <action>For each story in planned_stories, load its details from epic files</action>
  <action>Group stories by epic for display</action>
  <action>Calculate completed points and remaining points</action>
  
  <output>
═══════════════════════════════════════════════════════════════
📊 Sprint {{sprint_number}} Summary
═══════════════════════════════════════════════════════════════

┌─ Epic 1: [Epic Title] ─────────────────────────────────────────┐
│                                                                 │
│  ✅ 1.1 User Registration          3 pts   done      Alice     │
│  ✅ 1.2 Login Flow                 5 pts   done      Bob       │
│  ⚠️ 1.3 Password Reset             2 pts   in-progress Alice  │
│                                                                 │
│  Subtotal: 8/10 pts complete                                    │
└─────────────────────────────────────────────────────────────────┘

┌─ Epic 2: [Epic Title] ─────────────────────────────────────────┐
│                                                                 │
│  ✅ 2.1 Create Content             5 pts   done      Charlie   │
│  ❌ 2.2 Rich Text Editor           5 pts   planned   (none)    │
│                                                                 │
│  Subtotal: 5/10 pts complete                                    │
└─────────────────────────────────────────────────────────────────┘

═══════════════════════════════════════════════════════════════
📈 Sprint Totals:
  Planned: 20 pts
  Completed: 13 pts
  Incomplete: 7 pts
  
  Stories: 3/5 done
═══════════════════════════════════════════════════════════════
  </output>
</step>

<step n="3.5" goal="Check Requirement Coverage">
  <action>For each story with status "done" in this sprint:</action>
  <action>  Load the requirement IDs from the story's FRs addressed, NFRs addressed, Architecture requirements, and Additional requirements addressed metadata</action>
  <action>  For each requirement ID, load the full requirement text from the PRD/Architecture/epic's Requirements Inventory</action>
  <action>  Compare the requirement's full text against the story's acceptance criteria</action>
  <action>  If the AC clearly does not cover the full scope of the requirement, flag it as partially covered</action>
  <action>For each partially covered requirement:</action>
  <action>  Determine what remains unimplemented by comparing requirement text vs. AC</action>
  <action>  Prepare a note for the epic file's Partial Coverage Notes section in this format:</action>
  <action>    - [FR3] partially addressed by Story 1-2-user-auth (Sprint 1): social login and 2FA registration remain</action>

  <check if="partial coverage found">
    <output>
═══════════════════════════════════════════════════════════════
🔍 Requirements Coverage Check
═══════════════════════════════════════════════════════════════

| Requirement | Story | Status | What Remains |
|-------------|-------|--------|-------------|
{{for each checked requirement}}
| {{req_id}} | {{story_key}} | {{coverage_status}} | {{what_remains}} |
{{end}}
    </output>

    <ask>Review the partial coverage notes above. Confirm or adjust before writing them to the epic files? (Y/adjust)</ask>

    <action>Write confirmed notes to each epic file's ### Partial Coverage Notes section</action>
  </check>

  <check if="no partial coverage found">
    <output>
✅ All done stories fully cover their claimed requirements.
    </output>
  </check>
</step>

<step n="4" goal="Handle incomplete stories">
  <action>For each story with status != done:</action>
  <action>Ask user how to handle it</action>
  
  <check if="incomplete stories exist">
    <output>
📝 Handling Incomplete Stories:

Story 1.3 Password Reset (in-progress, 2 pts)
  What should happen to this story?
  [C] Carry over to next sprint
  [B] Return to backlog (reprioritize later)
  [S] Split into smaller stories
  [K] Keep working (don't close sprint yet)
    </output>
    
    <action>WAIT for user selection per incomplete story</action>
    
    <check if="user chooses Carry over">
      <action>Add to {{carryover_stories}} list</action>
    </check>
    
    <check if="user chooses Return to backlog">
      <action>Mark story status as 'backlog' in epic file</action>
    </check>
    
    <check if="user chooses Split">
      <action>Note: Stories are created during sprint-planning. Mark as 'backlog' and split will happen naturally during next sprint-planning.</action>
      <action>Mark story status as 'backlog' in epic file</action>
    </check>
    
    <check if="user chooses Keep working">
      <output>Sprint remains active. Continue working on stories and run sprint-review again when ready.</output>
      <action>End workflow - sprint not closed</action>
    </check>
  </check>
</step>

<step n="5" goal="Calculate and record velocity">
  <action>Calculate velocity = sum of points for stories with status = done</action>
  <action>Set {{sprint_velocity}}</action>
  
  <output>
📊 Sprint {{sprint_number}} Velocity: {{sprint_velocity}} points

Completed: {{completed_count}} stories, {{sprint_velocity}} points
Carryover: {{carryover_count}} stories, {{carryover_points}} points
  </output>
  
  <action>Update or create velocity-log.yaml</action>
  
  <check if="velocity-log.yaml does not exist">
    <action>Create velocity-log.yaml with header and first sprint entry</action>
  </check>
  
  <check if="velocity-log.yaml exists">
    <action>Append new sprint entry</action>
    <action>Recalculate averages</action>
  </check>
  
  <output>
Updated velocity-log.yaml

Historical Velocity:
  Sprint 1: 8 pts
  Sprint 2: 12 pts
  Sprint {{sprint_number}}: {{sprint_velocity}} pts
  
  Average: {{avg_velocity}} pts
  Trend: {{velocity_trend}}
  </output>
</step>

<step n="6" goal="Close sprint file">
  <action>Update sprint file with:</action>
  <action>  - status: completed</action>
  <action>  - actual_end: today's date</action>
  <action>  - completed_points: {{sprint_velocity}}</action>
  <action>  - velocity: {{sprint_velocity}}</action>
  <action>  - carryover: list of carried over stories</action>
  
  <output>
✅ Sprint {{sprint_number}} Closed!

Sprint file: sprint-{{sprint_number}}.yaml
Review file: sprint-{{sprint_number}}-review.md
Velocity log: velocity-log.yaml updated

═══════════════════════════════════════════════════════════════
📋 Next Steps:
═══════════════════════════════════════════════════════════════

1. Run `/bmad-bmm-retrospective` to capture learnings
2. Run `/bmad-bmm-sprint-planning` to create Sprint {{next_sprint_number}}
3. Review carried over stories - they'll be available for next sprint

Would you like to run retrospective now? (Y/N)
  </output>
  
  <action>WAIT for user response</action>
  
  <check if="user says Y">
    <action>Inform user to run /bmad-bmm-retrospective</action>
  </check>
</step>

<step n="7" goal="Create sprint review document">
  <action>Create sprint-{{sprint_number}}-review.md with:</action>
  <action>  - Sprint summary</action>
  <action>  - Completed stories by epic</action>
  <action>  - Incomplete stories and their disposition</action>
  <action>  - Velocity metrics</action>
  <action>  - Team performance notes</action>
  <action>  - Demo notes (if any)</action>
  
  <output>
✅ Sprint review document created: sprint-{{sprint_number}}-review.md
  </output>
</step>

</workflow>

## Sprint Status Values

- `active` - Sprint is in progress
- `completed` - Sprint finished normally
- `cancelled` - Sprint was cancelled

## Story Status Values (for reference)

- `planned` - Story selected for sprint, not started
- `in-progress` - Developer actively working
- `review` - Implementation complete, pending review
- `done` - Story completed and accepted
