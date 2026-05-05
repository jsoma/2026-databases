# Data and Databases

Third-semester core course in the Columbia Journalism School Data Journalism MS program. This course teaches students to work with datasets that require more than pandas and a laptop — government databases with millions of records, document dumps from real investigations, and long-term data projects that multiple journalists can access — and to publish what they find as journalism.

The course follows the ProPublica model: acquire a significant dataset, analyze it thoroughly over time, and build public-facing tools that both tell a story and let readers explore the data themselves. AI tooling now covers most scraping, so the spine of the course is databases proper — acquiring, querying, verifying, and publishing — with an investigation-shaped question at the center of every week.

> **Status:** This is the v3 design after a structured review and iteration cycle (May 2026). Methodology citations have been verified. The full design rationale is in `design-notes.md`; the canonical syllabus reference is `instructor-notes.md`.

## Course Materials

### Core Materials
- **[Curriculum](curriculum.md)** — week-by-week topics, concepts, and skills
- **[Assignments](assignments.md)** — Foundation / Extension / Innovation tiers, plus methodology-as-journalism deliverables
- **[Tech Stack](tech-stack.md)** — all tools and technologies with documentation links
- **[Readings](readings.md)** — investigations and methodologies for each week, verified May 2026
- **[Speed Run](speed-run.md)** — self-study guide for learning the tech stack independently

### Design References
- **[Instructor Notes](instructor-notes.md)** — comprehensive design reference: Must/Should/Nice outcomes, recurring threads, pre-semester checklist
- **[Design Notes](design-notes.md)** — decision rationale, rejections, open items
- **[Readings Sweep](readings-sweep.md)** — verification audit (KEEP / FIX / CUT / ADD)

## Quick Navigation

### By Week

#### **[Week 1](curriculum.md#week-1-sql-via-question-families--duckdb): SQL via Question Families + DuckDB**
- "You just queried 10 million rows on your laptop"
- DuckDB, SQL basics, question families
- [Assignment](assignments.md#week-1-sql-via-question-families--duckdb) • [Readings](readings.md#week-1-sql-via-question-families--duckdb) • [Tech](tech-stack.md#databases)

#### **[Week 2](curriculum.md#week-2-joins-dirty-data-and-domain-checking): JOINs, Dirty Data, Domain Checking**
- "You found three bugs in queries you didn't write"
- Multi-table joins, HMDA gotchas, AI-generated SQL verification
- [Assignment](assignments.md#week-2-joins-dirty-data-and-domain-checking) • [Readings](readings.md#week-2-joins-dirty-data-and-domain-checking) • [Tech](tech-stack.md#databases)

#### **[Week 3](curriculum.md#week-3-cloud-as-access): Cloud as Access**
- "You just shared a database with zero configuration"
- Datasette, Backblaze B2, cost reasoning
- [Assignment](assignments.md#week-3-cloud-as-access) • [Readings](readings.md#week-3-cloud-as-access) • [Tech](tech-stack.md#cloud-infrastructure)

#### **[Week 4](curriculum.md#week-4-long-term-tracking--build-the-bill-scraper): Long-term Tracking — Build the Bill Scraper**
- "Your scraper ran automatically while you slept"
- GitHub Actions, git-scraping, change detection
- [Assignment](assignments.md#week-4-long-term-tracking--build-the-bill-scraper) • [Readings](readings.md#week-4-long-term-tracking--build-the-bill-scraper) • [Tech](tech-stack.md#automation--workflows)

#### **[Week 5](curriculum.md#week-5-how-data-gets-acquired-and-grows-over-time): How Data Gets Acquired and Grows Over Time**
- "You've heard from someone who actually does this for a living"
- FOIA, scraping, leaks; long-term tracking craft; guest speaker
- [Assignment](assignments.md#week-5-how-data-gets-acquired-and-grows-over-time) • [Readings](readings.md#week-5-how-data-gets-acquired-and-grows-over-time)

#### **[Week 6](curriculum.md#week-6-openaleph--collaborative-investigation-infrastructure): OpenAleph — Collaborative Investigation Infrastructure**
- "Cross-newsroom, cross-border, cross-language, cross-everything investigations"
- OpenAleph public instance, FollowTheMoney ontology, entity graph thinking
- [Assignment](assignments.md#week-6-openaleph--collaborative-investigation-infrastructure) • [Readings](readings.md#week-6-openaleph--collaborative-investigation-infrastructure) • [Tech](tech-stack.md#document-processing--investigation)

#### **[Week 7](curriculum.md#week-7-self-hosted-aleph--ingest-your-own-data): Self-hosted Aleph + Ingest Your Own Data**
- "You traced an LLC to a person"
- NYC building ownership, Docker OpenAleph, FtM CSV mapping
- [Assignment](assignments.md#week-7-self-hosted-aleph--ingest-your-own-data) • [Readings](readings.md#week-7-self-hosted-aleph--ingest-your-own-data) • [Tech](tech-stack.md#document-processing--investigation)

#### **[Week 8](curriculum.md#week-8-networks-for-investigations-cypher): Networks for Investigations (Cypher)**
- "Connecting the dots of people and companies"
- Neo4j Sandbox, Cypher pattern matching, path tracing
- [Assignment](assignments.md#week-8-networks-for-investigations-cypher) • [Readings](readings.md#week-8-networks-for-investigations-cypher) • [Tech](tech-stack.md#databases)

#### **[Week 9](curriculum.md#week-9-build-a-baby-aleph--extraction--fuzzy-matching--linking): Build a Baby Aleph**
- "You measured how good AI was at finding duplicates"
- Anthropic structured outputs, rapidfuzz, dedupe, comparison framework
- [Assignment](assignments.md#week-9-build-a-baby-aleph--extraction--fuzzy-matching--linking) • [Readings](readings.md#week-9-build-a-baby-aleph--extraction--fuzzy-matching--linking) • [Tech](tech-stack.md#ai--machine-learning)

#### **[Week 10](curriculum.md#week-10-public-facing-tools--security--privacy--methodology): Public-Facing Tools + Security + Privacy + Methodology**
- "Your investigation tool is live on the internet — and you know what it would leak"
- Flask + SQLite + Render, security audit, methodology page
- [Assignment](assignments.md#week-10-public-facing-tools--security--privacy--methodology) • [Readings](readings.md#week-10-public-facing-tools--security--privacy--methodology) • [Tech](tech-stack.md#data-tools--publishing)

#### **[Week 11](curriculum.md#week-11-ai-extraction-at-depth--multilingual): AI Extraction at Depth + Multilingual**
- "AI just read 100 documents in 30 seconds — and you know which ones it got wrong"
- Structured outputs at scale, multilingual extraction, collaborative verification
- [Assignment](assignments.md#week-11-ai-extraction-at-depth--multilingual) • [Readings](readings.md#week-11-ai-extraction-at-depth--multilingual) • [Tech](tech-stack.md#ai--machine-learning)

#### **[Week 12](curriculum.md#week-12-methodology-article--bill-tracker-harvest--sustainability): Methodology + Bill Harvest + Sustainability**
- "You shipped a tool, a story, and a methodology — and your scraper is still running"
- Methodology-as-journalism, longitudinal harvest, sustainability planning
- [Assignment](assignments.md#week-12-methodology-article--bill-tracker-harvest--sustainability) • [Readings](readings.md#week-12-methodology-article--bill-tracker-harvest--sustainability)
