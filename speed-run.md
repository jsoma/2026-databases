# Speed-Run Guide: Data and Databases for Journalism

A hands-on walkthrough of the complete tech stack for the Data and Databases course. Each section pairs context reading with technical work and a small investigation, so you build operational fluency with every tool — DuckDB through OpenAleph through AI extraction.

The full week-by-week curriculum is in `curriculum.md`; this guide compresses the same material into a tighter format you can work through end-to-end.

## Prerequisites

### Python and pandas basics
Load any CSV; do a groupby. Refresher: [10-minute pandas guide](https://pandas.pydata.org/docs/user_guide/10min.html).

### Git fundamentals
Clone a repo, make changes, commit. Refresher: [GitHub Desktop tutorial](https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop).

### Command line basics
Navigate directories, run Python scripts. Refresher: [Command line crash course](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Command_line).

### AI coding tools
Have a working setup with Claude Code, Cursor, or similar. The course assumes you'll reach for AI for query writing, scaffolding, and debugging — and will teach you when not to trust it.

---

## Week 1-2: SQL via Question Families + DuckDB

### Context Reading
[ProPublica's Dollars for Docs methodology](https://www.propublica.org/article/about-the-dollars-for-docs-data) (historical), [Tips for Building a Database for Investigations (GIJN)](https://gijn.org/resource/tips-for-building-a-database-for-investigations/), and the [Reveal "Kept Out" Pulitzer page](https://www.pulitzer.org/finalists/aaron-glantz-and-emmanuel-martinez-reveal-center-investigative-reporting-emeryville-calif).

### Learn the Tech

#### DuckDB Basics
Install with `pip install duckdb`. Follow the [DuckDB Python guide](https://duckdb.org/docs/current/guides/overview). Download an HMDA county slice from [CFPB](https://ffiec.cfpb.gov/data-publication/) and try:
```python
import duckdb
conn = duckdb.connect()
# Query CSV directly without loading into memory
result = conn.execute("""
  SELECT action_taken, COUNT(*) AS n
  FROM 'hmda_county.csv'
  GROUP BY action_taken
  ORDER BY n DESC
""").fetchdf()
```

#### Question Families
Before writing more queries, learn the twelve question families (see `curriculum.md`). For your county slice, pose one question from each of: counts, rates, distributions, peer comparisons, tail analysis, geographic patterns, change detection, negation/absence, and reconciliation. Have AI write each query. Sanity-check each result.

#### JOINs and HMDA Gotchas
Pull lender info and Census tract demographics. Try a 3-table JOIN. Then read the [HMDA action_taken interpretations](https://www.consumerfinance.gov/rules-policy/regulations/1003/interp-4/) and [NCRC's race/ethnicity methodology](https://ncrc.org/ncrcs-hmda-2018-methodology-how-to-calculate-race-and-ethnicity/). Now go back to your queries — what would a domain-naive AI-written version get wrong?

#### Postgres When You Outgrow It
Install [Postgres.app](https://postgresapp.com/) or [PostgreSQL](https://www.postgresql.org/download/). Use the [DuckDB Postgres extension](https://duckdb.org/docs/current/core_extensions/postgres) to push your cleaned dataset into Postgres for sharing.

### Practice Investigation
Use HMDA data for one county to ask a redlining-style question requiring a 3-table JOIN. Verify your work — row counts, sanity check against published CFPB totals — and write up what you found in 300 words.

---

## Week 3: Cloud as Access

### Context Reading
[Heart of Nerd Darkness (ProPublica)](https://www.propublica.org/nerds/heart-of-nerd-darkness-why-the-dollars-for-docs-data-is-so-difficult), [How BuzzFeed Found Spy Planes (CJR)](https://www.cjr.org/watchdog/how-buzzfeed-news-revealed-hidden-spy-planes-in-us-airspace.php), [The Markup's Blacklight](https://themarkup.org/blacklight/2020/09/22/how-we-built-a-real-time-privacy-inspector).

### Learn the Tech

#### Datasette for Sharing
Install with `pip install datasette`. Convert a SQLite database to a published instance:
```bash
sqlite-utils insert hmda.db loans hmda_county.csv --csv
datasette serve hmda.db
```
For sharing without local infrastructure, try [Datasette Cloud](https://datasette.cloud/) with a free trial.

#### Backblaze B2 for Documents
B2 is simpler and cheaper than S3. With its S3-compatible endpoint, boto3 works as-is:
```python
import boto3
s3 = boto3.client('s3',
    endpoint_url='https://s3.us-west-002.backblazeb2.com',
    aws_access_key_id='your_key',
    aws_secret_access_key='your_secret')
s3.upload_file('document.pdf', 'investigation-bucket', 'docs/document.pdf')
```

#### Cost Reasoning
Estimate the order-of-magnitude cost of a 6-month investigation: 5TB of documents + 3 reporters with continuous access. Show your work. Aim for $X-not-$Y precision, not exact numbers.

### Practice Investigation
Publish an HMDA exploration to Datasette. Share the link with a partner playing "editor in DC." Both query the same data simultaneously without setup on their end. Estimate what 6 months of this would cost.

---

## Week 4-5: Long-term Tracking + Data Acquisition

### Context Reading
[Git scraping (Simon Willison, 2021)](https://simonwillison.net/2021/Mar/5/git-scraping/), the [PG&E outages tracker](https://github.com/simonw/pge-outages), [WaPo Fatal Force methodology](https://www.washingtonpost.com/investigations/2022/12/05/washington-post-fatal-police-shootings-methodology/).

### Learn the Tech

#### GitHub Actions for Scheduled Scraping
Follow the [GitHub Actions quickstart](https://docs.github.com/en/actions/quickstart). A workflow that runs daily:
```yaml
name: Daily scraper
on:
  schedule:
    - cron: '0 12 * * *'  # noon UTC daily
jobs:
  scrape:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      - run: pip install -r requirements.txt
      - run: python scrape.py
      - run: |
          git config user.name "Scraper"
          git config user.email "scraper@example.com"
          git add data/
          git diff --cached --quiet || git commit -m "Update $(date)"
          git push
```

#### Change Detection
A simple diff isn't enough. Build logic that surfaces *meaningful* change — new bills, status changes, vote outcomes — not just timestamp drift. Generate a daily changelog.

#### Data Acquisition Modes
Most scale data acquisition is one of: bulk download of public data, FOIA for non-public, leaks, or build-it-yourself via scraping. Long-term tracking projects often combine modes. Sketch out where you'd get a hypothetical large dataset for a story you care about — what's public, what would need a request, what's only available by ongoing scraping?

### Practice Investigation
Pick a state legislative bill database. Build a GitHub Actions scraper that runs daily, commits changes to git, and notifies you when it breaks. Let it run for several weeks. Then come back and analyze what's accumulated — apply the question families to your longitudinal data.

---

## Week 6-7: Collaborative Investigation Infrastructure (OpenAleph)

### Context Reading
[ICIJ Cyprus Confidential methodology](https://www.icij.org/investigations/cyprus-confidential/leaked-data-journalism-methodology/), [How ICIJ Deals with Massive Data Leaks](https://datajournalism.com/read/handbook/two/working-with-data/how-icij-deals-with-huge-data-dumps-like-the-panama-and-paradise-papers), [How We Built the Data Team Behind the Panama Papers](https://source.opennews.org/articles/how-we-built-data-team-behind-panama-papers/).

### Learn the Tech

#### OpenAleph Public Instance
Browse the [OCCRP public Aleph instance](https://aleph.occrp.org/) — Companies House, sanctions lists, ICIJ Offshore Leaks entity graphs. Note: OCCRP is sunsetting active maintenance after Dec 2025; [OpenAleph](https://openaleph.org/) is the going-forward fork.

Pick a name (politician, real-estate developer, contractor) and search across datasets. The same `Person` may appear in sanctions, Companies House, and Offshore Leaks — that's the cross-dataset hit.

#### FollowTheMoney (FtM) Ontology
Read the [FtM project page](https://followthemoney.tech/). FtM is the schema that makes everything joinable: `Person`, `Company`, `Directorship`, `Vessel`, `Payment`. Once you understand FtM, you understand why Aleph isn't just a search engine — it's a graph that emerges from documents you never tagged.

#### Self-hosted OpenAleph + FtM CSV Mapping
For a learning exercise, run OpenAleph in Docker. Map a CSV (e.g., NYC ACRIS deeds) into FtM types using `ftm map` and ingest. Review what the automatic entity resolution caught and what it missed.

#### DocumentCloud (alternative)
For smaller, US-focused document sets, [DocumentCloud](https://www.documentcloud.org/) has lower friction. Upload a few PDFs and try the annotation features. Use this when Aleph would be overkill.

### Practice Investigation
Pick an entity related to a story you care about. Cross-reference across the public OCCRP instance — Companies House, sanctions, Offshore Leaks. Then ingest a small dataset of your own (city deeds, contract filings) into a self-hosted OpenAleph and write a "who owns this" memo for one specific case.

---

## Week 8: Networks for Investigations (Cypher)

### Context Reading
[ICIJ's Use of Graph Databases](https://neo4j.com/customer-stories/icij/), [Linkurious Panama Papers visualization](https://linkurious.com/blog/panama-papers-how-linkurious-enables-icij-to-investigate-the-massive-mossack-fonseca-leaks/), [GIJN: How They Did It — Paradise Papers](https://gijn.org/stories/paradise-papers/).

### Learn the Tech

#### Neo4j Sandbox
Set up a [Neo4j Sandbox](https://sandbox.neo4j.com/) — free, browser-based, no install. Follow the [Cypher manual](https://neo4j.com/docs/cypher-manual/current/) for basic pattern matching. A small graph:
```cypher
CREATE (p:Person {name: 'John Doe'})
CREATE (c:Company {name: 'Shell Corp'})
CREATE (p)-[:OWNS]->(c)
```

#### Pattern Matching for Path Tracing
The graph-shaped question:
```cypher
MATCH path = (p:Person {name: 'Politician'})-[*..4]-(c:Company)
WHERE c.is_sanctioned = true
RETURN path
```
"Find every path of length up to 4 between this politician and any sanctioned company."

Don't go deep on Cypher syntax. The skill is recognizing when the question shape is graph-shaped.

### Practice Investigation
Load the entity export from your Week 7 Aleph work plus a slice of the public ICIJ Offshore Leaks entity graph. Pose one question that's graph-shaped — typically a path between two entities through some number of intermediate connections. Trace it. Write up what additional data would verify the connection.

---

## Week 9: Build a Baby Aleph

### Context Reading
[Pandora Papers: Technology Behind the Investigation](https://linkurious.com/blog/technology-pandora-papers-investigation/) — what big tools do.

### Learn the Tech

#### AI Entity Extraction (Structured Outputs)
Get an Anthropic or OpenAI API key (a few dollars covers the exercise). Follow [structured outputs documentation](https://platform.openai.com/docs/guides/structured-outputs). Extract entities from a document set into a defined JSON schema:
```python
from anthropic import Anthropic
client = Anthropic()
# Force the response into a strict JSON schema
```

#### rapidfuzz for Fuzzy Matching
Install `pip install rapidfuzz`. Try string similarity on names with known duplicates:
```python
from rapidfuzz import fuzz
fuzz.ratio("Robert Izsak", "R. Izsak")  # 75
fuzz.ratio("Izsak Realty LLC", "Izsak Realty LLC c/o J Smith")  # 86
```

#### dedupe for Record Linkage
For larger entity-resolution problems, [dedupe](https://docs.dedupe.io/en/latest/) does active-learning record linkage — you label a few pairs, it learns the rest.

### Practice Investigation
Take a document set with known entity duplicates. Run entity resolution three ways:
1. AI structured outputs
2. rapidfuzz
3. dedupe (or your earlier OpenAleph result if you have one)

Measure each against ground-truth labels. Write up which approach won where, and why.

---

## Week 10: Public-Facing Tools + Security + Privacy + Methodology

### Context Reading
[ProPublica's Nonprofit Explorer](https://projects.propublica.org/nonprofits/), [Texas Tribune Salaries](https://salaries.texastribune.org/), [ProPublica's News App Guides](https://www.propublica.org/nerds/propublicas-news-app-guides), [The Markup's Show Your Work](https://themarkup.org/series/show-your-work), [ProPublica COMPAS methodology](https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm).

### Learn the Tech

#### Flask + SQLite (read-only)
Follow the [Flask quickstart](https://flask.palletsprojects.com/en/latest/quickstart/). For a public read-only tool, SQLite is simpler than Postgres and has no credential surface to leak:
```python
from flask import Flask, render_template, request
import sqlite3

app = Flask(__name__)

def db():
    return sqlite3.connect('investigation.db')

@app.route('/')
def search():
    q = request.args.get('q', '')
    if q:
        conn = db()
        cur = conn.cursor()
        # Parameterized query — never f-string user input
        cur.execute("SELECT * FROM records WHERE name LIKE ? LIMIT 100", (f'%{q}%',))
        results = cur.fetchall()
        return render_template('results.html', results=results, q=q)
    return render_template('search.html')
```

#### Render Deployment
Follow [Render's Flask deployment guide](https://render.com/docs/deploy-flask). The free tier handles classroom-scale traffic. (Heroku's free tier is gone; Render replaces it.)

#### Security Audit
Walk through your tool with the eyes of an attacker:
- Where could SQL injection enter? (User input, search params, URL params.)
- What credentials are in the deployed environment? (Should be read-only DB user only.)
- What PII is exposed? (Names? Addresses? Tax IDs?)
- What happens under load? (Ten queries/sec? A hundred?)

#### Methodology Page
Read three real methodology pages (above). Then write yours. It's a journalism deliverable, not documentation. Provenance, choices, limitations, what would change the conclusion.

### Practice Investigation
Build a simple public-facing tool from your earlier work. Deploy. Write the security audit. Write the methodology page. Test with a non-technical user.

---

## Week 11: AI Document Processing (multilingual)

### Context Reading
[Quartz: How Quartz used AI for Luanda Leaks](https://qz.com/1786896/ai-for-investigations-sorting-through-the-luanda-leaks), [How BuzzFeed News Analyzed the FinCEN Files](https://www.buzzfeednews.com/article/jsvine/fincen-files-explainer-data-money-transactions), [AI for Data Journalism (Simon Willison)](https://simonw.substack.com/p/ai-for-data-journalism-demonstrating).

### Learn the Tech

#### Structured Outputs at Scale
With a multilingual document set (or your own language pair), extract entities using structured outputs. Process 50-100 documents in batch. Note your prompt explicitly — prompt is methodology.

#### Verification at Scale
You can't read all 100 docs to verify. Strategies:
- Sample-based audit (random 10-20 docs read manually, computed precision/recall on the sample)
- Two-prompt agreement scoring (run twice with different framings; disagreement is a flag)
- Held-back ground truth from a colleague

#### Cost-Quality Tradeoffs
Run the same task with Sonnet and Haiku. Measure quality drop. Measure cost ratio. Write up where Haiku is good enough.

#### Local Options (briefly)
[LM Studio](https://lmstudio.ai/) for sensitive documents. Try a small model like Llama 3 8B for the same extraction task. Note what you give up.

### Practice Investigation
Take 50-100 multilingual documents. Extract entities with structured outputs. Verify against a hand-checked subset. Write up what AI got wrong, the cost analysis, and a methodology page documenting the workflow.

---

## Week 12: Methodology Article + Sustainability

### Context Reading
[Washington Post Fatal Force methodology](https://www.washingtonpost.com/investigations/2022/12/05/washington-post-fatal-police-shootings-methodology/), [The Markup's Show Your Work series](https://themarkup.org/series/show-your-work), [What We've Learned About Sharing Our Data Analysis](https://source.opennews.org/articles/what-weve-learned-about-sharing-our-data-analysis/), [AP's Datakit](https://github.com/associatedpress/datakit-core).

### Learn the Tech

#### Methodology as Journalism
The methodology article is the final exam. Pick the strongest project from the previous weeks. Read three real methodology articles cover-to-cover. Then write yours to the same standard:
- Provenance (where the data came from, how you got it)
- Choices (what you decided to count and not count, and why)
- Limitations (what your numbers can't say)
- What would change the conclusion (the falsifiability test)

1,000-1,500 words. Write to publish.

#### Bill Tracker Harvest
Your Week 4 scraper has been running for ~8 weeks. Pull together a longitudinal analysis. Apply the question families to longitudinal questions. Write a 500-word memo: what changed, what's a story candidate, what's noise.

#### Sustainability
Pick one project you'd actually maintain. Write the runbook: scheduled updates, alerting when broken, who picks it up if you leave. Keep this realistic — most data projects die because the maintenance plan was aspirational.

### Practice Project
Take one project you built earlier and:
1. Write its methodology article (publication-quality)
2. If it's longitudinal, harvest the accumulated data and write the findings memo
3. Write the sustainability plan

This is what shipping looks like.

---

## Final Integration Project

Combine the threads into one investigation. Pick a real, public, non-trivial dataset. Build:
- A queryable local copy (DuckDB, then Postgres if you need persistence)
- A scheduled scraper for ongoing updates (GitHub Actions)
- An entity-graph view of the relevant relationships (OpenAleph or a baby-Aleph you build)
- A public-facing read-only tool (Flask + SQLite + Render)
- A methodology article documenting all of it

This is the shape of a real ProPublica-tier project — at smaller scale, but with the right pieces.

---

## Additional Resources

- [News Nerdery Slack](https://newsnerdery.org/) — community support
- [Data Is Plural](https://www.data-is-plural.com/) — weekly dataset newsletter
- [Simon Willison's Blog](https://simonwillison.net/) — Datasette and data journalism updates
- [GIJN Resources](https://gijn.org/resources/) — global investigative journalism reference
- [The Markup's Show Your Work](https://themarkup.org/series/show-your-work) — recurring methodology

## Quick Start

For the core experience without doing everything: install DuckDB and Datasette, download an HMDA county slice, query it with DuckDB, publish it with Datasette, browse the [OCCRP public Aleph instance](https://aleph.occrp.org/), and read the [Cyprus Confidential methodology](https://www.icij.org/investigations/cyprus-confidential/leaked-data-journalism-methodology/). That's the essential workflow: making large datasets queryable, sharable, and joinable across investigations.
