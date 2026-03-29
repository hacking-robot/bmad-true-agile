# Step Migrate-3: Archive and Finalize

## STEP GOAL:

To archive undone story files, replace the old epics.md, and finalize the migration.

## MANDATORY EXECUTION RULES (READ FIRST):

### Universal Rules:

- 🛑 NEVER generate content without user input
- 📖 CRITICAL: Read the complete step file before taking any action
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

### Archive Rules:

- 📦 Archive = move, not delete
- 🔒 Original files preserved for reference
- 📁 Create archive directory if needed
- ✅ Verify all operations before proceeding

---

## EXECUTION PROTOCOLS:

<workflow>

<step n="1" goal="Identify Files to Archive">

<action>Load migration analysis (done/not-done status)</action>

<action>Identify undone story files from {implementation_artifacts}/</action>

<output>
📦 **Identifying Files to Archive**
</output>

<check if="no undone story files">
  <output>
✅ **No files to archive** - All stories are done!
  </output>
  <action>Skip to Step 3</action>
</check>

<check if="undone story files exist">
  <output>
**Story files to archive (not done):**

{{for each undone story}}
  ├── {{story_file}} → archive/{{story_file}}
{{end}}

**Total:** {{undone_count}} files
  </output>

  <action>Proceed to Step 2</action>
</check>

</step>

<step n="2" goal="Create Archive and Move Files">

<action>Create archive directory: {implementation_artifacts}/archive/</action>

<action>For each undone story file:
  1. Move from {implementation_artifacts}/{{story_file}}
  2. To {implementation_artifacts}/archive/{{story_file}}
</action>

<output>
📁 **Archiving Undone Story Files**

Created: {implementation_artifacts}/archive/

{{for each undone story}}
  ✅ Moved: {{story_file}}
{{end}}

{{undone_count}} files archived.
</output>

<action>Proceed to Step 3</action>

</step>

<step n="3" goal="Replace Old Epics File">

<action>Create backup: Copy epics.md → epics-pre-migration-backup.md</action>

<action>Replace epics.md with epics-migrated.md</action>

<action>Delete the temporary epics-migrated.md</action>

<output>
📝 **Updated Epics File**

- Backup: epics-pre-migration-backup.md
- New file: epics.md (migrated format)
</output>

<action>Proceed to Step 4</action>

</step>

<step n="4" goal="Archive Legacy Files (Optional)">

<check if="sprint-status.yaml exists">
  <action>Move to archive/sprint-status.yaml</action>
  <output>
🗃️ Archived: sprint-status.yaml → archive/sprint-status.yaml
  </output>
</check>

<action>Proceed to Step 5</action>

</step>

<step n="5" goal="Proceed to Sprint History">

<output>
📦 **Archive Step Complete**

Archived: {{archived_count}} story files
Backup: epics-pre-migration-backup.md created
Replaced: epics.md with migrated version
</output>

<action>Proceed to step-migrate-04-sprint-history.md</action>

</step>

</workflow>

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- All undone story files archived (not deleted)
- Old epics.md backed up
- New epics.md valid and complete
- User informed of next steps

### ❌ SYSTEM FAILURE:

- Deleting files without archiving
- Losing story content
- Not creating backup
- Corrupted epics.md

**Master Rule:** Archive, never delete. Backup before replace.
