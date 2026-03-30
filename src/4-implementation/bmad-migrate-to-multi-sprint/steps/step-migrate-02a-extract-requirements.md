# Step Migrate-2a: Extract Requirements for Inventory (Sub-Step)

## STEP GOAL:

Extract all FRs, NFRs, Architecture requirements, and Additional Requirements (ARs) from the old epics.md and done stories to build the Requirements Inventory section.

## INPUTS (provided by parent step):

- `old_epics_file`: Path to the original epics.md (before migration)
- `done_stories`: Array of done stories with their full content
- `prd_file`: Path to PRD (for requirement descriptions if needed)
- `architecture_file`: Path to Architecture document (for ARCH requirement descriptions if needed)

## OUTPUTS (returned to parent step):

- `requirements_inventory`: Formatted Requirements Inventory section
- `coverage_map`: Requirements Coverage Map table
- `remaining_requirements`: List of requirements not yet covered by done stories

---

## EXECUTION PROTOCOL:

<workflow>

<step n="1" goal="Extract Requirements from Old Epics File">

<action>Scan the old epics.md for requirement patterns:</action>

### Functional Requirements (FRs)
Look for patterns like:
- `FR-001:`, `FR-1:`, `FR1:`, `FR 1:`
- `**FR-001**`, `**FR1**`
- Tables with FR identifiers
- Sections labeled "Functional Requirements" or "FRs"

### NonFunctional Requirements (NFRs)
Look for patterns like:
- `NFR-001:`, `NFR-1:`, `NFR1:`, `NFR 1:`
- `**NFR-001**`, `**NFR1**`
- Sections labeled "NonFunctional Requirements", "NFRs", "Quality Requirements"

### Architecture Requirements (ARCH)
Look for patterns like:
- `ARCH-001:`, `ARCH-1:`, `ARCH1:`, `ARCH 1:`
- `**ARCH-001**`, `**ARCH1**`
- Sections labeled "Architecture Requirements", "Technical Requirements"
- Lines starting with `**Architecture requirement:**` or `**ARCH**:`

### Additional Requirements (ARs)
Look for patterns like:
- `AR-001:`, `AR-1:`, `AR1:`, `AR 1:`
- `**AR-001**`, `**AR1**`
- Sections labeled "Additional Requirements", "Other Requirements"
- Any requirement not classified as FR, NFR, or ARCH

<critical>
Any requirement found that does not fit FR, NFR, or ARCH MUST be assigned an AR- prefix ID.
No other custom prefixes are allowed.
</critical>

<action>For each requirement found, capture:
  - ID (e.g., FR-001, NFR-002, ARCH-005)
  - Description/title
  - Source location (which epic section it was in)
</action>

<output>
📋 **Extracted from Old Epics File:**
- FRs: {{fr_count}}
- NFRs: {{nfr_count}}
- ARCH: {{arch_count}}
- ARs: {{ar_count}}
</output>

<action>Proceed to Step 2</action>

</step>

<step n="2" goal="Extract Requirements from Done Stories">

<action>For each done story, scan for requirement references:</action>

### Look in story content for:
- `**FRs addressed:** FR-001, FR-002, ...`
- `**NFRs addressed:** NFR-001, ...`
- `**Architecture requirements:** ARCH-005, ...`
- `**Additional requirements addressed:** AR-001, AR-002, ...`
- `**Covers:** FR-001, NFR-002`
- `**Requirements:** FR-001, FR-003`

<action>Build coverage mapping:
  - For each requirement ID found, note which story covers it
  - Track: requirement_id → [list of covering story keys]
</action>

<output>
📋 **Coverage from Done Stories:**
{{for each done story}}
- {{story.key}}: FRs [{{frs}}], NFRs [{{nfrs}}], ARCH [{{archs}}], ARs [{{ars}}]
{{end}}
</output>

<action>Proceed to Step 3</action>

</step>

<step n="3" goal="Cross-Reference with PRD/Architecture (Optional)">

<check if="PRD file exists">
  <action>Load PRD and extract full requirement descriptions</action>
  <action>Match FR/NFR IDs from epics to PRD for complete descriptions</action>
</check>

<check if="Architecture file exists">
  <action>Load Architecture and extract ARCH requirement descriptions</action>
  <action>Match ARCH IDs from epics to Architecture for complete descriptions</action>
</check>

<note>
If PRD/Architecture files don't exist or don't contain the requirements, use the descriptions found in the old epics.md.
</note>

<action>Proceed to Step 4</action>

</step>

<step n="4" goal="Build Requirements Inventory Section">

<action>Generate the consolidated Requirements Inventory:</action>

```markdown
## Requirements Inventory

### Functional Requirements

| ID | Requirement | Priority | Epic |
|----|-------------|----------|------|
{{for each FR}}
| {{fr_id}} | {{fr_description}} | {{fr_priority}} | Epic {{epic_num}} |
{{end}}

### NonFunctional Requirements

| ID | Requirement | Category | Epic |
|----|-------------|----------|------|
{{for each NFR}}
| {{nfr_id}} | {{nfr_description}} | {{nfr_category}} | Epic {{epic_num}} |
{{end}}

### Architecture Requirements

| ID | Requirement | Category | Epic |
|----|-------------|----------|------|
{{for each ARCH}}
| {{arch_id}} | {{arch_description}} | {{arch_category}} | Epic {{epic_num}} |
{{end}}

### Additional Requirements (ARs)

| ID | Requirement | Source | Epic |
|----|-------------|--------|------|
{{for each AR}}
| {{ar_id}} | {{ar_description}} | {{ar_source}} | Epic {{epic_num}} |
{{end}}
```

<note>
Coverage is tracked within each story's content. During sprint-planning, scan done stories in epic sections and extract their `**FRs addressed:**`, `**NFRs addressed:**`, `**Architecture requirements:**`, and `**Additional requirements addressed:**` sections to determine what's already covered.
</note>

<action>Proceed to Step 5</action>

</step>

<step n="5" goal="Return Results">

<action>Set outputs:
  - {{requirements_inventory}} = generated inventory section
  - {{remaining_requirements}} = list of all requirement IDs (for reference)
</action>

<output>
✅ **Requirements Inventory Built:**
- FRs: {{fr_count}}
- NFRs: {{nfr_count}}
- ARCH: {{arch_count}}
- ARs: {{ar_count}}
</output>

<action>Return results to parent step</action>

</step>

</workflow>

---

## REQUIREMENT EXTRACTION PATTERNS

### FR Patterns:
```
FR-001: Description
FR1: Description
**FR-001** - Description
| FR-001 | Description | ... |
- FR-001: Description
```

### NFR Patterns:
```
NFR-001: Description
NFR1: Description
**NFR-001** - Description
| NFR-001 | Description | ... |
- NFR-001: Description
```

### ARCH Patterns:
```
ARCH-001: Description
ARCH1: Description
**ARCH-001** - Description
**Architecture requirement:** ARCH-001
**Architecture requirements:** ARCH-001, ARCH-002
| ARCH-001 | Description | ... |
- ARCH-001: Description
```

### AR Patterns:
```
AR-001: Description
AR1: Description
**AR-001** - Description
| AR-001 | Description | ... |
- AR-001: Description
```

### Story Coverage Patterns:
```
**FRs addressed:** FR-001, FR-002, FR-003
**NFRs addressed:** NFR-001
**Architecture requirements:** ARCH-005
**Additional requirements addressed:** AR-001, AR-002
**Covers:** FR-001, NFR-002
```

---

## EDGE CASES

| Case | Handling |
|------|----------|
| Requirement ID mentioned but no description | Mark description as "From old epics.md - needs review" |
| Duplicate requirement IDs | Keep first occurrence, note duplicates |
| Requirements only in PRD, not in epics | Include from PRD, mark source as "PRD" |
| Requirements only in Architecture, not in epics | Include from Architecture, mark source as "Architecture" |
| No requirements found | Create empty inventory with placeholder text |
| Story references non-existent requirement | Create the requirement entry, mark as "Found in story" |

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- All FRs/NFRs/ARCHs/ARs from old epics.md captured
- Coverage map accurately reflects done stories
- Remaining requirements clearly identified
- Descriptions preserved or sourced from PRD/Architecture

### ❌ SYSTEM FAILURE:

- Missing requirements that were in old file
- Incorrect coverage mapping
- Duplicate requirements not handled
- Empty inventory when requirements existed

**Master Rule:** Preserve all requirement context - better to over-capture than miss requirements.
