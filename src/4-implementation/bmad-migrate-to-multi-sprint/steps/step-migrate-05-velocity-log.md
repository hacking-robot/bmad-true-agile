# Step Migrate-5: Create Velocity Log (Optional)

## STEP GOAL:

To create a velocity log file for tracking sprint velocity over time, enabling better planning.

## MANDATORY EXECUTION RULES (READ FIRST):

### Universal Rules:

- 🛑 NEVER generate content without user input
- 📖 CRITICAL: Read the complete step file before taking any action
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

### Optional Step Rules:

- This step is OPTIONAL - skip if user doesn't want velocity tracking
- Requires historical sprint(s) to exist (step-migrate-04)
- Future sprints will append to this log automatically
- Calculates averages from all historical sprints

---

## EXECUTION PROTOCOLS:

<workflow>

<step n="1" goal="Check Prerequisites">

<check if="no historical sprints were created">
  <output>
⏭️ **Skipping Velocity Log**

No historical sprints were created, so velocity tracking is not applicable.
  </output>
  <action>Proceed to step-migrate-06-finalize.md</action>
</check>

<check if="historical sprints exist">
  <output>
📊 **Velocity Log Creation (Optional)**

A velocity log enables:
- Sprint-planning recommendations based on average velocity
- Trend analysis (improving/declining/stable)
- Better capacity planning
- Historical comparison across sprints

Do you want to create a velocity log?
  </output>

  <ask>Create velocity log? (Y/n)</ask>

  <check if="user says no">
    <output>⏭️ Skipping velocity log creation.</output>
    <action>Proceed to step-migrate-06-finalize.md</action>
  </check>

  <check if="user says yes">
    <action>Proceed to Step 2</action>
  </check>
</check>

</step>

<step n="2" goal="Calculate Velocity from All Sprints">

<action>Load all sprint files from {sprints_dir}/sprint-*.yaml</action>

<action>For each sprint, extract:
  - sprint_number
  - planned_points (total_points from sprint file)
  - completed_points (stories_completed * avg points, or total_points if all done)
  - stories_completed
  - stories_carried (0 for historical)
  - date_completed
</action>

<action>Calculate aggregate metrics:
  - total_sprints: count of all historical sprints
  - total_points_completed: sum of all completed_points
  - average_velocity: total_points_completed / total_sprints
  - average_last_3: average of last 3 sprints (or all if fewer)
  - trend: "stable" | "improving" | "declining" (compare first half vs second half)
</action>

<output>
📈 **Velocity Calculation**

| Sprint | Points | Stories | Completed Date |
|--------|--------|---------|----------------|
{{for each sprint}}
| {{sprint_number}} | {{completed_points}} | {{stories_completed}} | {{date_completed}} |
{{end}}
| **Total** | **{{total_points}}** | **{{total_stories}}** | |

**Calculated Metrics:**
- Average Velocity: {{average_velocity}} pts/sprint
- Last 3 Sprints Avg: {{average_last_3}} pts
- Trend: {{trend}}
</output>

<action>Proceed to Step 3</action>

</step>

<step n="3" goal="Create Velocity Log File">

<action>Create {sprints_dir}/velocity-log.yaml</action>

<action>Write velocity log:
```yaml
# Velocity Log - Tracks sprint velocity for planning
# Auto-updated by sprint-review workflow

sprints:
{{for each sprint}}
  - sprint_number: {{sprint_number}}
    planned_points: {{planned_points}}
    completed_points: {{completed_points}}
    velocity: {{completed_points}}
    stories_completed: {{stories_completed}}
    stories_carried: 0
    date_completed: {{date_completed}}
    notes: "Historical sprint from migration"
{{end}}

# Calculated metrics (update after each sprint)
averages:
  last_3_sprints: {{average_last_3}}
  last_5_sprints: {{average_last_5_or_all}}
  all_time: {{average_velocity}}
  trend: "{{trend}}"  # improving | declining | stable

# Planning reference
recommendations:
  conservative: {{round(average_velocity * 0.8)}}  # 80% of average
  standard: {{round(average_velocity)}}             # 100% of average
  aggressive: {{round(average_velocity * 1.2)}}     # 120% of average

# Historical context
migration_note: |
  Velocity log created during migration to True Agile.
  Contains {{total_sprints}} historical sprint(s) as baseline.
  Sprint-planning uses these averages for capacity recommendations.
```
</action>

<output>
✅ **Velocity Log Created**

File: {sprints_dir}/velocity-log.yaml

**Historical Sprints:** {{total_sprints}}
**Total Points:** {{total_points}}
**Average Velocity:** {{average_velocity}} pts/sprint

**Planning Recommendations:**
- Conservative: {{round(average_velocity * 0.8)}} pts (80%)
- Standard: {{round(average_velocity)}} pts (100%)
- Aggressive: {{round(average_velocity * 1.2)}} pts (120%)

**Trend:** {{trend}}

This will be used by sprint-planning to recommend capacity.
</output>

<action>Proceed to step-migrate-06-finalize.md</action>

</step>

</workflow>

---

## Trend Calculation

| Condition | Trend |
|-----------|-------|
| Second half avg > First half avg by >10% | "improving" |
| Second half avg < First half avg by >10% | "declining" |
| Otherwise | "stable" |

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- Velocity log created with correct format
- All historical sprints included
- Averages calculated correctly
- Trend determined from data
- Planning recommendations populated

### ❌ SYSTEM FAILURE:

- Missing sprint data
- Incorrect calculations
- Invalid YAML format
- Not handling 1-sprint case

**Master Rule:** Velocity log aggregates all historical sprints for planning.
