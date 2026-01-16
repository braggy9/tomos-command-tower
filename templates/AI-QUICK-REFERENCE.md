# TomOS Ecosystem — AI Quick Reference

**One-page guide for directing AI assistants to the right documentation**

---

## 📍 Three Documentation Hubs

| Hub | Purpose | Location |
|-----|---------|----------|
| **Command Tower** | Architecture, specs, design system | `/Users/tombragg/Desktop/tomos-command-tower/` |
| **TomOS Dashboard** | Daily dashboard, iOS Shortcuts | `/Users/tombragg/Desktop/tomos-dashboard/` |
| **TomOS API** | Backend API, implementation | `/Users/tombragg/Desktop/Projects/TomOS/` |

---

## 🎯 Task → Documentation Map

| Task Type | Go To | Key File |
|-----------|-------|----------|
| **Architecture questions** | Command Tower | `ARCHITECTURE.md` |
| **Project specs** | Command Tower | `/projects/[name]/SPEC.md` |
| **Implementation guide** | Command Tower | `/projects/[name]/CLAUDE.md` |
| **Design system** | Command Tower | `/design/DESIGN-SYSTEM.md` |
| **Current status** | Command Tower | `CURRENT_WORK.md` |
| **API/backend work** | TomOS API | `CLAUDE_CODE_HANDOVER.md` |
| **Database migration** | TomOS API | `/docs/postgres-migration/` |
| **Dashboard/Shortcuts** | TomOS Dashboard | `README.md` |

---

## 🚀 Quick Start by Interface

### Claude.ai (Web/iOS)
```
1. Settings → Connectors → GitHub
2. Add: github.com/braggy9/tomos-command-tower
3. Create Project → Add knowledge source
4. Configure files → Select relevant folders
5. Sync before starting work
```

### Claude Code (CLI)
```bash
cd /Users/tombragg/Desktop/tomos-command-tower
# Full read/write access available
```

### Other LLMs
```
Use raw GitHub URLs:
https://raw.githubusercontent.com/braggy9/tomos-command-tower/main/[path]
```

---

## 📦 Active Projects

| Project | Docs | Status |
|---------|------|--------|
| **MatterOS** | `/projects/matteros/` | Spec Complete |
| **TomOS Launcher** | `/projects/tomos-launcher/` | Spec Complete |
| **TomOS API** | `/Users/tombragg/Desktop/Projects/TomOS/` | Active |
| **TomOS Dashboard** | `/Users/tombragg/Desktop/tomos-dashboard/` | Active |

---

## 🎨 Design System (Quick Ref)

- **Primary:** Tom Blue `#007AFF`
- **Accents:** Launcher (Slate), MatterOS (Forest), Notorious DAD (Purple)
- **Typography:** SF Pro (system)
- **Spacing:** 4pt grid
- **Icons:** SF Symbols only

Full details: `/design/DESIGN-SYSTEM.md`

---

## ⚡ Essential Commands

### Check Current Work
```bash
cat /Users/tombragg/Desktop/tomos-command-tower/CURRENT_WORK.md
```

### Find Project Docs
```bash
ls /Users/tombragg/Desktop/tomos-command-tower/projects/
```

### View Handover
```bash
cat /Users/tombragg/Desktop/tomos-command-tower/templates/MASTER-HANDOVER.md
```

---

## 🔑 Key Context

- **User:** Tom Bragg (Sydney, Australia)
- **Timezone:** Australia/Sydney (UTC+11)
- **Work:** Senior Legal Counsel + Consultancy
- **Style:** ADHD-friendly, structured, minimal friction
- **GitHub:** `github.com/braggy9/tomos-command-tower`

---

## ⚠️ Critical Rules

**ALWAYS:**
- ✅ Read the project's `CLAUDE.md` before coding
- ✅ Follow the design system
- ✅ Check `CURRENT_WORK.md` for status
- ✅ Use Sydney timezone for dates

**NEVER:**
- ❌ Skip documentation
- ❌ Ignore ADHD-friendly principles
- ❌ Add features without checking specs
- ❌ Break the design system

---

## 🎯 Quick Decision Tree

```
Need to work on TomOS ecosystem?
│
├─ Architecture question?
│  └─ Command Tower → ARCHITECTURE.md
│
├─ Project specification?
│  └─ Command Tower → /projects/[name]/SPEC.md
│
├─ Implementation guide?
│  └─ Command Tower → /projects/[name]/CLAUDE.md
│
├─ API/Backend work?
│  └─ TomOS API → CLAUDE_CODE_HANDOVER.md
│
├─ Database migration?
│  └─ TomOS API → /docs/postgres-migration/
│
└─ Dashboard/Shortcuts?
   └─ TomOS Dashboard → README.md
```

---

## 📋 Pre-Work Checklist

- [ ] Check `CURRENT_WORK.md` for latest status
- [ ] Read relevant project `CLAUDE.md`
- [ ] Review design system if touching UI
- [ ] Understand user context (ADHD, Sydney timezone)
- [ ] Sync Claude.ai Project if using web interface

---

*AI Quick Reference v1.0 • January 16, 2026*
