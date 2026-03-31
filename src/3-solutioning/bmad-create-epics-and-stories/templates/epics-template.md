---
stepsCompleted: []
inputDocuments: []
preservedEpics: []
---

# {{project_name}} - Epic Breakdown

## Overview

This document defines the epic structure for {{project_name}}, organizing requirements from the PRD, UX Design (if exists), and Architecture into user-value-focused epics.

**Note:** Stories are created just-in-time during sprint planning based on capacity, not upfront during epic design.

## Requirements Inventory

### Functional Requirements

{{fr_list}}

### NonFunctional Requirements

{{nfr_list}}

### Architecture Requirements

{{arch_list}}

### Additional Requirements (ARs)

| ID | Requirement | Source | Epic |
|----|-------------|--------|------|
{{ar_list}}

## Epic List

{{epics_list}}

## Requirements Coverage Map

{{requirements_coverage_map}}

<!-- =========================================================== -->
<!-- Repeat for each epic in epics_list (N = 1, 2, 3...)        -->
<!-- =========================================================== -->

## Epic {{N}}: {{epic_title_N}}

**Epic ID:** `epic-{{N}}`
**Goal:** {{epic_goal_N}}
**Requirements Covered:** {{epic_requirements_N}} <!-- ALWAYS list each requirement individually: FR1, FR2, NFR1, ARCH1, AR-001. NEVER use ranges like FR1-3. -->
**Estimated Points:** {{epic_points_N}} (rough estimate for sprint planning)
**Jira Epic Key:** {{jira_epic_key_N}} <!-- Optional: e.g., PROJ-100 -->

### Epic {{N}} Description

{{epic_description_N}}

### Epic {{N}} Scope

**In Scope:**
{{epic_in_scope_N}}

**Out of Scope:**
{{epic_out_of_scope_N}}

### Epic {{N}} Dependencies

{{epic_dependencies_N}}

---

<!-- End epic repeat -->
