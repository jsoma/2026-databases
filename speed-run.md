# Speed-Run Guide: Data and Databases for Journalism

A self-study guide to learn the complete data journalism tech stack through hands-on projects and real investigations.

## Prerequisites

### Python and pandas basics
Load any CSV, do a groupby operation. If you need a refresher: [10 minute pandas guide](https://pandas.pydata.org/docs/user_guide/10min.html)

### Git fundamentals  
Clone a repo, make changes, commit them. If you need a refresher: [GitHub Desktop tutorial](https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop)

### Command line basics
Navigate directories, run Python scripts. If you need a refresher: [Command line crash course](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Command_line)

## Week 1-2: Large Local Databases

### Context Reading
Read [ProPublica's Dollars for Docs methodology](https://projects.propublica.org/docdollars/) and skim [Tips for Building a Database for Investigations](https://gijn.org/resource/tips-for-building-a-database-for-investigations/).

### Learn the Tech

#### DuckDB Basics
Install with `pip install duckdb`. Follow the [DuckDB Python API quickstart](https://duckdb.org/docs/api/python/overview). Download [PPP loan data sample](https://data.sba.gov/dataset/ppp-foia) (just one state) and try this:
   ```python
   import duckdb
   conn = duckdb.connect()
   # Query CSV directly without loading into memory!
   result = conn.execute("SELECT * FROM 'ppp_data.csv' WHERE LoanAmount > 1000000").fetchdf()
   ```

#### Transition to Postgres
Install [Postgres.app](https://postgresapp.com/) (Mac) or [PostgreSQL](https://www.postgresql.org/download/). Follow the [PostgreSQL tutorial](https://www.postgresqltutorial.com/). Import that same PPP data into Postgres:
   ```bash
   createdb ppp_analysis
   psql ppp_analysis -c "CREATE TABLE loans (...);"
   # Use DuckDB to export to Postgres
   ```

#### Command-line alternatives
Install xsv with `brew install xsv` or download from [GitHub](https://github.com/BurntSushi/xsv). Try `xsv stats ppp_data.csv | xsv table` for instant statistics on any CSV.

### Practice Investigation
Download HMDA mortgage data for one county from [CFPB](https://ffiec.cfpb.gov/data-publication/). Use DuckDB to find top 10 lenders by volume, denial rates by race, and average loan amounts by census tract.

## Week 3-4: Cloud Infrastructure

### Context Reading
[How BuzzFeed Found Spy Planes](https://www.cjr.org/watchdog/how-buzzfeed-news-revealed-hidden-spy-planes-in-us-airspace.php) and [The Markup's Blacklight infrastructure](https://themarkup.org/blacklight/2020/09/22/how-we-built-a-real-time-privacy-inspector).

### Learn the Tech

#### Quick Win: Datasette Cloud
Start with [Datasette Cloud](https://datasette.cloud/) - upload your SQLite database and instantly share it with collaborators. No setup required, free tier available. This is the fastest way to understand cloud benefits.

#### Cloud Storage for Files
When you have many documents (PDFs, images, data files) to share with a team, use [Backblaze B2](https://www.backblaze.com/b2/). It's simpler than AWS S3 and much cheaper:
   ```python
   import boto3
   s3 = boto3.client('s3',
       endpoint_url='https://s3.us-west-002.backblazeb2.com',
       aws_access_key_id='your_key',
       aws_secret_access_key='your_secret')
   # Upload investigation documents
   s3.upload_file('document.pdf', 'investigation-bucket', 'docs/document.pdf')
   ```

#### Shared Database Access
Create a managed Postgres instance on DigitalOcean ($15/month). Instead of psql, use [TablePlus](https://tableplus.com/) for a friendly GUI. Import your local PPP data, then share read-only credentials with collaborators.

### Practice Investigation
Upload a SQLite database to Datasette Cloud. Share the link with someone else. Both query the same data simultaneously without any setup on their end.

## Week 5-6: Automation & Long-term Projects

### Context Reading
[Git scraping explained](https://simonwillison.net/2021/Mar/5/git-scraping/) and browse [Simon's PG&E scraper](https://github.com/simonw/pge-outages) with 40,000+ commits tracking power outages.

### Learn the Tech

#### GitHub Actions Basics
Follow the [GitHub Actions quickstart](https://docs.github.com/en/actions/quickstart). Create a workflow that runs daily:
   ```yaml
   name: Daily scraper
   on:
     schedule:
       - cron: '0 12 * * *'  # Noon daily
   jobs:
     scrape:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         - run: python scrape.py
         - run: |
             git add data.csv
             git commit -m "Update data"
             git push
   ```

#### Change Detection
Build a simple Python script that compares yesterday's data to today's. Use git diff to see what changed. Track a government webpage daily and alert when it changes.

### Practice Investigation
Pick a government data source that updates regularly (COVID data, crime stats, etc.). Build a GitHub Action that downloads the data daily, compares to yesterday, commits changes to git, and creates an issue if something significant changed.

## Weeks 7-8: Collaborative Investigation

### Context Reading
[How ICIJ coordinated Panama Papers](https://www.icij.org/investigations/panama-papers/) and [Inside Cyprus Confidential methodology](https://www.icij.org/investigations/cyprus-confidential/leaked-data-journalism-methodology/).

### Learn the Tech

#### Datasette for Exploration
Install with `pip install datasette`. Follow [Datasette getting started](https://docs.datasette.io/en/stable/getting_started.html). Convert your PPP database to SQLite and publish with Datasette:
   ```bash
   # Convert CSV to SQLite
   sqlite-utils insert ppp.db loans ppp_data.csv --csv
   # Launch Datasette
   datasette serve ppp.db
   ```

#### Document Processing Overview
Explore [DocumentCloud](https://www.documentcloud.org/) by creating a free account. Upload a few PDFs and try the annotation features. Look at the [Aleph demo instance](https://aleph.occrp.org/) (read-only) to understand how large-scale document processing works.

### Practice Investigation
Take your PPP loan database, publish it with Datasette, create saved queries for common questions, and share with a colleague for exploration.

## Week 9: Graph Databases

### Context Reading
[ICIJ's Use of Graph Databases](https://neo4j.com/customer-stories/icij/) and [Linkurious Panama Papers visualization](https://linkurious.com/blog/panama-papers-how-linkurious-enables-icij-to-investigate-the-massive-mossack-fonseca-leaks/).

### Learn the Tech

#### Neo4j Basics
Set up [Neo4j Sandbox](https://sandbox.neo4j.com/) (free, no install). Follow the [Cypher tutorial](https://neo4j.com/docs/cypher-manual/current/). Create a network of people, companies, and transactions:
   ```cypher
   CREATE (p:Person {name: 'John Doe'})
   CREATE (c:Company {name: 'Shell Corp'})
   CREATE (p)-[:OWNS]->(c)
   ```

#### Network Analysis
Find shortest paths, most connected nodes, and circular flows. Export visualizations for stories.

### Practice Investigation
Load investigation data with entities and relationships. Find hidden connections between politicians and companies through shell corporations.

## Weeks 10-11: Public-Facing Tools

### Context Reading
Explore [ProPublica's Nonprofit Explorer](https://projects.propublica.org/nonprofits/), [Texas Tribune Salaries](https://salaries.texastribune.org/), and [ProPublica's News App guides](https://github.com/propublica/guides).

### Learn the Tech

#### Flask Basics
Follow the [Flask quickstart](https://flask.palletsprojects.com/en/latest/quickstart/). Build a simple search interface:
   ```python
   from flask import Flask, render_template, request
   import psycopg2
   
   app = Flask(__name__)
   
   @app.route('/')
   def search():
       query = request.args.get('q')
       if query:
           # Search database
           conn = psycopg2.connect("...")
           cur = conn.cursor()
           cur.execute("SELECT * FROM loans WHERE borrower ILIKE %s", (f'%{query}%',))
           results = cur.fetchall()
           return render_template('results.html', results=results)
       return render_template('search.html')
   ```

#### Jinja2 Templates
Create basic HTML templates with Jinja2. Add pagination, filters, and CSV download functionality.

### Practice Investigation
Build a simple public-facing tool for your PPP data with search by company name, filter by loan amount ranges, CSV download, and deployment to Heroku free tier or GitHub Pages.

## Weeks 12-13: AI Document Processing

### Context Reading
[How Quartz used AI for Luanda Leaks](https://qz.com/1786896/ai-for-investigations-sorting-through-the-luanda-leaks) and [BuzzFeed FinCEN Files methodology](https://www.buzzfeednews.com/article/jsvine/fincen-files-explainer-data-money-transactions).

### Learn the Tech

#### NotebookLM for Exploration
Upload documents to [NotebookLM](https://notebooklm.google.com/). Ask questions and note how it cites sources. Upload 10 government reports and ask investigative questions.

#### Entity Extraction with AI
Get an OpenAI API key (a few dollars of credits). Follow the [Structured outputs tutorial](https://platform.openai.com/docs/guides/structured-outputs). Extract entities from 100 documents:
   ```python
   from openai import OpenAI
   client = OpenAI()
   
   response = client.chat.completions.create(
       model="gpt-4",
       messages=[{"role": "user", "content": f"Extract all people, companies, and dollar amounts from: {document_text}"}],
       response_format={"type": "json_object"}
   )
   ```

#### Local LLMs for Sensitive Docs
Download [LM Studio](https://lmstudio.ai/) and a small model like Llama 3.2 3B. Try the same entity extraction locally for sensitive documents.

### Practice Investigation
Take 50-100 PDFs (FOIA documents, court records, etc.). Use AI to extract all mentioned entities, build a simple database of people, organizations, and dates, then find connections between documents.

## Week 14: Sustainability

### Context Reading
[What We've Learned About Sharing Our Data Analysis](https://source.opennews.org/articles/what-weve-learned-about-sharing-our-data-analysis/) and [AP's Datakit](https://github.com/associatedpress/datakit).

### Learn the Tech

#### Documentation
Create a README template for data projects. Include data sources, methodology, update schedule, and contact information.

#### Automation for Longevity
Set up GitHub Actions for weekly data updates. Add error notifications (email or Slack) and create data quality checks.

### Practice Project
Take one of your earlier projects and make it sustainable. Write complete documentation, add automated updates, create a handoff document, and set up alerts for when things break.

## Final Integration Project

Combine everything into one investigation. Find a large government dataset (education, health, crime), load it with DuckDB/Postgres, set up automated updates with GitHub Actions, publish an exploration tool with Datasette, build a public interface with Flask, and document everything.

## Different Starting Points

### Strong on coding, new to journalism
Start by reading all the investigation case studies to understand the journalistic context.

### Strong on journalism, new to these tools
Work through the mini-projects in order, focusing on the technical tutorials.

### Need to get up to speed quickly
Jump to the Final Integration Project and learn by doing.

## Additional Resources

Join [News Nerdery Slack](https://newsnerdery.org/) for community support. Subscribe to [Data Is Plural](https://www.data-is-plural.com/) for weekly dataset discoveries. Follow [Simon Willison](https://simonwillison.net/) for Datasette updates. Browse [NICAR-L archives](https://www.ire.org/nicar-l-mailing-list/) for real questions from working journalists.

## Quick Start

If you want to get the core experience quickly, install DuckDB and Datasette, download some government data, query it with DuckDB, publish it with Datasette, and read about one major investigation like Panama Papers or the Washington Post's police shootings database. This gives you the essential workflow: making large datasets accessible for investigation and public use.