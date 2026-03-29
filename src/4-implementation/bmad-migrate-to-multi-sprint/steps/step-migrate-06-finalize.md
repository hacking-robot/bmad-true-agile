# Step Migrate-6: Finalize Migration

## STEP GOAL:

To display the final migration summary and guide the user to next steps.

## MANDATORY EXECUTION RULES (READ FIRST):

### Universal Rules:

- 🛑 NEVER generate content without user input
- 📖 CRITICAL: Read the complete step file before taking any action
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

---

## EXECUTION PROTOCOLS:

<workflow>

<step n="1" goal="Display Final Summary">

<output>
═══════════════════════════════════════════════════════════════
✅ **Migration Complete!**
═══════════════════════════════════════════════════════════════

**📊 Migration Summary:**

| Metric | Value |
|--------|-------|
| Epics Migrated | {{epic_count}} |
| Done Stories Preserved | {{done_story_count}} |
| Undone Stories Removed | {{undone_story_count}} |
| Story Files Archived | {{archived_count}} |
| Total Points Preserved | {{total_points}} |

**📁 Files Created/Modified:**

| File | Action |
|------|--------|
| epics.md | Replaced with migrated version |
| epics-pre-migration-backup.md | Created (backup) |
| archive/ | Created (archived story files) |

{{if historical sprints created}}
**📜 Historical Sprints Created:**

| Sprint | Stories | Points | Description |
|--------|---------|--------|-------------|
{{for each sprint}}
| {{sprint_number}} | {{story_count}} | {{points}} | {{description}} |
{{end}}
| **Total** | **{{total_stories}}** | **{{total_points}}** | |

| File | Action |
|------|--------|
{{for each sprint}}
| sprint-{{sprint_number}}.yaml | Created |
{{end}}
| velocity-log.yaml | Created (velocity tracking) |
{{end}}

{{if sprint-status.yaml existed}}
| archive/sprint-status.yaml | Archived (legacy) |
{{end}}

**📋 Epic Status:**

| Epic | Title | Status | Done Stories | Points |
|------|-------|--------|--------------|--------|
{{for each epic}}
| {{epic.number}} | {{epic.title}} | {{epic.status}} | {{epic.done_count}} | {{epic.points}} |
{{end}}

</output>

<action>Proceed to Step 2</action>

</step>

<step n="2" goal="Explain What Changed">

<output>
──────────────────────────────────────────────────────────────
**🎯 What Changed:**
──────────────────────────────────────────────────────────────

**Story Files:**
- ✅ Done story files remain in implementation-artifacts/
- 📦 Undone story files moved to implementation-artifacts/archive/
- 🔄 Sprint-planning will CREATE fresh story files when needed

**Epics File:**
- ✅ Done stories preserved with full content
- ❌ Undone stories REMOVED from epics.md
- ➕ Each story now has: key, points, status, jira_key metadata

**Historical Sprints:**
- 📜 Done stories split into {{sprint_count}} sprint(s)
- 📊 Velocity log provides planning recommendations
- 🎯 Sprint-planning uses averages for capacity suggestions

**No Longer Needed:**
- ❌ backlog-refinement workflow (deprecated)
- ❌ detail_level field (removed)
- ❌ Pre-planned undone stories (sprint-planning creates fresh)
</output>

<action>Proceed to Step 3</action>

</step>

<step n="3" goal="Show Velocity Planning Reference">

<check if="velocity log was created">
<output>
──────────────────────────────────────────────────────────────
**📈 Velocity Planning Reference:**
──────────────────────────────────────────────────────────────

From your {{sprint_count}} historical sprint(s):

| Planning Mode | Points | Use Case |
|---------------|--------|----------|
| Conservative | {{conservative}} | Team changes, holidays, learning curve |
| Standard | {{standard}} | Normal sprint, stable team |
| Aggressive | {{aggressive}} | Stretch goal, experienced team |

**Trend:** {{trend}}

When you run sprint-planning, it will:
1. Show these recommendations
2. Let you set custom capacity
3. Create stories sized to fit your target
</output>
</check>

<check if="no velocity log">
<output>
──────────────────────────────────────────────────────────────
**📈 Planning Reference:**
──────────────────────────────────────────────────────────────

No historical velocity data. For your first sprint:
- Start conservative (10-15 points)
- Track velocity over 2-3 sprints
- Velocity log will auto-populate after sprint reviews
</output>
</check>

<action>Proceed to Step 4</action>

</step>

<step n="4" goal="Guide Next Steps">

<output>
──────────────────────────────────────────────────────────────
**📋 Next Steps:**
──────────────────────────────────────────────────────────────

1. **Review the migrated epics.md**
   - Verify all done stories are present
   - Check story content is preserved
   - Confirm epic statuses are correct

2. **Run `/bmad-bmm-sprint-status`**
   - See overall epic progress
   - Verify migration worked correctly

3. **Plan your next sprint**
   - Run `/bmad-bmm-sprint-planning`
   - Set target capacity (use velocity recommendations if available)
   - Select epics to work on
   - Sprint-planning will CREATE new stories from requirements

4. **How sprint-planning works now:**
   - Analyzes PRD, Architecture, NFRs, and epic scope
   - Identifies FRs not yet covered by done stories
   - Creates stories sized to fit your capacity
   - Stories are just-in-time, not pre-planned

──────────────────────────────────────────────────────────────
**🎯 You are now on True Agile!**

- Stories are created during sprint-planning, not upfront
- backlog-refinement workflow is no longer needed
- Plan capacity, select epics, stories are sized to fit
- Archive preserves undone planning for reference only
- Historical sprints provide velocity baseline for planning
──────────────────────────────────────────────────────────────
</output>

</step>

</workflow>

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- Complete summary displayed
- All created files listed
- User understands what changed
- User knows next steps
- Velocity recommendations shown (if applicable)

### ❌ SYSTEM FAILURE:

- Incomplete summary
- Missing metrics
- Unclear next steps
- Not showing sprint files

**Master Rule:** User leaves with full understanding of migration outcome and planning reference.
