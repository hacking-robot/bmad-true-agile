---
name: epic-forecast
description: 'Estimate remaining sprints for an epic based on velocity and uncovered requirements. Use when the user says "forecast epic", "estimate epic", "how long for epic", or "epic timeline"'
phase: implementation
code: EF
agent: sm
sequence: 13
---

# Epic Forecast Workflow

**Goal:** Provide a data-backed estimate of how many sprints remain to complete an epic, using velocity history and requirements coverage analysis.

**Your Role:** You are a Scrum Master helping the team and stakeholders understand epic timelines. Be honest about uncertainty — provide ranges, not false precision.

---

## INITIALIZATION

### Configuration Loading

Load config from `{project-root}/_bmad/bmm/config.yaml` and resolve:

- `project_name`, `user_name`
- `communication_language`, `document_output_language`
- `planning_artifacts`, `implementation_artifacts`
- `date` as system-generated current datetime
- YOU MUST ALWAYS SPEAK OUTPUT in your Agent communication style with the config `{communication_language}`

### Paths

- `installed_path` = `{project-root}/_bmad/bmm/workflows/4-implementation/epic-forecast`
- `sprint_status_file` = `{implementation_artifacts}/sprint-status.yaml`
- `sprints_dir` = `{implementation_artifacts}/sprints`
- `velocity_log` = `{sprints_dir}/velocity-log.yaml`

### Input Files

| Input | Path | Load Strategy |
|-------|------|---------------|
| Sprint status | `{sprint_status_file}` | FULL_LOAD |
| Velocity log | `{velocity_log}` | FULL_LOAD |
| Epic files | `{planning_artifacts}/*epic*.md` | FULL_LOAD |

### Context

- `project_context` = `**/project-context.md` (load if exists)

---

## EXECUTION

<workflow>

<step n="1" goal="Select target epic">
  <action>Load {project_context} for project-wide patterns and conventions (if exists)</action>
  <action>Communicate in {communication_language} with {user_name}</action>

  <action>Load all epic files from {planning_artifacts}</action>
  <action>Load {sprint_status_file} if exists</action>

  <action>Display all epics with current status</action>

  <output>
═══════════════════════════════════════════════════════════════
📈 **Epic Forecast**
═══════════════════════════════════════════════════════════════

**Available Epics:**

| Epic | Title | Status | Done Stories | Total Reqs | Covered Reqs |
|------|-------|--------|--------------|------------|--------------|
{{for each epic}}
| {{epic_number}} | {{epic_title}} | {{epic_status}} | {{done_story_count}} | {{total_requirements}} | {{covered_requirements}} |
{{end}}
  </output>

  <check if="user already specified an epic number">
    <action>Set {{target_epic}} from user input</action>
  </check>

  <check if="user did not specify an epic">
    <ask>Which epic would you like to forecast? (Enter epic number, or "all" for all epics)</ask>
    <action>Set {{target_epic}} from user response</action>
  </check>

  <action>Proceed to Step 2</action>
</step>

<step n="2" goal="Analyze requirements coverage for target epic">

  <action>For the target epic, load the Requirements Inventory section</action>
  <action>Extract all FRs, NFRs, ARCH requirements, and ARs listed in the inventory</action>

  <action>Scan all done stories in this epic's section:</action>
  <action>  Extract `**FRs addressed:**` from each done story</action>
  <action>  Extract `**NFRs addressed:**` from each done story</action>
  <action>  Extract `**Architecture requirements:**` from each done story</action>
  <action>  Extract `**Additional requirements addressed:**` from each done story</action>

  <action>Calculate:
    - {{total_frs}} = count of all FRs in inventory
    - {{covered_frs}} = count of FRs addressed by done stories
    - {{remaining_frs}} = total - covered
    - {{total_nfrs}} = count of relevant NFRs
    - {{covered_nfrs}} = count of NFRs addressed by done stories
    - {{remaining_nfrs}} = total - covered
    - {{total_arch}} = count of ARCH requirements
    - {{covered_arch}} = count of ARCH addressed by done stories
    - {{remaining_arch}} = total - covered
    - {{total_ar}} = count of AR requirements
    - {{covered_ar}} = count of ARs addressed by done stories
    - {{remaining_ar}} = total - covered
    - {{coverage_pct}} = (covered_frs + covered_nfrs + covered_arch + covered_ar) / (total_frs + total_nfrs + total_arch + total_ar) * 100
  </action>

  <action>Also calculate points already completed for this epic from done stories</action>
  <action>Set {{completed_points_epic}} = sum of points from done stories in this epic</action>

  <output>
──────────────────────────────────────────────────────────────
📊 **Epic {{target_epic}}: {{epic_title}} — Requirements Coverage**
──────────────────────────────────────────────────────────────

| Requirement Type | Total | Covered | Remaining |
|------------------|-------|---------|-----------|
| Functional (FRs) | {{total_frs}} | {{covered_frs}} | {{remaining_frs}} |
| Non-Functional (NFRs) | {{total_nfrs}} | {{covered_nfrs}} | {{remaining_nfrs}} |
| Architecture (ARCH) | {{total_arch}} | {{covered_arch}} | {{remaining_arch}} |
| Additional (ARs) | {{total_ar}} | {{covered_ar}} | {{remaining_ar}} |
| **Total** | **{{total_all}}** | **{{covered_all}}** | **{{remaining_all}}** |

**Coverage:** {{coverage_pct}}%
**Points completed so far:** {{completed_points_epic}} pts across {{done_story_count}} stories
  </output>

  <check if="remaining_all == 0">
    <output>
✅ **Epic {{target_epic}} has full requirements coverage!**

All FRs, NFRs, ARCH requirements, and ARs are addressed by done stories.
This epic can be marked as `done`.
    </output>
    <action>End workflow for this epic</action>
  </check>

  <action>Proceed to Step 3</action>
</step>

<step n="3" goal="Estimate remaining effort">

  <action>Analyze the remaining uncovered requirements</action>
  <action>For each remaining requirement, assess complexity based on:
    - Requirement description and scope
    - Comparison with similar done stories and their point values
    - Dependencies between remaining requirements (can they be grouped?)
    - Architecture/technical complexity signals
  </action>

  <action>Group remaining requirements into logical story-sized chunks (3-8 pts each)</action>
  <action>Estimate points per chunk based on patterns from completed stories</action>

  <action>Calculate:
    - {{estimated_remaining_points}} = sum of estimated points for all remaining chunks
    - {{estimated_remaining_stories}} = number of story-sized chunks
  </action>

  <check if="done stories exist for this epic">
    <action>Calculate {{avg_points_per_requirement}} = completed_points_epic / covered_all</action>
    <action>Calculate {{cross_check_estimate}} = remaining_all * avg_points_per_requirement</action>
  </check>

  <output>
──────────────────────────────────────────────────────────────
🔮 **Remaining Effort Estimate**
──────────────────────────────────────────────────────────────

**Estimated remaining stories:** ~{{estimated_remaining_stories}}
**Estimated remaining points:** ~{{estimated_remaining_points}} pts

{{if cross_check available}}
**Cross-check (avg pts/requirement):** ~{{cross_check_estimate}} pts
  (Based on {{avg_points_per_requirement}} pts/req from {{done_story_count}} done stories)
{{end}}

**Proposed story breakdown:**

| # | Grouped Requirements | Est. Points | Rationale |
|---|---------------------|-------------|-----------|
{{for each chunk}}
| {{chunk_num}} | {{requirements_in_chunk}} | {{chunk_points}} | {{sizing_rationale}} |
{{end}}
  </output>

  <action>Proceed to Step 4</action>
</step>

<step n="4" goal="Load velocity and calculate forecast">

  <action>Load {velocity_log}</action>

  <check if="velocity log exists and has sprint data">
    <action>Extract velocity data:
      - {{avg_velocity_3}} = last 3 sprints average
      - {{avg_velocity_all}} = all-time average
      - {{trend}} = improving | stable | declining
      - {{min_velocity}} = lowest sprint velocity
      - {{max_velocity}} = highest sprint velocity
    </action>
  </check>

  <check if="no velocity data available">
    <output>
⚠️ **No velocity history available.**

Without velocity data, I can only provide a point estimate, not a sprint count.

**Estimated remaining work:** ~{{estimated_remaining_points}} pts

To get a sprint forecast, complete at least one sprint and run sprint-review to record velocity.
    </output>

    <ask>Would you like to provide a manual velocity estimate? (e.g., "12 points per sprint")</ask>

    <check if="user provides estimate">
      <action>Set {{avg_velocity_3}} = user provided value</action>
      <action>Set {{forecast_basis}} = "user-provided estimate"</action>
    </check>

    <check if="user declines">
      <action>End workflow</action>
    </check>
  </check>

  <check if="velocity data available">
    <action>Set {{forecast_basis}} = "last 3 sprints average"</action>
  </check>

  <action>Calculate forecasts:
    - {{optimistic_sprints}} = ceil(estimated_remaining_points / max_velocity)
    - {{likely_sprints}} = ceil(estimated_remaining_points / avg_velocity_3)
    - {{pessimistic_sprints}} = ceil(estimated_remaining_points / min_velocity)
  </action>

  <check if="trend == declining">
    <action>Add 1 sprint to pessimistic estimate</action>
    <action>Set {{trend_warning}} = "⚠️ Velocity trend is declining — pessimistic estimate adjusted upward"</action>
  </check>

  <check if="only 1-2 sprints of history">
    <action>Set {{confidence}} = "Low (limited velocity data)"</action>
  </check>

  <check if="3+ sprints of history">
    <action>Set {{confidence}} = "Medium"</action>
  </check>

  <check if="5+ sprints of history AND trend is stable">
    <action>Set {{confidence}} = "High"</action>
  </check>

  <action>Proceed to Step 5</action>
</step>

<step n="5" goal="Present forecast">

  <output>
═══════════════════════════════════════════════════════════════
📈 **Epic {{target_epic}} Forecast: {{epic_title}}**
═══════════════════════════════════════════════════════════════

**Requirements Coverage:** {{coverage_pct}}% ({{covered_all}}/{{total_all}})
**Points Completed:** {{completed_points_epic}} pts
**Estimated Remaining:** ~{{estimated_remaining_points}} pts

──────────────────────────────────────────────────────────────
**Velocity ({{forecast_basis}}):**
  Average (last 3): {{avg_velocity_3}} pts/sprint
  All-time: {{avg_velocity_all}} pts/sprint
  Range: {{min_velocity}} – {{max_velocity}} pts/sprint
  Trend: {{trend}}

──────────────────────────────────────────────────────────────
**Sprint Forecast:**

  🟢 Optimistic:   {{optimistic_sprints}} sprint(s)  (at {{max_velocity}} pts/sprint)
  🟡 Likely:       {{likely_sprints}} sprint(s)  (at {{avg_velocity_3}} pts/sprint)
  🔴 Pessimistic:  {{pessimistic_sprints}} sprint(s)  (at {{min_velocity}} pts/sprint)

  **Confidence:** {{confidence}}
  {{trend_warning}}

──────────────────────────────────────────────────────────────
**Key Assumptions & Risks:**

- Estimate based on {{remaining_all}} uncovered requirements
- New requirements discovered during implementation could extend timeline
- Story point estimates are rough until stories are formally created in sprint planning
- Velocity may vary if team composition or focus changes
- Cross-epic work in sprints means not all velocity goes to this epic

──────────────────────────────────────────────────────────────
💡 **For your stakeholder:**

"Epic {{target_epic}} is {{coverage_pct}}% complete by requirements coverage.
Based on our velocity, we estimate {{likely_sprints}} more sprint(s) to finish,
with a range of {{optimistic_sprints}}–{{pessimistic_sprints}} sprints depending on complexity."
═══════════════════════════════════════════════════════════════
  </output>

  <ask>Options:
1. Forecast another epic
2. Show detailed remaining requirements
3. Exit

Choice:</ask>

  <check if="choice == 1">
    <action>Go back to Step 1</action>
  </check>

  <check if="choice == 2">
    <output>
**Remaining Uncovered Requirements for Epic {{target_epic}}:**

**FRs:**
{{for each remaining_fr}}
- {{fr_id}}: {{fr_description}}
{{end}}

**NFRs:**
{{for each remaining_nfr}}
- {{nfr_id}}: {{nfr_description}}
{{end}}

**ARCH:**
{{for each remaining_arch_req}}
- {{arch_id}}: {{arch_description}}
{{end}}

**ARs:**
{{for each remaining_ar_req}}
- {{ar_id}}: {{ar_description}}
{{end}}
    </output>
  </check>

  <check if="choice == 3">
    <action>End workflow</action>
  </check>
</step>

</workflow>
