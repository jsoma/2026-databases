# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this repository.

## Repository Overview

Curriculum repository for the Data and Databases course at Columbia Journalism School's Data Journalism MS program — a third-semester core course. It contains course materials, not executable code. The course teaches journalists to work with large datasets using the ProPublica model: acquire significant datasets, analyze thoroughly over time, and build public-facing tools.

The course was revamped from a 1st-semester scraping+regex version. AI tooling now covers most scraping, so the spine is databases proper — with a stronger journalistic direction.

## Document Architecture

The course is **12 weeks**. All student-facing files reflect the 12-week design.

### Student-facing files
- **README.md** — Entry point: brief intro, navigation by week
- **curriculum.md** — Master document with weekly topics, concepts, skills, and "Quick Win" motivational checkpoints
- **assignments.md** — Foundation / Extension / Innovation tiers; methodology-as-journalism deliverables at W5, W7, W10, W12
- **tech-stack.md** — All technologies with documentation links and week numbers
- **readings.md** — Curated investigations and methodologies per week, verified May 2026
- **speed-run.md** — Hands-on walkthrough of the complete tech stack (instructor-oriented; doesn't say so on the page)

### Design artifacts
- **instructor-notes.md** — Comprehensive design reference: Must/Should/Nice outcomes, recurring threads, pre-semester checklist, file-by-file rewrite plan
- **design-notes.md** — Decision log and process trail for the v3 revamp
- **review-prompt.md** — Historical review prompt that drove the redesign

### Week structure (12 weeks)
- W1: SQL via Question Families + DuckDB
- W2: JOINs, Dirty Data, Domain Checking
- W3: Cloud as Access
- W4: Long-term Tracking — Build the Bill Scraper
- W5: How Data Gets Acquired and Grows Over Time
- W6: OpenAleph — Collaborative Investigation Infrastructure
- W7: Self-hosted Aleph + Ingest Your Own Data
- W8: Networks for Investigations (Cypher)
- W9: Build a Baby Aleph — Extraction + Fuzzy Matching + Linking
- W10: Public-Facing Tools + Security + Privacy + Methodology
- W11: AI Extraction at Depth + Multilingual
- W12: Methodology Article + Bill Tracker Harvest + Sustainability

## Key Principles

### Technology Philosophy
- Practical newsroom tools over cutting-edge tech
- DuckDB for big-CSV-on-laptop SQL; Postgres when persistence/sharing matters; SQLite for read-only public tools
- Datasette for sharing investigation databases
- OpenAleph (community fork; OCCRP sunsetting Aleph after Dec 2025) for entity-graph document investigations
- DocumentCloud for normal-sized document collections (lighter alternative to Aleph)
- Neo4j + Cypher for graph-shaped investigation questions (Week 8)
- rapidfuzz / dedupe for fuzzy matching and entity resolution (Week 9)
- Backblaze B2 over AWS S3 (simpler and cheaper)
- Flask + SQLite + Render for public read-only tools
- Anthropic / OpenAI structured outputs for AI extraction
- BigQuery, Heroku, AWS S3 are NOT taught; mentioned only

### Pedagogical Approach
- Focus on journalistic applications, not abstract technology
- Writing prompts pose specific newsroom scenarios
- Respect instructor autonomy (avoid prescriptive instructions)
- Common issues should be specific and actionable
- AI verification: every claim needs a source; sample-based audit at scale

### Recurring threads (cross-week)
- **Question families** — 12 categorical question shapes introduced in W1
- **Methodology-as-journalism** — recurring deliverable building W5 → W7 → W10 → W12
- **Verification mindset** — threads through W2, W7, W9, W11
- **AI as default** — woven through; not a separate unit

## Maintenance Guidelines

### When Updating Content
- Maintain cross-references between docs (anchor links: `#week-N-...`)
- Keep week numbers consistent across all files
- Include both low-prep and high-prep options in assignments
- Add real journalism examples to readings; verify URLs before assigning
- Preserve "Quick Wins" in curriculum.md

### Writing Style
- Concise and practical
- Use real investigations as examples (Panama Papers, Cyprus Confidential, ProPublica's work)
- Avoid condescending language or time estimates
- Be specific about requirements ("10-20 documents" not "SHORT PDFs")
- No emoji unless instructor explicitly asks

### Course Philosophy
Every technical skill should enable better journalism. The course follows the ProPublica model. Students take this concurrently with their thesis and a mapping class.

The bar: teach things students wouldn't pick up at a typical newsroom data desk. Anchor on what an exceptional desk does. Commodity newsroom skills don't belong here. The original course's failure mode was a tool tour; every week should answer "what investigation does this unlock?", not "what tool does this teach?"

### Student prerequisites
Entering students have completed:
- One semester of intro Python/pandas and basic scraping
- One semester building data viz / data-driven story projects
- Static web publishing with GitHub Pages, HTML/CSS fundamentals
- Regular use of AI coding tools (Claude Code, Cursor)

They have NOT touched SQL or any database. Their git experience is solo commits only.

### Off-limits (deliberately not taught)
- Collaborative git workflows — students learn this on the job
- Rote SQL syntax drills — AI writes SQL well; verification is the skill
- Schema design / normalization theory beyond "spreadsheets you join them"
- Spatial / GIS — covered in concurrent mapping class
- BigQuery — bill-running risk
- A semester-long final investigation — modular weekly assignments instead
