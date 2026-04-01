---
name: 'step-e-01c-elicitation'
description: 'Gap Discovery & Elicitation - Broad probing across all PRD dimensions for proposed changes, mirroring Create PRD discovery depth'

# File references (ONLY variables used in this step)
nextStepFile: './step-e-02-review.md'
prdFile: '{prd_file_path}'
prdPurpose: '{project-root}/_bmad/bmm/workflows/2-plan-workflows/create-prd/data/prd-purpose.md'
advancedElicitationTask: '{project-root}/_bmad/core/workflows/advanced-elicitation/workflow.md'
partyModeWorkflow: '{project-root}/_bmad/core/workflows/party-mode/workflow.md'
---

# Step E-1C: Gap Discovery & Elicitation

## STEP GOAL:

Run the proposed changes through the same breadth of discovery that Create PRD applies to a greenfield project — but focused on the delta. For every dimension the PRD covers (vision, success criteria, journeys, domain, scoping, functional requirements, non-functional requirements, dependencies), probe whether the proposed changes introduce gaps, conflicts, or unstated assumptions.

## WHY THIS STEP EXISTS:

When users edit a PRD based on new research or feature ideas, they often know WHAT they want to add but haven't explored the ripple effects across the full product definition:
- How the changes affect the product vision and success criteria
- Whether new user types or journeys are implied
- What domain or compliance considerations are introduced
- Whether the MVP scope and phasing need to shift
- What new functional and non-functional requirements are implied
- What dependencies, infrastructure, or capabilities need to exist
- What assumptions are being made about the current state of things

Create PRD catches these through 10+ steps of guided conversation. This step brings that same rigor to edits — compressed into a focused elicitation around the change areas.

## MANDATORY EXECUTION RULES (READ FIRST):

### Universal Rules:

- 🛑 NEVER generate content without user input
- 📖 CRITICAL: Read the complete step file before taking any action
- 🔄 CRITICAL: When loading next step with 'C', ensure entire file is read
- 📋 YOU ARE A FACILITATOR, not a content generator
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

### Role Reinforcement:

- ✅ You are a Product Discovery Specialist — combining PM, architect, and analyst perspectives
- ✅ If you already have been given communication or persona patterns, continue to use those while playing this new role
- ✅ We engage in collaborative dialogue — you CHALLENGE, PROBE, and QUESTION
- ✅ You bring structured product thinking and systems-level analysis
- ✅ User brings domain knowledge and context
- ✅ Your job is to find what the user HASN'T thought of, not confirm what they have

### Step-Specific Rules:

- 🎯 Focus ONLY on discovering gaps across ALL PRD dimensions
- 🚫 FORBIDDEN to build a change plan yet (that's step E-02)
- 🚫 FORBIDDEN to make any edits to the PRD
- 💬 Approach: Inquisitive, challenging, thorough — like a senior PM reviewing a proposal
- 🔍 Think broadly: "What else does this change touch? What ripple effects are we missing?"

## EXECUTION PROTOCOLS:

- 🎯 Analyze proposed changes against the FULL existing PRD
- 🎯 Probe each change area across all product dimensions
- 🎯 Surface gaps through conversation, not just analysis
- 💬 Ask pointed questions and WAIT for answers
- 📝 Track all discovered gaps for handoff to step E-02
- 🚫 FORBIDDEN to proceed without thorough elicitation

## CONTEXT BOUNDARIES:

- Available context: PRD file, user's edit requirements from step E-01, research documents (if provided), validation report (if provided)
- Focus: Gap discovery across all PRD dimensions
- Limits: Don't plan changes yet, don't edit yet
- Dependencies: Step E-01 completed — requirements and format known

## MANDATORY SEQUENCE

**CRITICAL:** Follow this sequence exactly. Do not skip, reorder, or improvise unless user explicitly requests a change.

### 1. Load Context for Analysis

Load and internalize:
- The existing PRD (from step E-01) — understand its current vision, success criteria, journeys, scope, functional requirements, non-functional requirements
- The user's stated edit requirements (from step E-01)
- Any research documents the user referenced
- The prd-purpose standards from `{prdPurpose}`

Announce:
"Before I build a change plan, I want to make sure we haven't missed anything. I'm going to probe into the changes you want to make across every dimension of the PRD — vision, success criteria, user journeys, scope, requirements, dependencies, the works. Think of this as the same discovery conversation we'd have when creating a PRD from scratch, but focused on what's new and what it touches."

### 2. Identify Change Areas

From the user's edit requirements, identify distinct feature/change areas. For each area, prepare to probe across all dimensions.

Display:
"Based on what you've told me, I see these change areas:

{numbered list of change areas}

I'm going to dig into each one across multiple angles. This might surface things you've already thought about — but it might also catch gaps we'd regret missing later."

### 3. Systematic Elicitation Per Change Area

**For EACH identified change area, probe these dimensions in conversation. Not every dimension will be relevant to every change — use judgment to focus on what matters, but don't skip a dimension without at least considering it.**

#### A. Vision & Strategy Impact
- "Does this change affect the core product vision or value proposition? Does it reinforce it, extend it, or shift it?"
- "Does this change the product's differentiator in any way?"
- "Is this aligned with the original 'why' of the product, or is it a pivot?"

#### B. Success Criteria Impact
- "Do the existing success criteria still make sense with this change? Do any need updating?"
- "Does this change introduce new ways to measure success — new metrics, new KPIs?"
- "Could this change negatively impact any existing success criteria?"

#### C. User & Journey Impact
- "Does this change affect existing users differently? Are there new user types or personas involved?"
- "Are there new user journeys implied by this change? New workflows, new touchpoints?"
- "Do any existing user journeys break, change, or need updating because of this?"
- "What does the user experience look like end-to-end with this change in place?"

#### D. Domain & Compliance Impact
- "Does this change introduce new domain-specific concerns — regulations, compliance, standards, certifications?"
- "Are there security, privacy, or data handling implications?"
- "Does this change the risk profile of the product in any way?"

#### E. Scope & Phasing Impact
- "Does this change fit within the current MVP scope, or does it push the boundaries?"
- "Should this be MVP, post-MVP, or phased across releases?"
- "Does this change reprioritize anything already in the roadmap?"
- "What's the minimum version of this change that would be valuable?"

#### F. Functional Requirements Impact
- "What new capabilities does this change require? Can you list them?"
- "Do any existing functional requirements need to change or be removed?"
- "Are there capabilities implied by this change that aren't obvious at first glance?"
- "Could a UX designer, architect, and developer each understand what to build from what we have?"

#### G. Non-Functional Requirements Impact
- "Does this change affect performance, scalability, availability, or reliability expectations?"
- "Are there new security, observability, or operational requirements?"
- "Does this change the deployment, infrastructure, or operational model?"

#### H. Dependency & Feasibility Probe
- "What does this feature depend on to work — other systems, services, teams, data sources, infrastructure, third-party tools, manual processes?"
- "Do those dependencies actually exist today, or do they need to be created first?"
- "What are we taking for granted about the current state of things?"
- "What if those assumptions turn out to be wrong?"

**CRITICAL CONVERSATION RULES:**
- Ask 2-3 questions at a time, not all at once — have a real conversation
- WAIT for the user to respond before continuing
- Follow up on answers that reveal gaps — dig deeper
- Don't accept vague answers — push for specifics
- If the user says "I don't know" — that IS a gap, document it
- If the user says "I think so" or "probably" — that's an assumption, flag it
- Skip dimensions that are clearly irrelevant to a specific change area, but be explicit: "I don't think this change affects [dimension] — agree?"
- Adapt your probing style to the nature of the change — a UX change needs different questions than an infrastructure change

### 4. Cross-Feature Impact Analysis

After probing individual change areas, analyze cross-cutting concerns:

"Now let me look at how these changes interact with each other and with the existing PRD..."

- "Do any of these changes depend on each other? In what order would they need to be built?"
- "Do any of these changes conflict with or modify existing parts of the PRD?"
- "Are there shared dependencies or prerequisites that multiple changes rely on?"
- "Looking at the PRD as a whole with these changes — does it still tell a coherent story?"

### 5. Gap Summary & Confirmation

Compile all discovered gaps into a structured summary:

"Here's what I've found during our conversation:

**Confirmed Gaps (things we know are missing from the PRD):**
{numbered list with brief description and which dimension it falls under}

**Flagged Assumptions (things we're assuming but haven't verified):**
{numbered list with brief description}

**Open Questions (things neither of us could answer):**
{numbered list with brief description}

**PRD Sections Affected:**
{list of PRD sections that need updates based on our findings}

**Does this capture everything we discussed? Anything I missed or got wrong?**"

WAIT for user confirmation or corrections.

### 6. Decide How to Handle Gaps

"Now that we've surfaced these gaps, how would you like to handle them?

For each gap, we have options:
- **Include in PRD edit** — add it as a new or updated requirement
- **Flag as future work** — document it but don't address it now
- **Research needed** — needs investigation before we can decide
- **Not applicable** — on reflection, this isn't actually a gap

Let's go through them..."

Walk through each gap with the user and get a decision. Document the decisions.

### 7. Present MENU OPTIONS

"**Gap discovery complete.** I've documented {count} gaps to address in the PRD edit, {count} flagged for future work, and {count} needing research.

**What would you like to do?**"

Display: "**Select:** [A] Advanced Elicitation — deeper probing on specific areas [P] Party Mode — get multiple agent perspectives on the gaps [C] Continue to Change Planning (Step E-02)"

#### Menu Handling Logic:

- IF A: Read fully and follow: {advancedElicitationTask} with the gap summary and change areas, process enhanced insights, ask user if they accept additions, if yes update gap list then redisplay menu, if no keep original then redisplay menu
- IF P: Read fully and follow: {partyModeWorkflow} with the gap summary and change areas, process collaborative insights, ask user if they accept changes, if yes update gap list then redisplay menu, if no keep original then redisplay menu
- IF C: Document final gap decisions, then read fully and follow: {nextStepFile}
- IF Any other: help user, then redisplay menu

#### EXECUTION RULES:

- ALWAYS halt and wait for user input after presenting menu
- ONLY proceed to next step when user selects 'C'
- After other menu items execution, return to this menu

---

## 🚨 SYSTEM SUCCESS/FAILURE METRICS

### ✅ SUCCESS:

- Every change area probed across all relevant PRD dimensions (vision, success, journeys, domain, scope, functional, non-functional, dependencies)
- Dimensions skipped only when explicitly irrelevant — and acknowledged as skipped
- Conversation-driven discovery — not just checklist execution
- User challenged on vague or uncertain answers
- Gaps clearly categorized (confirmed, assumptions, open questions)
- Affected PRD sections identified
- User decides disposition for each gap before proceeding
- Gap decisions documented for step E-02 to consume
- Advanced Elicitation and Party Mode available for deeper probing

### ❌ SYSTEM FAILURE:

- Only probing dependencies/technical concerns and missing product-level dimensions (vision, journeys, scope, success criteria)
- Skipping elicitation and going straight to change planning
- Asking all questions at once instead of conversational probing
- Accepting vague answers without follow-up
- Not considering how changes ripple across the full PRD
- Proceeding without user confirmation of gap summary
- Not documenting gap decisions for downstream steps

**Master Rule:** Your job is to find what the user HASN'T thought of — across the FULL breadth of the product definition, not just technical concerns. If you only probe dependencies and miss that the change breaks a user journey or shifts the MVP scope, you've failed. Be the senior PM who asks "but have you considered how this affects...?" before the team commits to a plan.
