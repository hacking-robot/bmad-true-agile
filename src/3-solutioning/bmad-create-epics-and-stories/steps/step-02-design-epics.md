---
name: 'step-02-design-epics'
description: 'Design and approve the epics_list that will organize all requirements into user-value-focused epics'

# Path Definitions
workflow_path: '{project-root}/_bmad/bmm/workflows/3-solutioning/create-epics'

# File References
thisStepFile: './step-02-design-epics.md'
nextStepFile: './step-03-final-validation.md'
workflowFile: '{workflow_path}/workflow.md'
outputFile: '{planning_artifacts}/epics.md'

# Task References
advancedElicitationTask: '{project-root}/_bmad/core/workflows/advanced-elicitation/workflow.xml'
partyModeWorkflow: '{project-root}/_bmad/core/workflows/party-mode/workflow.md'

# Template References
epicsTemplate: '{workflow_path}/templates/epics-template.md'
---

# Step 2: Design Epic List

## STEP GOAL:

To design and get approval for the epics_list that will organize all requirements into user-value-focused epics.

## MANDATORY EXECUTION RULES (READ FIRST):

### Universal Rules:

- 🛑 NEVER generate content without user input
- 📖 CRITICAL: Read the complete step file before taking any action
- 🔄 CRITICAL: When loading next step with 'C', ensure entire file is read
- 📋 YOU ARE A FACILITATOR, not a content generator
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

### Role Reinforcement:

- ✅ You are a product strategist and technical specifications writer
- ✅ If you already have been given communication or persona patterns, continue to use those while playing this new role
- ✅ We engage in collaborative dialogue, not command-response
- ✅ You bring product strategy and epic design expertise
- ✅ User brings their product vision and priorities

### Step-Specific Rules:

- 🎯 Focus ONLY on creating the epics_list
- 🚫 FORBIDDEN to create individual stories in this step
- 💬 Organize epics around user value, not technical layers
- 🚪 GET explicit approval for the epics_list
- 🔗 **CRITICAL: Each epic must be standalone and enable future epics without requiring future epics to function**

## EXECUTION PROTOCOLS:

- 🎯 Design epics collaboratively based on extracted requirements
- 💾 Update {{epics_list}} in {outputFile}
- 📖 Document the requirements coverage mapping
- 🚫 FORBIDDEN to load next step until user approves epics_list

## EPIC DESIGN PROCESS:

### 1. Review Extracted Requirements and Preserved Epics

Load {outputFile} and review:

- **Functional Requirements:** Count and review FRs from Step 1
- **Non-Functional Requirements:** Review NFRs that need to be addressed
- **Additional Requirements (ARs):** Review AR- requirements from Architecture and UX
- **Preserved Epics:** Check frontmatter for `preservedEpics` array. If present, these epics and their stories were kept from a previous iteration and MUST be included as-is in the final epic list. Their requirements coverage counts toward the coverage map.

### 2. Explain Epic Design Principles

**EPIC DESIGN PRINCIPLES:**

1. **User-Value First**: Each epic must enable users to accomplish something meaningful
2. **Requirements Grouping**: Group related FRs that deliver cohesive user outcomes
3. **Incremental Delivery**: Each epic should deliver value independently
4. **Logical Flow**: Natural progression from user's perspective
5. **🔗 Dependency-Free Within Epic**: Stories within an epic must NOT depend on future stories

**⚠️ CRITICAL PRINCIPLE:**
Organize by USER VALUE, not technical layers:

**✅ CORRECT Epic Examples (Standalone & Enable Future Epics):**

- Epic 1: User Authentication & Profiles (users can register, login, manage profiles) - **Standalone: Complete auth system**
- Epic 2: Content Creation (users can create, edit, publish content) - **Standalone: Uses auth, creates content**
- Epic 3: Social Interaction (users can follow, comment, like content) - **Standalone: Uses auth + content**
- Epic 4: Search & Discovery (users can find content and other users) - **Standalone: Uses all previous**

**❌ WRONG Epic Examples (Technical Layers or Dependencies):**

- Epic 1: Database Setup (creates all tables upfront) - **No user value**
- Epic 2: API Development (builds all endpoints) - **No user value**
- Epic 3: Frontend Components (creates reusable components) - **No user value**
- Epic 4: Deployment Pipeline (CI/CD setup) - **No user value**

**🔗 DEPENDENCY RULES:**

- Each epic must deliver COMPLETE functionality for its domain
- Epic 2 must not require Epic 3 to function
- Epic 3 can build upon Epic 1 & 2 but must stand alone

### 3. Design Epic Structure Collaboratively

**Step A: Identify User Value Themes**

- If preserved epics exist, list them first and note which requirements (FR, NFR, ARCH, AR) they already cover
- Identify which requirements are NOT yet covered by preserved epics — these need new epics
- Look for natural groupings in the uncovered requirements
- Identify user journeys or workflows
- Consider user types and their goals

**Step B: Propose Epic Structure**
For each proposed epic:

1. **Epic Title**: User-centric, value-focused
2. **User Outcome**: What users can accomplish after this epic
3. **Requirements Coverage**: Which requirement IDs this epic addresses (FR, NFR, ARCH, AR)
   - ⚠️ **ALWAYS list every requirement individually** (e.g., `FR1, FR2, FR3, FR4`). NEVER use range notation like `FR1-4` or `FR1-FR4`. This applies to ALL requirement types: FR, NFR, ARCH, AR. Downstream tools cannot parse ranges.
4. **Implementation Notes**: Any technical or UX considerations

**Step C: Create the epics_list**

Format the epics_list as (preserved epics retain their original numbering and content):

```
## Epic List

### Epic 1: [Epic Title] *(preserved — X stories kept)*
[Original epic goal statement — unchanged]
**Requirements covered:** FR1, FR2, NFR1, ARCH1, AR-001, etc.

### Epic 2: [Epic Title] *(new)*
[Epic goal statement - what users can accomplish]
**Requirements covered:** FR4, FR5, NFR2, AR-002, etc.

[Continue for all epics — preserved first, then new]
```

**⚠️ PRESERVED EPIC RULES:**
- Do NOT modify preserved epic titles, goals, or story content
- Preserved epics may be renumbered if needed for logical flow, but their content stays intact
- If new FRs need to be added to a preserved epic, note this and ask the user whether to add them or create a new epic

### 4. Present Epic List for Review

Display the complete epics_list to user with:

- Total number of epics
- Requirements coverage per epic (FR, NFR, ARCH, AR)
- User value delivered by each epic
- Any natural dependencies

### 5. Create Requirements Coverage Map

⚠️ **NEVER use range notation** (e.g., `FR1-14`, `NFR1-3`, `AR-001-005`). Always list each requirement individually: `FR1, FR2, FR3, ...`. This applies to ALL requirement types: FR, NFR, ARCH, AR.

Create {{requirements_coverage_map}} showing how each requirement maps to an epic:

```
### Requirements Coverage Map

**Functional Requirements:**
FR1: Epic 1 - [Brief description]
FR2: Epic 1 - [Brief description]
FR3: Epic 2 - [Brief description]
...

**Non-Functional Requirements:**
NFR1: Epic 1 - [Brief description]
NFR2: Epic 3 - [Brief description]
...

**Architecture Requirements:**
ARCH1: Epic 1 - [Brief description]
ARCH2: Epic 2 - [Brief description]
...

**Additional Requirements:**
AR-001: Epic 1 - [Brief description]
AR-002: Epic 2 - [Brief description]
...
```

This ensures no requirements are missed.

### 6. Collaborative Refinement

Ask user:

- "Does this epic structure align with your product vision?"
- "Are all user outcomes properly captured?"
- "Should we adjust any epic groupings?"
- "Are there natural dependencies we've missed?"

### 7. Get Final Approval

**CRITICAL:** Must get explicit user approval:
"Do you approve this epic structure for proceeding to story creation?"

If user wants changes:

- Make the requested adjustments
- Update the epics_list
- Re-present for approval
- Repeat until approval is received

## CONTENT TO UPDATE IN DOCUMENT:

After approval, update {outputFile}:

1. Replace {{epics_list}} placeholder with the approved epic list
2. Replace {{requirements_coverage_map}} with the coverage map
3. Ensure all requirements (FR, NFR, ARCH, AR) are mapped to epics
4. **Preserved epics:** Keep their full sections (including all story content) intact in the document. Only update their position/numbering if the epic order changed.

### 8. Present MENU OPTIONS

Display: "**Select an Option:** [A] Advanced Elicitation [P] Party Mode [C] Continue"

#### Menu Handling Logic:

- IF A: Read fully and follow: {advancedElicitationTask}
- IF P: Read fully and follow: {partyModeWorkflow}
- IF C: Save approved epics_list to {outputFile}, update frontmatter, then read fully and follow: {nextStepFile}
- IF Any other comments or queries: help user respond then [Redisplay Menu Options](#8-present-menu-options)

#### EXECUTION RULES:

- ALWAYS halt and wait for user input after presenting menu
- ONLY proceed to next step when user selects 'C'
- After other menu items execution completes, redisplay the menu
- User can chat or ask questions - always respond when conversation ends, redisplay the menu options

## CRITICAL STEP COMPLETION NOTE

ONLY WHEN C is selected and the approved epics_list is saved to document, will you then read fully and follow: {nextStepFile} to begin story creation step.

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- Epics designed around user value
- All requirements (FR, NFR, ARCH, AR) mapped to specific epics
- epics_list created and formatted correctly
- Requirements coverage map completed
- User gives explicit approval for epic structure
- Document updated with approved epics

### ❌ SYSTEM FAILURE:

- Epics organized by technical layers
- Missing requirements (FR, NFR, ARCH, or AR) in coverage map
- Using range notation for requirements (e.g., `FR1-14`, `NFR1-3`, `AR-001-005` instead of listing each individually)
- No user approval obtained
- epics_list not saved to document

**Master Rule:** Skipping steps, optimizing sequences, or not following exact instructions is FORBIDDEN and constitutes SYSTEM FAILURE.
