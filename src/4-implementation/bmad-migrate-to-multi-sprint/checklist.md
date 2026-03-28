# Migration Checklist

Use this checklist to verify the migration to true agile is complete and correct.

---

## Pre-Migration

- [ ] Epic files backed up
- [ ] sprint-status.yaml backed up
- [ ] Git status clean (or changes committed)
- [ ] User available to answer point estimation questions

---

## Epic File Updates

- [ ] Epic status field added: `not-started` | `in-progress` | `done`
- [ ] FRs Covered section added to each epic
- [ ] Story Summary table added to each epic section
- [ ] All DONE stories have `points: N` field
- [ ] All stories have `key: X-Y-slug` field
- [ ] All stories in table have `status: done` (undone stories removed)
- [ ] detail_level field REMOVED (no longer used)
- [ ] Epic status matches story completion (all done → done, some done → in-progress)

---

## Story File Archiving

- [ ] `implementation-artifacts/archive/` directory created
- [ ] All NOT-DONE story files moved to archive/
- [ ] All DONE story files remain in implementation-artifacts/
- [ ] Archive preserves original filenames

---

## Sprint Files (Optional - for velocity tracking)

- [ ] `sprints/` directory created
- [ ] Historical sprint file created (if applicable)
- [ ] Sprint file has correct status (completed)
- [ ] Sprint dates populated
- [ ] All done stories from that sprint included
- [ ] Metrics calculated correctly (total_points, completed_points)

---

## Velocity Log (Optional)

- [ ] `sprints/velocity-log.yaml` created
- [ ] Sprint velocities calculated correctly
- [ ] Averages populated

---

## Legacy File Handling

- [ ] sprint-status.yaml archived (not deleted)
- [ ] Change proposal files preserved (not modified)

---

## Post-Migration Verification

- [ ] Run `/bmad-bmm-sprint-status` - shows epic progress correctly
- [ ] Epic files render correctly (no broken formatting)
- [ ] Sprint files are valid YAML
- [ ] Archive directory contains correct files
- [ ] Sprint-planning analyzes requirements fresh to create stories

---

## Understanding the New Approach

After migration, the workflow is:

1. **sprint-planning CREATES stories** based on capacity (not selects from backlog)
2. **Done stories in epics.md** are preserved as historical record
3. **Undone stories are removed** from epics.md (files archived for reference)
4. **Sprint-planning analyzes fresh** from FRs, NFRs, Architecture, and epic scope
5. **backlog-refinement workflow** is DEPRECATED (no longer exists)

When you run sprint-planning:
- Agent loads PRD, Architecture, NFRs, and epic scope
- Agent identifies remaining FRs not covered by done stories
- Agent creates stories sized to fit your capacity target

---

## Common Issues

| Issue | Solution |
|-------|----------|
| Epic status wrong | Update status field in epics.md manually |
| Points seem wrong | Update directly in epics.md Story Summary table |
| Missing FRs | Add to FRs Covered section in epic |
| Story in wrong epic | Move row in Story Summary table |
| Want fresh stories | Sprint-planning always creates fresh stories from requirements analysis |
