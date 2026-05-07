# Data and Databases — Syllabus v3

**Term:** 3rd semester | **Length:** 12 weeks | **Program:** Columbia Journalism School Data Journalism MS

This is the canonical revised syllabus after a structured review and iteration cycle. The course is being revamped from a 1st-semester scraping + regex course into a 3rd-semester databases-and-investigation course. AI tooling now covers most scraping, so that's no longer the spine.

This document integrates research findings on HMDA gotchas, OpenAleph built-ins, NYC investigation candidates, and verified methodology readings. See `design-notes.md` for the decision trail and `readings-sweep.md` for the readings audit. This syllabus is the source for rewriting `curriculum.md`, `assignments.md`, `readings.md`, `tech-stack.md`, `speed-run.md`, and `README.md`.

---

## The bar

Two questions frame every weekly decision:

1. **Does this teach something they wouldn't pick up at a typical data desk?** Anchor on what an *exceptional* desk does (Reveal / ProPublica / ICIJ / THE CITY tier). If a week teaches commodity newsroom skill, flag it for cut or upgrade.
2. **Does this week answer "what investigation does this unlock?"** — not "what tool does this teach?" The original course's failure mode was a tool tour. Avoid it.

If a week doesn't answer either, it's a candidate for cut or rework.

---

## Prerequisites

Students arrive comfortable with:
- Python, pandas, CSV/JSON/APIs
- Basic scraping (BeautifulSoup-level)
- Command line basics
- Static web publishing with GitHub Pages, HTML/CSS fundamentals
- AI coding tools (Claude Code, Cursor) as default
- One semester building data viz / data-driven story projects
- Have shipped Flask apps **with AI help** — but have NOT actually learned Flask, backend, deployment, databases, APIs, or production workflows

Students do NOT arrive with:
- SQL or any database experience
- Git collaboration beyond solo commits

---

## What this course deliberately does NOT teach

- **Collaborative git workflows.** Students learn this live on the job; class time is wasted.
- **Rote SQL syntax drills.** AI writes SQL well; students need verification fluency, not authorship.
- **Schema design / normalization theory** beyond "different spreadsheets you join them."
- **Spatial / GIS** — covered in concurrent mapping class.
- **BigQuery** — bill-running risk; mention only.
- **A semester-long final investigation.** Big investigations are too unpredictable to grade fairly. Modular weekly assignments build conceptual depth.

---

## Working premises

- **AI is woven throughout, not isolated.** Students use AI by default. Course emphasis: judgment, verification, schema reasoning, query reading, provenance, explaining what generated code does.
- **Exception:** the AI-extraction unit (W11) is dedicated because it's specifically about AI-as-data-transformation-tool — a distinct journalism skill, not a general "how to use AI" tour.
- **Cloud / API costs are acceptable.** Don't constrain to free tier.
- **Methodology-as-journalism throughout.** Students publish, not just submit. First taste in W5; recurring through W7, W10, W12.
- **Concurrent course load:** thesis + a mapping class. Treat as normal class load.

---

## What students leave with — Must / Should / Nice

### Must-have

- Read AI-generated SQL and spot when it's semantically wrong for the domain — joining on a near-duplicate key, filtering with a code value that means something else, GROUP BY hiding distinct entities.
- Sanity-check query results against domain knowledge or an independent count. ("29 hospitals reported zero deaths" should make you suspicious before publication, not after.)
- Write JOINs across messy real data and debug row-count drops journalistically — "did we lose 30% of cases or did the join just deduplicate?"
- Take a CSV too big for pandas and turn it into queryable storage (DuckDB → Postgres if persistence/sharing matters), articulating *why*.
- Verify any AI-generated extraction at scale with an explicit error-rate measurement, not vibes.
- Diagnose why a published query returned wrong numbers across three dimensions: bad source data, wrong join, semantic misunderstanding of a column.
- Publish a methodology article alongside any data work — provenance, choices made and rejected, known limitations, what would change the conclusion.
- Set up a scheduled scraper (GitHub Actions) that survives a year unattended, including failure-mode handling.
- Know when to reach for AI in a database workflow and when not to (column mapping: yes; "is this row suspicious" without a defined rule: no).

### Should-have

- Full-text search at scale; know why naive LIKE fails.
- Stand up a public-facing read-only database with caching, pagination, CSV export — and know the security failure modes.
- Embeddings + similarity search to find "documents like this one"; know failure modes.
- Reason about cloud / API costs at order-of-magnitude.
- Use Datasette to make an investigation database explorable, with editor-readable saved queries.
- Read an investigation methodology article and identify the underlying database operations.
- Track meaningful change in a dataset over time; distinguish meaningful change from noise.

### Nice-to-have

- Network analysis with Cypher / Neo4j — worth seeing once because students won't elsewhere.
- Local LLMs (LM Studio) for sensitive documents.
- pgvector for embeddings storage.
- OpenAleph operations beyond ingestion.
- Faceted search and advanced filters.
- Production-grade monitoring beyond "it broke."
- Window functions.

---

## Recurring threads

These appear across multiple weeks rather than living in one.

### Question families

Introduced in W1 as a graduate-level habit-of-mind. Twelve categorical shapes of journalistic questions, applied to any new dataset:

1. **Counts/totals** — "how many of X"
2. **Rates/proportions** — "X per 1,000, % of total"
3. **Comparisons across peers** — "this hospital vs similar hospitals"
4. **Distributions** — "median vs mean, what does the spread look like"
5. **Trends over time** — "period-over-period, year-over-year"
6. **Tail analysis** — "top/bottom N, outliers"
7. **Concentration** — "1% of landlords filed 20% of evictions"
8. **Geographic patterns** — "clustered or dispersed"
9. **Change detection** — "snapshot vs snapshot"
10. **Negation/absence** — "who's missing, what didn't happen"
11. **Reconciliation** — "does dataset A match dataset B"
12. **Counterfactual** — "what if we excluded X"

The default journalistic instinct is "averages and top-10 lists." This list lifts students past that. Used in W1 deliverables and revisited as a planning tool throughout.

### Methodology-as-journalism

Recurring deliverable. Students publish, not just submit. Build the habit:
- W5: 300-word methodology blurb in their bill scraper repo README
- W7: Aleph investigation methodology memo
- W10: Full methodology page linked from their public tool
- W12: Publication-quality methodology article on one project from earlier

### Verification mindset

Threads through W2 (HMDA gotchas), W7 (Aleph entity resolution review), W9 (entity-resolution comparison against ground truth), W11 (collaborative AI extraction verification with class leaderboard).

### AI as default

Students bring AI to every week. The course emphasizes when AI helps, when it silently misleads, and how to verify its output. This is not a separate unit — it's how they work.

---

## The dataset spine

Layered datasets each play multi-week roles:

| Weeks | Dataset | Role |
|---|---|---|
| 1-3 | HMDA county slice + lender + Census tract | Big enough to need DuckDB; domain rich enough for the "find what's wrong with this query" pedagogy |
| 4-12 | Bill tracker — each student picks a state legislative bill DB | Long-term project: built W4, harvested W12. 7-8 weeks of accumulated data by the end. |
| 5 | Case studies + own bill scraper still running | Step-back week; folds in FOIA via guest speaker |
| 6 | OpenAleph public datasets — Companies House, sanctions, Offshore Leaks | Conceptual exposure on real infrastructure |
| 7 | NYC building ownership — ACRIS + HPD + NYS DOS | Self-hosted Aleph ingestion |
| 8 | NYC entities exported from Aleph + ICIJ Offshore Leaks public graph | Network thinking |
| 9 | Subset of W7 NYC docs (or curated set with ground truth) | Entity resolution comparison |
| 10 | Student picks from earlier work (HMDA, bill tracker, NYC) | Public-facing tool of their choosing |
| 11 | Multilingual document set; each student brings own language pair | Plays to international student strength |
| 12 | Their semester's work + bill tracker harvest | Methodology + longitudinal payoff |

---

## The 12-week arc

### Week 1 — SQL via question families + DuckDB

**Core question:** How do you handle datasets too big for Excel but not worth the cloud?

**Dataset:** HMDA county slice.

**Skills:**
- DuckDB on big CSV (querying without loading into memory)
- Basic SQL: SELECT, WHERE, GROUP BY, simple JOIN
- The 12-question-families checklist as habit of mind
- AI as a SQL co-pilot: write the query, run it, sanity-check the number, push to pandas for analysis

**Deliverable:** Pose 5 questions across 5 different families about HMDA. For each: have AI write the query, run it, sanity-check the result, summarize the finding in 2-3 sentences.

**What students should be able to explain:** which question family each of their 5 belongs to and what story shape it implies.

**Readings:**
- Question-families primer (handout, written for this course)
- Investigations as exemplars per family — TBD compilation, two or three picks

**Quick win:** "You just queried 10 million rows on your laptop."

---

### Week 2 — JOINs, dirty data, and domain checking

**Core question:** When does an AI-generated query produce a confidently wrong answer?

**Dataset:** HMDA + lender info + Census tract demographics.

**Skills:**
- Multi-table JOINs (LEFT vs INNER, the silent collapse where WHERE on the right side turns LEFT into INNER)
- HMDA-specific domain pitfalls
- Reading and correcting AI-generated SQL

**Pedagogy hinge:** Instructor presents three AI-written HMDA queries with planted bugs from the gotcha bank below. Students run them, find the bugs, write up what domain knowledge would have caught each.

**Deliverable:** A redlining-style investigation question requiring a 3-table join. Verification required: row counts before/after, sanity checks against published CFPB totals, written explanation of any quirks.

**What students should be able to explain:** what domain knowledge would have caught each bug; what story would change if the bug had gone unnoticed.

**Readings:**
- Reveal "Kept Out" — use [Pulitzer finalists page](https://www.pulitzer.org/finalists/aaron-glantz-and-emmanuel-martinez-reveal-center-investigative-reporting-emeryville-calif) as the stable anchor
- [MLK50: "CFPB Moves to Limit Home Loan Data"](https://mlk50.com/2019/05/23/consumer-financial-protection-bureau-moves-to-limit-home-loan-data/) — what happens when journalism actually publishes findings

**HMDA gotcha bank (instructor reference for planted bugs):**

1. `action_taken = 3` only counts denials. HMDA codes: 1 originated, 2 approved-not-accepted, 3 denied, 4 withdrawn, 5 file-closed-incomplete, 6 purchased, 7 preapproval-denied, 8 preapproval-approved-not-accepted. Banks can pressure marginal applicants to withdraw, making raw "denied" rate look low while real adverse-outcome rate (3+4+5+7) is much higher. Reveal "Kept Out" thought through which codes counted as adverse.
2. `action_taken = 1` vs 6. Code 6 = purchased loan (bought from another lender's secondary market), not originated. Counting all action_taken codes double-counts and inflates loan-buying banks' apparent footprint.
3. Refinances swamp purchase analysis. `loan_purpose` distinguishes purchase, refinance, cash-out refi, etc. In low-rate years refis outnumber purchases 3:1. Same property reappears across years. "Lending in census tract X" without filtering by purpose measures churn (mostly white refis), not access.
4. Race is 5 columns + ethnicity column + parallel co-applicant fields. `WHERE race_1 = 'Black'` silently drops multi-race Black applicants. Hispanic ethnicity is separate from race. NCRC publishes an explicit derivation methodology.
5. Denial reasons under-counted. Up to 4 reasons reported; pre-2018 reporting was optional. Naive `GROUP BY denial_reason_1` shows only first-listed. Worse: Minneapolis Fed showed lender-reported reasons don't statistically explain racial disparities — so "Black applicants denied for 'credit history'" reports lender narrative, not finding.
6. The 2018 regulatory break. Pre-2018 HMDA has no credit score, DTI, LTV, interest rate, or term. From 2018 on, qualifying lenders report ~25 new fields. Coverage expanded (HELOCs, reverse mortgages, closed-end home-equity); reporting threshold moved (25 → 100 loans). 2016-vs-2019 trend lines are apples-to-oranges.
7. Geography NA values. Census-tract analyses break when `census_tract` is blank or NA — common for manufactured-home loans, multifamily, some purchased loans. `GROUP BY census_tract` quietly drops these. Tract boundaries also re-baseline each decennial census.

---

### Week 3 — Cloud as access

**Core question:** When does a dataset stop being yours and start needing to be shared?

**Dataset:** Continued HMDA work.

**Skills:**
- Datasette setup (student-managed self-hosted, or paid student accounts — TBD before semester)
- Backblaze B2 for documents and data files (S3-compatible, simpler/cheaper than AWS)
- Cost reasoning at order-of-magnitude
- Connection strings, permissions, read-only access

**Deliverable:** Publish HMDA exploration via Datasette to a partner playing "editor in DC." Create 5 saved queries an editor would actually use. Write a one-page OOM cost estimate for a hypothetical 6-month version with 5TB of documents and 3 reporters.

**What students should be able to explain:** when cloud makes sense (collaboration, public access, continuous processing) and when it doesn't (one-off, sensitive, budget); how to estimate costs without running the bill.

**Readings:**
- [ProPublica: "Heart of Nerd Darkness — Why the Dollars for Docs Data Was So Difficult"](https://www.propublica.org/nerds/heart-of-nerd-darkness-why-the-dollars-for-docs-data-is-so-difficult)
- [The Markup's Blacklight: How We Built a Real-Time Privacy Inspector](https://themarkup.org/blacklight/2020/09/22/how-we-built-a-real-time-privacy-inspector)
- [Simon Willison: How Datasette Helps With Investigative Reporting](https://www.newsroomrobots.com/p/how-datasette-helps-with-investigative)

**Cut from this week:** BigQuery (mention only). TablePlus (optional GUI note).

**Quick win:** "You just shared a database with zero configuration."

---

### Week 4 — Long-term tracking: build the bill scraper

**Core question:** How do you build infrastructure that watches a dataset for months while you're working on other things?

**Dataset:** Each student picks their own state legislative bill DB, with bill titles, summaries, statuses, text URLs, and full text when feasible.

**Skills:**
- GitHub Actions (cron-scheduled jobs, secrets management)
- Git-scraping pattern (Simon Willison's PG&E approach)
- Change detection vs noise
- Light search comparison over bill text: keyword/full-text vs semantic search in Datasette, with fuzzy search as a conceptual contrast
- Error notifications when scrapers break

**Deliverable:** Working scraper running daily, committing changes to git. A "what would make this break in 6 months" memo. In class, students use their bill table or an instructor demo to compare keyword/full-text and semantic search, using fuzzy search as the near-string comparison case. The scraper continues running through the rest of the semester.

**What students should be able to explain:** the difference between meaningful change and noise; what makes a scraper survive a year unattended; why fuzzy search handles spelling variation while semantic search handles different wording, and why neither is proof without reading the underlying bill.

**Readings:**
- [Simon Willison: "Git scraping: Track Changes Over Time"](https://simonwillison.net/2021/Mar/5/git-scraping/)
- [Simon Willison's PG&E Outages tracker](https://github.com/simonw/pge-outages) as the model (tens of thousands of commits)
- ProPublica's Nerd Guides: Design and Structure of a News Application; Data Style Guide; Data Bulletproofing Guide

**Quick win:** "Your scraper ran automatically while you slept."

---

### Week 5 — How data gets acquired and grows over time

**Core question:** Where does scale data come from, and what changes when you live with a dataset for a year?

**Framing:** Two threads connect naturally — data acquisition (FOIA, scraping, leaks, bulk public, build-it-yourself) and long-term tracking. Most scale acquisition involves *ongoing* relationships with data sources. Bill scrapers, recurring FOIAs, and tracker projects are the same shape.

**Skills:**
- Modes of data acquisition (FOIA basics, scraping, leaks, public bulk download, build-it-yourself)
- Long-term tracking craft
- Planning a longitudinal investigation

**Guest speaker:** A working journalist whose work involves both long-term tracking and FOIA — they pair naturally because long-term work usually means recurring records requests. Students prep questions in advance.

**Deliverable:** One-page memo, two parts:
1. "What could your bill scraper become if you ran it for a year?" — apply the question-families checklist to longitudinal questions.
2. "For the NYC investigation coming up in W6, where would you get the data and what would be hard about it?"

**Plus:** Interview writeup pulled forward from W7 — students arrange a 20-min interview with a working journalist on a related investigation; writeup due this week.

**Plus:** First methodology blurb (300 words) in their bill scraper repo README.

**What students should be able to explain:** the modes of data acquisition and which fits which kind of story; how a long-term tracker generates a story over time.

**Readings:**
- Simon Willison's PG&E tracker (revisit as longitudinal example)
- [WaPo Fatal Force methodology](https://www.washingtonpost.com/investigations/2022/12/05/washington-post-fatal-police-shootings-methodology/) — explicitly long-term, FOIA + scraping + ongoing local sourcing
- One FOIA-focused piece — TBD before semester (likely a MuckRock guide or a "how we got it" sidebar from a real investigation)

---

### Week 6 — OpenAleph: collaborative investigation infrastructure

**Core question:** How do hundreds of journalists across newsrooms search the same documents without stepping on each other?

**Dataset:** [OpenAleph public OCCRP instance](https://aleph.occrp.org/) — Companies House, sanctions lists, ICIJ Offshore Leaks entity graph.

**Note on terminology:** OCCRP is sunsetting active maintenance of Aleph after Dec 2025. [OpenAleph](https://openaleph.org/) is the going-forward fork. Functionally equivalent for what we're teaching; refer to OpenAleph in materials.

**Skills:**
- **FollowTheMoney (FtM) ontology** — `Person`, `Company`, `Directorship`, `Vessel`, `Payment`. Taught W6 day 1; everything else falls out of it.
- OpenAleph search and entity navigation
- Cross-dataset entity matching

**Deliverable:** Cross-reference a NYC-related name (or a name students bring) across Companies House, sanctions, and Offshore Leaks. Write a triage memo identifying any cross-dataset hits and their evidentiary value.

**Homework launched:** Each student arranges a 20-min interview with a journalist who worked on a related investigation. Writeup is due W5 (interviews can happen between W6 and W5; the *arrangement* is the W6-launched piece). The pull-forward spreads load across the semester.

**What students should be able to explain:** the FtM ontology and why it makes datasets joinable; what OpenAleph does that a SQL database doesn't.

**Readings:**
- [ICIJ Cyprus Confidential methodology](https://www.icij.org/investigations/cyprus-confidential/leaked-data-journalism-methodology/) — literally the workflow they're about to replicate
- "How ICIJ Made the Pandora Papers Searchable" (Pierre Romera ICIJ tech blog)
- OpenAleph homepage — see the actual tool

**Note:** ICIJ leak documents (Panama, Pandora, Paradise) are NOT public. Only entity graphs are. Plan around the network, not document review.

**Quick win:** "Cross-newsroom, cross-border, cross-language, cross-everything investigations."

---

### Week 7 — Self-hosted Aleph + ingest your own data

**Core question:** How do you make a pile of documents into a queryable entity graph?

**Dataset:** NYC building ownership — ACRIS deeds + HPD owner registrations (with the magic "head officer" field that pierces LLCs) + DOB BIS permits + NYS Department of State corporate filings.

**Skills:**
- Docker OpenAleph instance (instructor-run; students don't operate the server)
- FtM CSV mapping for ingestion (`ftm map`)
- Reviewing OpenAleph's automatic entity resolution

**Deliverable:** Pick a neighborhood (10-30 buildings). Identify shared head-officers / shared addresses across LLCs. Write a "who owns this block" memo, including a methodology section on how the data was ingested and what entity resolution caught and missed.

**What students should be able to explain:** how OpenAleph's built-in entity resolution worked on the data; where it succeeded and where it failed; what would make resolution stronger.

**Readings:**
- [ICIJ: "How ICIJ Deals with Massive Data Leaks" (Data Journalism Handbook chapter)](https://datajournalism.com/read/handbook/two/working-with-data/how-icij-deals-with-huge-data-dumps-like-the-panama-and-paradise-papers)
- [Mar Cabra: "How We Built the Data Team Behind the Panama Papers"](https://source.opennews.org/articles/how-we-built-data-team-behind-panama-papers/) (Source)
- [THE CITY: "How to Search Property Building Records Like a Reporter"](https://www.thecity.nyc/2023/10/13/how-to-search-property-building-records/)
- [JustFix's Who Owns What](https://whoownswhat.justfix.org/) — working open-source precedent

**Risk noted:** NYC data accessibility unverified at semester scale. See pre-semester checklist.

---

### Week 8 — Networks for investigations (Cypher, light week)

**Core question:** When does graph thinking beat relational thinking?

**Dataset:** NYC entities exported from Aleph + public ICIJ Offshore Leaks entity graph.

**Skills:**
- Neo4j Sandbox setup (no install)
- Cypher pattern matching: `MATCH-WHERE-RETURN`, shortest paths, basic centrality
- "Don't write 16 joins, write some Cypher"

**Deliverable:** Trace a path through the entity graph (e.g., NYC LLC → shared address → Companies House parent → Offshore Leaks officer). Write up: "if this connection is real, what story does it suggest, and what additional data would verify it?"

**What students should be able to explain:** when a graph beats a relational query for an investigation question; how to formulate a journalistic question as a Cypher pattern.

**Readings:**
- [ICIJ's Use of Graph Databases](https://neo4j.com/customer-stories/icij/) (Neo4j case study)
- [Linkurious and the Panama Papers](https://linkurious.com/blog/panama-papers-how-linkurious-enables-icij-to-investigate-the-massive-mossack-fonseca-leaks/)
- GIJN "How They Did It: Paradise Papers"

**Light week.** Don't teach Cypher beyond pattern matching basics. The week is about *thinking in networks*, not Cypher syntax.

**Quick win:** "Connecting the dots of people and companies."

---

### Week 9 — Build a baby Aleph: extraction + fuzzy matching + linking

**Core question:** When OpenAleph isn't available (no licensing, sensitive data, smaller-scale work), how do you build the same capabilities yourself?

**Dataset:** Subset of W7 NYC docs with known entity duplicates, OR a curated test set with held-back ground truth.

**Skills:**
- AI extraction (Anthropic / OpenAI structured outputs) for entity recognition
- rapidfuzz / dedupe for fuzzy matching (alternative: splink for record linkage)
- Cross-dataset linking with confidence scoring

**Deliverable:** Comparison exercise. Take the same entity resolution task and run it three ways:
1. OpenAleph's built-in result from W7
2. A fuzzy-matching library (rapidfuzz or dedupe)
3. AI structured outputs

Measure all three against ground-truth subset. Write up which performed better, where each failed, and why. Include a recommendation: which approach for which kind of investigation.

**What students should be able to explain:** when each approach wins; what the failure modes look like in real data; how you'd combine them.

**Readings:**
- rapidfuzz / dedupe library documentation
- [Pandora Papers: Technology Behind the Investigation](https://linkurious.com/blog/technology-pandora-papers-investigation/) — for context on what big tools do
- A recent piece on entity resolution in journalism — TBD (likely ICIJ or OCCRP tech blog)

---

### Week 10 — Public-facing tools + security + privacy + methodology

**Core question:** How do you let readers explore your investigation without exposing them or yourself?

**Dataset:** Student picks from earlier work — HMDA, bill tracker, NYC Aleph results, or W11 extraction (depending on order).

**Skills:**
- Flask + SQLite (read-only) + Render deployment
- Security failure modes: SQL injection, exposed write credentials, PII leakage, what happens under load
- Privacy and harm reduction: doxxing, harassment, the difference between "public record" and "publicly searchable"
- Methodology-as-journalism: the methodology page linked from the tool

**Deliverable (three parts):**
1. Deployed tool with search, pagination, CSV download
2. **Security audit document** — where could this leak data, where could it be SQL-injected, what happens under load, what PII shouldn't be exposed and why
3. **Methodology page** linked from the tool — provenance, choices made and rejected, known limitations, what would change the conclusion

**What students should be able to explain:** what their tool exposes if they got something wrong; the choices they made about what to publish vs withhold and why.

**Readings:**
- [ProPublica: "How We Analyzed the COMPAS Recidivism Algorithm"](https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm) — gold standard for algorithm/data audit methodology
- [The Markup's "Show Your Work" series](https://themarkup.org/series/show-your-work)
- [ProPublica's News App Guides (Nerd Guides)](https://www.propublica.org/nerds/propublicas-news-app-guides)
- Reference implementations: [ProPublica's Nonprofit Explorer](https://projects.propublica.org/nonprofits/), [Texas Tribune Salaries](https://salaries.texastribune.org/)

**Cut from this week:** the "second Flask week" of pagination + CSV export (folds into the deploy + security work). API endpoints for other newsrooms (finished-early extension, not core).

**Quick win:** "Your investigation tool is live on the internet."

---

### Week 11 — AI extraction at depth + multilingual

**Core question:** When AI reads documents for you, how do you know what it got right and wrong?

**Dataset:** Multilingual document set; international students bring their own language pair.

**Skills:**
- Anthropic / OpenAI structured outputs for entity extraction
- Multilingual extraction (cross-lingual entity recognition)
- **Collaborative verification** — each student verifies 20 docs from a shared 100-doc set against a held-back instructor "ground truth." Class leaderboard for both extraction quality AND verification quality.
- Cost analysis (Sonnet vs Haiku on the same task)

**Deliverable:** Extraction + measured error rate against the ground-truth subset + cost analysis writeup. The collaborative verification turns "verify your work" from an AI-fakeable solo task into a peer-checked competitive exercise.

**What students should be able to explain:** what AI got confidently wrong; how to design a verification workflow that scales; the cost-quality tradeoff at order-of-magnitude.

**Readings:**
- [Quartz: "How Quartz used AI to sort through the Luanda Leaks"](https://qz.com/1786896/ai-for-investigations-sorting-through-the-luanda-leaks) (Jeremy B. Merrill) — sentence-level vector embeddings + Annoy nearest-neighbor + Apertium Portuguese MT
- [ICIJ: "How We Mined More Than 715,000 Luanda Leaks Records"](https://www.icij.org/investigations/luanda-leaks/how-we-mined-more-than-715000-luanda-leaks-records/)
- [Simon Willison: "AI for Data Journalism"](https://simonw.substack.com/p/ai-for-data-journalism-demonstrating) (Apr 2024)
- [BuzzFeed: How BuzzFeed News Analyzed the FinCEN Files](https://www.buzzfeednews.com/article/jsvine/fincen-files-explainer-data-money-transactions)

**Cut as taught topics:** Haystack (mention only), LM Studio (mention), NotebookLM (mention).

**Quick win:** "AI just read 100 documents in 30 seconds — and you know which ones it got wrong."

---

### Week 12 — Methodology article + bill tracker harvest + sustainability

**Core question:** What does it mean to ship the work — not just the tool, but the writeup?

**Datasets:**
- Their bill tracker (now with 7-8 weeks of accumulated data)
- Their semester's work

**Skills:**
- Methodology-as-journalism (publication-quality, not documentation)
- Longitudinal analysis (what 7-8 weeks of bill tracking tells you that a snapshot doesn't)
- Sustainability planning: scheduled updates, alerting, handoff documentation

**Deliverable (three parts):**
1. **Publication-quality methodology article** on one project from earlier in the semester. Choose the strongest candidate; read three real ones first; write to publish.
2. **Bill tracker findings memo** — "I've watched X for 8 weeks; here's what changed and what story it suggests."
3. **Sustainability plan** — scheduled-update plan + a one-page "what would I do differently" reflection.

**What students should be able to explain:** what makes a methodology article journalism rather than documentation; what their longitudinal data shows that a snapshot wouldn't.

**Readings:**
- [Washington Post Fatal Force methodology](https://www.washingtonpost.com/investigations/2022/12/05/washington-post-fatal-police-shootings-methodology/) — canonical model for "how we collected this"
- Stalph & Borges-Rey, "Data Journalism Sustainability," *Digital Journalism* 6(8), 2018 — paywalled, available via Columbia Libraries
- [AP Datakit](https://github.com/associatedpress/datakit-core) (datakit-core repo + plugins)
- The Markup's "Show Your Work" series (revisit)

**This is the longitudinal payoff for W4's scraper.**

**Quick win:** "You shipped a tool, a story, and a methodology — and your scraper is still running."

---

## Tools — taught, mentioned, cut

### Taught (students use directly)

| Tool | Weeks | Role |
|---|---|---|
| DuckDB | W1-2 | SQL on big files, CSV-as-database |
| PostgreSQL | W2-3 | When persistence/sharing matters |
| Datasette | W3, W4, W6, W7, W10 | SQL exploration, sharing, search over bill text, instant publishing |
| Backblaze B2 | W3 | S3-compatible object storage |
| GitHub Actions | W4, W12 | Scheduled scrapers, automation |
| OpenAleph | W6, W7, W9 | Entity-graph platform; FtM ontology |
| Neo4j + Cypher | W8 | Graph database, network analysis |
| rapidfuzz / dedupe | W9 | Fuzzy matching, entity resolution |
| datasette-embeddings | W4 | Lightweight semantic search over bill text |
| Anthropic / OpenAI structured outputs | W9, W11 | AI extraction |
| Flask + SQLite | W10 | Read-only public tools |
| Render | W10 | Deployment |

### Mentioned (concept exposure, not taught)

- BigQuery (W3) — "this exists, here's when you'd reach for it"
- TablePlus (W3) — optional GUI for Postgres
- DocumentCloud (W6-7) — when you'd use it instead of Aleph
- LM Studio (W11) — for sensitive documents
- Haystack (W11) — RAG patterns
- NotebookLM (W11) — exploration with citations
- pgvector (W11) — embeddings in Postgres if ambitious
- OpenCorporates (W6) — cautionary tale: was free, now isn't (£2,250+/yr)
- xsv / miller / csvkit / jq / ripgrep — bonus appendix

### Cut from prior curriculum

- BigQuery as a taught tool (bill-running risk)
- PostGIS (covered better in concurrent mapping class)
- Heroku (Render replaces; free tier dead)
- Capstone Project Prep section (capstone is concurrent and unlikely to be database-heavy)
- Collaborative git workflows (students learn this on the job)
- Bonus command-line tools as a taught block

---

## Assessment

Each week produces a deliverable. Three-tier structure (Foundation / Extension / Innovation) where it makes pedagogical sense; otherwise a single deliverable.

Recurring deliverables (not tier-based):
- Methodology blurbs (W5, W7) → full methodology page (W10) → publication-quality methodology article (W12)
- Bill scraper running W4 onward; harvest W12

Final = the W12 three-part deliverable. No semester-long single investigation; the body of weekly work IS the portfolio.

Writing prompts focus on journalistic applications, not technical description. Critical thinking about limitations and possibilities matters more than technical exhaustion.

---

## Pre-semester checklist for instructor

Before the semester starts:

1. **OpenAleph instance.** Stand up a self-hosted OpenAleph (Docker) for W7 ingestion. Verify ingestion works with a test FtM CSV before W6.
2. **NYC data acquisition test.** Verify ACRIS / HPD / DOB / NYS DOS data can be acquired at semester scale. Risk: bulk download is harder than expected and a week is burned on data wrangling. Have a fallback dataset ready.
3. **HMDA county slice.** Pre-download HMDA + lender + Census tract. Verify the planted-bug queries work and reproduce the gotcha pedagogy.
4. **Datasette setup decision.** Confirm whether students self-host, use paid Datasette Cloud accounts, or use an instructor cloud instance. Affects W3 and W10 instructions.
5. **Guest speaker (W5).** Recruit a journalist whose work involves both long-term tracking AND FOIA. Confirm scheduling.
6. **Multilingual document set (W11).** Curate a 100-document multilingual set with held-back ground truth for collaborative verification.
7. **Bill scraper safety nets.** Identify 3 state legislative DBs known to scrape cleanly, in case students pick something that won't.
8. **Reading verification.** Re-verify methodology URLs the week before they're assigned (URLs rot).

---

## Open items going into B (rewrite)

These are the smaller decisions still pending; B can proceed with reasonable defaults and the instructor can adjust:

- **W5 FOIA reading** — pick a specific piece (MuckRock guide or "how we got it" sidebar from a real investigation)
- **Datasette Cloud setup mode** — confirm before tech-stack.md is rewritten
- **Window functions** — drop, brief explainer, or keep as nice-to-have
- **Question-families primer (W1 handout)** — needs writing
- **Investigations-per-question-family compilation (W1 reading)** — needs assembling

---

## What changes in the repo files when B runs

| File | Action |
|---|---|
| README.md | Rewrite: "third semester," 12 weeks, link to v3 arc, drop the "haven't verified investigations" caveat (we've now verified) |
| curriculum.md | Replace with the 12-week arc above |
| assignments.md | Replace with the deliverable spec for each week (Foundation/Extension/Innovation where applicable) |
| tech-stack.md | Update: OpenAleph not Aleph, drop BigQuery as taught, drop Heroku, fix DuckDB doc URLs |
| readings.md | Apply readings-sweep.md fixes — KEEP the verified, FIX the broken descriptions, CUT the dead, ADD the 8 new |
| speed-run.md | Update: 12 weeks not 14, Render not Heroku, OpenAleph not Aleph, current LLM model names |
| CLAUDE.md | Already updated. Stable. |
