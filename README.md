# BMad True Agile

A fork of [BMad Method](https://github.com/bmad-code-org/BMAD-METHOD) with True Agile enhancements.

## What's Different

This fork extends BMad Method with:

- **Deviation Detection**: Step 4 in sprint planning scans your actual codebase and compares it against planning documents, surfacing discrepancies before story creation
- **Capacity-First Sprint Planning**: Full sprint planning workflow (not just sprint status generation)
- **Incomplete FR Detection**: Identifies functional requirements from "done" stories that may not be fully implemented

## Installation

```bash
npx bmad-true-agile install
```

## Key Features

### Deviation Detection (Step 4)

Before creating stories, the workflow checks:

1. **Architecture Drift** - File structure, dependencies, patterns vs. architecture spec
2. **PRD Drift** - Project scope, features vs. PRD requirements  
3. **Incomplete FRs** - Previous sprint stories marked done but may have unimplemented requirements

User always chooses whether to:
- Cancel and run `correct-course` to reconcile
- Proceed aware of the drift

## Module Code

- `bta` - BMad True Agile (BMM-based)

## Upstream

Forked from [bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)

To sync with upstream:

```bash
git fetch upstream
git merge upstream/master
```

## License

MIT
