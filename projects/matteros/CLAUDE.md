# Working with Claude Code on MatterOS

**Guide for Claude Code when building MatterOS**

---

## 🎯 Project Context

You are helping Tom Bragg build **MatterOS**, a legal matter management system. This project shares a PostgreSQL database with **TomOS** (his personal productivity system) and will eventually integrate into **NexusOS** (the unified system).

**Important context:**
- Tom is a Senior Legal Counsel at Publicis Groupe (Sydney)
- He has ADHD and values clear structure, minimal friction
- He's technical and comfortable with code
- He wants ADHD-friendly tools that actually work for him

---

## 📚 Essential Files to Read First

**Before writing any code, READ these files:**

1. **SPEC.md** — Complete technical specification
   - Database schema
   - UI mockups
   - Features
   - Use cases

2. **README.md** — Project overview and setup
   - Quick start
   - Development workflow
   - Deployment

3. **TomOS Prisma Schema** — `prisma/schema.prisma`
   - Understand existing models (Task, Project, Tag)
   - MatterOS will extend this schema

---

## 🗄️ Database Architecture

### Shared Database Strategy

**TomOS and MatterOS share the same PostgreSQL database.**

**Why:**
- Cross-system queries (e.g., "Show all tasks for Matter X")
- Single source of truth
- Foundation for NexusOS
- Performance (no cross-database joins)

**Naming conventions:**
- TomOS tables: `tasks`, `projects`, `tags`, `task_tags`
- MatterOS tables: `matters`, `matter_documents`, `matter_events`, `matter_notes`, `matter_relations`

### Adding MatterOS Tables

**When adding MatterOS schema to Prisma:**

```prisma
// Add to existing prisma/schema.prisma (don't create new file)

// ============================================
// MATTEROS
// ============================================
model Matter {
  id              String   @id @default(uuid())
  title           String
  client          String
  type            String
  status          String   @default("active")
  // ... rest of fields from SPEC.md
  
  @@map("matters")
  @@index([status])
}

// ... other MatterOS models
```

**Migration:**
```bash
npx prisma migrate dev --name add_matteros_schema
```

---

## 🏗️ Project Structure

```
tomos-api/
├── prisma/
│   ├── schema.prisma         # SHARED schema (TomOS + MatterOS)
│   └── migrations/
├── pages/
│   └── api/
│       ├── tasks.ts          # TomOS endpoints
│       ├── projects.ts
│       ├── matters.ts        # NEW: MatterOS endpoints
│       ├── matters/
│       │   └── [id].ts
│       └── matter-documents.ts
├── lib/
│   ├── prisma.ts             # SHARED Prisma client
│   └── matter-helpers.ts     # NEW: MatterOS utilities
├── types/
│   ├── task.ts               # TomOS types
│   ├── matter.ts             # NEW: MatterOS types
│   └── matter-document.ts
└── scripts/
    ├── test-db-connection.ts
    └── test-matter-api.ts    # NEW: MatterOS tests
```

---

## 🎨 API Design Patterns

### Follow TomOS Conventions

**MatterOS APIs should match TomOS patterns:**

**TomOS pattern:**
```typescript
// pages/api/tasks.ts
export default async function handler(req, res) {
  if (req.method === 'GET') {
    const tasks = await prisma.task.findMany({...})
    res.status(200).json(tasks)
  }
  if (req.method === 'POST') {
    const task = await prisma.task.create({...})
    res.status(201).json(task)
  }
}
```

**MatterOS pattern (same structure):**
```typescript
// pages/api/matters.ts
export default async function handler(req, res) {
  if (req.method === 'GET') {
    const matters = await prisma.matter.findMany({...})
    res.status(200).json(matters)
  }
  if (req.method === 'POST') {
    const matter = await prisma.matter.create({...})
    res.status(201).json(matter)
  }
}
```

### Standard Response Formats

**Success:**
```json
// GET /api/matters
[
  {
    "id": "abc-123",
    "title": "NDA - Acme Corp",
    "client": "Acme Corp",
    "type": "contract",
    "status": "active",
    "priority": "high",
    ...
  }
]

// POST /api/matters
{
  "id": "def-456",
  "title": "...",
  ...
}
```

**Error:**
```json
{
  "error": "Matter not found"
}
```

### Status Codes

- `200` — Success (GET, PATCH)
- `201` — Created (POST)
- `204` — No content (DELETE)
- `400` — Bad request (validation error)
- `404` — Not found
- `500` — Server error

---

## 🔗 TomOS Integration

### Linking Tasks to Matters

**When a task is created from a matter:**

```typescript
// pages/api/tasks.ts
if (req.method === 'POST') {
  const { title, matterId, ...rest } = req.body
  
  const task = await prisma.task.create({
    data: {
      title,
      matterId, // NEW: Link to matter
      ...rest
    }
  })
  
  // Create event in matter timeline
  if (matterId) {
    await prisma.matterEvent.create({
      data: {
        matterId,
        type: 'task_created',
        title: `Task created: ${title}`,
        actor: 'Tom Bragg',
      }
    })
  }
}
```

**When querying tasks by matter:**

```typescript
// GET /api/tasks?matterId=abc-123
const { matterId } = req.query

const tasks = await prisma.task.findMany({
  where: matterId ? { matterId } : {},
  include: {
    project: true,
    matter: true, // Include matter info
  }
})
```

### Matter Field in Task Schema

**Add to `prisma/schema.prisma`:**

```prisma
model Task {
  id          String    @id @default(uuid())
  // ... existing fields
  matterId    String?   // NEW: Link to matter
  
  // ... existing relations
  matter      Matter?   @relation(fields: [matterId], references: [id], onDelete: SetNull)
  
  @@map("tasks")
  @@index([matterId]) // NEW: Index for fast queries
}

model Matter {
  // ... matter fields
  tasks       Task[]    // Reverse relation
}
```

---

## 🚦 Development Workflow

### Step 1: Plan the Feature

**Before writing code:**
1. Read relevant section in SPEC.md
2. Understand data model
3. Identify dependencies
4. Plan API endpoints
5. Plan UI (if applicable)

### Step 2: Update Schema (if needed)

```bash
# Add models to prisma/schema.prisma
npx prisma migrate dev --name add_feature_name
npx prisma generate
```

### Step 3: Create API Endpoints

```bash
# Create files
touch pages/api/matters.ts
touch pages/api/matters/[id].ts
touch types/matter.ts
```

### Step 4: Write Tests

```typescript
// scripts/test-matter-api.ts
async function testMatterAPI() {
  // Test create
  const matter = await createMatter({...})
  
  // Test read
  const matters = await getMatters()
  
  // Test update
  await updateMatter(matter.id, {...})
  
  // Test delete
  await deleteMatter(matter.id)
  
  console.log('✅ All tests passed')
}
```

### Step 5: Manual Testing

```bash
# Start dev server
npm run dev

# Test API
curl http://localhost:3000/api/matters

# Open Prisma Studio
npx prisma studio
```

### Step 6: Commit

```bash
git add .
git commit -m "feat(matteros): add matter CRUD endpoints"
git push
```

---

## 🎯 MVP Implementation Order

**Build features in this order:**

### Week 1: Foundation
1. Add Matter schema to Prisma
2. Create Matter CRUD API
3. Create Matter types
4. Test with Prisma Studio

### Week 2: Integration
5. Add `matterId` field to Task schema
6. Update Task API to support matter linking
7. Test task-matter linking

### Week 3: Events & Timeline
8. Add MatterEvent schema
9. Auto-create events on matter changes
10. Create timeline API endpoint

### Week 4: Documents & Notes
11. Add MatterDocument schema
12. Add MatterNote schema
13. Create document/note CRUD APIs

### Week 5: Polish
14. Add filters and search
15. Optimize queries
16. Write comprehensive tests

---

## ⚠️ Common Pitfalls

### 1. Don't Create Separate Database

**WRONG:**
```prisma
// matteros/prisma/schema.prisma  ❌
datasource db {
  provider = "postgresql"
  url      = env("MATTEROS_DATABASE_URL")
}
```

**RIGHT:**
```prisma
// prisma/schema.prisma  ✓
// Single schema for both TomOS and MatterOS
```

### 2. Don't Break TomOS

**When adding MatterOS:**
- All TomOS endpoints must keep working
- Don't change existing TomOS schema without careful testing
- Test TomOS thoroughly after each MatterOS change

### 3. Don't Duplicate Code

**WRONG:**
```typescript
// lib/matter-prisma.ts  ❌
const prisma = new PrismaClient()
```

**RIGHT:**
```typescript
// Use existing lib/prisma.ts  ✓
import { prisma } from '@/lib/prisma'
```

### 4. Don't Forget Indexes

**WRONG:**
```prisma
model Matter {
  status String
  // No index ❌
}
```

**RIGHT:**
```prisma
model Matter {
  status String
  
  @@index([status])  ✓
}
```

### 5. Don't Skip Error Handling

**WRONG:**
```typescript
const matter = await prisma.matter.create({...})  // No try/catch ❌
```

**RIGHT:**
```typescript
try {
  const matter = await prisma.matter.create({...})
} catch (error) {
  console.error('Error creating matter:', error)
  res.status(500).json({ error: 'Failed to create matter' })
}
```

---

## 🧪 Testing Checklist

**Before considering a feature "done":**

- [ ] Schema migrated successfully
- [ ] API endpoints return correct data
- [ ] Error cases handled (404, 500, etc.)
- [ ] TypeScript types defined
- [ ] Integration with TomOS tested
- [ ] Manual testing in Prisma Studio
- [ ] API tests written and passing
- [ ] No breaking changes to TomOS

---

## 📖 Helpful Commands

```bash
# Database
npx prisma studio                        # Open DB GUI
npx prisma migrate dev                   # Create migration
npx prisma generate                      # Generate Prisma Client
npx prisma db push                       # Push schema (dev only, skip migration)

# Testing
npm run dev                              # Start dev server
npx ts-node scripts/test-matter-api.ts   # Run API tests

# Debugging
npx prisma validate                      # Check schema syntax
npx prisma migrate status                # Check migration status
```

---

## 🎨 UI Development (Later)

**When building the web UI:**

### Tech Stack
- Next.js / React
- TypeScript
- Tailwind CSS
- shadcn/ui components

### UI Structure
```
pages/
├── index.tsx           # TomOS dashboard
├── matteros/
│   ├── index.tsx       # MatterOS dashboard
│   └── [id].tsx        # Matter detail
```

### Component Pattern

```typescript
// components/matteros/MatterCard.tsx
interface MatterCardProps {
  matter: Matter
  onUpdate?: (matter: Matter) => void
}

export function MatterCard({ matter, onUpdate }: MatterCardProps) {
  return (
    <Card>
      <CardHeader>
        <CardTitle>{matter.title}</CardTitle>
        <Badge>{matter.status}</Badge>
      </CardHeader>
      <CardContent>
        {/* Matter details */}
      </CardContent>
    </Card>
  )
}
```

---

## 💡 Tips for Success

### 1. Read SPEC.md Thoroughly
Every question about "how should this work" is answered in SPEC.md.

### 2. Follow TomOS Patterns
MatterOS should feel like a natural extension of TomOS.

### 3. Start Simple
Build the MVP first. Fancy features can wait.

### 4. Test Incrementally
Don't build everything then test. Test each piece as you build it.

### 5. Ask Questions
If something in SPEC.md is unclear, ask Tom for clarification.

---

## 🚀 Example: Building Matter CRUD

**Full example of implementing Matter CRUD:**

### Step 1: Update Schema

```prisma
// prisma/schema.prisma

model Matter {
  id          String   @id @default(uuid())
  title       String
  client      String
  type        String
  status      String   @default("active")
  priority    String   @default("medium")
  description String?
  dueDate     DateTime?
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  
  @@map("matters")
  @@index([status])
  @@index([client])
}
```

```bash
npx prisma migrate dev --name add_matters_table
```

### Step 2: Create Types

```typescript
// types/matter.ts

export type Matter = {
  id: string
  title: string
  client: string
  type: string
  status: string
  priority: string
  description: string | null
  dueDate: string | null
  createdAt: string
  updatedAt: string
}

export type CreateMatterInput = {
  title: string
  client: string
  type: string
  status?: string
  priority?: string
  description?: string
  dueDate?: string
}

export type UpdateMatterInput = Partial<CreateMatterInput>
```

### Step 3: Create API

```typescript
// pages/api/matters.ts

import { prisma } from '@/lib/prisma'
import type { NextApiRequest, NextApiResponse } from 'next'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  if (req.method === 'GET') {
    try {
      const { status, client } = req.query
      
      const where: any = {}
      if (status) where.status = status
      if (client) where.client = client
      
      const matters = await prisma.matter.findMany({
        where,
        orderBy: [
          { status: 'asc' },
          { priority: 'desc' },
          { dueDate: 'asc' },
        ],
      })
      
      res.status(200).json(matters)
    } catch (error) {
      console.error('Error fetching matters:', error)
      res.status(500).json({ error: 'Failed to fetch matters' })
    }
  }
  
  if (req.method === 'POST') {
    try {
      const { title, client, type, ...rest } = req.body
      
      if (!title || !client || !type) {
        return res.status(400).json({ 
          error: 'Title, client, and type are required' 
        })
      }
      
      const matter = await prisma.matter.create({
        data: {
          title,
          client,
          type,
          ...rest,
        }
      })
      
      res.status(201).json(matter)
    } catch (error) {
      console.error('Error creating matter:', error)
      res.status(500).json({ error: 'Failed to create matter' })
    }
  }
}
```

```typescript
// pages/api/matters/[id].ts

import { prisma } from '@/lib/prisma'
import type { NextApiRequest, NextApiResponse } from 'next'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const { id } = req.query
  
  if (typeof id !== 'string') {
    return res.status(400).json({ error: 'Invalid matter ID' })
  }
  
  if (req.method === 'GET') {
    try {
      const matter = await prisma.matter.findUnique({
        where: { id }
      })
      
      if (!matter) {
        return res.status(404).json({ error: 'Matter not found' })
      }
      
      res.status(200).json(matter)
    } catch (error) {
      console.error('Error fetching matter:', error)
      res.status(500).json({ error: 'Failed to fetch matter' })
    }
  }
  
  if (req.method === 'PATCH') {
    try {
      const updateData = req.body
      
      const matter = await prisma.matter.update({
        where: { id },
        data: updateData,
      })
      
      res.status(200).json(matter)
    } catch (error) {
      console.error('Error updating matter:', error)
      res.status(500).json({ error: 'Failed to update matter' })
    }
  }
  
  if (req.method === 'DELETE') {
    try {
      await prisma.matter.delete({
        where: { id }
      })
      
      res.status(204).end()
    } catch (error) {
      console.error('Error deleting matter:', error)
      res.status(500).json({ error: 'Failed to delete matter' })
    }
  }
}
```

### Step 4: Test

```typescript
// scripts/test-matter-api.ts

const API_URL = 'http://localhost:3000/api'

async function testMatterAPI() {
  console.log('🧪 Testing Matter API...\n')
  
  try {
    // Test POST
    console.log('Test 1: POST /api/matters')
    const createRes = await fetch(`${API_URL}/matters`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        title: 'NDA - Test Corp',
        client: 'Test Corp',
        type: 'contract',
        priority: 'high',
      })
    })
    const matter = await createRes.json()
    console.log('✓ Created matter:', matter.id)
    
    // Test GET (list)
    console.log('\nTest 2: GET /api/matters')
    const listRes = await fetch(`${API_URL}/matters`)
    const matters = await listRes.json()
    console.log('✓ Fetched', matters.length, 'matters')
    
    // Test GET (single)
    console.log('\nTest 3: GET /api/matters/[id]')
    const getRes = await fetch(`${API_URL}/matters/${matter.id}`)
    const fetchedMatter = await getRes.json()
    console.log('✓ Fetched matter:', fetchedMatter.title)
    
    // Test PATCH
    console.log('\nTest 4: PATCH /api/matters/[id]')
    const updateRes = await fetch(`${API_URL}/matters/${matter.id}`, {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ status: 'completed' })
    })
    const updatedMatter = await updateRes.json()
    console.log('✓ Updated status:', updatedMatter.status)
    
    // Test DELETE
    console.log('\nTest 5: DELETE /api/matters/[id]')
    await fetch(`${API_URL}/matters/${matter.id}`, { method: 'DELETE' })
    console.log('✓ Deleted matter')
    
    console.log('\n✅ ALL TESTS PASSED\n')
  } catch (error) {
    console.error('❌ Error:', error)
    process.exit(1)
  }
}

testMatterAPI()
```

---

**You now have everything you need to build MatterOS. Start with the MVP and iterate from there!**

*Claude Code Guide v1.0 • January 15, 2026*
