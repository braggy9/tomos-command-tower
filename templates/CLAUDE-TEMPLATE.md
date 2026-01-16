# [Project Name] — Claude Context

**Template for creating CLAUDE.md files in new projects**

---

## Quick Reference
- **Repo:** github.com/braggy9/[repo]
- **Type:** [iOS App | macOS App | API | Web | Tool]
- **Stack:** [Swift/SwiftUI | TypeScript/Next.js | etc.]
- **Status:** [Active Development | Maintenance | Planning]
- **Command Tower:** [Link to relevant spec in tomos-command-tower]

---

## Project Purpose

[2-3 sentences describing what this project does and why it exists]

**Fits into TomOS ecosystem as:** [How it relates to TomOS, MatterOS, LegalOS, etc.]

---

## Architecture Overview

[High-level architecture, key components, data flow]

**Key Components:**
- Component 1: [description]
- Component 2: [description]
- Component 3: [description]

**Data Flow:**
```
[User Input] → [Processing] → [Storage] → [Output]
```

---

## Design System

This project follows the **TomOS Family Design System**.

**Reference:** `/design/DESIGN-SYSTEM.md` in tomos-command-tower

**Key design elements:**
- **Accent Color:** [Color hex and name]
- **Typography:** SF Pro (system font)
- **Spacing:** 4pt grid system
- **Icons:** SF Symbols

**Project-specific deviations:**
- [Any project-specific design notes]
- [Custom components or patterns]

---

## Key Files & Entry Points

**Core Files:**
- `[path]` — [description]
- `[path]` — [description]
- `[path]` — [description]

**Configuration:**
- `[config file]` — [description]

**Entry Points:**
- `[main entry]` — [where execution starts]

---

## Current State

### ✅ What's Working
- [Feature 1]
- [Feature 2]
- [Feature 3]

### 🚧 In Progress
- [Feature being built]
- [Next milestone]

### ❌ Known Issues
- [Issue 1 with workaround if known]
- [Issue 2]

### 📋 Backlog
- [Future feature 1]
- [Future feature 2]

---

## Dependencies & Integrations

### Internal Dependencies
- **TomOS:** [how it integrates]
- **MatterOS:** [how it integrates]
- **Other TomOS Family apps:** [details]

### External Services
- **Notion:** [usage details]
- **Vercel:** [deployment details]
- **Database:** [PostgreSQL/other]
- **Other services:** [details]

### Package Dependencies
```json
{
  "key-dependency": "version",
  "another-dependency": "version"
}
```

---

## Development Notes

### Prerequisites
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

### Running Locally

```bash
# Clone repo
git clone [repo-url]
cd [repo-name]

# Install dependencies
[install command]

# Set up environment
cp .env.example .env
# Edit .env with your values

# Run development server
[run command]
```

### Environment Variables

**Required:**
- `VAR_NAME` — [description and where to get it]
- `VAR_NAME` — [description]

**Optional:**
- `VAR_NAME` — [description and default]

**Where stored:**
- Development: `.env.local` (gitignored)
- Production: Vercel environment variables

### Building

```bash
# Build for production
[build command]

# Test build
[test command]
```

### Testing

```bash
# Run tests
[test command]

# Manual testing checklist:
- [ ] Test case 1
- [ ] Test case 2
- [ ] Test case 3
```

---

## AI Assistant Guidelines

### ✅ Do

**Code Style:**
- [Preferred approaches]
- [Naming conventions]
- [File organization patterns]

**Communication:**
- Explain significant architectural decisions
- Ask questions if requirements are unclear
- Provide context for complex changes

**Process:**
- Read this file first before making changes
- Test thoroughly before committing
- Update documentation as you work

### ❌ Don't

**Anti-patterns:**
- [Things that have caused issues]
- [Patterns that don't fit this project]

**Common mistakes:**
- [Mistake 1]
- [Mistake 2]

**Avoid:**
- Breaking changes without discussion
- Adding dependencies without justification
- Skipping tests

---

## Deployment

### Production Deployment

**Platform:** [Vercel/TestFlight/App Store/etc.]

**Process:**
```bash
# Deployment command
[deploy command]
```

**Post-deployment checklist:**
- [ ] Check production logs
- [ ] Verify key features working
- [ ] Monitor error rates

### Rollback Plan

If deployment fails:
1. [Step to rollback]
2. [Step to diagnose]
3. [Step to fix]

---

## Troubleshooting

### Common Issues

**Issue: [Problem description]**
- **Cause:** [Why it happens]
- **Solution:** [How to fix]

**Issue: [Problem description]**
- **Cause:** [Why it happens]
- **Solution:** [How to fix]

### Debug Checklist
- [ ] Check environment variables
- [ ] Verify dependencies installed
- [ ] Check logs for errors
- [ ] Test with clean install

---

## Related Documentation

**In Command Tower:**
- [Link to spec: `/projects/[project]/SPEC.md`]
- [Link to design system: `/design/DESIGN-SYSTEM.md`]
- [Link to architecture: `/ARCHITECTURE.md`]

**External Resources:**
- [Documentation link]
- [Tutorial link]
- [Reference link]

---

## Contact & Support

**Maintainer:** Tom Bragg

**Questions?** Ask Claude Code with this file as context!

---

*Last updated: [Date]*
*Version: [Version number]*
