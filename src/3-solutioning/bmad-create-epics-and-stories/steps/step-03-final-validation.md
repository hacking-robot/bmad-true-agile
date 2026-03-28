---
name: 'step-03-final-validation'
description: 'Validate complete coverage of all requirements by epics and prepare for sprint planning'

# Path Definitions
workflow_path: '{project-root}/_bmad/bmm/workflows/3-solutioning/create-epics'

# File References
thisStepFile: './step-03-final-validation.md'
workflowFile: '{workflow_path}/workflow.md'
outputFile: '{planning_artifacts}/epics.md'

# Task References
advancedElicitationTask: '{project-root}/_bmad/core/workflows/advanced-elicitation/workflow.xml'
partyModeWorkflow: '{project-root}/_bmad/core/workflows/party-mode/workflow.md'

# Template References
epicsTemplate: '{workflow_path}/templates/epics-template.md'
---

# Step 3: Final Validation

## STEP GOAL:

To validate complete coverage of all requirements by epics and ensure epics are ready for sprint planning.

## MANDATORY EXECUTION RULES (READ FIRST):

### Universal Rules:

- 🛑 NEVER generate content without user input
- 📖 CRITICAL: Read the complete step file before taking any action
- 🔄 CRITICAL: Process validation sequentially without skipping
- 📋 YOU ARE A FACILITATOR, not a content generator
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

### Role Reinforcement:

- ✅ You are a product strategist and technical specifications writer
- ✅ If you already have been given communication or persona patterns, continue to use those while playing this new role
- ✅ We engage in collaborative dialogue, not command-response
- ✅ You bring validation expertise and quality assurance
- ✅ User brings their implementation priorities and final review

### Step-Specific Rules:

- 🎯 Focus ONLY on validating epic coverage and structure
- 🚫 FORBIDDEN to skip any validation checks
- 💬 Validate FR coverage and epic dependencies
- 🚫 FORBIDDEN to create stories - stories are created during sprint planning

## EXECUTION PROTOCOLS:

- 🎯 Validate every requirement has epic coverage
- 💾 Check epic dependencies and flow
- 📖 Verify architecture alignment
- 🚫 FORBIDDEN to approve incomplete coverage

## CONTEXT BOUNDARIES:

- Available context: Complete epic breakdown from Step 2
- Focus: Final validation of requirements coverage and epic structure
- Limits: Validation only, no new content creation
- Dependencies: Completed epic design from Step 2

## VALIDATION PROCESS:

### 1. FR Coverage Validation

Review the epic breakdown to ensure EVERY FR is covered:

**CRITICAL CHECK:**

- Go through each FR from the Requirements Inventory
- Verify it appears in at least one epic's FR coverage list
- No FRs should be left uncovered
- Present a coverage matrix showing FR → Epic mapping

### 2. Architecture Alignment Validation

**Check for Starter Template Setup:**

- Does Architecture document specify a starter template?
- If YES: Epic 1 should account for initial project setup
- This includes cloning, installing dependencies, initial configuration

**Database/Entity Creation Approach:**

- Are database tables/entities planned for just-in-time creation?
- ❌ WRONG: Epic 1 creates all tables upfront
- ✅ RIGHT: Tables created as needed during sprint development

### 3. Epic Structure Validation

**Check that:**

- Epics deliver user value, not technical milestones
- Dependencies flow naturally
- Foundation epics only setup what's needed
- No big upfront technical work

### 4. Dependency Validation (CRITICAL)

**Epic Independence Check:**

- Does each epic deliver COMPLETE functionality for its domain?
- Can Epic 2 function without Epic 3 being implemented?
- Can Epic 3 function standalone using Epic 1 & 2 outputs?
- ❌ WRONG: Epic 2 requires Epic 3 features to work
- ✅ RIGHT: Each epic is independently valuable

### 5. Complete and Save

If all validations pass:

- Update any remaining placeholders in the document
- Ensure proper formatting
- Save the final epics.md

**Present Final Menu:**
**All validations complete!** [C] Complete Workflow

When C is selected, the workflow is complete and the epics.md is ready for sprint planning.

## NEXT STEPS

Epics complete. Stories will be created during sprint planning based on capacity.

**To start development:**
Run `/bmad-bmm-sprint-planning` to create your first sprint and stories.

Upon Completion of task output: offer to answer any questions about the Epics.
