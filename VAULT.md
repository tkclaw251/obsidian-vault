# Obsidian Research Vault

> Teddy's personal knowledge management system for PhD prep, research, and career navigation.

## Purpose

This vault is the second brain for a CS researcher splitting time between side projects and ML/AI research. It captures sources, ideas, experiments, career moves, and learned lessons — all indexed and cross-linked.

## Three-Layer Architecture

```
Layer 1: Raw Sources      → 01-reading-log/     (papers, articles, raw notes)
Layer 2: Wiki / Synthesis → 02-research/       (ideas, experiments, nexus)
Layer 3: Schema / Output  → 03-academic/        (CV, SOP, applications, contacts)
```

Each layer builds on the previous. Agents should:
- **Ingest** raw sources into Layer 1
- **Query** Layer 1+2 when answering research questions
- **Write outputs** to Layer 3 or project files
- **Never** write to Layer 1 without a source citation

## Vault Operations

### Ingest
New papers, articles, or notes go to `01-reading-log/` with date-stamped entries and per-paper notes in `paper-notes/`. Every ingest entry gets added to `log.md`.

### Query
Before answering research questions, agents MUST scan the vault:
1. Check `00-meta/index.md` for the catalog
2. Search `02-research/` for relevant ideas/experiments
3. Check `01-reading-log/` for cited papers
4. Check `06-career/opportunities/` for active leads

### Lint (monthly)
- All `YYYY-MM-DD.md` files in `01-reading-log/` should have valid frontmatter
- All research idea files should have `status: active|rejected|archived` in frontmatter
- `log.md` should have an entry for every vault write
- Run: `rg "\-\ \[ \]" 01-reading-log/ --count` — count should be low (completed items)

## Git Protocol

**Before any write:**
```bash
cd ~/obsidian-vault
git pull origin main
```

**After any write:**
```bash
cd ~/obsidian-vault
git add -A
git commit -m "<type>: <short description>"
git push origin main
```

**Conflict resolution:** If push fails due to remote changes, pull first (`git pull --rebase`), resolve conflicts manually, then push. Never force-push to main.

## Directory Ownership

| Directory | Owner | Purpose |
|---|---|---|
| `00-meta/` | system-agent | Vault schema, identity, index |
| `01-reading-log/` | research-scout | Paper ingestion, scoring, daily logs |
| `02-research/ideas/` | research-analyst | Research ideas (active/rejected/archived) |
| `02-research/papers/` | research-analyst | Past submissions, arXiv links |
| `02-research/experiments/` | coding | Experiment results, ablations |
| `02-research/nexus/` | planner | Weekly technique synthesis |
| `03-academic/` | career-scout | CV, SOP, professor lists, applications |
| `04-projects/` | coding | App/project notes and specs |
| `05-learning/` | knowledge-agent | Course notes, learning logs |
| `06-career/` | career-scout | Opportunities, contacts |
| `07-knowledge/` | knowledge-agent | Cross-domain connections |
| `log.md` | any | Append-only activity log (shared write) |

## Note Format Conventions

### Frontmatter (required on all notes)
```yaml
---
title: Note title
created: YYYY-MM-DD
tags: [tag1, tag2]
---

Content here.
```

### Links
- Wiki-links: `[[note-name]]`
- External: `[label](url)`
- Headings in same file: `[[note-name#heading]]`

### Dates
- Format: `YYYY-MM-DD` (ISO 8601)
- Examples: `2026-04-29`, `2026-04-01`

### Status Tags (research ideas)
```yaml
status: active    # in progress, worth pursuing
status: rejected  # tried, rejected with reason noted
status: archived  # dead end, but worth revisiting later
```

## Quick Links

- [Vault Index](00-meta/index.md)
- [Identity](00-meta/identity.md)
- [Reading Log](01-reading-log/README.md)
- [Active Research Ideas](02-research/ideas/active/)
- [Application Tracker](03-academic/applications/tracking.md)
- [Activity Log](log.md)
