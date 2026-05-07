# Database Curriculum Revamp — Working Design Notes

**Last updated:** 2026-05-05
**Status:** mid-design iteration; pre-rewrite of repo files
**Purpose:** capture decisions, reasoning, rejections, and research so we don't lose nuance to context compaction before the actual rewrite of curriculum.md / assignments.md / readings.md / tech-stack.md / speed-run.md.

---

## TL;DR

Course is "Data and Databases," 3rd semester, 12 weeks, Columbia Journalism School Data Journalism MS. We're revamping a 1st-semester scraping + regex course into a 3rd-semester databases-and-investigation course. AI tooling now covers most scraping, so that's no longer the spine. The bar: teach things students wouldn't pick up at a typical newsroom data desk.

The repo currently contains stale 14-week / 2nd-semester docs. CLAUDE.md was updated this session to reflect 12 weeks / 3rd semester / current premises. All other docs need rewriting from the design captured here.

Three options for next step (user has not picked yet):
- **C** — sweep readings.md for more hallucinations (one already confirmed)
- **A** — produce a v3 syllabus document as canonical reference
- **B** — rewrite the actual repo files (curriculum, assignments, readings, tech-stack, speed-run)

Recommended order if doing all three: C → A → B.

---

## Course identity

- **12 weeks** (was 14)
- **3rd semester** (was 2nd)
- **Concurrent classes:** thesis + a mapping class (was: stats, data-driven projects, multiple reporting classes)
- **Capstone:** runs the same semester, separately. Unlikely to involve heavy database work.
- **Class load assumption:** treat as a normal class load. Don't optimize for spare cycles. Don't worry about outside-class time budget.

### Student prerequisites (real, from instructor-confirmed interview)

Comfortable with:
- Python, pandas, CSV/JSON/APIs
- Basic scraping (BeautifulSoup-level)
- Command line basics
- Static web publishing with GitHub Pages
- HTML/CSS fundamentals
- AI coding tools (Claude Code, Cursor, etc.) — they reach for AI by default

Have shipped:
- Small Flask apps with AI help — but have NOT actually learned Flask, backend, deployment, databases, APIs, or production workflows. They guided AI through them.

Have NOT:
- Touched SQL or any database
- Collaborated on git beyond solo commits

---

## The bar (editorial test for every weekly decision)

Two questions frame every design choice:

1. **Does this teach something they wouldn't pick up at a typical data desk?** Anchor on what an *exceptional* desk does (Reveal/ProPublica/ICIJ/THE CITY tier), not on what's standard.
2. **Does this week answer "what investigation does this unlock?"** — not "what tool does this teach?" The original course's failure mode was a tool tour. The revamp must avoid this.

If a week answers neither, it's a candidate for cut or upgrade.

---

## Working premises (open to challenge during design; some have been challenged and resolved)

- **AI is woven through the course, not isolated.** Students use AI by default. Course emphasis: judgment, verification, schema reasoning, query reading, provenance, explaining what generated code does — not rote syntax.
- **Exception:** a dedicated AI-extraction unit is fine, because it's specifically about *AI-as-data-transformation-tool* (extraction → reformat → analyze), which is a distinct journalism skill, not a general "how to use AI" tour.
- **No big semester-long investigation as the final.** Modular weekly assignments that build conceptual depth are acceptable.
- **Cloud / API costs are acceptable.** Don't constrain to free tier.
- **Tech stack is fully open** to challenge.

### What's explicitly OFF the table

- **Collaborative git workflows.** Students learn this live on the job; class time on it is wasted.
- **Pressure-testing "AI handles scraping"** unless we want to argue specifically for a scrape→DB→clean→present spine. We don't; AI does cover scraping for our needs.
- **Defaulting to "more SQL practice."** AI writes SQL well; students need *enough* SQL fluency to read, verify, and modify what AI produces. They are not becoming SQL experts.

---

## Decisions log (with reasoning, including rejections)

### SQL framing — RESOLVED

- **Tried:** "Make SQL the spine of the course; teach JOINs deeply; rote SQL drills."
- **Pushback:** AI writes SQL well. Outside of dirty-join domain knowledge, AI-generated SQL beats human SQL. Our students don't know SQL yet — making them SQL writers when AI does it for them is the wrong target.
- **Tried next:** "Teach SQL via question families instead — checking, not writing."
- **Refined pushback:** They still need basic SQL. Question families aren't a replacement; they're how you couch SQL.
- **Settled framing:** Teach SQL basics (SELECT/WHERE/GROUP BY/JOIN) via DuckDB. *Couch* the SQL in a "question families" checklist that gives students a graduate-level habit-of-mind for analyzing any new dataset. Verification, not authorship, is the differentiating skill — supported by domain-specific gotcha pedagogy (HMDA bugs, see Research Findings).

### Question families — design tool

A teaching frame for Week 1-2. Students learn ~12 categorical shapes of journalistic questions and apply them to any new dataset. Default journalistic instinct is "averages and top 10 lists"; this lifts them past that.

The 12 families:
1. Counts/totals — "how many of X"
2. Rates/proportions — "X per 1000, % of total"
3. Comparisons across peers — "this hospital vs similar hospitals"
4. Distributions — "median vs mean, what does the spread look like"
5. Trends over time — "period-over-period, year-over-year"
6. Tail analysis — "top/bottom N, outliers"
7. Concentration — "what share by what %" ("1% of landlords filed 20% of evictions")
8. Geographic patterns — "clustered or dispersed"
9. Change detection — "snapshot vs snapshot"
10. Negation/absence — "who's missing, what didn't happen"
11. Reconciliation — "does dataset A match dataset B"
12. Counterfactual — "what would the answer be if we excluded X"

### Schema design — RESOLVED light

- User's stance: "It's like different spreadsheets you just join them" is enough.
- No normalization theory. No 3NF lessons. Joins are taught (they matter); deeper schema theory is not.

### Indexes — RESOLVED conditional

- Indexes only when data demands them. Probably appears once when DuckDB or Postgres queries get slow at scale. Not a CS-databases-class topic.

### BigQuery — CUT

- User: bill-running risk. Cut entirely. One-paragraph mention only ("this exists, here's when you'd reach for it"). Not in tech-stack.md as a taught tool.

### Aleph — RESOLVED operate it

- **Tried:** tour mode (don't operate Aleph; use OCCRP demo only).
- **Pushback:** "Aleph seems pretty cool honestly. It might be worth giving them an Aleph setup and having them do an investigation." User can run the instance.
- **Settled:** W6 starts in the public OCCRP Aleph instance for exposure (Companies House + sanctions + Offshore Leaks). W7 moves to instructor-run self-hosted Aleph where students ingest NYC data via FtM CSV mapping. Then W9 they build a baby version of Aleph's entity resolution themselves (rapidfuzz/dedupe vs AI vs Aleph's built-in, comparison exercise).
- **Conceptual backbone:** **FollowTheMoney (FtM) ontology** is the spine of the unit — `Person`, `Company`, `Directorship`, etc. Once students learn FtM, every dataset becomes joinable. Teach FtM W6 day 1.

### Neo4j / Cypher / NetworkX — RESOLVED keep Cypher

- **Tried:** swap Cypher for NetworkX (simpler).
- **Pushback:** "Cypher is better than NetworkX *because* it's a query language and you can explain what you want easily and it can sit in Python — all realms they're familiar with. So it is 'don't do 16 joins, just write some Cypher.'"
- **Settled:** Keep Neo4j + Cypher. Light week (W8). Frame as "thinking in networks for investigations," not Cypher syntax bootcamp.

### Public-facing tools (Flask) — RESOLVED 1 week

- **Tried:** 2 weeks of Flask.
- **Pushback:** "Your 'Public-facing tools (2)' week is just like 10 minutes asking an LLM to do CSV export and add pagination, nothing interesting or needing time."
- **Settled:** 1 week. Focus shifts from "more Flask features" to **security + privacy + methodology-as-journalism**. Flask + SQLite (not Postgres — read-only public tools don't need it) + Render. Deliverable includes a **security audit document** alongside the deployed tool, plus a methodology page linked from the tool.

### Bill tracker — RESOLVED build now, harvest at semester end

- **Tried:** 2-week assignment (W4-5).
- **Pushback:** "Legislative bill DB isn't going to change that much in 2 weeks on a daily basis, we'll need to come back to it nearer to the end of the semester."
- **Settled:** W4 builds the scraper; it runs in the background for the rest of the semester. **W12 is the harvest** — students return to their scraper with 7+ weeks of accumulated data and write up longitudinal findings as a methodology article. This actually strengthens the pedagogy: "scrapers are about time; here's what waiting buys you."
- **Each student picks** their own state legislative bill DB. Trades comparable evaluation for ownership and more interesting writeups.
- **Open:** what's W5 now? Bill scraper is W4-only. Candidates: data cleaning / validation as journalism; cross-dataset reconciliation; a first case-study + interview week.

### NYC as through-line — REVERSED

- **Tried:** NYC building ownership / Aleph investigation as a 5-week through-line spanning W6-W10.
- **Pushback:** "NYC does NOT need to be through-line on dataset/analysis, nope."
- **Settled:** NYC investigation is **contained to W6-W7** (the Aleph weeks). W8 (Cypher), W9 (extraction comparison), W10 (public tool) can use whatever dataset makes sense per student — could revisit NYC, could revisit HMDA, could revisit bill tracker.

### NYC dataset options — RESOLVED present as opportunities

- **Primary candidate:** NYC building ownership / LLC piercing. Cleanest data (ACRIS, HPD owner registrations with the magic "head officer" field, DOB BIS, NYS Department of State). Real entity-resolution problem built in. Every student gets a personal story (their own block). 2024 NYC LLC Transparency Act makes it newly timely. JustFix's "Who Owns What" is a working open-source precedent.
- **Innovation tier:** Adams orbit / NYC CFB campaign finance — extends naturally from the LLC graph (real estate donors → candidates → city contracts). Hell Gate's Table of Success exists as inspiration but isn't a methodology article.
- **Stretch:** democratic machine reporting (NYS BOE filings, County Committee, judicial appointments). Messier data; THE CITY has done strong work here.
- **Risk noted by user:** "no guarantee we'll get anything from it." Need to verify data accessibility before semester.

### Datasette Cloud — TENTATIVE student-managed

- **Options:** paid student accounts ($25/mo each); instructor cloud; students set up their own self-hosted Datasette.
- **User leaning:** "Maybe they set them up, part of learning infra?"
- **Implication:** W3 (cloud as access) becomes more substantive — Datasette setup as an infra-learning moment, not just "publish to a service."
- **Open:** confirm direction before W3 is finalized.

### W5 content — RESOLVED case study + FOIA + craft

- **Tried:** "data cleaning / validation as journalism."
- **Pushback:** fuzzy matching, dedupe, validation, and entity resolution all belong in W9 (build a baby Aleph). Data cleaning as a standalone W5 was duplicating W9 work.
- **Tried next:** "case study / guest speaker + long-term tracking dovetail."
- **Refined:** user noted "Data acquisition / FOIA / sourcing is something we NEED to cover if we're talking about scale" — but didn't want a separate week.
- **Settled:** W5 = "How data gets acquired and grows over time." Folds FOIA into the case-study/long-term-tracking week. Long-term tracking journalists usually deal with FOIA anyway (recurring records requests). Guest speaker covers FOIA practically rather than as abstract lecture. Readings combine scraping-as-tracking (Simon Willison PG&E) with FOIA-heavy long-term work (WaPo Fatal Force). Interview writeup pulled forward from W7 to spread load.

### Documentation as journalism — RESOLVED recurring

- Folded into multiple weeks. First taste in W5 (or W4-5 depending on what W5 becomes), again in W7 (Aleph methodology), W10 (public tool methodology page), W12 (publication-quality methodology article as final).
- **Reading anchors:** ICIJ Cyprus Confidential, Pandora Papers, ProPublica COMPAS, Washington Post Fatal Force.

### Journalist interviews — RESOLVED homework, with rolodex

- User has rolodex; can recruit working investigative journalists if needed.
- Default: students reach out as homework — practice itself is part of the learning. Each student arranges a 20-min interview with a journalist from a relevant investigation (pre-W7).
- Optional in-class panel possible.
- No fallback needed (rolodex is the safety net).

### Multilingual extraction (W11) — RESOLVED keep

- International student body is a strength. Each student brings their own language pair.
- Anchor: Quartz Luanda Leaks methodology (Apertium for Portuguese MT + sentence-level vector embeddings).

### Semantic search intro — RESOLVED W4 lightweight

- User noted the course was becoming very NER/entity-resolution heavy and asked where semantic/hybrid search could shine.
- Settled: first exposure belongs in W4 because state legislative bills are the first naturally text-heavy dataset. Keep it lightweight through Datasette: keyword/full-text vs semantic search, with fuzzy search as the simple "not exact match" comparison.
- W11 remains the deeper treatment: multilingual semantic search, extraction, and verification at scale.

### Verification of AI-generated SQL — RESOLVED via HMDA gotchas

- **Concern:** "I don't know how we're going to find a wrong query or a mismatched result. Open to ideas though. Artificial dataset doesn't seem great. Probably domain-knowledge on HMDA is where we'll find it."
- **Resolution:** Research agent returned 7 concrete HMDA gotchas with citations from real journalism. W2 pedagogy hinge: instructor presents 3 AI-written HMDA queries with planted bugs from this list; students find the bugs.

### Window functions / "queries that win IRE awards" — Claude bullshitted

- I claimed Week 9 could be replaced with "advanced SQL — recursive CTEs, window functions, the queries that win IRE awards." User pushed back: "can you specifically name the stories because otherwise i'm suspicious." I retracted — I had no specific stories in mind.
- Window functions stay nice-to-have. Open: drop entirely, brief explainer, or research?

### Reuters Guantanamo (in current readings.md) — HALLUCINATION confirmed

- Research agent confirmed there's no Reuters Guantanamo methodology article. The 2011 WikiLeaks Detainee Assessment Briefs went to NYT/NPR/Guardian/McClatchy/WaPo, not Reuters.
- Validates user's README warning ("Have I verified that any of these investigations it's talking about exist? Absolutely not!").
- Step C (full readings sweep) completed 2026-05-05. **Good news:** no other outright hallucinations. **Less good:** ~6 dead URLs, ~9 duplicates, 3 archived/deprecated resources, several attribution mismatches. Full sweep at `readings-sweep.md (deleted after B; superseded by rewritten readings.md)`.

### OpenAleph fork — RESOLVED update terminology

- Readings sweep surfaced: OCCRP is sunsetting active maintenance of Aleph after Dec 2025. **OpenAleph** is the going-forward fork at openaleph.org.
- For a 2026 syllabus, terminology should default to OpenAleph. Update tech-stack.md, readings.md, and syllabus references.
- Functionally equivalent for what we're teaching; the fork is about who maintains it.

### Aleph reading anchor — CYPRUS CONFIDENTIAL

- ICIJ Cyprus Confidential methodology *literally describes our W6-W7 workflow*: leak → extract entities → cross-reference against OpenSanctions / PEPs / Forbes / Dow Jones. Students read it as the model, then replicate the pattern with NYC data instead of Cypriot offshore.

### Tools cut from taught content

- BigQuery (mention only)
- LM Studio (mention)
- Haystack (mention)
- NotebookLM (mention)
- pgvector (mention as "if ambitious")
- TablePlus (mention as optional GUI)
- xsv / miller / csvkit / jq / ripgrep (bonus appendix)
- PostGIS (out of scope)
- Capstone Project Prep section (entire section — reflects old course identity)

---

## Still open

1. **NYC building ownership data accessibility** — verify before semester that ACRIS / HPD / DOS data can be acquired at the right scale.
2. **Datasette Cloud setup mode** — student-managed self-hosted (user leaning) vs paid student accounts vs instructor-run.
3. **Window functions** — drop, brief explainer, or research specific investigations?
4. **Step order RESOLVED:** user picked **C → A → B**. C (readings sweep) running in background as of 2026-05-05. A and B follow.
5. ~~W5 FOIA reading~~ — RESOLVED 2026-05-05. Picked: Marshall Project (Alysia Santo, July 2025) + MuckRock release notes on DocumentCloud agency monitoring + MuckRock FOIA Log Explorer.

---

## The 12-week arc — current state

Each week: dataset, skills, deliverable, reading, what students should be able to *explain*.

### Week 1 — SQL via question families + DuckDB
- **Dataset:** HMDA county slice
- **Skills:** DuckDB on big CSV; basic SQL (SELECT, WHERE, GROUP BY, simple JOIN); the 12-question-families checklist; AI as a SQL co-pilot
- **Deliverable:** pose 5 questions across 5 different families about HMDA; for each, have AI write a query, run it, sanity-check the number against domain knowledge, summarize the finding
- **Reading:** TBD — question-families primer (likely needs to be written) + a real investigation as a "what story shape did this question family produce" example
- **Explain:** which question family each of your 5 belongs to and what story shape it implies

### Week 2 — JOINs + dirty data + domain checking
- **Dataset:** HMDA + lender info + Census tract demographics
- **Skills:** multi-table joins; HMDA-specific domain pitfalls
- **Deliverable:** redlining-style investigation question requiring a 3-table join, with verification of row counts and sanity-checks against domain
- **Pedagogy hinge:** instructor presents 3 AI-written HMDA queries with planted bugs from the gotcha list (action_taken miscoding, refinance vs purchase, race column collapse). Students find bugs.
- **Reading:** Reveal "Kept Out" (use Pulitzer page as stable anchor) + MLK50 piece on CFPB rolling back HMDA
- **Explain:** what domain knowledge would have caught each bug; what story would change if you'd missed it

### Week 3 — Cloud as access (compressed from 2 weeks)
- **Dataset:** continue HMDA work
- **Skills:** Datasette setup (likely student-managed self-hosted, pending decision); Backblaze B2 for documents; cost reasoning at order-of-magnitude only
- **Deliverable:** publish HMDA exploration via Datasette to a teammate playing "editor in DC"; write a one-page OOM cost estimate for a hypothetical 6-month version
- **Reading:** ProPublica "Heart of Nerd Darkness" (custom OCR tool to parse 50+ disclosure PDFs)
- **Cut:** BigQuery (mention only); TablePlus (optional)

### Week 4 — Bill scraper build
- **Dataset:** each student picks a state legislative bill DB
- **Skills:** GitHub Actions, git-scraping pattern, change detection, error notification
- **Deliverable:** scraper running daily, committing changes to git; a "what would break this in 6 months" memo
- **Reading:** Simon Willison git-scraping post; Simon's PG&E scraper as the model
- **The scraper keeps running for the rest of the semester. W12 is the harvest.**
- **Explain:** what's the difference between a meaningful change and noise

### Week 5 — How data gets acquired and grows over time
- **Framing:** the two threads — data acquisition (FOIA, scraping, leaks, bulk public, build-it-yourself) and long-term tracking — connect naturally. Most scale acquisition involves *ongoing* relationships with data sources. Bill scrapers, recurring FOIAs, and tracker projects are the same shape.
- **Skills:** modes of data acquisition (FOIA basics, scraping, leaks, public bulk); long-term tracking craft; planning a longitudinal investigation
- **Readings:** Simon Willison PG&E tracker (scraping-as-tracking); WaPo Fatal Force methodology (FOIA + scraping + ongoing local sourcing — explicitly long-term); one FOIA-focused piece (TBD pending readings sweep — likely MuckRock or a "how we got it" sidebar from a real investigation)
- **Guest speaker:** a journalist whose work involves both long-term tracking AND FOIA — they pair naturally; instructor's rolodex
- **Interview writeup due** (pulled forward from W7 so the load is spread, not stacked)
- **Workshop deliverable:** one-page memo with two parts:
  1. "What could your bill scraper become if you ran it for a year?" — apply the question-families checklist to longitudinal questions
  2. "For the NYC investigation coming up in W6, where would you get the data and what would be hard about it?" — forces them to think about acquisition before being handed the dataset
- Bill scrapers continue running in background

### Week 6 — NYC kickoff via public Aleph
- **Dataset:** NYC + Aleph public (Companies House, sanctions, Offshore Leaks entity graphs)
- **Skills:** **FtM ontology day 1**; public Aleph search; entity-as-graph thinking
- **Deliverable:** cross-reference a NYC-related name across Companies House, sanctions, and Offshore Leaks; write a triage memo identifying any cross-dataset hits
- **Reading:** [ICIJ Cyprus Confidential methodology](https://www.icij.org/investigations/cyprus-confidential/leaked-data-journalism-methodology/) (literally the workflow they're about to replicate)
- **Homework:** arrange a 20-minute interview with a journalist who worked on a related investigation — due W7

### Week 7 — NYC self-hosted Aleph + ingest
- **Dataset:** NYC building ownership (ACRIS deeds + HPD owner registrations, ingested via FtM CSV mapping)
- **Skills:** Docker Aleph instance (instructor-run); FtM CSV mapping; entity ingestion; entity resolution as Aleph performs it
- **Deliverable:** pick a neighborhood (10-30 buildings); identify shared head-officers / shared addresses across LLCs; write "who owns this block" memo
- **Reading:** ICIJ "How ICIJ deals with massive data leaks" (Data Journalism Handbook chapter); Mar Cabra Source piece on Panama Papers data team
- **Interview writeup due** ("a day in this investigation" — what was tedious, what broke, what was the actual journalism)
- **Risk noted:** NYC data accessibility unverified before semester

### Week 8 — Networks for investigations (Cypher, light week)
- **Dataset:** NYC entities exported from Aleph + public Offshore Leaks entity graph
- **Skills:** Neo4j Sandbox; Cypher pattern matching; path tracing; "don't write 16 joins, write some Cypher"
- **Deliverable:** trace a path through the entity graph; write up "if this connection is real, what story does it suggest, and what would verify it?"
- **Reading:** GIJN "How They Did It: Paradise Papers"
- **Light week.** Don't teach Cypher beyond pattern matching basics.

### Week 9 — Build a baby Aleph (extraction + fuzzy matching + linking, together)
- **Dataset:** small document set with known entity duplicates (could be a subset of W7's NYC docs, or sanctions/Companies House subsets)
- **Skills:** AI extraction (Anthropic structured outputs); rapidfuzz/dedupe; cross-dataset linking
- **Deliverable:** **comparison exercise** — take the same entity resolution task; run it three ways (Aleph's built-in result from W7, rapidfuzz/dedupe library, AI structured outputs); measure all against ground truth; write up which performed better and why
- **This is the "rebuild what Aleph builds" moment.** Connects W7 (used Aleph) to W11 (use AI extraction at scale).
- **Reading:** rapidfuzz/dedupe documentation; relevant AI papers if any

### Week 10 — Public-facing tools + security + privacy + methodology
- **Dataset:** student picks from earlier work (HMDA, bill tracker, NYC Aleph results)
- **Skills:** Flask + SQLite (read-only) + Render; security failure modes (SQL injection, exposed write creds, PII leakage); privacy / harm reduction; methodology-as-journalism
- **Deliverable:** deployed tool + **security audit document** (where could this leak data, where could it be SQL-injected, what happens under load, what PII shouldn't be there) + **methodology page** linked from the tool
- **Reading:** ProPublica "How We Analyzed COMPAS" (gold standard for algorithm audit methodology); THE CITY "How to Search Property Building Records"
- **Cut:** the second Flask week. Most of week 14 sustainability folds in here.

### Week 11 — AI extraction at depth + multilingual
- **Dataset:** multilingual document set; international students bring their own language pair
- **Skills:** Anthropic/OpenAI structured outputs; multilingual extraction; **collaborative verification** (each student verifies 20 docs from a shared 100-doc set against a held-back instructor "ground truth"; class leaderboard for both extraction quality and verification quality)
- **Deliverable:** extraction + measured error rate against the ground-truth subset + cost analysis (Sonnet vs Haiku on the same task)
- **Reading:** Quartz Luanda Leaks AI methodology + ICIJ companion piece
- **Cut as taught topics:** Haystack, LM Studio, NotebookLM (mention each, don't teach)

### Week 12 — Methodology article + bill tracker harvest + sustainability
- **Datasets:** their bill tracker (now with 7-8 weeks of accumulated data) + their semester's work
- **Skills:** methodology-as-journalism; longitudinal analysis; sustainability planning; scheduled-update logic
- **Deliverable:**
  1. Publication-quality methodology article on one project from earlier in the semester (read 3 real ones first)
  2. Bill tracker findings memo — "I've watched X for 8 weeks; here's what changed and what story it suggests"
  3. Scheduled-update plan + a "what would I do differently" reflection
- **Reading:** Washington Post Fatal Force methodology (the canonical "how we collected this" page); AP Datakit
- **This is the longitudinal payoff for W4's scraper.**

---

## Research findings — the bank of material

### HMDA gotchas (for W2 pedagogy hinge)

Seven concrete bugs available for "find what's wrong with this AI-written query":

1. **`action_taken = 3` only counts denials.** HMDA codes: 1 originated, 2 approved-not-accepted, 3 denied, 4 withdrawn, 5 file-closed-incomplete, 6 purchased, 7 preapproval-denied, 8 preapproval-approved-not-accepted. Banks can pressure marginal applicants to withdraw, making raw "denied" rate look low while the real adverse-outcome rate (3+4+5+7) is much higher. Reveal "Kept Out" had to think through which codes counted as adverse.
2. **`action_taken = 1` vs 6.** Code 6 = purchased loan (bought from another lender's secondary market), not originated. Counting all action_taken codes double-counts and inflates loan-buying banks' apparent footprint.
3. **Refinances swamp purchase analysis.** `loan_purpose` distinguishes purchase, refinance, cash-out refi, etc. In low-rate years refis outnumber purchases 3:1. Same property reappears across years. "Lending in census tract X" without filtering by purpose measures churn (mostly white refis), not access. ProPublica's modern-redlining work and Federal Reserve studies always restrict to conventional home-purchase originations.
4. **Race is 5 columns + ethnicity column + parallel co-applicant fields.** `WHERE race_1 = 'Black'` silently drops multi-race Black applicants. Hispanic ethnicity is separate from race. NCRC publishes an explicit derivation methodology.
5. **Denial reasons under-counted.** Up to 4 reasons reported; pre-2018 reporting was optional. Naive `GROUP BY denial_reason_1` shows only first-listed. Worse: Minneapolis Fed showed lender-reported reasons don't statistically explain racial disparities — so "Black applicants denied for 'credit history'" reports lender narrative, not finding.
6. **The 2018 regulatory break.** Pre-2018 HMDA has no credit score, DTI, LTV, interest rate, or term. From 2018 on, qualifying lenders report ~25 new fields. Coverage expanded (HELOCs, reverse mortgages, closed-end home-equity); reporting threshold moved (25 → 100 loans). 2016-vs-2019 trend lines are apples-to-oranges. Reveal "Kept Out" (2018) couldn't control for credit score because it used pre-2018 data — central critique from American Bankers Association.
7. **Geography NA values.** Census-tract analyses break when `census_tract` is blank or NA — common for manufactured-home loans, multifamily, some purchased loans. `GROUP BY census_tract` quietly drops these. Tract boundaries also re-baseline each decennial census.

**Bonus reading:** [MLK50 "CFPB moves to limit home loan data"](https://mlk50.com/2019/05/23/consumer-financial-protection-bureau-moves-to-limit-home-loan-data/) — what happens when journalism actually publishes HMDA findings. Great W2 closer.

### Aleph built-ins (for W6 — public OCCRP instance)

- **Sanctions lists, all live on public OCCRP, no login:** US OFAC SDN/non-SDN, EU EEAS consolidated, UK HM Treasury, Swiss SECO, UN Security Council, plus Canadian/Australian/Ukrainian/World Bank-debarred contractors. CC-BY 4.0, refreshed daily via OpenSanctions.
- **OpenSanctions (the federation layer):** merges sanctions feeds with PEPs and crime/debarment data, deduplicates as FtM entities, assigns canonical IDs.
- **PEPs:** sourced through OpenSanctions from CIA World Leaders, European Parliament, EveryPolitician, Wikidata, country-specific scrapers. Coverage uneven — strong EU/UK/US, weaker Africa/Asia.
- **UK Companies House:** full dataset ([/datasets/809](https://aleph.occrp.org/datasets/809)) + People with Significant Control register ([/datasets/2053](https://aleph.occrp.org/datasets/2053)). Public, clean, large, journalistically famous. **Best teaching dataset.**
- **OpenCorporates:** **no longer free** (£2,250+/year). OCCRP retains historical snapshots. Teach as a cautionary tale about depending on third-party infrastructure.
- **ICIJ leaks (Panama, Paradise, Pandora, Bahamas, Offshore):** structured entity data only (~810k entities) is public via offshoreleaks.icij.org and importable via FtM. **Source documents are NOT public** — restricted to ICIJ partner journalists. Plan assignments around the entity network, not document review.
- **Other:** court records and gazettes (PACER samples, EU Official Journal, national gazettes), maritime/aviation registries (IMO ships, FAA aircraft), selected city land registries, World Bank/IMF contractor lists, country-specific company registries scraped by OCCRP. ~250+ datasets, 400M+ entities total on the public instance.
- **FtM ontology:** standardized types — `Person`, `Company`, `Vessel`, `Payment`, `Directorship`, `Email`. Once students learn FtM, every dataset becomes joinable. Also the import format: any CSV becomes ingestible by mapping columns to an FtM schema via `ftm map`.

**Recommendation from research agent:** build W6 around the public instance (sanctions + Companies House + ICIJ entity graph) — no infrastructure required. Reserve the self-hosted W7 exercise for FtM CSV ingestion. Skip OpenCorporates as a primary source.

### NYC investigation candidates (for W6-W7)

**1. Building ownership / LLC piercing — PRIMARY CANDIDATE**
- Why strongest: cleanest data, clearest entity-resolution lesson, every student gets a personal story.
- Sources: ACRIS deeds/mortgages, HPD owner registrations (the magic "head officer" field that pierces LLCs), DOB BIS permits, DOF rent-stabilization data, NYS Department of State corporate filings, Worst Landlord Watchlist.
- Real entity-resolution problem in the data: same human appears as "Robert Izsak," "R. Izsak," "Izsak Realty LLC c/o…"
- Open-source precedent: [JustFix's Who Owns What](https://whoownswhat.justfix.org/).
- Reader-facing how-to to use as reading: [THE CITY "How to Search Property Building Records"](https://www.thecity.nyc/2023/10/13/how-to-search-property-building-records/).
- Newly timely: 2024 NYC LLC Transparency Act.
- Outlets covering this beat: THE CITY, Documented, NYCity News Service. (NOT primarily ProPublica — earlier curriculum draft incorrectly attributed.)

**2. Adams orbit / NYC CFB campaign finance — INNOVATION TIER**
- Hell Gate's "Table of Success" exists ([https://tableofsuccess.hellgatenyc.com/](https://tableofsuccess.hellgatenyc.com/)) but is a relationship-mapping editorial project, not a data-methodology article.
- Naturally extends from #1's LLC graph: real estate donors → candidates → city contracts.
- Sources: NYC CFB bulk data ([https://www.nyccfb.info/follow-the-money/](https://www.nyccfb.info/follow-the-money/)), NYS Board of Elections, COIB lobbying, Checkbook NYC, nonprofit 990s.
- Strong because CFB bulk data is structured and downloadable.

**3. Democratic machine reporting — STRETCH**
- THE CITY has done strong work: judicial donations to Brooklyn Democratic machine, "Ghost Appointments" County Committee piece.
- Data is messier than #1 or #2 (NYS BOE petitions/filings less digitized).
- Hold as Innovation/optional for students who want messier data.

### Methodology articles (verified URLs)

| Source | URL | Use |
|---|---|---|
| ProPublica Dollars for Docs | https://www.propublica.org/article/about-the-dollars-for-docs-data | General methodology model |
| ProPublica "Heart of Nerd Darkness" | https://www.propublica.org/nerds/heart-of-nerd-darkness-why-dollars-for-docs-was-so-difficult | W3 reading: custom OCR tool Farrago |
| ICIJ Cyprus Confidential | https://www.icij.org/investigations/cyprus-confidential/leaked-data-journalism-methodology/ | **W6 anchor** — the workflow students replicate |
| ICIJ Pandora Papers | https://www.icij.org/investigations/pandora-papers/about-pandora-papers-leak-dataset/ | Tech stack reference |
| ICIJ Panama Papers | https://www.icij.org/investigations/panama-papers/data-tech-team-icij/ | Tech stack reference |
| ICIJ "How ICIJ deals with massive data leaks" (DJH) | https://datajournalism.com/read/handbook/two/working-with-data/how-icij-deals-with-huge-data-dumps-like-the-panama-and-paradise-papers | **W7 anchor** — cross-investigation reflection |
| Mar Cabra "How we built the data team" (Source) | https://source.opennews.org/articles/how-we-built-data-team-behind-panama-papers/ | W7 companion |
| Quartz Luanda Leaks AI | https://qz.com/1786896/ai-for-investigations-sorting-through-the-luanda-leaks | **W11 anchor** — multilingual + embeddings |
| ICIJ Luanda Leaks companion | https://www.icij.org/investigations/luanda-leaks/how-we-mined-more-than-715000-luanda-leaks-records/ | W11 companion |
| Washington Post Fatal Force | https://www.washingtonpost.com/investigations/2022/12/05/washington-post-fatal-police-shootings-methodology/ | **W12 anchor** — model for student methodology articles |
| WaPo Fatal Force GitHub | https://github.com/washingtonpost/data-police-shootings/blob/master/README.md | Open backup if methodology page blocks |
| BuzzFeed FinCEN Files | https://www.buzzfeednews.com/article/jsvine/fincen-files-explainer-data-money-transactions | Document-investigation methodology |
| Reveal "Kept Out" (Pulitzer page) | https://www.pulitzer.org/finalists/aaron-glantz-and-emmanuel-martinez-reveal-center-investigative-reporting-emeryville-calif | **W2 anchor** — stable archive of canonical HMDA investigation |
| MLK50 "CFPB moves to limit home loan data" | https://mlk50.com/2019/05/23/consumer-financial-protection-bureau-moves-to-limit-home-loan-data/ | W2 closer — consequences of journalism |
| THE CITY "How to Search Property Building Records" | https://www.thecity.nyc/2023/10/13/how-to-search-property-building-records/ | **W7 reader-facing how-to** |
| ProPublica COMPAS methodology | https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm | **W10 anchor** — algorithm audit |
| GIJN "How They Did It: Paradise Papers" | https://gijn.org/stories/paradise-papers/ | **W8 reading** |
| The Markup Citizen Browser | https://themarkup.org/citizen-browser | Bonus — instrumentation methodology |
| Bellingcat OSINT toolkit | https://bellingcat.gitbook.io/toolkit | Reference resource |

### Confirmed hallucinations (in current readings.md)

- **Reuters Guantanamo methodology** — does not exist. The 2011 WikiLeaks Detainee Assessment Briefs went to NYT/NPR/Guardian/McClatchy/WaPo. Replace with NPR's [Guantanamo Papers](https://www.npr.org/series/135694663/the-guantanamo-papers).

### Likely problems (need verification — feeds Step C)

- BuzzFeed/The Trace "Cities Failing to Arrest Shooters" — research agent didn't verify
- Various unstable Reveal URLs after CIR/Mother Jones merger
- "Hell Gate Eric Adams methodology" — exists as project but not as data-methodology article (frame appropriately)
- "THE CITY/ProPublica NYC building ownership investigation methodologies" — no single canonical "how we did it" exists; series of reader-facing how-tos

---

## Voice / tone for the rewrite

From CLAUDE.md and reinforced through this design process:

- Concise and practical
- Real investigations as examples (Panama Papers, Cyprus Confidential, ProPublica's work)
- Avoid condescending language
- Avoid time estimates (instructor decides)
- Be specific about requirements ("10-20 documents" not "SHORT PDFs")
- Investigation-shaped, not tool-shaped — every week answers "what investigation does this unlock?"
- Methodology-as-journalism throughout — students publish, not just submit
- No emoji unless instructor explicitly asks
- Don't over-document — comments only when WHY is non-obvious

---

## State of repo files

| File | State | What needs to happen |
|---|---|---|
| CLAUDE.md | UPDATED this session | Already reflects 12 weeks, 3rd semester, prereqs, bar. Stable. |
| README.md | STALE | Says 2nd semester. Lists 14 weeks. Has the user's "haven't verified investigations" warning. **Rewrite needed.** |
| curriculum.md | STALE | 14 weeks, "second semester," lists stats/data-driven/reporting as concurrent classes. Has SQL listed as a prereq (contradicts reality). Has "Capstone Project Prep" leftover section. **Major rewrite needed.** |
| assignments.md | STALE | 14-week structure. Some Foundation assignments fakeable with AI. Some writing prompts strong (redlining, editor-in-DC); some weak ("reflect on the semester"). **Major rewrite needed.** |
| tech-stack.md | STALE | BigQuery still listed as taught. References Heroku-era. Mentions instructor-guide and weekly-checklist files that don't exist. **Major rewrite needed.** |
| readings.md | STALE + HALLUCINATED | Contains confirmed Reuters Guantanamo hallucination. Likely more. **Verification sweep (Step C) needed before rewrite.** |
| speed-run.md | STALE | Heroku free tier mentioned. 14-week structure. **Rewrite needed.** |
| review-prompt.md | CREATED this session | Canonical review prompt that drove this iteration. Keep as historical reference. |
| design-notes.md | THIS FILE | Working design doc. Update as decisions happen. |
| readings-sweep.md (deleted after B; superseded by rewritten readings.md) | CREATED 2026-05-05 | Full Step C output: KEEP / FIX / CUT / ADD report on every readings.md entry. Feeds A and B. |
| instructor-notes.md | TO BE CREATED (Step A) | Canonical revised syllabus, single doc, source for B's rewrites. |

Files referenced in CLAUDE.md but not present in repo:
- `instructor-guide.md` — listed but doesn't exist
- `weekly-checklist.md` — listed but doesn't exist

---

## Process trail — non-obvious wrong turns

Capturing what was tried and rejected, so we don't relitigate:

1. **"Make SQL the spine."** Rejected: AI writes SQL well; goal is verification, not authorship. *But:* basics still need teaching, couched in question families.
2. **"Replace Cypher with NetworkX."** Rejected: Cypher is a query language that fits students' SQL/Python world. "Don't write 16 joins, write some Cypher."
3. **"Two weeks of Flask."** Rejected: week 2 is "10 minutes asking an LLM." Compressed to 1 week; reclaimed time goes to security + privacy + methodology.
4. **"NYC as a 5-week through-line."** Rejected: contained to W6-W7. Other weeks free for varied datasets.
5. **"Aleph as a tour, not operations."** Rejected: instructor will run Aleph; students do real ingestion. Builds a baby version themselves in W9.
6. **"BigQuery as a taught tool."** Rejected: bill-running risk. Mention only.
7. **"Schema design as a real topic."** Rejected: "spreadsheets you join them" is enough. No normalization theory.
8. **"Queries that win IRE awards" framing for Week 9 advanced SQL alternative.** Retracted: Claude couldn't name specific stories.
9. **"AI as a final unit (1st-semester pattern)."** Rejected as the framing — AI is woven through. *But:* a dedicated extraction unit is fine because it's about AI-as-data-transformation specifically.
10. **"2-week bill scraper."** Rejected: "Legislative bill DB isn't going to change that much in 2 weeks on a daily basis." Build in W4, harvest in W12.
11. **"Reuters Guantanamo as a reading."** Cut: hallucination. Replace with NPR Guantanamo Papers.
12. **"W5 = data cleaning / validation as journalism."** Rejected: fuzzy matching, dedupe, validation, and entity resolution all belong in W9. W5 was duplicating W9 work. Resolved as "How data gets acquired and grows over time" — folds FOIA into the case-study/long-term-tracking week instead of inventing a 13th week.
13. **"FOIA as its own week."** Rejected: covered via guest speaker + readings + memo deliverable inside W5. Long-term tracking and FOIA pair naturally because long-term work usually involves recurring records requests.

---

## Next steps

**C — Readings sweep.** COMPLETE 2026-05-05. Output saved to `readings-sweep.md (deleted after B; superseded by rewritten readings.md)`. No major hallucinations beyond Reuters Guantanamo. Findings: ~6 dead URLs, ~9 duplicates, 3 archived/deprecated, attribution issues. OpenAleph fork is the major terminology update. 8 strong adds suggested.

**A — v3 syllabus document.** COMPLETE 2026-05-05. Saved to `instructor-notes.md`. Includes: course identity, the bar, prereqs, working premises, Must/Should/Nice tiers, recurring threads (question families, methodology-as-journalism, verification mindset, AI as default), dataset spine, full 12-week arc with readings, tools taught/mentioned/cut, assessment notes, pre-semester checklist, and the file-by-file rewrite plan for B.

**B — Rewrite repo files.** COMPLETE 2026-05-05. All six files rewritten in parallel from `instructor-notes.md` and `readings-sweep.md (deleted after B; superseded by rewritten readings.md)`:
- README.md — third semester, 12 weeks, navigation with new quick-win taglines
- curriculum.md — full 12-week arc, recurring threads section, HMDA gotcha context, OpenAleph terminology
- assignments.md — Foundation/Extension/Innovation per week where applicable; W12 multi-part final; methodology-as-journalism deliverables called out (W5/W7/W10/W12)
- tech-stack.md — OpenAleph, rapidfuzz/dedupe added; BigQuery/Heroku/AWS dropped; DuckDB URLs fixed; "Cut from prior versions" section for transparency
- readings.md — sweep findings applied: dead URLs cut, descriptions fixed (Dollars for Docs as historical, COVID Tracking as ended 2021, etc.), 8 ADDs distributed per week, OpenAleph + Cyprus Confidential anchors prominent
- speed-run.md — 12 weeks, Render not Heroku, OpenAleph not Aleph, current code samples and model references

Files referenced in CLAUDE.md but missing from repo: `instructor-guide.md` and `weekly-checklist.md` — neither rewritten because neither exists; CLAUDE.md should be updated to drop those references in a follow-up edit.
