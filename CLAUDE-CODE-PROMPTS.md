# Claude Code Prompts — TomOS PostgreSQL Migration

**Copy these prompts directly into Claude Code for each phase**

---

## 🚀 Phase 0: Pre-Flight Setup

### ⚡ Before Starting Claude Code

**You need to do this manually first:**

1. **Set up Vercel Postgres:**
   ```bash
   # Go to vercel.com/dashboard
   # Navigate to Storage → Create Database → Postgres
   # Copy both connection strings:
   # - POSTGRES_URL (with pooling)
   # - POSTGRES_URL_NON_POOLING (direct connection)
   ```

2. **Add to your TomOS API `.env.local`:**
   ```bash
   # Copy this template:
   DATABASE_URL="your-postgres-url-with-pooling"
   DIRECT_URL="your-postgres-url-non-pooling"
   ```

3. **Save connection strings somewhere safe** (you'll need them for deployment)

**Time:** 15-30 minutes

---

## 📋 Phase 1: Database Setup (Session 1)

### 🎯 Claude Code Prompt

```
I'm starting TomOS PostgreSQL migration - Session 1: Database Setup

Context files to read:
- /Users/tombragg/Desktop/Projects/TomOS/docs/postgres-migration/FULL-CONVERSATION.md
- /Users/tombragg/Desktop/Projects/TomOS/docs/postgres-migration/SESSION-1.md
- /Users/tombragg/Desktop/Projects/TomOS/CLAUDE.md

My setup:
- Database: Vercel Postgres (already created)
- DATABASE_URL: [I have this in .env.local]
- DIRECT_URL: [I have this in .env.local]
- TomOS API repo: /Users/tombragg/Desktop/Projects/TomOS/
- Current timezone: Australia/Sydney

Task:
Follow SESSION-1.md exactly to:
1. Install Prisma and dependencies
2. Create prisma/schema.prisma with Task, Project, Tag models
3. Configure database connection
4. Create initial migration
5. Test connection
6. Commit changes

Please start with Phase 1: Install Prisma and Dependencies.
```

### ✅ Success Criteria
- Prisma installed
- Schema defined with all models
- Migration created and applied
- Test connection script works
- Changes committed to git

**Expected time:** 2-3 hours

---

## 🔌 Phase 2: API Migration (Session 2)

### 🎯 Claude Code Prompt

```
I'm starting TomOS PostgreSQL migration - Session 2: API Migration

Context files to read:
- /Users/tombragg/Desktop/Projects/TomOS/docs/postgres-migration/FULL-CONVERSATION.md
- /Users/tombragg/Desktop/Projects/TomOS/docs/postgres-migration/SESSION-2.md
- /Users/tombragg/Desktop/Projects/TomOS/CLAUDE.md

Status from Session 1:
- ✅ Prisma installed
- ✅ Schema created and migrated
- ✅ Database connection tested
- ✅ Changes committed

Task:
Follow SESSION-2.md exactly to:
1. Create Prisma Client singleton (lib/prisma.ts)
2. Define TypeScript types
3. Migrate task API endpoints to use Prisma
4. Migrate project API endpoints to use Prisma
5. Comment out (don't delete) old Notion code
6. Test all endpoints
7. Commit changes

Please start with Phase 1: Create Prisma Client Singleton.
```

### ✅ Success Criteria
- Prisma Client created
- All endpoints migrated to Postgres
- Type safety maintained
- Tests passing
- Old Notion code preserved (commented)
- Changes committed to git

**Expected time:** 2-3 hours

---

## 📦 Phase 3: Data Migration (Session 3)

### 🎯 Claude Code Prompt

```
I'm starting TomOS PostgreSQL migration - Session 3: Data Migration

Context files to read:
- /Users/tombragg/Desktop/Projects/TomOS/docs/postgres-migration/FULL-CONVERSATION.md
- /Users/tombragg/Desktop/Projects/TomOS/docs/postgres-migration/SESSION-3.md
- /Users/tombragg/Desktop/Projects/TomOS/CLAUDE.md

Status from Session 2:
- ✅ Prisma Client created
- ✅ All API endpoints migrated to Postgres
- ✅ Tests passing
- ✅ Changes committed

Task:
Follow SESSION-3.md exactly to:
1. Create export script to download all Notion data
2. Export data to JSON file
3. Create import script with ID mapping
4. Import all data to Postgres
5. Verify data integrity (counts match)
6. Save ID mappings
7. Create database backup
8. Start 24-48 hour parallel testing period
9. Commit changes

⚠️ IMPORTANT: After this session, WAIT 24-48 hours before Session 4.

Please start with Phase 1: Create Export Script.
```

### ✅ Success Criteria
- All data exported from Notion
- All data imported to Postgres
- Counts match (no data loss)
- Relationships preserved
- ID mappings saved
- Backup created
- Both systems running in parallel
- Changes committed

**Expected time:** 1-2 hours

**⚠️ STOP HERE - Test for 24-48 hours**

---

## 🧪 Phase 3.5: Parallel Testing (24-48 hours)

### 📱 Manual Testing Checklist

**DO NOT use Claude Code for this phase. This is manual testing.**

**Test with iOS app:**
- [ ] Dashboard loads quickly (should be much faster!)
- [ ] Tasks appear correctly
- [ ] Can create new tasks
- [ ] Can update tasks
- [ ] Can delete tasks
- [ ] Can create projects
- [ ] Task-project relationships work
- [ ] Tags work correctly

**Test API directly:**
```bash
# Get all tasks
curl https://tomos-task-api.vercel.app/api/tasks

# Create a test task
curl -X POST https://tomos-task-api.vercel.app/api/task \
  -H "Content-Type: application/json" \
  -d '{"task":"Test PostgreSQL task"}'

# Verify it appears in both Notion and Postgres
```

**Compare data:**
- [ ] Task counts match between Notion and Postgres
- [ ] All fields populated correctly
- [ ] No missing data
- [ ] No errors in logs

**If any issues found:**
- Stay on Notion
- Debug the issue
- Fix before proceeding to Session 4

**If everything works perfectly:**
- Wait full 24-48 hours
- Continue to Session 4

---

## 🚀 Phase 4: Cutover (Session 4)

### 🎯 Claude Code Prompt

**⚠️ ONLY use this prompt after successful 24-48 hour parallel testing**

```
I'm starting TomOS PostgreSQL migration - Session 4: Cutover

Context files to read:
- /Users/tombragg/Desktop/Projects/TomOS/docs/postgres-migration/FULL-CONVERSATION.md
- /Users/tombragg/Desktop/Projects/TomOS/docs/postgres-migration/SESSION-4.md
- /Users/tombragg/Desktop/Projects/TomOS/CLAUDE.md

Status from Session 3 + Parallel Testing:
- ✅ All data migrated successfully
- ✅ 24-48 hour parallel testing completed
- ✅ iOS app works perfectly
- ✅ No data loss or corruption
- ✅ Performance significantly improved

Task:
Follow SESSION-4.md exactly to:
1. Verify parallel testing results one final time
2. Remove all Notion dependencies from package.json
3. Delete Notion API code (safely preserved in git history)
4. Remove NOTION_API_KEY from environment variables
5. Optimize Prisma configuration
6. Deploy to Vercel production
7. Test production deployment
8. Set up monitoring
9. Commit final changes

This is the point of no return - Notion will be completely removed.

Please start with Phase 1: Verify Parallel Testing Results.
```

### ✅ Success Criteria
- Notion completely removed from codebase
- Production deployment successful
- All endpoints working in production
- Monitoring configured
- Performance targets met
- No errors in production
- Changes committed

**Expected time:** 1 hour

---

## 🎉 Phase 5: Post-Migration Monitoring

### 📊 First Week Checklist

**Monitor daily:**
```bash
# Check Vercel logs
vercel logs --prod --follow

# Check database usage
# Go to Vercel dashboard → Storage → Postgres → Metrics

# Test iOS app
# Use it normally and watch for any issues
```

**Performance metrics to track:**
- [ ] Dashboard load time (should be < 200ms)
- [ ] Task creation time (should be < 100ms)
- [ ] API response times
- [ ] Error rates (should be zero)
- [ ] Database query performance

**If issues found:**
- Check Vercel logs for errors
- Review Prisma queries for slow operations
- Ask Claude Code for optimization help

---

## 🚨 Emergency Rollback

### If Something Goes Wrong

**Before Session 4 (Still have Notion):**
```
Just stop using Postgres and continue with Notion.
All data is safe in Notion.
Debug the issue before continuing.
```

**After Session 4 (Notion removed):**
```
⚠️ This is why parallel testing is critical!

Rollback steps:
1. Restore from Notion export JSON
2. Restore from Postgres backup
3. Revert git commits
4. Redeploy previous version
5. Contact me immediately for help
```

**Prevention:**
- ✅ Always wait full 24-48h for parallel testing
- ✅ Keep Notion workspace intact for 30 days
- ✅ Keep JSON exports for 30+ days
- ✅ Test thoroughly before Session 4

---

## 📞 Getting Help During Migration

### When Stuck

**Try in this order:**
1. Re-read the SESSION guide for your current phase
2. Check QUICK-REF.md for common issues
3. Review FULL-CONVERSATION.md for context
4. Search Prisma docs: https://prisma.io/docs
5. Ask Claude Code: "I'm stuck on [specific issue], here's the error: [paste error]"

### Common Issues

**"Database connection failed"**
→ Check DATABASE_URL in .env.local

**"Prisma schema errors"**
→ Run `npx prisma validate`

**"Migration failed"**
→ Run `npx prisma migrate reset` (dev only!)

**"Data counts don't match"**
→ Check export script filters, verify all data exported

---

## ✨ Tips for Success

### Working with Claude Code

**Best practices:**
1. **Start each session fresh** with the full prompt above
2. **Reference all context files** in your initial prompt
3. **Test after each phase** before moving to next
4. **Commit frequently** so you can rollback if needed
5. **Ask questions** if anything is unclear

**Don't:**
- ❌ Skip reading context files
- ❌ Rush through phases
- ❌ Skip testing
- ❌ Delete Notion data immediately

### ADHD-Friendly Approach

**Session structure:**
- 🔵 Phase 1: 2-3 hours (can split into 2 sessions)
- 🟢 Phase 2: 2-3 hours (can split into 2 sessions)
- 🟡 Phase 3: 1-2 hours (single session)
- ⚪ Phase 3.5: 24-48 hours (passive testing)
- 🟣 Phase 4: 1 hour (single session)

**Take breaks:**
- After each phase completion
- When tired or losing focus
- After hitting any blockers

**Progress tracking:**
Update `/tomos-command-tower/CURRENT_WORK.md` after each session!

---

## 🎯 Next Steps After Migration

### Once Migration Complete

**Immediate (Week 1):**
- Monitor performance and errors
- Fix any issues discovered
- Update documentation
- Celebrate! 🎉

**Short-term (Weeks 2-4):**
- Fix iOS app bug (tasks view)
- Optimize any slow queries
- Add database indexes if needed

**Medium-term (Q1 2026):**
- Start MatterOS development
- Or build TomOS Launcher
- Or add new TomOS features

**All specs ready in:**
`/Users/tombragg/Desktop/tomos-command-tower/projects/`

---

*Claude Code Prompts v1.0*
*Created: January 16, 2026*
*Ready to copy and paste into Claude Code*
*Location: `/tomos-command-tower/CLAUDE-CODE-PROMPTS.md`*
