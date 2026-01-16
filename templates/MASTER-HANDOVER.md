# TomOS Ecosystem — Master Handover Document

**For Claude AI Chats, Claude Code, and Other AI Assistants**

---

## 🎯 Quick Context

You're working on the **TomOS Ecosystem** — a suite of ADHD-friendly productivity and legal automation tools built by Tom Bragg.

**Key Principle:** Single source of truth, minimal context switching, calm focus.

---

## 📍 Where to Find Documentation

### **Command Tower (Architecture & Specs)**
**📁 Location:** `/Users/tombragg/Desktop/tomos-command-tower/`
**🌐 GitHub:** `github.com/braggy9/tomos-command-tower` (Public)

**This is your PRIMARY reference for:**
- Architecture and design decisions
- Project specifications
- Design system (colors, typography, components)
- Cross-project documentation

**Key Files:**
- `README.md` — Navigation index
- `ARCHITECTURE.md` — Documentation architecture
- `CURRENT_WORK.md` — Living status document
- `design/DESIGN-SYSTEM.md` — Complete design system
- `projects/[project]/SPEC.md` — Project specifications
- `projects/[project]/CLAUDE.md` — Implementation guides

---

### **TomOS Dashboard (Productivity System)**
**📁 Location:** `/Users/tombragg/Desktop/tomos-dashboard/`

**Purpose:** ADHD-friendly daily dashboard generator
**Features:**
- Google Calendar integration
- iOS Shortcuts (5 shortcuts)
- Task prioritization
- Notion sync
- Weather integration

**Key Files:**
- `README.md` — System overview
- `TAG_GUIDELINES.md` — Tagging system
- `ios-shortcuts/INSTALLATION-GUIDE.md` — iOS Shortcuts setup

---

### **TomOS API (Backend)**
**📁 Location:** `/Users/tombragg/Desktop/Projects/TomOS/`
**🌐 GitHub:** `github.com/braggy9/TomOS` (Public)
**🌐 Production:** `https://tomos-task-api.vercel.app`

**Purpose:** Next.js 14 serverless backend
**Features:**
- Task management with Notion
- APNs push notifications (iOS/macOS)
- NLP task parsing (Claude AI)
- Google Calendar sync
- Scheduled notifications

**Key Files:**
- `CLAUDE_CODE_HANDOVER.md` — Primary handover for API work
- `CLAUDE.md` — Comprehensive API documentation
- `docs/postgres-migration/` — Notion → PostgreSQL migration guides

---

## 🗂️ Project Map

### **Active Projects**

| Project | Purpose | Docs Location | Status |
|---------|---------|---------------|--------|
| **TomOS App** | Core productivity & capture app (iOS) | `/projects/tomos-app/` | Active |
| **TomOS Launcher** | Fast app launcher (macOS) | `/projects/tomos-launcher/SPEC.md` | Spec Complete |
| **MatterOS** | Legal matter management | `/projects/matteros/` | Spec Complete |
| **Notorious DAD** | AI playlist generator | `/projects/notorious-dad/` | Planning |
| **LegalOS** | Legal automation tools | `/projects/legalos/` | Planning |
| **Nexus** | TomOS × LegalOS integration | `/projects/nexus/` | Planning |

### **Supporting Systems**

| System | Purpose | Location |
|--------|---------|----------|
| **TomOS Dashboard** | Daily dashboard generator | `/Users/tombragg/Desktop/tomos-dashboard/` |
| **TomOS API** | Backend API (Vercel) | `/Users/tombragg/Desktop/Projects/TomOS/` |
| **TomOS-Apps** | iOS/macOS Swift apps | `/Users/tombragg/Desktop/TomOS-Apps/` |

---

## 🎨 Design System

All projects follow the **TomOS Family Design System**.

**Core Principles:**
- Calm Focus — No unnecessary noise
- ADHD-Friendly — Minimal decisions, clear hierarchy
- Native Feel — Respect platform conventions
- Consistent but Contextual — Unified language, context-specific accents

**Key Design Elements:**
- **Primary Color:** Tom Blue `#007AFF`
- **Typography:** SF Pro (system font)
- **Spacing:** 4pt grid system
- **Icons:** SF Symbols exclusively

**Per-App Accents:**
- Launcher: Slate `#64748B`
- Notorious DAD: Purple `#AF52DE`
- LegalOS/MatterOS: Forest `#30D158`

**Full Reference:** `/design/DESIGN-SYSTEM.md` in Command Tower

---

## 🔄 How to Use This Documentation

### **For Architecture Questions**
👉 **Go to Command Tower** → `ARCHITECTURE.md`

### **For Project Implementation**
👉 **Go to Command Tower** → `/projects/[project]/CLAUDE.md`

### **For API/Backend Work**
👉 **Go to TomOS API** → `CLAUDE_CODE_HANDOVER.md`

### **For Dashboard/iOS Shortcuts**
👉 **Go to TomOS Dashboard** → `README.md`

---

## 🚀 Getting Started Checklist

**In Claude.ai (Web/iOS):**
1. Go to Settings → Connectors → GitHub
2. Connect repository: `braggy9/tomos-command-tower`
3. Create Project for your workstream
4. Add repo as knowledge source
5. Click "Configure files" → Select relevant `/projects/` folders
6. Always **Sync** before starting new work

**In Claude Code (CLI):**
```bash
cd /Users/tombragg/Desktop/tomos-command-tower
# Full read/write access via MCP
```

**In Claude Desktop:**
- Configure GitHub MCP connector
- Point to local Command Tower directory

**For Other LLMs (ChatGPT, etc.):**
```
Raw GitHub URLs:
https://raw.githubusercontent.com/braggy9/tomos-command-tower/main/README.md
https://raw.githubusercontent.com/braggy9/tomos-command-tower/main/ARCHITECTURE.md
```

---

## 💡 Key Decisions & Context

### **Documentation Philosophy**
- **Command Tower** = Architecture (what we build)
- **Project repos** = Implementation (how we build)
- Write in Claude Code → Commit → Push → Sync in Claude.ai

### **TomOS Ecosystem Structure**
- **TomOS** = Personal productivity, tasks, capture
- **LegalOS** = Legal tools (contract analysis, clauses)
- **MatterOS** = Legal matter management (work tracking)
- **NexusOS** = Bridge between personal and professional contexts

### **Backend Migration (In Progress)**
- Migrating from Notion API → PostgreSQL
- 20-60x performance improvement
- Foundation for MatterOS, LegalOS, NexusOS
- Docs: `/Users/tombragg/Desktop/Projects/TomOS/docs/postgres-migration/`

### **User Context**
- **User:** Tom Bragg
- **Location:** Sydney, Australia (AEDT, UTC+11)
- **Timezone:** All dates/times use Australia/Sydney
- **Work:** Senior Legal Counsel at Publicis Groupe
- **ADHD:** Needs structured, systematic approaches with minimal friction
- **Technical:** Experienced with iOS dev, APIs, automation

---

## 📋 Common Tasks & Where to Go

| Task | Go To |
|------|-------|
| Understanding overall architecture | Command Tower → `ARCHITECTURE.md` |
| Finding project specifications | Command Tower → `/projects/[project]/SPEC.md` |
| Implementing a feature in MatterOS | Command Tower → `/projects/matteros/CLAUDE.md` |
| Working on TomOS backend API | TomOS API → `CLAUDE_CODE_HANDOVER.md` |
| Setting up iOS Shortcuts | TomOS Dashboard → `ios-shortcuts/INSTALLATION-GUIDE.md` |
| Understanding design system | Command Tower → `/design/DESIGN-SYSTEM.md` |
| Checking current work status | Command Tower → `CURRENT_WORK.md` |
| Creating a new project | Command Tower → `/templates/CLAUDE-TEMPLATE.md` |
| PostgreSQL migration | TomOS API → `/docs/postgres-migration/MASTER.md` |

---

## 🎯 Project-Specific Quick References

### **MatterOS (Legal Matter Management)**
**Purpose:** Track legal matters, deadlines, tasks, documents
**Docs:** `/projects/matteros/` in Command Tower
**Tech Stack:** SwiftUI, SwiftData, TypeScript, Vercel, PostgreSQL
**Integration:** Shares database with TomOS, tasks flow to TomOS

**Quick Start:**
1. Read `/projects/matteros/SPEC.md` — Full specification
2. Read `/projects/matteros/CLAUDE.md` — Implementation guide (803 lines!)
3. Read `/projects/matteros/README.md` — Project overview

---

### **TomOS Launcher (App Launcher)**
**Purpose:** Sub-second app switching for macOS
**Docs:** `/projects/tomos-launcher/SPEC.md` in Command Tower
**Tech Stack:** SwiftUI, AppKit, WidgetKit
**Integration:** URL schemes to launch apps

**Quick Start:**
1. Read `/projects/tomos-launcher/SPEC.md` — Complete specification

---

### **TomOS Backend (API)**
**Purpose:** Task management backend with push notifications
**Docs:** `/Users/tombragg/Desktop/Projects/TomOS/CLAUDE_CODE_HANDOVER.md`
**Tech Stack:** Next.js 14, Vercel, Notion API (migrating to PostgreSQL)
**Production:** `https://tomos-task-api.vercel.app`

**Quick Start:**
1. Read `CLAUDE_CODE_HANDOVER.md` — Primary handover document
2. Check `CLAUDE.md` — Comprehensive API docs
3. If working on migration: Read `/docs/postgres-migration/QUICK-REF.md`

---

### **TomOS Dashboard**
**Purpose:** ADHD-friendly daily dashboard with task prioritization
**Docs:** `/Users/tombragg/Desktop/tomos-dashboard/README.md`
**Tech Stack:** iOS Shortcuts, Google Calendar, Notion, Make.com

**Quick Start:**
1. Read `README.md` — Complete system overview
2. For iOS Shortcuts: Read `ios-shortcuts/INSTALLATION-GUIDE.md`

---

## ⚠️ Important Notes

### **DO:**
- ✅ Always check `CURRENT_WORK.md` for latest status
- ✅ Follow the design system (`/design/DESIGN-SYSTEM.md`)
- ✅ Read project-specific `CLAUDE.md` before coding
- ✅ Use ADHD-friendly patterns (clear structure, minimal decisions)
- ✅ Sync Claude.ai Projects before starting work
- ✅ Commit to Command Tower from Claude Code

### **DON'T:**
- ❌ Skip reading the relevant CLAUDE.md file
- ❌ Ignore the design system
- ❌ Make changes without understanding the architecture
- ❌ Add features without checking specifications
- ❌ Forget that user is in Sydney timezone (UTC+11)

---

## 🔗 Cross-Project Relationships

```
┌─────────────────────────────────────────────────────────┐
│                  TomOS Command Tower                     │
│              (Architecture & Specs)                      │
└────────────┬────────────────────────────────────────────┘
             │
    ┌────────┴────────┬─────────────┬──────────────┐
    │                 │             │              │
    ▼                 ▼             ▼              ▼
┌─────────┐     ┌───────────┐  ┌─────────┐  ┌──────────┐
│  TomOS  │────▶│ MatterOS  │  │LegalOS  │  │ Launcher │
│  (Core) │     │  (Work)   │  │(Tools)  │  │ (macOS)  │
└────┬────┘     └─────┬─────┘  └────┬────┘  └──────────┘
     │                │             │
     └────────┬───────┴─────────────┘
              │
              ▼
        ┌──────────┐
        │  Nexus   │
        │ (Bridge) │
        └──────────┘
```

**Shared Infrastructure:**
- PostgreSQL database (post-migration)
- Vercel hosting
- APNs push notifications
- Claude AI (NLP, parsing, analysis)

---

## 📞 Need Help?

**Check in this order:**
1. `CURRENT_WORK.md` → What's the latest status?
2. Project `CLAUDE.md` → Implementation guide
3. Project `SPEC.md` → Full specification
4. `ARCHITECTURE.md` → System architecture
5. Design system → Visual language

**Still stuck?**
- Ask Claude Code with relevant CLAUDE.md as context
- Check GitHub issues in Command Tower repo
- Review session notes in `/Staging/` folder

---

## 🎉 Ready to Build!

You now have everything you need to work on any part of the TomOS ecosystem.

**Remember:**
- Command Tower = Your map
- Project CLAUDE.md = Your guide
- Design System = Your visual language
- CURRENT_WORK.md = Your status board

**Start here:**
1. Read `CURRENT_WORK.md` to understand what's active
2. Navigate to the relevant project docs
3. Read the `CLAUDE.md` for that project
4. Build with confidence!

---

*Master Handover v1.0*
*Created: January 16, 2026*
*Maintained by: Tom Bragg + Claude Code*
*Location: `/templates/MASTER-HANDOVER.md` in Command Tower*
