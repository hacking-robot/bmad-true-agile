# BMad True Agile

A drop-in replacement for the official BMad Method `bmm` module with True Agile enhancements: deviation detection, capacity-first sprint planning, and extended sprint lifecycle workflows.

## What's Different from Official BMM

- **No stories during epic creation** -- epics are containers only; stories are created just-in-time during sprint planning
- **Deviation detection** -- automatically detect drift between PRD/Architecture and the actual codebase during sprint planning
- **Capacity-first planning** -- plan sprints based on realistic team capacity, not wishful estimates
- **Full sprint lifecycle** -- sprint review, backlog refinement, retrospectives, story refresh
- **JIRA integration** -- link BMAD stories to JIRA issues
- **Multi-sprint migration** -- convert single-sprint projects to multi-sprint structure

## Installation

```bash
git clone https://github.com/hacking-robot/bmad-true-agile.git
npx bmad-method install --custom-source ./bmad-true-agile/src
```

The installer will prompt *How would you like to proceed?* — choose **Modify BMAD Installation**. When prompted to select modules, **do not select the official `bmm` module** -- this repo replaces it.

To update, pull the latest and re-run the same command.

### Requirements

- Node.js 18+
- [BMad Method CLI](https://github.com/bmad-code-org/BMAD-METHOD) (`npx bmad-method`)
- Claude Code

## Usage

### Phase 1 - Analysis

```
bmad-brainstorming            optional - explore ideas
bmad-market-research          optional - competitive landscape
bmad-domain-research          optional - industry deep dive
bmad-technical-research       optional - tech feasibility
bmad-product-brief            nail down the product idea
```

### Phase 2 - Planning

```
bmad-create-prd               required - product requirements document
bmad-validate-prd             recommended - validate the PRD
bmad-create-ux-design         if the project has a UI
```

### Phase 3 - Solutioning

```
bmad-create-architecture      required - technical decisions
bmad-create-epics-and-stories creates epic containers only (no stories)
bmad-check-implementation-readiness   validate alignment across all docs
```

### Phase 4 - Sprint Cycle

```
bmad-sprint-planning          select stories from epics, create sprint plan (stories start as backlog)
  |
  v
+--------------------------------------------------+
| Story Cycle (repeat for each story in sprint)     |
|                                                   |
|  bmad-create-story      prepare story with detail |
|         |                                         |
|         v                                         |
|  bmad-dev-story         implement the story       |
|         |                                         |
|         v                                         |
|  bmad-code-review       adversarial review        |
|         |                                         |
|         +-- issues found --> back to dev-story    |
|         +-- approved ------> next story           |
|                                                   |
|  bmad-qa-generate-e2e-tests   optional            |
+--------------------------------------------------+
  |
  v
bmad-sprint-review             close sprint, calculate velocity
  |
  v
bmad-retrospective             at epic end - review lessons learned
  |
  v
bmad-sprint-planning           plan next sprint (loop back)
```

### Anytime

| Skill | Code | When to use |
|-------|------|-------------|
| `bmad-correct-course` | CC | Major change needed mid-sprint |
| `bmad-refresh-story` | RS | *(legacy)* Story went stale, needs fresh context |
| `bmad-sprint-status` | SS | Check where things stand |
| `bmad-backlog-refinement` | BR | Upgrade story detail level before planning |
| `bmad-jira-link` | JL | Link stories to JIRA |
| `bmad-migrate-to-multi-sprint` | MTMS | Convert single-sprint to multi-sprint |
| `bmad-quick-dev` | QQ | Skip ceremony - intent in, code out |

### Quick Start

Already have a PRD and architecture? Jump straight to:

```
bmad-sprint-planning    ->    bmad-create-story    ->    bmad-dev-story
```

Or skip all ceremony:

```
bmad-quick-dev          intent in, code out
```

## License

MIT

## Compatibility

This module is compatible with [BMad Studio](https://github.com/hacking-robot/bmad-studio).

## Credits

Based on [BMad Method](https://github.com/bmad-code-org/BMAD-METHOD) by Brian (BMad) Madison
