# Data and Databases Curriculum

A third-semester core course in the Columbia Journalism School Data Journalism MS. Students arrive comfortable with Python, pandas, scraping, GitHub Pages, and AI coding tools. They have not touched SQL or any database. This course teaches them to work with datasets that require more than pandas and a laptop — government databases with millions of records, document dumps from real investigations, and long-term data projects — and to publish what they find as journalism.

## Course Overview

The course follows the ProPublica model: acquire a significant dataset, analyze it thoroughly over time, and build public-facing tools that both tell a story and let readers explore the data themselves. AI tooling now covers most scraping, so the spine of the course is databases proper — acquiring, querying, verifying, and publishing — with an investigation-shaped question at the center of every week.

The bar is what you'd learn at an exceptional data desk (Reveal / ProPublica / ICIJ / THE CITY tier), not what you'd pick up at any newsroom. Every week answers "what investigation does this unlock?" — not "what tool does this teach?"

## Core Philosophy

- **Start with the story.** Every technical decision should enable better journalism.
- **Build for longevity.** Many investigations take months; your data infrastructure should last.
- **Assume messiness.** Government data is dirty, incomplete, and inconsistently formatted.
- **Plan for collaboration.** You'll rarely work alone on big investigations.
- **Think about readers.** How will the public interact with your findings?
- **Methodology is journalism.** The writeup of *how* is part of the work, not separate from it.

## Week 0: Prerequisites

Students should arrive comfortable with:
- **Python and pandas** for data manipulation
- **CSV, JSON, and APIs** as data formats and access patterns
- **Basic scraping** (BeautifulSoup-level)
- **Command line basics** — navigating directories, running scripts
- **GitHub Pages** for static publishing
- **HTML/CSS fundamentals**
- **AI coding tools** (Claude Code, Cursor) — they reach for AI by default

Students do **not** need to arrive with SQL, any database experience, or git collaboration. The course teaches what they need.

## What this course does NOT teach (deliberately)

- **Collaborative git workflows** — students learn this live on the job.
- **Rote SQL syntax drills** — AI writes SQL well; students need verification fluency.
- **Schema design / normalization theory** beyond "different spreadsheets you join them."
- **Spatial / GIS** — covered in concurrent mapping class.
- **A semester-long final investigation** — modular weekly assignments that build conceptual depth instead.

## Recurring Threads

Threads that appear across multiple weeks rather than living in one.

### Question families

Introduced in Week 1 as a graduate-level habit-of-mind for analyzing any new dataset. Twelve categorical shapes of journalistic questions:

1. Counts/totals
2. Rates/proportions
3. Comparisons across peers
4. Distributions (median vs mean, spread)
5. Trends over time
6. Tail analysis (top/bottom N, outliers)
7. Concentration (what % accounts for what share)
8. Geographic patterns
9. Change detection
10. Negation/absence (who's missing, what didn't happen)
11. Reconciliation (does dataset A agree with dataset B)
12. Counterfactual (what if we excluded X)

The default journalistic instinct is "averages and top-10 lists." This list lifts students past that.

### Methodology-as-journalism

A recurring deliverable, not a final-week add-on. First taste in Week 5 (a 300-word blurb in the bill scraper repo README), again in Week 7 (Aleph investigation methodology), Week 10 (a full methodology page linked from the public tool), Week 12 (a publication-quality methodology article on one project).

### Verification mindset

Threads through Week 2 (HMDA gotchas), Week 7 (Aleph entity resolution review), Week 9 (entity-resolution comparison against ground truth), Week 11 (collaborative AI extraction verification with class leaderboard).

### AI as default

Students bring AI to every week. The course emphasizes when AI helps, when it silently misleads, and how to verify its output. This is not a separate unit — it's how they work.

## Weekly Breakdown

### Week 1: SQL via Question Families + DuckDB
**Core Question:** How do you handle datasets too big for Excel but not worth the cloud?
→ [Assignment](assignments.md#week-1-sql-via-question-families--duckdb) | [Readings](readings.md#week-1-sql-via-question-families--duckdb) | [Tech](tech-stack.md#databases)

**Journalistic Context:**
You've just received HMDA mortgage data (25 million rows), PPP loans (11 million rows), or Medicare payments to doctors. These datasets are too large for pandas to handle comfortably but don't require distributed computing. You need to quickly explore them, find patterns, and identify potential stories — and you need to know what *kinds* of questions to ask, not just one or two. Most reporters reach for averages and top-10 lists. Exceptional ones know there are a dozen question shapes that surface different stories from the same data.

**Must-Haves:**
- **Concepts:** when a dataset is "big" vs just "bigger than Excel"; memory vs disk tradeoffs; the twelve question families as a planning tool
- **Technical Skills:**
  - DuckDB to query CSVs directly without loading into memory
  - Basic SQL: SELECT, WHERE, GROUP BY, simple JOIN
  - SQL → pandas: filter in SQL, analyze in pandas
  - Using AI as a SQL co-pilot: write, run, sanity-check, summarize

**Stories This Enables:**
- Redlining patterns in mortgage data
- PPP fraud detection through anomaly analysis
- Doctor payment outliers suggesting fraud

**Quick Win:** "You just queried 10 million rows on your laptop."

---

### Week 2: JOINs, Dirty Data, and Domain Checking
**Core Question:** When does an AI-generated query produce a confidently wrong answer?
→ [Assignment](assignments.md#week-2-joins-dirty-data-and-domain-checking) | [Readings](readings.md#week-2-joins-dirty-data-and-domain-checking) | [Tech](tech-stack.md#databases)

**Journalistic Context:**
AI writes SQL well. It writes most queries faster and with fewer typos than a journalist who's been working with SQL for ten years. What it does poorly: notice when a column code means something different than its name suggests, when a JOIN silently drops 30% of rows, when a refinance is masquerading as a new mortgage. Reveal's "Kept Out" investigation had to think carefully about which HMDA action codes counted as adverse outcomes — banks that pressure marginal applicants to withdraw can make their raw "denied" rate look great. This week is about being the editor of the AI's queries, not the typist.

**Must-Haves:**
- **Concepts:** semantic correctness vs syntactic correctness; domain knowledge as the journalism skill; the "find what's wrong with this query" pedagogy
- **Technical Skills:**
  - Multi-table JOINs across HMDA + lender info + Census tracts
  - LEFT vs INNER JOIN — and the silent collapse when WHERE is on the right side
  - Reading and correcting AI-generated SQL
  - Sanity-checking results against published totals

**Stories This Enables:**
- Modern redlining investigations (the right way)
- Bank-by-bank lending pattern comparisons
- Demographic disparities surviving controls

**Nice-to-Haves:**
- Recursive CTEs for hierarchical queries

**Quick Win:** "You found three bugs in queries you didn't write."

---

### Week 3: Cloud as Access
**Core Question:** When does a dataset stop being yours and start needing to be shared?
→ [Assignment](assignments.md#week-3-cloud-as-access) | [Readings](readings.md#week-3-cloud-as-access) | [Tech](tech-stack.md#cloud-infrastructure)

**Journalistic Context:**
Three scenarios force you to the cloud: your investigation team needs to query the same database simultaneously; you're building something the public will access (like ProPublica's Nonprofit Explorer); or you're processing data continuously (like election results). The cloud isn't about "big data" — it's about access and availability. And it can cost a lot if you're not paying attention.

**Must-Haves:**
- **Concepts:** when cloud makes sense (collaboration, public access, continuous processing) and when it doesn't (one-off, sensitive, budget); cost reasoning at order-of-magnitude
- **Technical Skills:**
  - Datasette setup for sharing investigation databases
  - Backblaze B2 for documents and data files (S3-compatible)
  - Connection strings, permissions, read-only access
  - Cost estimation for hypothetical 6-month investigations

**Stories This Enables:**
- Collaborative investigations like Panama Papers (shared database)
- Public databases like ProPublica's Nonprofit Explorer
- Live election results tracking

**Quick Win:** "You just shared a database with zero configuration."

---

### Week 4: Long-term Tracking — Build the Bill Scraper
**Core Question:** How do you build infrastructure that watches a dataset for months while you're working on other things?
→ [Assignment](assignments.md#week-4-long-term-tracking--build-the-bill-scraper) | [Readings](readings.md#week-4-long-term-tracking--build-the-bill-scraper) | [Tech](tech-stack.md#automation--workflows)

**Journalistic Context:**
Most important investigations aren't built on breaking news — they're built on systematic analysis of data over time. ProPublica's Dollars for Docs took months to clean, verify, and build. Simon Willison's PG&E outage tracker has tens of thousands of commits. The LA Times' LAPD crime statistics investigation required comparing years of reports. You need systems that let you incrementally improve data quality, track your cleaning decisions, and update analyses as new data arrives.

This week you build the scraper. It runs in the background for the rest of the semester. Week 12 is the harvest.

**Must-Haves:**
- **Concepts:** data versioning; reproducibility (can another reporter verify your work?); documentation as you go (you won't remember why you excluded those rows in eight weeks)
- **Technical Skills:**
  - GitHub Actions for scheduled scrapers
  - Git-scraping pattern (Simon Willison's approach)
  - Change detection — finding new records, deleted records, modifications
  - Error notifications when scrapers break

**Stories This Enables:**
- Long-term tracking of police complaints
- Monitoring changes in government databases
- Building institutional knowledge about datasets

**Quick Win:** "Your scraper ran automatically while you slept."

---

### Week 5: How Data Gets Acquired and Grows Over Time
**Core Question:** Where does scale data come from, and what changes when you live with a dataset for a year?
→ [Assignment](assignments.md#week-5-how-data-gets-acquired-and-grows-over-time) | [Readings](readings.md#week-5-how-data-gets-acquired-and-grows-over-time)

**Journalistic Context:**
Two threads connect naturally — data acquisition (FOIA, scraping, leaks, public bulk download, build-it-yourself) and long-term tracking. Most scale acquisition involves *ongoing* relationships with data sources. Bill scrapers, recurring FOIAs, and tracker projects are the same shape. The Washington Post's Fatal Force project combines FOIA, scraping, and ongoing local sourcing for a database it's run since 2015. Long-term tracking journalists usually have FOIA war stories because long-term work usually means recurring records requests.

This is a step-back week. The technical work continues in the background; the focus is on craft and process.

**Must-Haves:**
- **Concepts:** modes of data acquisition and which fits which kind of story; how a long-term tracker generates a story over time; the journalist's work *outside* the query
- **Technical Skills:**
  - Light: applying question families to longitudinal data
  - Light: planning data acquisition for an investigation you haven't started

**Plus:** First methodology-as-journalism deliverable — a 300-word blurb in the bill scraper repo README.

**Plus:** Guest speaker from a working journalist whose work spans long-term tracking and FOIA.

**Stories This Enables:**
- The shape of a multi-year investigation
- Ongoing accountability projects (salary databases, complaint trackers)

**Quick Win:** "You've heard from someone who actually does this for a living."

---

### Week 6: OpenAleph — Collaborative Investigation Infrastructure
**Core Question:** How do hundreds of journalists across newsrooms search the same documents without stepping on each other?
→ [Assignment](assignments.md#week-6-openaleph--collaborative-investigation-infrastructure) | [Readings](readings.md#week-6-openaleph--collaborative-investigation-infrastructure) | [Tech](tech-stack.md#document-processing--investigation)

**Journalistic Context:**
During the Panama Papers, hundreds of journalists needed to search the same document set without stepping on each other's toes. ICIJ built infrastructure that combined entity extraction, cross-document matching, OCR, and a graph view of relationships that emerged from documents nobody had tagged. That infrastructure is OpenAleph (formerly Aleph at OCCRP, now a community fork at openaleph.org). This week you use the public OCCRP instance — Companies House, sanctions, ICIJ Offshore Leaks entity graphs — to learn what entity-graph thinking actually looks like.

For most journalism, you won't reach for OpenAleph — you'll use DocumentCloud (hosted by MuckRock since 2021). DocumentCloud does OCR, annotation, redaction, and provides embeddable document viewers so your story can include source documents inline. It's the standard newsroom tool when you have 200 PDFs from a FOIA. OpenAleph is what you reach for at ICIJ scale — hundreds of thousands of documents, multilingual, multi-newsroom collaboration. The course teaches OpenAleph because it's the harder, more revealing tool; DocumentCloud is briefly compared so you know when to pick which.

**Must-Haves:**
- **Concepts:**
  - The FollowTheMoney (FtM) ontology — `Person`, `Company`, `Directorship`, `Vessel`, `Payment`. Once you know FtM, every dataset becomes joinable.
  - Entity-as-graph thinking (vs entity-as-row)
  - What OpenAleph does that a SQL database doesn't
- **Technical Skills:**
  - OpenAleph search and entity navigation
  - Cross-dataset entity matching against sanctions lists, Companies House, Offshore Leaks

**Stories This Enables:**
- Multi-newsroom collaborations
- Shell company networks revealed via cross-dataset hits
- Sanctioned individual surfacing in unexpected places

**Note:** ICIJ leak documents (Panama, Pandora, Paradise) are not public — only entity graphs are. Plan around the network, not document review.

**Homework launched:** Each student arranges a 20-min interview with a working journalist on a related investigation; writeup is due Week 5 (this is intentional — it spreads load).

**Quick Win:** "Cross-newsroom, cross-border, cross-language, cross-everything investigations."

---

### Week 7: Self-hosted Aleph + Ingest Your Own Data
**Core Question:** How do you make a pile of documents into a queryable entity graph?
→ [Assignment](assignments.md#week-7-self-hosted-aleph--ingest-your-own-data) | [Readings](readings.md#week-7-self-hosted-aleph--ingest-your-own-data) | [Tech](tech-stack.md#document-processing--investigation)

**Journalistic Context:**
Last week you searched a public Aleph instance with curated, well-known datasets. This week you load real, messy data nobody has prepared for you. New York City's building ownership records — ACRIS deeds, HPD owner registrations with the magic "head officer" field that pierces LLCs, DOB BIS permits, NYS Department of State corporate filings — describe every property in the city. Sometimes the same human appears as "Robert Izsak," "R. Izsak," and "Izsak Realty LLC c/o…" at three different addresses. JustFix's "Who Owns What" tool exists because tracing this is the difference between knowing who your landlord is and knowing who actually controls your block.

**Must-Haves:**
- **Concepts:** data ingestion as a lossy process; entity resolution as journalism; methodology of "who owns this block"
- **Technical Skills:**
  - Docker OpenAleph instance (instructor-run; students don't operate the server)
  - FtM CSV mapping for ingestion (`ftm map`)
  - Reviewing OpenAleph's automatic entity resolution: where it succeeded, where it failed

**Plus:** Methodology-as-journalism — Aleph investigation methodology memo.

**Stories This Enables:**
- "Who owns this block" investigations
- LLC-piercing in any city with public deed records
- Coordinated landlord network identification

**Nice-to-Haves:**
- Entity resolution across more than two source datasets

**Quick Win:** "You traced an LLC to a person."

---

### Week 8: Networks for Investigations (Cypher)
**Core Question:** When does graph thinking beat relational thinking?
→ [Assignment](assignments.md#week-8-networks-for-investigations-cypher) | [Readings](readings.md#week-8-networks-for-investigations-cypher) | [Tech](tech-stack.md#databases)

**Journalistic Context:**
Some questions are graph-shaped. "Find every politician connected to a sanctioned company through three or fewer shell entities" is impossible to write cleanly as a SQL query — you'd need 16 self-joins, recursive CTEs, and a lot of pain. As a Cypher query it's six lines. ICIJ used Linkurious (a Neo4j frontend) for Panama, Paradise, and Pandora because graph thinking matched the journalism. You don't need to become a Neo4j expert — but you do need to know when the question shape calls for a graph.

**Must-Haves:**
- **Concepts:** when graphs beat relational databases; nodes and edges; thinking journalistically in patterns
- **Technical Skills:**
  - Neo4j Sandbox setup (free, no install)
  - Cypher pattern matching: `MATCH-WHERE-RETURN`
  - Shortest paths, basic centrality

**Stories This Enables:**
- Shell company networks in financial investigations
- Power broker identification in political stories
- Connected-LLC cluster identification

**Light week.** Don't go deep on Cypher syntax. The week is about *thinking in networks*.

**Quick Win:** "Connecting the dots of people and companies."

---

### Week 9: Build a Baby Aleph — Extraction + Fuzzy Matching + Linking
**Core Question:** When OpenAleph isn't available, how do you build the same capabilities yourself?
→ [Assignment](assignments.md#week-9-build-a-baby-aleph--extraction--fuzzy-matching--linking) | [Readings](readings.md#week-9-build-a-baby-aleph--extraction--fuzzy-matching--linking) | [Tech](tech-stack.md#ai--machine-learning)

**Journalistic Context:**
You won't always have OpenAleph. Maybe your data is sensitive and can't go to a hosted service. Maybe you're working at a smaller outlet without infrastructure budget. Maybe the project is small enough that standing up Aleph is overkill. The components that make Aleph useful — entity extraction, fuzzy matching, cross-dataset linking — can all be reproduced with Python libraries and AI. Doing this yourself reveals what entity resolution actually does and what its failure modes look like.

**Must-Haves:**
- **Concepts:** entity extraction, fuzzy matching, record linkage as separate problems with separate tradeoffs
- **Technical Skills:**
  - AI extraction (Anthropic / OpenAI structured outputs) for entity recognition
  - rapidfuzz / dedupe for fuzzy matching
  - Comparison framework: same task, multiple approaches, measurement against ground truth

**Deliverable:** Comparison exercise — same task run three ways (OpenAleph result from W7, fuzzy-matching library, AI structured outputs), measured against ground-truth subset.

**Stories This Enables:**
- Cross-dataset linkage in places without big infrastructure
- Sensitive-document investigations
- Small-scale entity resolution

**Quick Win:** "You measured how good AI was at finding duplicates — and you have the numbers."

---

### Week 10: Public-Facing Tools + Security + Privacy + Methodology
**Core Question:** How do you let readers explore your investigation without exposing them or yourself?
→ [Assignment](assignments.md#week-10-public-facing-tools--security--privacy--methodology) | [Readings](readings.md#week-10-public-facing-tools--security--privacy--methodology) | [Tech](tech-stack.md#data-tools--publishing)

**Journalistic Context:**
ProPublica's Nonprofit Explorer lets anyone research tax-exempt organizations. The Texas Tribune's salary database lets citizens see what public employees earn. These tools extend journalism's impact and build trust through transparency. They also expose data — sometimes sensitive data — to anyone with an internet connection. A read-only public tool is not a research tool: a stalker can use a salary database to find someone's workplace; a SQL injection can exfiltrate the underlying database. This week is about publishing responsibly: the tool, the security audit, and the methodology page.

**Must-Haves:**
- **Concepts:** designing for non-technical users; privacy and ethical considerations (doxxing, harassment); maintenance burden (who updates this in 2 years?); methodology as part of publication
- **Technical Skills:**
  - Flask + SQLite (read-only) + Render
  - Security failure modes: SQL injection, exposed write credentials, PII leakage, load
  - Privacy and harm reduction
  - Methodology-as-journalism — full methodology page linked from the tool

**Deliverable (three parts):** deployed tool + security audit document + methodology page.

**Stories This Enables:**
- Salary databases for government transparency
- Hospital quality comparisons
- Building ownership lookup tools (drawing on Week 7)

**Nice-to-Haves:**
- Embed widgets for other newsrooms
- API access for developers

**Quick Win:** "Your investigation tool is live on the internet — and you know what it would leak if you got it wrong."

---

### Week 11: AI Extraction at Depth + Multilingual
**Core Question:** When AI reads documents for you, how do you know what it got right and wrong?
→ [Assignment](assignments.md#week-11-ai-extraction-at-depth--multilingual) | [Readings](readings.md#week-11-ai-extraction-at-depth--multilingual) | [Tech](tech-stack.md#ai--machine-learning)

**Journalistic Context:**
Quartz's Luanda Leaks investigation used AI to make 715,000 Portuguese-language records searchable in English, finding documents semantically related to seeds (a board minutes example) without keyword crafting. ICIJ has done similar work for FinCEN Files. The technique is straightforward; the verification is hard. AI confidently extracts entities that aren't there, mistranslates, and contaminates findings across documents. The journalism skill is designing a verification workflow that scales.

**Must-Haves:**
- **Concepts:** semantic search vs keyword search; verification at scale; cost-quality tradeoffs; what AI can't do (counting, math, facts without sources)
- **Technical Skills:**
  - Anthropic / OpenAI structured outputs for entity extraction
  - Multilingual extraction
  - Collaborative verification — each student verifies 20 docs from a shared 100-doc set against a held-back instructor "ground truth"; class leaderboard for both extraction and verification quality
  - Cost analysis: Sonnet vs Haiku on the same task

**Stories This Enables:**
- Cross-border investigations with multilingual documents
- Finding similar patterns across thousands of police reports
- Extracting structured data from government PDFs

**Nice-to-Haves:**
- pgvector for similarity search
- Local LLMs (LM Studio) for sensitive documents

**Quick Win:** "AI just read 100 documents in 30 seconds — and you know which ones it got wrong."

---

### Week 12: Methodology Article + Bill Tracker Harvest + Sustainability
**Core Question:** What does it mean to ship the work — not just the tool, but the writeup?
→ [Assignment](assignments.md#week-12-methodology-article--bill-tracker-harvest--sustainability) | [Readings](readings.md#week-12-methodology-article--bill-tracker-harvest--sustainability)

**Journalistic Context:**
Many data projects die after publication — the reporter moves on, the data goes stale, the tools break. The best investigations live on, updated annually, used by other newsrooms, cited in lawsuits. The Washington Post's Fatal Force has run since 2015. ProPublica updates Nonprofit Explorer every year. The bill scraper you built in Week 4 has been running for eight weeks now — what story has accumulated? And how do you write up your semester's work as journalism, not just submission?

**Must-Haves:**
- **Concepts:** documentation as journalism (methodology articles); handoff planning; maintenance burden vs impact; longitudinal analysis
- **Technical Skills:**
  - Methodology-as-journalism (publication-quality)
  - Bill tracker harvest — analyzing 7-8 weeks of accumulated change
  - Sustainability planning: scheduled updates, alerting, handoff docs

**Deliverable (three parts):**
1. Publication-quality methodology article on one project from earlier in the semester
2. Bill tracker findings memo — "I've watched X for 8 weeks; here's what changed and what story it suggests"
3. Sustainability plan + reflection

**Stories This Enables:**
- Annual updates to public databases
- Long-term tracking projects worth handing off
- Building institutional memory across reporter rotations

**Nice-to-Haves:**
- Usage analytics to track impact
- Automated reports to editors

**Quick Win:** "You shipped a tool, a story, and a methodology — and your scraper is still running."
