---
title: Reading Log README
created: 2026-04-29
tags: [vault, reading, protocol, research]
---

# 01 — Reading Log

> How papers, articles, and sources are logged and tracked.

## Purpose

The reading log captures every source Teddy or agents encounter — papers, articles, threads, guides. It is the **Layer 1** (raw sources) of the vault's three-layer architecture.

## Logging Format

### Daily Entry File: `YYYY-MM-DD.md`

Each day a new file is created (or appended to) in `01-reading-log/`:

```yaml
---
title: "YYYY-MM-DD Reading Log"
created: YYYY-MM-DD
tags: [daily-log, YYYY-MM]
---

## Morning Reads

-

## Afternoon Reads

-

## Notes

-
```

### Paper Entry (in daily log)

```
- **Title:** Paper Title
  **Authors:** Author 1, Author 2
  **Source:** [arXiv URL or venue]
  **URL:** https://...
  **Score:** X/Y
  **Relevance:** high|medium|low
  **Notes:** |
    Key claims: ...
    Weaknesses: ...
    Connection to current work: ...
  → [[paper-notes/short-title]]
```

### Article/Thread Entry

```
- **Title:** Article Title
  **Source:** publication or platform name
  **URL:** https://...
  **Score:** X/Y
  **Relevance:** high|medium|low
  **Notes:** |
    Summary: ...
    Action items: ...
```

## Per-Paper Notes: `paper-notes/{short-title}.md`

For papers that deserve deep treatment, create a dedicated note:

```yaml
---
title: "Paper Short Title"
authors: "Author 1, Author 2"
year: YYYY
venue: "Venue or arXiv"
url: https://...
score: X/Y
relevance: high|medium|low
created: YYYY-MM-DD
tags: [paper, research-area]
status: read|reading|to-read
---

# Summary

> One-paragraph summary here.

# Key Contributions

1. ...
2. ...
3. ...

# Methodology

-

# Results

-

# Strengths

-

# Weaknesses / Open Questions

-

# Connection to Current Work

-

# Citation

```
@article{...,
  title={...},
  author={...},
  year={...}
}
```
```

## Scoring System

| Score | Meaning |
|---|---|
| 9-10/Y | Must-read for current research; highly relevant + well-executed |
| 7-8/Y | Very relevant; read selectively |
| 5-6/Y | Some relevance; skim results/methodology |
| 3-4/Y | Low relevance; scan abstract + conclusion only |
| 1-2/Y | Not relevant; skip |

## Relevance Ratings

- **high:** Directly relevant to current research direction or active project
- **medium:** Tangentially relevant; may inform future work
- **low:** Background knowledge; filed for context

## Agent Protocol

1. **Ingest:** Log source to today's `YYYY-MM-DD.md` file
2. **Score:** Assign score and relevance within the entry
3. **Deep-note:** If score ≥ 7 or relevance = high, create per-paper note
4. **Update log:** Append ingest entry to `log.md`
5. **Update index:** Add to `00-meta/index.md` reading log section
6. **Git commit:** Follow git protocol

## Searching the Reading Log

```bash
# Find papers by keyword
cd ~/obsidian-vault
rg "keyword" 01-reading-log/ --md

# Find papers by relevance
rg "^relevance: high" 01-reading-log/ --files-with-matches

# List all paper notes
ls 01-reading-log/paper-notes/
```
