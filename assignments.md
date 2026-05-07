# Data and Databases: Assignments

12-week assignment structure. Three-tier exercises (Foundation / Extension / Innovation) where it makes pedagogical sense; a single multi-part deliverable where it doesn't. Each week includes technical work and written reflection — methodology-as-journalism is a recurring deliverable, not a final-week add-on.

## For Instructors

Each assignment includes:
- **Instructor Provides:** materials/templates with low-prep and high-prep options
- **Common Issues:** problems to anticipate
- **Writing Prompt:** reflection focused on journalistic applications

Three-tier structure where applicable:
- **Foundation** is required and addresses the core question
- **Extension** is recommended and shows deeper understanding
- **Innovation** is optional and creative

## Week 1: SQL via Question Families + DuckDB
[Curriculum](curriculum.md#week-1-sql-via-question-families--duckdb) | [Tech stack](tech-stack.md#databases)

### Instructor Provides
- **Low-prep:** Links to PPP / HMDA data, the question-families primer handout
- **High-prep:** Pre-downloaded county-scale HMDA slice, DuckDB tutorial notebook with worked examples per question family

### Foundation: Five Questions, Five Families
**Dataset:** HMDA county slice (or PPP loans for one state, instructor's choice)

Pose five investigation-shaped questions about the dataset, drawn from five *different* question families. For each:
1. State the question and which family it belongs to
2. Write the SQL query in DuckDB
3. Run it; sanity-check the result against domain knowledge
4. Summarize the finding in 2-3 sentences and the story shape it implies

**Writing Prompt** (300 words): "Of your five questions, which family produced the most interesting story candidate? Which family produced the most boring one? What does that tell you about how to approach a new dataset?"

### Extension: SQL → pandas Workflow
Demonstrate the right tool for each step:
- Filter and aggregate in DuckDB SQL
- Pull results into pandas for visualization or further computation
- Document the boundary — at what point did you switch and why?

### Innovation: A Sixth Family
Find a question that fits one of the question families *not* already covered in your five. Write it up; comment on why this family is harder to ask of HMDA than the others.

**Common Issues:** memory errors when pandas-loading the full file; SQL syntax confusion when AI's query references columns that don't exist in this slice; conflating questions ("what's the average?" is two families collapsed).

---

## Week 2: JOINs, Dirty Data, and Domain Checking
[Curriculum](curriculum.md#week-2-joins-dirty-data-and-domain-checking) | [Tech stack](tech-stack.md#databases)

### Instructor Provides
- **Low-prep:** The HMDA gotcha bank (in curriculum.md), three planted-bug queries to share in class
- **High-prep:** Pre-loaded HMDA + lender + Census tract dataset; Reveal "Kept Out" methodology summary; in-class buggy-query worksheet

### Foundation: Find the Bug + Build the Right Query
**Part 1 — Find the bug.** Three AI-written HMDA queries are presented in class. Each contains a planted domain bug. Run all three; for each, identify what's wrong, what story it would distort, and what domain knowledge would have caught it.

**Part 2 — Build the right query.** Construct a redlining-style investigation question requiring a 3-table JOIN (HMDA + lender + Census tract demographics). Verify your work: row counts before/after, sanity check against published CFPB totals, written explanation of any quirks.

**Writing Prompt** (300 words): "Which of the three planted bugs scared you the most — meaning, which one are you most likely to make on your own work? Why?"

### Extension: Your Own Gotcha
Find an additional HMDA gotcha not in the gotcha bank. Write it up the way the bank entries are written: the wrong query, what it would falsely conclude, what catches it.

### Innovation: Method Comparison
Run your investigation question two ways — your hand-written query and an AI-generated one. Compare results, query style, and which version you'd trust to publish.

**Common Issues:** LEFT JOIN that silently becomes INNER because of a WHERE clause on the right side; row counts dropping after a JOIN without students noticing; AI hallucinating column names that don't exist in this version of HMDA.

---

## Week 3: Cloud as Access
[Curriculum](curriculum.md#week-3-cloud-as-access) | [Tech stack](tech-stack.md#cloud-infrastructure)

### Instructor Provides
- **Low-prep:** Datasette setup tutorial, Backblaze B2 trial credit, sample editor-in-DC scenario
- **High-prep:** Pre-configured Datasette instance for class fallback, Backblaze bucket with sample document collection

### Foundation: Investigation Infrastructure
1. Publish your HMDA exploration via Datasette so a partner playing "editor in DC" can query it
2. Create five saved queries an editor would actually use — each with a clear name and a sentence of context
3. Upload 20+ investigation documents to Backblaze B2 with a sensible naming convention
4. Hand off all three (Datasette URL, B2 bucket, sample queries) to your partner

**Writing Prompt** (300 words): "Your editor is in DC, you're in NYC, and a source in Miami just sent documents. How does cloud infrastructure change this investigation? What couldn't you do before?"

### Extension: Order-of-Magnitude Cost Estimate
Calculate the actual costs for a 6-month investigation with 5TB of documents, 3 reporters, and continuous Datasette access. Show your work — bandwidth, storage, query, and compute. Don't aim for precision; aim for the right order of magnitude. Note where you'd cut if the budget were halved.

### Innovation: Multi-User Database
Set up a shared Postgres for a hypothetical team investigation. Create read-only access for fact-checkers, write access for reporters. Document the permission model.

**Common Issues:** read vs write permission confusion; B2 bucket naming restrictions; Datasette publishing rate limits; cost estimates that round to free.

---

## Week 4: Long-term Tracking — Build the Bill Scraper
[Curriculum](curriculum.md#week-4-long-term-tracking--build-the-bill-scraper) | [Tech stack](tech-stack.md#automation--workflows)

### Instructor Provides
- **Low-prep:** GitHub Actions starter template, list of 3 state legislative DBs known to scrape cleanly with bill titles, summaries, statuses, and text URLs
- **High-prep:** A working scraper example with change detection, alert system, error notification, and a small Datasette semantic-search demo over bill text

### Foundation: Your State Legislative Bill Scraper
Pick a state legislative bill database. Build a GitHub Actions workflow that:
1. Scrapes the database daily (or on whatever cadence makes sense)
2. Stores each day's snapshot in git, including enough text for search: title, summary, status, bill text URL, and full text when feasible
3. Notifies you (issue, email, or other) if the scraper breaks

The scraper continues running for the rest of the semester. Week 12 is the harvest.

**Writing Prompt** (300 words): "What about your chosen database is most likely to change in interesting ways over a semester? What would change in *boring* ways? How will you tell them apart?"

### In-Class Mini: Search Beyond Exact Match
Using your latest bill table in Datasette — or the instructor's demo table if your scraper is not ready yet — compare keyword/full-text search and semantic search. Use fuzzy search as the contrast case: spelling variation vs different wording. Search for a policy idea or description without requiring the exact words to appear. Write down:
- The query you tried
- One result that exact or full-text search found
- One result that semantic search found
- Which result you would actually inspect first as a reporter, and why

The point is vocabulary and judgment, not building a perfect search engine.

### Extension: Smart Change Detection
Detect meaningful changes (not just timestamps). Build a diff system that surfaces additions, deletions, and meaningful modifications. Generate a daily changelog.

### Innovation: Multi-source Coordination
Track a second related source alongside your primary. Surface coordinated changes across both — when a bill changes status AND a related agency announcement changes, that's a story.

**Common Issues:** sites that block GitHub Actions IPs; dynamic content that changes every load (timestamps, CSRF tokens) creating noise; rate-limit blocking; secrets management mistakes; treating a semantic-search hit as evidence without reading the text.

---

## Week 5: How Data Gets Acquired and Grows Over Time
[Curriculum](curriculum.md#week-5-how-data-gets-acquired-and-grows-over-time) | [Readings](readings.md#week-5-how-data-gets-acquired-and-grows-over-time)

### Instructor Provides
- **Low-prep:** Reading list, FOIA reading suggestions, journalist interview question template
- **High-prep:** Confirmed guest speaker (long-term tracking + FOIA journalist), in-class workshop materials

### Foundation: The Acquisition + Longitudinal Memo
A single one-page memo with two parts:

**Part 1: "What could your bill scraper become if you ran it for a year?"** Apply the question-families checklist to longitudinal questions specifically. What story shapes are only visible over time? What's the minimum amount of accumulated data you'd need before publishing?

**Part 2: "For the NYC investigation coming up in Week 6, where would you get the data and what would be hard about it?"** Identify likely sources (FOIA, public bulk, scraping, leaks). Anticipate friction.

**Plus:** Submit your interview writeup. By now you've completed your 20-min interview with a working journalist on a related investigation — write it up as a 300-word "a day in this investigation" piece focusing on what was tedious, what broke, and what was the actual journalism.

**Plus:** First methodology blurb (300 words) in your bill scraper repo README. This is the first taste of methodology-as-journalism that builds across W7, W10, and W12.

**Guest speaker session:** A working journalist whose work involves long-term tracking AND FOIA. Prep questions in advance.

### Extension: Records Request Draft
Draft an actual FOIA request for the most-useful data you identified. Include scope, format, and reasonable response time.

### Innovation: Cross-Tracker Story
Identify a second tracker (yours or someone else's) whose data could combine with your bill scraper to surface a story neither could alone. Write up the hypothetical.

**Common Issues:** memos that describe what the scraper *currently shows* rather than what it *could* show; interview writeups that summarize the journalist's career rather than the specific investigation's workflow.

---

## Week 6: OpenAleph — Collaborative Investigation Infrastructure
[Curriculum](curriculum.md#week-6-openaleph--collaborative-investigation-infrastructure) | [Tech stack](tech-stack.md#document-processing--investigation)

### Instructor Provides
- **Low-prep:** OpenAleph public instance walkthrough, FtM ontology cheat sheet, target-name list for cross-referencing
- **High-prep:** Curated NYC name list with known cross-dataset hits; pre-prepared FtM mapping examples

### Foundation: Cross-Dataset Triage Memo
Pick a NYC-related entity (politician, real-estate developer, contractor, or your own choice). Cross-reference the name across:
1. Companies House (UK) on the public OpenAleph instance
2. Sanctions lists (OFAC, EU, UK Treasury) on OpenAleph
3. ICIJ Offshore Leaks entity graph

Write a triage memo identifying any cross-dataset hits, their evidentiary value, and what additional verification would strengthen each lead.

**Writing Prompt** (300 words): "What's the difference between an entity hit and a story? What would convince you a cross-dataset connection is real and worth pursuing?"

### Extension: FtM Sketch
Sketch out an FtM-style schema for a small set of entities you care about (could be your own data, could be NYC building data). Identify which FtM types you'd use for each. Show one cross-source link the schema enables.

### Innovation: Public-Aleph Tour Guide
Pick three OpenAleph features non-obvious to a new user. Write a short tour guide a fellow journalist could follow to find their first cross-dataset hit.

**Homework launched:** Each student arranges a 20-min interview with a working journalist on a related investigation. The writeup is due Week 5 (yes, that's in the past from this week's perspective — the interview can happen between Week 6 and Week 5 calendar-wise, but the *arrangement* happens here. The pull-forward spreads load across the semester.)

**Common Issues:** searching for entities that don't exist in any dataset (false zero); confusing OpenAleph's automated entity resolution with verified identity; assuming that an entity hit *is* the story.

---

## Week 7: Self-hosted Aleph + Ingest Your Own Data
[Curriculum](curriculum.md#week-7-self-hosted-aleph--ingest-your-own-data) | [Tech stack](tech-stack.md#document-processing--investigation)

### Instructor Provides
- **Low-prep:** Pre-running OpenAleph Docker instance, ACRIS + HPD sample data already in FtM-mappable form, FtM mapping templates
- **High-prep:** Per-student access to the instance, larger neighborhood-scale dataset, JustFix's Who Owns What as a reference target

### Foundation: Who Owns This Block
Pick a NYC neighborhood (10-30 buildings). Ingest the relevant ACRIS deeds and HPD owner registrations into the class OpenAleph instance using FtM CSV mapping. Then:
1. Identify shared head-officers across LLCs in your block
2. Identify shared addresses (mailing addresses, registered agent addresses)
3. Trace at least one LLC chain to a likely human owner
4. Write a "who owns this block" memo of 500-800 words

**Methodology section (required):** Document how you ingested the data, what FtM types you used, where OpenAleph's automatic entity resolution succeeded, and where it failed. This is methodology-as-journalism — write it the way you'd write the methodology page that runs alongside the eventual story.

### Extension: Cross-Borough Comparison
Pick a comparable block in a different borough. Repeat the analysis. Compare the ownership patterns. Hypothesize what's driving any differences.

### Innovation: Linked Investigation
Connect your block-level ownership findings to W6's cross-dataset work. Are any of your head-officers in Companies House, sanctions, or Offshore Leaks?

**Common Issues:** ACRIS deed scans that don't OCR cleanly; HPD registrations missing the head-officer field; FtM mapping errors that silently drop rows; declaring entity resolution "right" without spot-checking.

---

## Week 8: Networks for Investigations (Cypher)
[Curriculum](curriculum.md#week-8-networks-for-investigations-cypher) | [Tech stack](tech-stack.md#databases)

### Instructor Provides
- **Low-prep:** Neo4j Sandbox account links, sample entity export from Week 7, Cypher pattern library
- **High-prep:** Pre-loaded graph database with NYC entities + ICIJ Offshore Leaks subset

### Foundation: Trace the Path
Load entity data into Neo4j Sandbox (your W7 export plus the ICIJ Offshore Leaks public entity graph). Pose one investigation question that's graph-shaped — typically a path between two entities through some number of intermediate connections. Then:
1. Express the question as a Cypher pattern
2. Run it; collect the matching paths
3. Write up: "if this connection is real, what story does it suggest, and what additional data would verify it?"

**Writing Prompt** (300 words): "Which of your paths feels strongest? Which feels coincidental? What's the difference?"

### Extension: Centrality Ranking
Compute centrality metrics for the entities in your graph. Identify the most-connected nodes. Hypothesize what centrality means in journalistic terms — does most-connected mean most-powerful, most-public, or just most-recorded?

### Innovation: Visualize for an Editor
Build a graph visualization an editor with no graph experience could read. Don't aim for completeness; aim for legibility. Caption everything.

**Common Issues:** Cypher syntax confusion with relationship directionality; treating all edges as equal when they shouldn't be (a directorship is not a payment); over-claiming evidence from a structural pattern.

---

## Week 9: Build a Baby Aleph — Extraction + Fuzzy Matching + Linking
[Curriculum](curriculum.md#week-9-build-a-baby-aleph--extraction--fuzzy-matching--linking) | [Tech stack](tech-stack.md#ai--machine-learning)

### Instructor Provides
- **Low-prep:** Subset of W7 NYC docs with known entity duplicates and ground-truth labels
- **High-prep:** Curated test set with held-back labels; rapidfuzz/dedupe starter code; Anthropic API key with credits

### Foundation: Three-Way Comparison
Take the test document set. Run entity resolution three ways:
1. OpenAleph's built-in result from W7
2. A fuzzy-matching library (rapidfuzz or dedupe)
3. AI structured outputs (Anthropic / OpenAI)

For each, measure precision, recall, and F1 against the held-back ground truth. Write up:
- Where each approach won
- Where each approach failed (specific failure cases)
- Which approach you'd use for which kind of investigation, and why
- What a hybrid approach might look like

**Writing Prompt** (300 words): "AI loses to a simple fuzzy-matching library on at least one of these tasks. Which one, and what does that tell you about when not to reach for AI?"

### Extension: Adversarial Cases
Add five adversarial cases to the test set — entities designed to fool one of the three approaches. Re-run. Show how the failure modes shift.

### Innovation: Hybrid Pipeline
Build a hybrid system that uses two approaches together. Show that it beats any single approach on at least one of precision, recall, or speed.

**Common Issues:** ground-truth that isn't actually ground truth (instructor mistakes); over-fitting to the test set; reporting a single accuracy number when precision and recall tell different stories.

---

## Week 10: Public-Facing Tools + Security + Privacy + Methodology
[Curriculum](curriculum.md#week-10-public-facing-tools--security--privacy--methodology) | [Tech stack](tech-stack.md#data-tools--publishing)

### Instructor Provides
- **Low-prep:** Flask + SQLite starter template, Render account, deployment guide
- **High-prep:** Complete frontend scaffold, security audit checklist, methodology page template

### Foundation: Three-Part Public Tool
Pick a dataset from your earlier semester work — HMDA, your bill tracker, your NYC Aleph results. Build and deploy a public-facing tool with three components:

**1. The tool.** Flask + SQLite (read-only) + Render. Search, pagination, CSV export. Designed for a non-technical user — test with one before submission.

**2. The security audit document.** A 500-word writeup answering:
- Where could this leak data?
- Where could it be SQL-injected?
- What happens under load?
- What PII shouldn't be exposed and why?

**3. The methodology page.** Linked from the tool itself. Provenance, choices made and rejected, known limitations, what would change the conclusion. Read three real methodology pages first (see readings).

**Writing Prompt** (300 words): "A reader used your tool to find their employer taking questionable payments. They want the raw data. What do you provide? What are the ethical considerations?"

### Extension: Power-User Features
Add advanced filters that real users would need. Add a download endpoint with documentation. Make the methodology page interactive — let readers see how filtering changes counts.

### Innovation: Story Leads
Add a feature that surfaces potential story leads automatically — patterns, outliers, or combinations a reader wouldn't think to search for. Document why this isn't editorializing.

**Common Issues:** production database credentials exposed in repo; SQL injection via search box; the tool crashing under modest load; methodology pages that read like documentation, not journalism.

---

## Week 11: AI Extraction at Depth + Multilingual
[Curriculum](curriculum.md#week-11-ai-extraction-at-depth--multilingual) | [Tech stack](tech-stack.md#ai--machine-learning)

### Instructor Provides
- **Low-prep:** Anthropic API key with credits, multilingual document set, structured-output examples
- **High-prep:** Curated 100-document multilingual set with held-back ground truth; verification rubric

### Foundation: Collaborative Verification Race
Each student verifies 20 documents from a shared 100-doc set against held-back instructor "ground truth." The 100 docs are multilingual — each student picks a language pair appropriate to their background.

**Three deliverables:**
1. Your structured-output extraction across 20 docs
2. Your measured precision/recall against ground truth
3. A cost analysis comparing Sonnet and Haiku on the same task — quality gap and cost gap

**Class leaderboard:** Two rankings — one for extraction quality, one for verification quality.

**Writing Prompt** (300 words): "What kind of error did the AI make most often on documents in your language pair? What would catch it before publication?"

### Extension: Multilingual Cross-Reference
Pick an entity that appears in documents across multiple languages. Verify it's the same entity across languages — what differs in how the AI represents it? What rules would you write to canonicalize?

### Innovation: Beat-Specific Tool
Customize the prompts and verification for your beat (housing, healthcare, courts, immigration, etc.). Show why beat-specific prompting beats generic.

**Common Issues:** API rate limits; cost runaway from over-aggressive batching; verification by AI ("self-grading") inflating accuracy; overlooking systematic errors that the leaderboard would surface.

---

## Week 12: Methodology Article + Bill Tracker Harvest + Sustainability
[Curriculum](curriculum.md#week-12-methodology-article--bill-tracker-harvest--sustainability) | [Readings](readings.md#week-12-methodology-article--bill-tracker-harvest--sustainability)

### Instructor Provides
- **Low-prep:** Three real methodology articles (WaPo Fatal Force, ProPublica COMPAS, ICIJ Cyprus Confidential), AP Datakit reference, methodology rubric
- **High-prep:** Per-student feedback rubric with publication-grade criteria; pre-arranged peer review

### Foundation: Three-Part Final
A single, multi-part deliverable. Not three-tier — one substantive package.

**1. Methodology article (publication-quality).** Pick the strongest project from earlier in the semester. Read three real methodology articles. Write yours to the same standard — provenance, choices, limitations, what would change the conclusion. 1,000-1,500 words. This is the final exam, written as journalism.

**2. Bill tracker findings memo.** "I've watched X for 8 weeks. Here's what changed, here's what's a story, and here's what's noise." Include a longitudinal chart and at least one specific story candidate.

**3. Sustainability plan + reflection.** A scheduled-update plan for one of your projects (which one would you actually maintain?). Plus a one-page reflection: "What would I do differently if I started this semester over?"

**Writing Prompt:** Folded into the methodology article itself.

### Extension: Production-Ready
Take one of your tools (W4 scraper, W7 Aleph workflow, W10 Flask app) to production-ready. Add error handling, monitoring, and a simple admin interface. Document the runbook for whoever inherits it.

### Innovation: Newsroom Toolkit
Package your strongest work as a reusable template another newsroom could adapt. Include a setup guide and example datasets.

**Common Issues:** methodology articles that read like documentation (steps and code) rather than journalism (decisions and rationale); harvest memos that report change without identifying the story; sustainability plans that promise more maintenance than anyone will actually do.

---

## Assessment Structure

### Technical Work
- **Foundation:** Must work and address the core question
- **Extension:** Shows deeper understanding
- **Innovation:** Creative application

### Written Reflections
- Weekly 300-500 word reflections
- Focus on journalistic applications, not technical description
- Critical thinking about limitations and possibilities

### Methodology-as-Journalism (recurring)
- Week 5: 300-word blurb in scraper README
- Week 7: Aleph investigation methodology memo
- Week 10: Full methodology page linked from public tool
- Week 12: Publication-quality methodology article (the final)

## Tips for Instructors

### Managing Different Skill Levels
Extension work can replace Foundation for advanced students. Innovation tier is genuinely optional — don't let it become a backdoor requirement.

### Pre-semester Tasks
See `instructor-notes.md` for the pre-semester checklist (OpenAleph instance, NYC data acquisition test, HMDA pre-download, guest speaker recruitment, multilingual document set curation).

## Resources
- [Curriculum](curriculum.md) — week-by-week overview
- [Tech stack](tech-stack.md) — all tools documented
- [Readings](readings.md) — context and examples
- [Speed-run guide](speed-run.md) — for catching up
