# Data and Databases: Technology Stack

The technologies students use directly, with documentation and tutorials. Tools are organized by category with the week they appear.

A small set of tools are *mentioned* (concept exposure, not taught) and noted as such. A separate appendix at the end lists optional command-line tools.

## Core Technologies

### Databases

#### DuckDB
- **Docs:** https://duckdb.org/docs/current/
- **Python guide:** https://duckdb.org/docs/current/guides/overview
- **Postgres extension:** https://duckdb.org/docs/current/core_extensions/postgres
- **Weeks:** 1-2
- **Why:** Query CSVs directly without importing; bridges pandas and SQL; on-ramp for SQL-naive students.

#### PostgreSQL
- **Docs:** https://www.postgresql.org/docs/
- **Tutorial:** https://www.postgresqltutorial.com/
- **Weeks:** 2-3, 9-10 (read-only public tool uses SQLite by default; Postgres for shared/multi-user)
- **Why:** Persistence, multi-user access, production databases. When DuckDB hits its limits.

#### SQLite
- **Docs:** https://www.sqlite.org/docs.html
- **Tutorial:** https://www.sqlitetutorial.net/
- **Weeks:** 6, 10 (powers Datasette, default for read-only Flask apps)
- **Why:** File-based database with zero infrastructure. The right default for a public read-only tool.

#### Neo4j
- **Docs:** https://neo4j.com/docs/
- **Cypher Manual:** https://neo4j.com/docs/cypher-manual/current/
- **Neo4j Sandbox (free, no install):** https://sandbox.neo4j.com/
- **Week:** 8
- **Why:** Graph database for entity relationships; Cypher's pattern matching beats 16-table SQL joins for traversal questions.

#### BigQuery (mentioned only)
- **Docs:** https://cloud.google.com/bigquery/docs
- **Status:** Not taught — easy to run up high bills accidentally. One-paragraph mention in Week 3.
- **Why know it exists:** Genuinely massive datasets that won't fit in DuckDB or Postgres.

### Cloud Infrastructure

#### Backblaze B2
- **Docs:** https://www.backblaze.com/b2/docs/
- **Quick start:** https://www.backblaze.com/b2/docs/quick_account.html
- **Python (S3-compatible):** Use boto3 with B2's S3-compatible endpoint
- **Week:** 3
- **Why:** Simpler and cheaper than S3 for journalism workflows; S3-compatible API means existing tools work.

#### boto3
- **Docs:** https://boto3.amazonaws.com/v1/documentation/api/latest/
- **S3 guide:** https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3.html
- **Week:** 3
- **Why:** Python SDK for any S3-compatible storage including B2.

#### DigitalOcean (optional)
- **Managed Databases:** https://docs.digitalocean.com/products/databases/
- **Week:** 3 (optional, for shared Postgres in Extension tier)
- **Why:** Managed Postgres without operating a server.

### Automation & Workflows

#### GitHub Actions
- **Docs:** https://docs.github.com/en/actions
- **Quickstart:** https://docs.github.com/en/actions/quickstart
- **Python guide:** https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python
- **Weeks:** 4, 12
- **Why:** Scheduled scrapers that run unattended; the backbone of the bill-tracker project that spans Weeks 4-12.

#### Git
- **Pro Git Book:** https://git-scm.com/book/en/v2
- **Week:** 4 (git-scraping pattern)
- **Why:** Version control; tracking data changes through commit history (Simon Willison's git-scraping approach).

### Data Tools & Publishing

#### Datasette
- **Docs:** https://docs.datasette.io/
- **Getting Started:** https://docs.datasette.io/en/stable/getting_started.html
- **Tutorials:** https://datasette.io/tutorials/
- **Datasette Cloud:** https://datasette.cloud/
- **Weeks:** 3, 6, 7, 10
- **Why:** Internal SQL exploration, instant cloud sharing, the right default for "let an editor query this."

#### TablePlus (optional)
- **Download:** https://tableplus.com/
- **Week:** 3 (optional GUI for Postgres)
- **Why:** Friendly GUI for Postgres if students prefer to GUI over psql.

#### Flask
- **Docs:** https://flask.palletsprojects.com/
- **Quickstart:** https://flask.palletsprojects.com/en/latest/quickstart/
- **Tutorial:** https://flask.palletsprojects.com/en/latest/tutorial/
- **Week:** 10
- **Why:** Building public-facing data tools without React overhead. Pairs with Jinja2 templates and SQLite read-only.

#### Jinja2
- **Docs:** https://jinja.palletsprojects.com/
- **Week:** 10
- **Why:** HTML templates for Flask.

#### Render
- **Homepage:** https://render.com/
- **Docs:** https://render.com/docs
- **Deploy Flask guide:** https://render.com/docs/deploy-flask
- **Week:** 10
- **Why:** Simple Flask deployment. Replaces Heroku (whose free tier is gone).

### Document Processing & Investigation

#### OpenAleph
- **Homepage:** https://openaleph.org/
- **Docs:** https://docs.aleph.occrp.org/ (heritage docs from OCCRP Aleph; OpenAleph is the maintained fork as of 2026)
- **Public OCCRP instance:** https://aleph.occrp.org/
- **OCCRP Investigative Dashboard:** https://id.occrp.org/
- **Weeks:** 6, 7, 9
- **Why:** Entity-graph platform for collaborative document investigations. The platform behind ICIJ's Panama, Paradise, Pandora, and Cyprus Confidential. Teach FtM ontology day 1; everything else falls out of it.
- **Note:** OCCRP is sunsetting active maintenance of Aleph after Dec 2025. OpenAleph (community fork at openaleph.org) is the going-forward project. Functionally equivalent for what we're teaching.

#### FollowTheMoney (FtM)
- **Homepage:** https://followthemoney.tech/
- **GitHub:** https://github.com/alephdata/followthemoney
- **Week:** 6 (the conceptual backbone of the Aleph unit)
- **Why:** The shared schema that makes datasets joinable across sanctions, Companies House, Offshore Leaks, and your own ingested documents.

#### DocumentCloud
- **Platform:** https://www.documentcloud.org/
- **API Docs:** https://www.documentcloud.org/help/api
- **Help & Tutorials:** https://www.documentcloud.org/help
- **Weeks:** 6, 7 (alternative to OpenAleph for smaller projects), 10 (publishing source documents alongside a tool)
- **Why:** The standard newsroom tool for document-backed journalism. Hosted by MuckRock since 2021; free for journalists. Built-in OCR, annotation, redaction, and embeddable document viewers (so a story can include the actual document inline). The right default for "I have these PDFs and want to share, annotate, or publish them." OpenAleph is what you reach for at ICIJ scale; DocumentCloud is what you reach for when 200 PDFs from a FOIA arrive in your inbox.

### Entity Resolution (W9)

#### rapidfuzz
- **Docs:** https://rapidfuzz.github.io/RapidFuzz/
- **GitHub:** https://github.com/maxbachmann/RapidFuzz
- **Week:** 9
- **Why:** Fast fuzzy string matching in Python. The "library" benchmark in the W9 three-way comparison.

#### dedupe
- **Docs:** https://docs.dedupe.io/en/latest/
- **GitHub:** https://github.com/dedupeio/dedupe
- **Week:** 9 (alternative to rapidfuzz)
- **Why:** Active-learning record linkage; better than rapidfuzz for nontrivial entity resolution.

### AI & Machine Learning

#### Anthropic API
- **Docs:** https://docs.anthropic.com/
- **Getting Started:** https://docs.anthropic.com/claude/docs/getting-started
- **Weeks:** 9, 11
- **Why:** Claude for entity extraction and structured outputs. Sonnet for quality, Haiku for cost.

#### OpenAI API
- **Docs:** https://platform.openai.com/docs
- **Structured Outputs:** https://platform.openai.com/docs/guides/structured-outputs
- **Weeks:** 9, 11
- **Why:** Alternative AI provider; structured outputs guarantee parseable JSON.

#### Google NotebookLM (mentioned)
- **Platform:** https://notebooklm.google.com/
- **Week:** 11 (mentioned, not taught)
- **Why know it exists:** Initial document exploration with citations; lower-friction than building your own RAG.

#### LM Studio (mentioned)
- **Download:** https://lmstudio.ai/
- **Week:** 11 (mentioned, not taught)
- **Why know it exists:** Local LLMs for sensitive documents that can't go to a hosted service.

#### Haystack (mentioned)
- **Docs:** https://docs.haystack.deepset.ai/
- **Week:** 11 (mentioned, not taught)
- **Why know it exists:** RAG patterns in Python.

#### pgvector (mentioned)
- **Docs:** https://github.com/pgvector/pgvector
- **Week:** 11 (mentioned, not taught — "if ambitious")
- **Why know it exists:** Vector similarity search inside Postgres.

## Prerequisites (assumed from prior semesters)

### Python & Libraries
- **pandas** — https://pandas.pydata.org/docs/ — assumed comfort
- **requests** — https://requests.readthedocs.io/ — assumed comfort
- **BeautifulSoup** — basic scraping, assumed comfort

### Tools
- **Git / GitHub** — solo workflow assumed; collaborative git intentionally NOT taught
- **Command line basics** — assumed comfort
- **GitHub Pages** — static publishing, assumed
- **HTML / CSS** — fundamentals assumed

### AI tools
- Students arrive using Claude Code, Cursor, or similar by default. The course builds on this rather than introducing it.

## Bonus Command-Line Tools (optional appendix)

Useful for data journalists; mentioned but not part of the taught curriculum.

### ripgrep (rg)
- **Docs:** https://github.com/BurntSushi/ripgrep
- **Why:** Fastest text search; better than grep for large file sets.

### xsv
- **Docs:** https://github.com/BurntSushi/xsv
- **Why:** Lightning-fast CSV operations; great when DuckDB is overkill.

### miller (mlr)
- **Docs:** https://miller.readthedocs.io/
- **Why:** Like awk/sed but for structured data (CSV, JSON, TSV).

### jq
- **Docs:** https://stedolan.github.io/jq/
- **Why:** Query and transform JSON.

### csvkit
- **Docs:** https://csvkit.readthedocs.io/
- **Why:** Swiss army knife for CSVs; great for exploration.

## Cut from prior versions of this curriculum

For transparency about what's not here:

- **BigQuery as a taught tool** — bill-running risk; one-paragraph mention only
- **PostGIS** — covered in concurrent mapping class
- **Heroku** — Render replaces; free tier dead
- **AWS S3** — Backblaze B2 is simpler and cheaper for journalism scale
- **OpenCorporates** — was free, now £2,250+/yr; mentioned as a cautionary tale about depending on third-party data infrastructure

## Notes

- This list only includes tools explicitly used in the curriculum
- Cloud service free tiers are sufficient for foundation-tier work; modest spend (under $50/student/semester) covers the rest
- Tools marked "mentioned" appear in lecture or readings but aren't required for any assignment
