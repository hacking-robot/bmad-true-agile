# BMad True Agile (BMM)

A BMad module for True Agile development with deviation detection and capacity-first sprint planning. This is a drop-in replacement for the official `bmm` module.

## What is True Agile?

True Agile extends BMad Method with:

- **Deviation Detection**: Automatically detect drift between planning documents (PRD, Architecture) and actual codebase
- **Capacity-First Sprint Planning**: Plan sprints based on realistic team capacity, not wishful estimates
- **Human-Centric Workflows**: Designed for human developers using git history as source of truth

## Installation

### Via bmadboard

Install as a GitHub module to override the official bmm module:

1. Open bmadboard
2. Add GitHub module: `hacking-robot/bmad-true-agile`
3. Install with core module (this module will be used as `bmm`)

### Manual Installation (with official bmad-method CLI)

```bash
# Install core module first
npx bmad-method install --modules core

# Clone and use as custom content
git clone https://github.com/hacking-robot/bmad-true-agile.git
npx bmad-method install --custom-content ./bmad-true-agile/src
```

## Module Code

`bmm` - Uses the same module code as the official BMad Method module.

## Features

### Deviation Detection (Step 4 in Sprint Planning)

During sprint planning, automatically:
1. Scan codebase for architecture drift (file structure, dependencies, implementation patterns)
2. Check PRD drift (scope, features, requirements)
3. Identify incomplete FRs from previous sprint

Each deviation is presented with options:
- **Cancel** (recommended for architecture/PRD drift)
- **Proceed aware** (continue with knowledge of drift)
- **Include in sprint** (for incomplete FRs)

### Capacity-First Sprint Planning

1. Start with team capacity (hours available)
2. Load velocity history
3. Select epics based on realistic capacity
4. Create stories that fit the sprint

### Additional Workflows

- **Sprint Review**: Structured sprint review with completion metrics
- **Refresh Story**: Update story status based on actual git commits
- **Backlog Refinement**: Prepare stories for future sprints
- **Jira Link**: Connect stories to external issue trackers

## License

MIT

## Credits

Based on [BMad Method](https://github.com/bmad-code-org/BMAD-METHOD) by Brian (BMad) Madison
