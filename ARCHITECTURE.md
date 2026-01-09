# TomOS Command Tower — Documentation & Knowledge Architecture

## Overview

This document defines the canonical architecture for managing documentation, project knowledge, and AI context across the TomOS ecosystem. The **Command Tower** is the central knowledge hub that powers all TomOS Family projects.

**Goals:**
- Single source of truth for all project documentation
- Consistent access across all Claude interfaces (Code CLI, web, iOS, Desktop)
- Support for other LLMs (ChatGPT, etc.) via fallback methods
- Clear separation between shared resources and project-specific docs
- ADHD-friendly: minimal friction to find, read, and update docs

---

## Platform Access Matrix

| Platform | Method | Read | Write | Sync |
|----------|--------|------|-------|------|
| **Claude Code CLI** | GitHub MCP | ✅ | ✅ | Real-time |
| **Claude.ai (web)** | GitHub Integration | ✅ | ❌ | Manual sync button |
| **Claude.ai (iOS)** | GitHub Integration | ✅ | ❌ | Manual sync button |
| **Claude Desktop** | Local GitHub MCP | ✅ | ✅ | Real-time |
| **ChatGPT / Other** | Raw GitHub URLs | ✅ | ❌ | Manual fetch |

**Write workflow:** All documentation edits happen in Claude Code → commit → push → other platforms sync/fetch.

---

## Repository Structure

### Primary Repo: `tomos-command-tower`

```
tomos-command-tower/
├── README.md                           # Index & quick reference
├── CONVENTIONS.md                      # Shared coding/naming conventions
├── ARCHITECTURE.md                     # This document (system overview)
│
├── design/                             # 🎨 Shared Design System
│   ├── DESIGN-SYSTEM.md               # Master design reference
│   ├── foundations/
│   │   ├── colors.md                  # Palette, semantic colors, dark mode
│   │   ├── typography.md              # Fonts, scales, hierarchy
│   │   ├── spacing.md                 # Grid, margins, padding system
│   │   ├── iconography.md             # SF Symbols usage, custom icons
│   │   └── motion.md                  # Animation principles
│   ├── components/
│   │   ├── buttons.md
│   │   ├── cards.md
│   │   ├── inputs.md
│   │   ├── lists.md
│   │   ├── navigation.md
│   │   └── modals.md
│   └── patterns/
│       ├── onboarding.md
│       ├── empty-states.md
│       ├── error-handling.md
│       ├── loading-states.md
│       └── accessibility.md
│
├── projects/                           # 📁 Project-Specific Documentation
│   ├── tomos-app/
│   │   ├── CLAUDE.md                  # AI context file
│   │   ├── README.md                  # Project overview
│   │   ├── ROADMAP.md                 # Feature roadmap
│   │   └── API.md                     # API documentation
│   │
│   ├── tomos-launcher/
│   │   ├── CLAUDE.md
│   │   ├── README.md
│   │   └── SPEC.md                    # Feature specification
│   │
│   ├── notorious-dad/
│   │   ├── CLAUDE.md
│   │   ├── README.md
│   │   └── API.md
│   │
│   ├── legalos/
│   │   ├── CLAUDE.md
│   │   ├── README.md
│   │   └── tools/                     # Tool-specific docs
│   │
│   └── nexus/                         # TomOS × LegalOS integration layer
│       ├── CLAUDE.md
│       └── INTEGRATION.md
│
├── templates/                          # 📝 Reusable Templates
│   ├── CLAUDE-TEMPLATE.md             # Template for new project CLAUDE.md
│   ├── PROJECT-README-TEMPLATE.md
│   └── SPEC-TEMPLATE.md
│
└── .claude/                            # 🤖 Claude-specific config
    ├── project-mappings.md            # Which docs → which Claude Projects
    └── context-guidelines.md          # How to load context effectively
```

---

## CLAUDE.md Standard Format

Every project gets a `CLAUDE.md` file as the primary AI context document.

### Template Structure

```markdown
# [Project Name] — Claude Context

## Quick Reference
- **Repo:** github.com/braggy9/[repo]
- **Type:** [iOS App | macOS App | API | Web | Tool]
- **Stack:** [Swift/SwiftUI | TypeScript/Next.js | etc.]
- **Status:** [Active Development | Maintenance | Planning]

## Project Purpose
[2-3 sentences describing what this project does and why it exists]

## Architecture Overview
[High-level architecture, key components, data flow]

## Design System
This project follows the TomOS Family Design System.
Reference: /design/DESIGN-SYSTEM.md

Key deviations or project-specific additions:
- [Any project-specific design notes]

## Key Files & Entry Points
- `[path]` — [description]
- `[path]` — [description]

## Current State
### What's Working
- [Feature 1]
- [Feature 2]

### In Progress
- [Feature being built]

### Known Issues
- [Issue 1]

## Dependencies & Integrations
- **Notion:** [how it integrates]
- **Vercel:** [deployment details]
- **Other services:** [details]

## Development Notes
### Running Locally
[Commands to run/build]

### Environment Variables
[Required env vars, where they're stored]

### Testing
[How to test]

## AI Assistant Guidelines
### Do
- [Preferred approaches]
- [Coding style preferences]

### Don't
- [Anti-patterns to avoid]
- [Things that have caused issues]

## Related Documentation
- [Link to related docs in this repo]
- [Link to external resources]
```

---

## Claude Project Setup (claude.ai)

### Recommended Project Structure

Create separate Claude Projects for each major workstream:

| Claude Project | GitHub Files to Include | Purpose |
|----------------|------------------------|---------|
| **TomOS App Development** | `/projects/tomos-app/*`, `/design/*` | Core app development |
| **TomOS Launcher** | `/projects/tomos-launcher/*`, `/design/*` | Launcher development |
| **Notorious DAD** | `/projects/notorious-dad/*` | Playlist generator |
| **LegalOS Tools** | `/projects/legalos/*`, `/projects/nexus/*` | Legal automation |
| **TomOS Design System** | `/design/*` | UI/UX work across all apps |

### Project Setup Steps

1. **Create Project** in claude.ai
2. **Add GitHub Knowledge:**
   - Click "+" in Project Knowledge
   - Select "GitHub"
   - Choose `tomos-command-tower` repo
   - Use "Configure files" to select relevant folders
3. **Add Project Instructions** (in Project settings):
   ```
   This project uses documentation from the TomOS Family ecosystem.
   Always reference the CLAUDE.md file first for project context.
   Follow the Design System guidelines in /design/.
   ```
4. **Sync regularly** before starting new work sessions

---

## Cross-Platform Workflow

### Starting a New Feature (Claude Code)

```bash
# 1. Ensure docs are current
cd ~/projects/tomos-command-tower
git pull origin main

# 2. Start Claude Code in the relevant project
cd ~/projects/tomos-app
claude

# 3. Claude Code has MCP access to read/write docs
# "Update the CLAUDE.md to reflect the new quick capture feature"
```

### Continuing Work (Claude.ai Web/iOS)

1. Open the relevant Claude Project
2. Click "Sync" on GitHub knowledge to pull latest
3. Start chatting — context is loaded
4. If you need to update docs, note what needs changing
5. Later: make edits in Claude Code and push

### Ad-hoc Questions (Any Platform)

**Claude.ai:** 
- Click "+" → "Add from GitHub" → select specific file
- Or just paste the raw GitHub URL and ask Claude to fetch

**ChatGPT/Other:**
- Provide raw URL: `https://raw.githubusercontent.com/braggy9/tomos-command-tower/main/projects/tomos-app/CLAUDE.md`
- Ask to fetch and use as context

---

## Sync & Update Protocol

### When to Sync

| Trigger | Action |
|---------|--------|
| Starting new chat in a Project | Sync GitHub knowledge |
| Before major feature discussion | Sync to ensure current state |
| After Claude Code session | Push changes, then sync in web |
| Weekly maintenance | Review and sync all Projects |

### Update Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                    Documentation Update Flow                 │
└─────────────────────────────────────────────────────────────┘

1. IDENTIFY need for update (in any Claude interface)
   │
   ▼
2. CAPTURE the change needed (note, memory, or inline)
   │
   ▼
3. EDIT in Claude Code (has write access)
   │
   ├── claude "Update CLAUDE.md with the new API endpoint"
   │
   ▼
4. COMMIT & PUSH
   │
   ├── git add . && git commit -m "docs: update API endpoint" && git push
   │
   ▼
5. SYNC in Claude.ai Projects (click sync button)
   │
   ▼
6. VERIFY context is current in next chat
```

---

## Naming Conventions

### Files
- `CLAUDE.md` — AI context (always uppercase)
- `README.md` — Human-readable overview
- `SPEC.md` — Feature specifications
- `API.md` — API documentation
- Lowercase with hyphens for other docs: `error-handling.md`

### Branches
- `main` — Production/current docs
- `feature/[name]` — Doc updates tied to features
- `update/[area]` — General doc improvements

### Commits
- `docs:` prefix for all documentation changes
- `docs(project): message` for project-specific updates
- Examples:
  - `docs: add TomOS Launcher spec`
  - `docs(design): update color palette for dark mode`
  - `docs(tomos-app): document quick capture feature`

---

## Migration Plan

### Phase 1: Repository Setup
- [ ] Create `tomos-command-tower` repo on GitHub
- [ ] Set up folder structure
- [ ] Create README.md with index
- [ ] Create ARCHITECTURE.md (this document)

### Phase 2: Design System Foundation
- [ ] Create DESIGN-SYSTEM.md master file
- [ ] Document current TomOS app colors/typography
- [ ] Define component patterns

### Phase 3: Project Migration
- [ ] Migrate existing CLAUDE.md files from individual repos
- [ ] Create CLAUDE.md for projects that don't have one
- [ ] Link individual repos to central docs

### Phase 4: Claude Project Setup
- [ ] Create Claude Projects for each workstream
- [ ] Add GitHub knowledge to each
- [ ] Test sync workflow
- [ ] Document any issues/learnings

### Phase 5: Workflow Adoption
- [ ] Train yourself on the sync workflow
- [ ] Set up weekly sync reminder
- [ ] Iterate based on friction points

---

## Related Ecosystem Repos

| Repo | Purpose | Docs Location |
|------|---------|---------------|
| `braggy9/TomOS` | TomOS web API backend | `/projects/tomos-app/` |
| `braggy9/TomOS-Apps` | Native Swift apps | `/projects/tomos-app/` |
| `braggy9/tomos-launcher` | Launcher app (new) | `/projects/tomos-launcher/` |
| `braggy9/notorious-dad` | MixMaster/DJ generator | `/projects/notorious-dad/` |

**Note:** Individual repos may retain their own `CLAUDE.md` that references back to `tomos-command-tower` for shared resources. The central repo is the source of truth for shared design/architecture.

---

## FAQ

**Q: Should each app repo have its own CLAUDE.md too?**
A: Yes, but it should be lightweight and reference the central docs. Example:
```markdown
# TomOS App — Local Claude Context
For full documentation, see: github.com/braggy9/tomos-command-tower/projects/tomos-app/
This file contains local development notes only.
```

**Q: What if I need to reference docs in a chat without a Project?**
A: Use "Add from GitHub" in the chat, or paste the raw URL and ask Claude to fetch it.

**Q: How do I handle conflicting information between local and central docs?**
A: Central docs (`tomos-command-tower`) are authoritative. Update central first, then sync local references.

**Q: Can I use this with Claude Code's `--project` flag?**
A: Yes. Point Claude Code at the `tomos-command-tower` repo for documentation sessions, or at individual app repos for coding sessions (they'll reference central docs via MCP).

---

*Last updated: [Date]*
*Maintained by: Tom Bragg*
*Architecture version: 1.0*
