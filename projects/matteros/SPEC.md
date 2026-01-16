# MatterOS — Complete Specification

**Legal Matter Management System for Tom Bragg**

---

## 🎯 Vision

MatterOS is a comprehensive legal matter management system designed for Tom Bragg's legal practice at Publicis Groupe and The Bison consultancy. It provides a single source of truth for all legal matters, integrating seamlessly with TomOS (personal productivity) and feeding into NexusOS (the unified system).

**Core Philosophy:**
- **ADHD-friendly** — Clear structure, minimal friction, quick capture
- **Single source of truth** — All matter information in one place
- **Context-rich** — Full history, related items, intelligent linking
- **Integration-first** — Works with existing tools (Outlook, Teams, SharePoint)

---

## 📊 System Architecture

### Database (PostgreSQL + Prisma)

**Shared with TomOS:**
- Same PostgreSQL database
- Separate schemas/tables for organization
- Shared infrastructure (Prisma, Vercel)

**Why PostgreSQL:**
- Complex queries across matters and tasks
- Data integrity (foreign keys, constraints)
- Performance for large datasets
- Foundation for NexusOS cross-system queries

### Core Entities

```
Matter
├── Basic Info (title, client, type, status, priority)
├── Dates (created, due, completed, last activity)
├── Financial (budget, actual spend, billing)
├── People (client contacts, internal team, external counsel)
├── Documents (contracts, emails, memos, etc.)
├── Tasks (linked TomOS tasks)
├── Events (timeline of all activity)
├── Notes & Decisions
└── Related Matters

Client
├── Basic Info (name, industry, location)
├── Contacts (primary, legal, procurement, etc.)
├── Matters (all matters for this client)
├── Templates & Preferences
└── History

Template
├── Matter templates (common matter types)
├── Document templates (contracts, memos, etc.)
├── Workflow templates (standard processes)
└── Clause library
```

---

## 📋 Database Schema

### Matters Table

```prisma
model Matter {
  id              String   @id @default(uuid())
  
  // Basic Info
  title           String
  description     String?
  client          String   // Client name or ID (later: relation to Client table)
  matterNumber    String?  @unique // e.g., "PUB-2026-001"
  type            String   // Contract, Dispute, Compliance, Advisory, etc.
  status          String   @default("active") // active, on_hold, completed, archived
  priority        String   @default("medium") // low, medium, high, urgent
  
  // Dates
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt
  dueDate         DateTime?
  completedAt     DateTime?
  lastActivityAt  DateTime @default(now())
  
  // Financial
  budget          Decimal? @db.Decimal(12, 2)
  actualSpend     Decimal? @db.Decimal(12, 2)
  billingStatus   String?  // billable, non_billable, fixed_fee, time_and_materials
  
  // People
  clientContact   String?  // Primary client contact
  leadCounsel     String?  // Lead lawyer (internal)
  teamMembers     String[] // Array of team member names/IDs
  externalCounsel String[] // External law firms/lawyers
  
  // Organization
  practiceArea    String?  // Commercial, IP, Employment, etc.
  jurisdiction    String?  // NSW, VIC, Commonwealth, etc.
  tags            String[] // Custom tags
  
  // Relations
  tasks           Task[]   // Link to TomOS tasks
  documents       MatterDocument[]
  events          MatterEvent[]
  notes           MatterNote[]
  relatedMatters  MatterRelation[]
  
  @@map("matters")
  @@index([status])
  @@index([priority])
  @@index([client])
  @@index([type])
  @@index([dueDate])
  @@index([lastActivityAt])
}
```

### Matter Documents Table

```prisma
model MatterDocument {
  id          String   @id @default(uuid())
  matterId    String
  
  title       String
  type        String   // contract, email, memo, correspondence, court_filing, etc.
  description String?
  
  // Storage
  fileUrl     String?  // SharePoint, Google Drive, etc.
  localPath   String?  // Local file path
  
  // Metadata
  version     String?  // v1.0, v2.0, etc.
  status      String?  // draft, final, executed, superseded
  author      String?
  reviewedBy  String?
  
  // Dates
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  signedAt    DateTime?
  expiresAt   DateTime?
  
  // Relations
  matter      Matter   @relation(fields: [matterId], references: [id], onDelete: Cascade)
  
  @@map("matter_documents")
  @@index([matterId])
  @@index([type])
}
```

### Matter Events Table

```prisma
model MatterEvent {
  id          String   @id @default(uuid())
  matterId    String
  
  type        String   // status_change, document_added, task_completed, note_added, etc.
  title       String
  description String?
  
  // Metadata
  actor       String?  // Who did the action
  metadata    Json?    // Additional structured data
  
  createdAt   DateTime @default(now())
  
  // Relations
  matter      Matter   @relation(fields: [matterId], references: [id], onDelete: Cascade)
  
  @@map("matter_events")
  @@index([matterId])
  @@index([type])
  @@index([createdAt])
}
```

### Matter Notes Table

```prisma
model MatterNote {
  id          String   @id @default(uuid())
  matterId    String
  
  title       String?
  content     String   // Rich text / Markdown
  type        String   @default("general") // general, decision, analysis, research
  
  // Metadata
  author      String?
  tags        String[]
  
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  
  // Relations
  matter      Matter   @relation(fields: [matterId], references: [id], onDelete: Cascade)
  
  @@map("matter_notes")
  @@index([matterId])
  @@index([type])
}
```

### Matter Relations Table

```prisma
model MatterRelation {
  id              String   @id @default(uuid())
  sourceMatterId  String
  targetMatterId  String
  
  relationship    String   // related_to, follows, supersedes, amends, etc.
  description     String?
  
  createdAt       DateTime @default(now())
  
  // Relations
  sourceMatter    Matter   @relation("SourceMatter", fields: [sourceMatterId], references: [id], onDelete: Cascade)
  targetMatter    Matter   @relation("TargetMatter", fields: [targetMatterId], references: [id], onDelete: Cascade)
  
  @@map("matter_relations")
  @@index([sourceMatterId])
  @@index([targetMatterId])
}
```

---

## 🎨 User Interface

### Dashboard View

**Quick Stats:**
- Active matters count
- Matters due this week
- High-priority matters
- Recent activity

**Matter List:**
- Filterable by: client, type, status, priority
- Sortable by: due date, last activity, priority
- Quick actions: open, archive, create task

**Layout:**
```
┌─────────────────────────────────────────────┐
│ MatterOS Dashboard                          │
├─────────────────────────────────────────────┤
│ 📊 Quick Stats                              │
│   Active: 12  |  Due this week: 3  |  High: 5 │
├─────────────────────────────────────────────┤
│ 🔍 Filters                                  │
│   [All Clients ▼]  [All Types ▼]  [Active ▼] │
├─────────────────────────────────────────────┤
│ 📋 Matters                                  │
│                                             │
│ 🔴 [HIGH] NDA - Acme Corp                  │
│     Contract • Due: Jan 20 • Publicis       │
│                                             │
│ 🟡 [MED] Privacy Review - Beta Inc         │
│     Compliance • Due: Jan 25 • Bison        │
│                                             │
│ 🟢 [LOW] Template Update - Standard MSA    │
│     Template • No due date • Publicis       │
└─────────────────────────────────────────────┘
```

### Matter Detail View

**Header:**
- Matter title, client, type
- Status badge
- Priority indicator
- Quick actions (edit, archive, create task, add document)

**Tabs:**
1. **Overview** — Summary, key info, financial, people
2. **Tasks** — Linked TomOS tasks (filterable)
3. **Documents** — All related documents (versioned)
4. **Timeline** — Chronological event log
5. **Notes** — All notes and decisions
6. **Related** — Related matters, predecessors, successors

**Layout:**
```
┌─────────────────────────────────────────────┐
│ 🔴 [HIGH] NDA - Acme Corp              [Edit] │
│ Contract • Active • Publicis                │
├─────────────────────────────────────────────┤
│ [Overview] [Tasks] [Documents] [Timeline] [Notes] [Related] │
├─────────────────────────────────────────────┤
│ 📋 Overview                                 │
│                                             │
│ Client: Acme Corp                          │
│ Matter #: PUB-2026-001                     │
│ Due Date: Jan 20, 2026                     │
│ Lead: Tom Bragg                            │
│                                             │
│ 💼 Financial                                │
│ Budget: $5,000 | Actual: $3,200            │
│ Billing: Fixed Fee                         │
│                                             │
│ 👥 People                                   │
│ Client Contact: Jane Smith (Acme)          │
│ Team: Tom Bragg, Sarah Jones               │
│                                             │
│ 📄 Description                              │
│ Standard NDA for partnership discussion... │
└─────────────────────────────────────────────┘
```

---

## ⚡ Core Features

### 1. Quick Matter Creation

**Minimal friction to start a new matter:**

```
[+ New Matter]

Required:
- Title
- Client
- Type (dropdown: Contract, Dispute, Compliance, Advisory, Other)

Optional (can add later):
- Description
- Due date
- Priority
- Matter number
- Budget
- Team members
```

**Result:** Matter created, appears in dashboard immediately.

### 2. Task Integration (TomOS)

**Seamless linking between matters and tasks:**

- Create task from matter → Auto-links to matter
- View all matter tasks in matter detail
- Filter TomOS dashboard by matter
- Matter status updates when all tasks complete

**Example:**
```
Matter: NDA - Acme Corp
Tasks:
  [x] Review draft NDA (Completed)
  [ ] Add data breach provisions (In Progress)
  [ ] Client review (To Do)
  [ ] Execute NDA (Blocked: waiting for signatures)
```

### 3. Document Management

**Not a document editor, but a smart linker:**

- Link to SharePoint documents
- Link to Google Drive documents
- Upload files (stored in cloud storage)
- Version tracking (v1.0, v2.0, etc.)
- Status tracking (draft, final, executed)

**Smart features:**
- Auto-detect document type from filename
- Suggest related documents
- Track who reviewed
- Link documents to specific tasks

### 4. Timeline & Activity Log

**Automatic event tracking:**

- Matter created
- Status changed (active → on_hold)
- Document added/updated
- Task completed
- Note added
- Deadline approaching
- Budget exceeded

**Manual event entry:**
- Phone call logged
- Meeting summary
- Client email
- Decision recorded

### 5. Templates & Quick Actions

**Pre-built templates for common matters:**

- Standard NDA review
- Service agreement review
- Privacy compliance check
- Data breach response
- IP assignment review

**Each template includes:**
- Default tasks
- Standard documents to request
- Typical team members
- Budget estimates
- Workflow checklist

### 6. Search & Filters

**Powerful search across:**
- Matter title, description
- Client name
- Documents (by title, type)
- Notes content
- Tasks
- Tags

**Advanced filters:**
- Date range (created, due, completed)
- Client
- Type
- Status
- Priority
- Practice area
- Lead counsel
- Budget range

### 7. Reporting & Analytics

**Dashboard charts:**
- Matters by status (pie chart)
- Matters by client (bar chart)
- Matters due this month (list)
- Budget vs actual (comparison)

**Export options:**
- Matter list (CSV)
- Financial summary (Excel)
- Activity report (PDF)

---

## 🔗 Integrations

### Phase 1: Manual Entry
- Add documents via file upload
- Copy/paste document links
- Manual event logging

### Phase 2: Email Integration
- Forward emails to matter (matter-123@tomos.run)
- Auto-parse subject, body, attachments
- Link emails to matters automatically

### Phase 3: SharePoint/Drive Integration
- Browse SharePoint from MatterOS
- Link documents with one click
- Two-way sync (changes reflected)

### Phase 4: Calendar Integration
- Link Outlook/Google Calendar events to matters
- Auto-create matters from calendar invites
- Track matter-related meetings

### Phase 5: AI Assistance
- Suggest related matters
- Auto-categorize documents
- Extract key info from emails
- Summarize long documents
- Risk flagging (missing clauses, deadlines)

---

## 🚀 Development Roadmap

### MVP (Month 1)
- [ ] Database schema (Prisma)
- [ ] Matter CRUD (Create, Read, Update, Delete)
- [ ] Task linking (to TomOS)
- [ ] Basic dashboard
- [ ] Document linking (manual)
- [ ] Timeline (auto-events)
- [ ] Notes

### Phase 2 (Month 2)
- [ ] Advanced filters
- [ ] Search
- [ ] Templates
- [ ] Quick actions
- [ ] Financial tracking
- [ ] Reporting (basic charts)

### Phase 3 (Month 3)
- [ ] Email integration
- [ ] SharePoint/Drive integration
- [ ] Calendar integration
- [ ] Advanced analytics

### Phase 4 (Month 4+)
- [ ] AI assistance
- [ ] Mobile app optimization
- [ ] Client portal
- [ ] NexusOS integration (cross-system queries)

---

## 📱 Platform Strategy

### Web App (Primary)
- Next.js / React
- Responsive design
- Works on desktop + tablet + mobile

### iOS App (TomOS extension)
- New "MatterOS" tab in TomOS app
- Native iOS UI
- Optimized for quick capture
- Offline support

### API
- REST API (same pattern as TomOS)
- Shared with web + mobile
- Public API for future integrations

---

## 🎯 Success Metrics

**Adoption:**
- All active matters tracked in MatterOS (100%)
- Daily active usage (at least once per day)
- Average matters per week (10-15)

**Efficiency:**
- Time to create matter: <30 seconds
- Time to find matter: <5 seconds
- Time to add document: <10 seconds

**Data Quality:**
- All matters have due dates (100%)
- All matters have linked tasks (80%+)
- All high-priority matters reviewed daily (100%)

**Integration:**
- 50%+ of tasks linked to matters
- 80%+ of documents linked to matters
- Matter context visible in TomOS when working on tasks

---

## 💡 Key Design Principles

### 1. ADHD-Friendly
- **Quick capture** — Create matter in <30 seconds
- **Clear structure** — Predictable layout, consistent patterns
- **Minimal friction** — Defaults for everything, optional details
- **Visual cues** — Color coding, icons, status badges

### 2. Single Source of Truth
- **No duplication** — One place for each piece of info
- **Always current** — Real-time updates, no manual sync
- **Complete history** — Nothing lost, full audit trail

### 3. Context-Rich
- **Related items** — See tasks, documents, notes together
- **Timeline view** — Understand what happened when
- **Smart linking** — Automatic connections between items

### 4. Integration-First
- **Work where you are** — Integrates with Outlook, Teams, SharePoint
- **Data portability** — Export anytime, open standards
- **API-first** — All features available via API

---

## 🛠️ Technical Stack

**Backend:**
- PostgreSQL (shared with TomOS)
- Prisma ORM
- Next.js API routes
- Vercel hosting

**Frontend:**
- Next.js / React
- TypeScript
- Tailwind CSS
- shadcn/ui components

**Mobile:**
- React Native (iOS)
- Shared API with web

**Integrations:**
- Microsoft Graph API (Outlook, Teams, SharePoint)
- Google Drive API
- Zapier/Make.com (automation)

---

## 📖 Use Cases

### Use Case 1: New Contract Review

**Scenario:** Tom receives a service agreement to review for a client.

**Steps:**
1. Create matter: "Service Agreement - Client X"
2. Set due date (1 week)
3. Add document link (SharePoint)
4. Create tasks:
   - Initial review
   - Draft redlines
   - Client call
   - Finalize
5. Log events as work progresses
6. Mark complete when executed

**Result:** Full history of the matter, all tasks tracked, document versions saved.

### Use Case 2: Data Breach Response

**Scenario:** Client reports potential data breach.

**Steps:**
1. Create matter from template: "Data Breach Response - Client X"
2. Template auto-creates tasks:
   - Assess scope
   - Notify Privacy Officer
   - Draft notification
   - Regulatory filing
3. Link relevant documents (incident report, policies)
4. Log all activities in timeline
5. Track budget vs actual spend
6. Generate final report

**Result:** Structured response, nothing missed, full audit trail.

### Use Case 3: Matter Portfolio Review

**Scenario:** Tom wants to review all active matters on Friday afternoon.

**Steps:**
1. Open MatterOS dashboard
2. Filter: "Active" + "Due this month"
3. Review each matter:
   - Check progress on tasks
   - Review recent activity
   - Update notes
   - Adjust priorities
4. Create tasks for next week
5. Send updates to clients

**Result:** Clear view of all active work, proactive management.

---

**This is MatterOS. Built on the same PostgreSQL foundation as TomOS, ready to integrate into NexusOS.**

*Specification v1.0 • January 15, 2026*
