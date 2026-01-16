# MatterOS

**Legal matter management system for Tom's legal practice**

---

## Overview

MatterOS is a comprehensive legal matter management system designed for Tom Bragg's legal work at Publicis Groupe and The Bison consultancy. It provides matter-centric organization, deadline tracking, document management, and client relationship tools.

---

## Key Features

### Matter Management
- Matter cards with client, type, deadlines
- Status tracking (Active, Pending, On Hold, Closed)
- Priority levels (Critical, High, Medium, Low)
- Matter types (Contract Review, Negotiation, Dispute, Advisory, etc.)

### Deadline Tracking
- Court dates, filing deadlines, meeting dates
- Automated reminders (7 days, 3 days, 1 day, same day)
- Calendar integration
- Visual deadline indicators

### Document Management
- Document repository per matter
- Version control
- Document types (Contract, Correspondence, Internal Memo, etc.)
- Quick access from matter cards

### Time Tracking
- Time entries linked to matters
- Billable vs non-billable
- Activity types (Research, Drafting, Meeting, etc.)
- Reporting and invoicing prep

### Client Management
- Client profiles with contact info
- Matter history per client
- Communication log
- Relationship tracking

---

## Architecture

**Built on TomOS foundation:**
- Uses same PostgreSQL database
- Shares core data models (Tasks, Projects)
- Extends with legal-specific models (Matters, Clients, Documents)

**Data Models:**
- Matter (extends Project)
- Client (extends Person/Contact)
- Document (linked to Matter)
- TimeEntry (linked to Matter)
- Deadline (linked to Matter)

---

## Integration Points

### With TomOS
- Matters appear as Projects in TomOS
- Tasks can be linked to Matters
- Shared calendar and reminders
- Unified search

### With External Tools
- Outlook/Gmail (email integration)
- Calendar (deadline sync)
- Document storage (OneDrive, Google Drive)
- Notion (initial data source)

---

## Implementation Plan

### Phase 1: Data Models
- Define Prisma schema extensions
- Create migrations
- Set up relationships

### Phase 2: API Layer
- CRUD endpoints for Matters
- Client management API
- Document handling
- Time tracking

### Phase 3: UI Components
- Matter dashboard
- Matter detail view
- Client list
- Deadline calendar

### Phase 4: Advanced Features
- Document versioning
- Automated reminders
- Reporting and analytics
- Template system

---

## File Structure

```
matteros/
├── SPEC.md           # Full technical specification
├── CLAUDE.md         # Claude Code instructions
├── README.md         # This file
├── prisma/
│   └── schema.prisma # Database schema extensions
├── src/
│   ├── api/          # API endpoints
│   ├── types/        # TypeScript types
│   ├── lib/          # Utilities
│   └── components/   # UI components (future)
└── scripts/
    └── migrate-notion-matters.ts
```

---

## Getting Started

### Prerequisites
- TomOS installed and running
- PostgreSQL database set up
- Notion data exported (for initial migration)

### Setup

1. **Read the full spec:**
   ```bash
   open SPEC.md
   ```

2. **Review Claude Code instructions:**
   ```bash
   open CLAUDE.md
   ```

3. **Run with Claude Code:**
   ```bash
   # Open CLAUDE.md in Claude Code
   # Follow the session-by-session implementation guide
   ```

---

## Usage Examples

### Create a Matter
```typescript
const matter = await prisma.matter.create({
  data: {
    title: "Contract Review - Acme Corp MSA",
    clientId: "client_123",
    matterType: "CONTRACT_REVIEW",
    status: "ACTIVE",
    priority: "HIGH",
    openDate: new Date(),
    deadlines: {
      create: [
        {
          title: "First draft due",
          dueDate: new Date("2026-01-30"),
          type: "INTERNAL"
        }
      ]
    }
  }
});
```

### Track Time
```typescript
const timeEntry = await prisma.timeEntry.create({
  data: {
    matterId: matter.id,
    date: new Date(),
    hours: 2.5,
    billable: true,
    activity: "CONTRACT_REVIEW",
    description: "Reviewed MSA terms and conditions"
  }
});
```

### Search Matters
```typescript
const activeMatters = await prisma.matter.findMany({
  where: {
    status: "ACTIVE",
    priority: { in: ["HIGH", "CRITICAL"] }
  },
  include: {
    client: true,
    deadlines: {
      where: {
        dueDate: { gte: new Date() }
      },
      orderBy: { dueDate: "asc" }
    }
  }
});
```

---

## Status

**Current:** Planning phase
**Next:** Database schema design (see SPEC.md)
**Target:** Q1 2026 implementation

---

## Documentation

- **SPEC.md** — Full technical specification with data models, API design, and UI mockups
- **CLAUDE.md** — Step-by-step implementation guide for Claude Code
- **TomOS docs** — See parent project for core architecture

---

## Support

Built by Tom Bragg for Tom Bragg 😄

Questions? Check the SPEC.md or ask Claude Code!

---

*README v1.0 • January 15, 2026*
