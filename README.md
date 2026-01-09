# 🏰 TomOS Command Tower

**Central knowledge hub and documentation for the TomOS ecosystem**

The Command Tower is the single source of truth for all TomOS Family projects — design system, architecture patterns, project specifications, and AI context files. Built to work seamlessly across all Claude interfaces (Code CLI, web, iOS, Desktop) and other LLMs.

---

## 📚 Quick Navigation

### Core Documentation
- **[ARCHITECTURE.md](ARCHITECTURE.md)** — System overview, repository structure, and workflows
- **[Design System](design/DESIGN-SYSTEM.md)** — Unified visual language for all TomOS apps
- **[Conventions](CONVENTIONS.md)** — Shared coding and naming conventions *(coming soon)*

### Active Projects
- **[TomOS App](projects/tomos-app/)** — Core productivity and capture app
- **[TomOS Launcher](projects/tomos-launcher/)** — Fast app launcher ([Spec](projects/tomos-launcher/SPEC.md))
- **[Notorious DAD](projects/notorious-dad/)** — AI playlist generator
- **[LegalOS](projects/legalos/)** — Legal automation tools
- **[Nexus](projects/nexus/)** — TomOS × LegalOS integration

### Resources
- **[Templates](templates/)** — Reusable templates for new projects
- **[.claude](.claude/)** — Claude-specific configuration

---

## 🎯 What's Inside

### Design System (`/design`)
Complete design language shared across all TomOS applications:

| Category | Files |
|----------|-------|
| **Foundations** | Colors, Typography, Spacing, Icons, Motion |
| **Components** | Buttons, Cards, Inputs, Lists, Navigation, Modals |
| **Patterns** | Onboarding, Empty States, Error Handling, Loading, Accessibility |

👉 **[View Design System](design/DESIGN-SYSTEM.md)**

### Project Documentation (`/projects`)
Each project has its own folder with:
- **CLAUDE.md** — AI context and development guidelines
- **README.md** — Project overview
- **SPEC.md** / **ROADMAP.md** / **API.md** — As needed

Current projects:
- `tomos-app/` — TomOS productivity suite
- `tomos-launcher/` — Quick launcher app
- `notorious-dad/` — Music playlist generator
- `legalos/` — Legal automation platform
- `nexus/` — Cross-ecosystem integration

---

## 🤖 Using with Claude

### Claude Code CLI (Recommended for Editing)
```bash
# Clone this repo
git clone https://github.com/braggy9/tomos-command-tower.git
cd tomos-command-tower

# Claude Code has full read/write access via GitHub MCP
claude

# Example commands:
# "Update the TomOS Launcher spec with the new widget feature"
# "Add a new color to the design system for error states"
```

### Claude.ai (Web/iOS)
1. Create a **Claude Project** for your workstream
2. Add GitHub Knowledge:
   - Click **"+"** → **"GitHub"**
   - Select `braggy9/tomos-command-tower`
   - Use **"Configure files"** to select relevant folders
3. Click **"Sync"** before starting new work sessions

**Project Setup Examples:**
| Project Name | Folders to Include |
|--------------|-------------------|
| TomOS App Development | `/projects/tomos-app/*`, `/design/*` |
| TomOS Launcher | `/projects/tomos-launcher/*`, `/design/*` |
| Design System | `/design/*` |

### Claude Desktop
- Configure GitHub MCP for local access
- Same workflow as Claude Code CLI

### ChatGPT / Other LLMs
Paste raw GitHub URLs to fetch documentation:
```
https://raw.githubusercontent.com/braggy9/tomos-command-tower/main/ARCHITECTURE.md
```

---

## 🔄 Workflow

### Making Documentation Updates

**Step-by-step:**
1. **Identify** — Notice doc needs updating (in any interface)
2. **Edit** — Use Claude Code (has write access)
3. **Commit** — `git add . && git commit -m "docs: update" && git push`
4. **Sync** — Click "Sync" in Claude.ai Projects
5. **Verify** — Next chat has updated context

### When to Sync
- Starting a new chat in a Claude Project
- Before major feature discussions
- After Claude Code editing sessions
- Weekly maintenance

---

## 📂 Repository Structure

```
tomos-command-tower/
├── README.md                    # This file
├── ARCHITECTURE.md              # System architecture & workflows
├── CONVENTIONS.md               # Coding standards (planned)
│
├── design/                      # 🎨 Design System
│   ├── DESIGN-SYSTEM.md        # Master design reference
│   ├── foundations/            # Colors, typography, spacing, icons, motion
│   ├── components/             # Buttons, cards, inputs, lists, navigation
│   └── patterns/               # Onboarding, errors, loading, accessibility
│
├── projects/                    # 📁 Project-Specific Docs
│   ├── tomos-app/              # TomOS productivity app
│   ├── tomos-launcher/         # App launcher
│   ├── notorious-dad/          # Playlist generator
│   ├── legalos/                # Legal tools
│   └── nexus/                  # Integration layer
│
├── templates/                   # 📝 Reusable Templates
│   ├── CLAUDE-TEMPLATE.md
│   ├── PROJECT-README-TEMPLATE.md
│   └── SPEC-TEMPLATE.md
│
└── .claude/                     # 🤖 Claude Config
    ├── project-mappings.md
    └── context-guidelines.md
```

---

## 🚀 Getting Started

### For New Projects
1. Copy template from `/templates/`
2. Create folder in `/projects/your-project/`
3. Add `CLAUDE.md` and `README.md`
4. Reference design system: `/design/DESIGN-SYSTEM.md`
5. Commit and push

### For AI Assistants
1. Read `ARCHITECTURE.md` for system overview
2. Check project-specific `CLAUDE.md` for context
3. Reference `design/DESIGN-SYSTEM.md` for UI work
4. Follow naming conventions in commits

### For Developers
1. Clone this repo alongside your project repos
2. Reference design tokens and patterns
3. Keep docs in sync as features evolve
4. Use templates for new projects

---

## 🎨 Design System Highlights

**Principles:**
- Calm Focus — minimal noise, clear hierarchy
- ADHD-Friendly — obvious actions, minimal decisions
- Native Feel — follows Apple HIG
- Consistent but Contextual — shared language, unique personality

**Key Resources:**
- [Color Palette](design/DESIGN-SYSTEM.md#colors) — Tom Blue + semantic colors
- [Typography](design/DESIGN-SYSTEM.md#typography) — SF Pro type scale
- [Components](design/DESIGN-SYSTEM.md#components) — Buttons, cards, forms
- [Patterns](design/DESIGN-SYSTEM.md#patterns) — Empty states, loading, errors

---

## 📖 Platform Access Matrix

| Platform | Method | Read | Write | Sync |
|----------|--------|------|-------|------|
| **Claude Code CLI** | GitHub MCP | ✅ | ✅ | Real-time |
| **Claude.ai (web)** | GitHub Integration | ✅ | ❌ | Manual sync |
| **Claude.ai (iOS)** | GitHub Integration | ✅ | ❌ | Manual sync |
| **Claude Desktop** | Local GitHub MCP | ✅ | ✅ | Real-time |
| **ChatGPT / Other** | Raw GitHub URLs | ✅ | ❌ | Manual fetch |

**Write workflow:** All edits → Claude Code → commit → push → other platforms sync

---

## 🏗️ Roadmap

### Phase 1: Foundation ✅
- [x] Create repository
- [x] Set up folder structure
- [x] Add ARCHITECTURE.md
- [x] Add DESIGN-SYSTEM.md
- [x] Add TomOS Launcher spec

### Phase 2: Design System (In Progress)
- [ ] Create foundation docs (colors, typography, spacing, icons, motion)
- [ ] Document component patterns
- [ ] Add accessibility guidelines
- [ ] Create Figma integration guide

### Phase 3: Project Migration
- [ ] Migrate existing CLAUDE.md files
- [ ] Create missing project docs
- [ ] Set up cross-repo links

### Phase 4: Templates & Automation
- [ ] Create project templates
- [ ] Add commit templates
- [ ] Set up automated sync reminders

---

## 🤝 Contributing

This is a personal ecosystem, but the structure and approach may be useful for others building multi-project documentation systems.

**For Tom (me):**
- Update docs as projects evolve
- Keep design system current
- Sync regularly in Claude Projects
- Use Claude Code for all edits

**Commit Convention:**
```bash
docs: general documentation updates
docs(design): design system changes
docs(project-name): project-specific updates
```

---

## 📞 Related Repositories

| Repo | Purpose | Docs |
|------|---------|------|
| [TomOS](https://github.com/braggy9/TomOS) | Web API backend | [/projects/tomos-app/](projects/tomos-app/) |
| [TomOS-Apps](https://github.com/braggy9/TomOS-Apps) | Native Swift apps | [/projects/tomos-app/](projects/tomos-app/) |
| [tomos-launcher](https://github.com/braggy9/tomos-launcher) | Launcher app | [/projects/tomos-launcher/](projects/tomos-launcher/) |
| [notorious-dad](https://github.com/braggy9/notorious-dad) | Playlist generator | [/projects/notorious-dad/](projects/notorious-dad/) |

---

## 📄 License

Private personal documentation. Not licensed for reuse.

---

## 📬 Contact

**Tom Bragg** — Creator & Maintainer
- GitHub: [@braggy9](https://github.com/braggy9)

---

<p align="center">
  <strong>🏰 Command Tower</strong><br>
  <em>Knowledge hub for the TomOS ecosystem</em><br>
  <sub>Last updated: January 2026 • v1.0</sub>
</p>
