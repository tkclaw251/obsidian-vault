---
title: Agent Index
created: 2026-04-29
tags: [vault, meta, agents, protocol]
---

# Agent Index — READ THIS FIRST

This file is the schema and protocol reference for all agents operating in the vault. Every agent must read this before reading any other vault file.

## Vault Overview

- **Owner:** Teddy (Tewodros)
- **Location:** `~/obsidian-vault/` (GitHub: `github.com/tkclaw251/obsidian-vault`)
- **Purpose:** Second brain for PhD prep, ML/AI research, and career navigation
- **Architecture:** Three layers — Raw Sources → Wiki/Synthesis → Schema/Output

## Directory Ownership Map

| Directory | Primary Owner | Write Rule |
|---|---|---|
| `00-meta/` | system-agent | Schema files only |
| `01-reading-log/` | research-scout | All paper ingestion |
| `02-research/ideas/active/` | research-analyst | Active research ideas |
| `02-research/ideas/rejected/` | research-analyst | Rejected ideas with reason |
| `02-research/ideas/archived/` | research-analyst | Dead ends worth revisiting |
| `02-research/papers/` | research-analyst | Submissions, arXiv links |
| `02-research/experiments/` | coding | Experiment logs and results |
| `02-research/nexus/` | planner | Weekly nexus synthesis |
| `03-academic/` | career-scout | CV, SOP, applications |
| `04-projects/` | coding | App/project specs and notes |
| `05-learning/` | knowledge-agent | Course notes |
| `06-career/` | career-scout | Opportunities, contacts |
| `07-knowledge/` | knowledge-agent | Cross-domain mappings |
| `log.md` | any agent | Append-only, one line per vault write |

## Note Format Conventions

### Required Frontmatter
Every note file MUST start with:
```yaml
---
title: Note Title
created: YYYY-MM-DD
tags: [tag1, tag2]
---
```

### Status Frontmatter (research ideas)
```yaml
status: active     # currently pursuing
status: rejected  # tried, rejected — include reason in body
status: archived  # not now, but might be useful later
```

### Date Format
Always `YYYY-MM-DD` (ISO 8601). No exceptions.

### Wiki Links
- Internal notes: `[[note-name]]`
- External links: `[label](URL)`
- Same-file headings: `[[note-name#heading]]`

### Scoring Format (papers)
When logging papers, include:
```yaml
score: X/Y
relevance: high|medium|low
notes: |
  Key claims: ...
  Weaknesses: ...
  Connection to current work: ...
```

## Ingest Protocol

When processing a new source (paper, article, note):

1. **Source → `01-reading-log/`**
   - Create `01-reading-log/YYYY-MM-DD.md` with date-stamped entry
   - For papers: create `01-reading-log/paper-notes/{short-title}.md` with deep notes
   - Frontmatter must include: title, authors, source URL, date, tags

2. **Update `log.md`**
   - Append one line: `- [agent-id] ingested {title} → {path}`

3. **Update `00-meta/index.md`**
   - Add entry to the reading log index section

4. **Git commit** (see Git Protocol below)

## Query Protocol

Before answering research questions, scan vault in this order:

1. Check `00-meta/index.md` for the full catalog
2. Scan `02-research/ideas/active/` for relevant ideas
3. Check `02-research/nexus/` for recent technique syntheses
4. Scan `01-reading-log/` for cited papers
5. Check `06-career/opportunities/` for active career leads

Use `rg` (ripgrep) for fast searching:
```bash
cd ~/obsidian-vault
rg "keyword" --md  # search all markdown files
```

## Lint Protocol (run monthly)

1. Check all date files in `01-reading-log/` have valid frontmatter
2. Check all research idea files have `status:` field
3. Verify `log.md` has entry for every vault write this month
4. Check `rg "\-\ \[ \]" 01-reading-log/` — low count = clean

Run full lint:
```bash
cd ~/obsidian-vault
echo "=== Frontmatter check ===" && rg "^---" *.md | head -20
echo "=== Status check ===" && rg "^status:" 02-research/ideas/ --files-with-matches
echo "=== Log check ===" && wc -l log.md
```

## Git Protocol

**BEFORE any write:**
```bash
cd ~/obsidian-vault
git pull origin main
```

**AFTER any write:**
```bash
cd ~/obsidian-vault
git add -A
git commit -m "<type>: <short description>"
git push origin main
```

**Commit type prefixes:** `feat:`, `fix:`, `add:`, `ingest:`, `update:`, `lint:`

**Conflict resolution:** Pull first (`git pull --rebase`), resolve manually, then push. Never force-push to main.

**Update `log.md`:** After every write, append to `log.md`:
```
- [agent-id] <type>: <description> — YYYY-MM-DD
```

## Quick Reference

- Vault index: `00-meta/index.md`
- Identity: `00-meta/identity.md`
- This file: `00-meta/agent-index.md`
- Activity log: `log.md`
