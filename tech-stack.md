# Data and Databases: Technology Stack

A comprehensive list of all technologies mentioned in the course curriculum, with documentation and tutorials.

## Core Technologies (From Curriculum)

### Databases

#### DuckDB
- **Docs**: https://duckdb.org/docs/
- **Quickstart**: https://duckdb.org/docs/guides/python/install
- **Week**: 1-2
- **Why**: Query CSVs directly without importing, bridges pandas and SQL

#### PostgreSQL
- **Docs**: https://www.postgresql.org/docs/
- **Tutorial**: https://www.postgresqltutorial.com/
- **Week**: 1-2, 3-4, 9-10
- **Why**: Persistence, multi-user access, production databases

#### SQLite
- **Docs**: https://www.sqlite.org/docs.html
- **Tutorial**: https://www.sqlitetutorial.net/
- **Week**: Implied with Datasette
- **Why**: File-based database that powers Datasette

#### BigQuery
- **Docs**: https://cloud.google.com/bigquery/docs
- **Quickstart**: https://cloud.google.com/bigquery/docs/quickstarts
- **Week**: 3-4
- **Why**: For truly massive datasets (mentioned as "know it exists")

### Cloud Infrastructure

#### Digital Ocean
- **Managed Databases**: https://docs.digitalocean.com/products/databases/
- **Getting Started**: https://docs.digitalocean.com/products/databases/postgresql/quickstart/
- **Week**: 3-4
- **Why**: Managed Postgres option

#### AWS S3
- **Docs**: https://docs.aws.amazon.com/s3/
- **Getting Started**: https://aws.amazon.com/s3/getting-started/
- **Week**: 3-4
- **Why**: File storage for processed data

#### Google Cloud Storage
- **Docs**: https://cloud.google.com/storage/docs
- **Quickstart**: https://cloud.google.com/storage/docs/quickstart-console
- **Week**: 3-4
- **Why**: Alternative to S3

#### Backblaze B2
- **Docs**: https://www.backblaze.com/b2/docs/
- **Week**: 3-4
- **Why**: Affordable S3-compatible storage

#### boto3
- **Docs**: https://boto3.amazonaws.com/v1/documentation/api/latest/
- **S3 Guide**: https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3.html
- **Week**: 3-4
- **Why**: Python SDK for S3/cloud storage

### Automation & Workflows

#### GitHub Actions
- **Docs**: https://docs.github.com/en/actions
- **Quickstart**: https://docs.github.com/en/actions/quickstart
- **Python Guide**: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python
- **Week**: 5-6, 13
- **Why**: Automated scrapers, monitoring, scheduled updates

#### Git
- **Pro Git Book**: https://git-scm.com/book/en/v2
- **GitHub Desktop**: https://desktop.github.com/
- **Week**: 5-6 (git-scraping mentioned)
- **Why**: Version control, tracking data changes

### Data Tools & Publishing

#### Datasette
- **Docs**: https://docs.datasette.io/
- **Getting Started**: https://docs.datasette.io/en/stable/getting_started.html
- **Tutorial**: https://datasette.io/tutorials/
- **Week**: 7-8
- **Why**: Internal SQL exploration, data sharing

#### Flask
- **Docs**: https://flask.palletsprojects.com/
- **Quickstart**: https://flask.palletsprojects.com/en/latest/quickstart/
- **Tutorial**: https://flask.palletsprojects.com/en/latest/tutorial/
- **Week**: 9-10
- **Why**: Building public-facing data tools

#### Jinja2
- **Docs**: https://jinja.palletsprojects.com/
- **Tutorial**: https://jinja.palletsprojects.com/en/latest/intro/
- **Week**: 9-10
- **Why**: HTML templates for Flask

### Document Processing & Investigation

#### Aleph/OpenAleph
- **Docs**: https://docs.aleph.occrp.org/
- **Getting Started**: https://docs.aleph.occrp.org/developers/installation/
- **Week**: 7-8
- **Why**: Pre-populated instances for document investigations

#### DocumentCloud
- **Platform**: https://www.documentcloud.org/
- **API Docs**: https://www.documentcloud.org/help/api
- **Week**: 7-8
- **Why**: Smaller document sets, US-focused

### AI & Machine Learning

#### OpenAI API
- **Docs**: https://platform.openai.com/docs
- **Quickstart**: https://platform.openai.com/docs/quickstart
- **Structured Outputs**: https://platform.openai.com/docs/guides/structured-outputs
- **Week**: 11-12
- **Why**: Entity extraction, embeddings

#### Anthropic API
- **Docs**: https://docs.anthropic.com/
- **Getting Started**: https://docs.anthropic.com/claude/docs/getting-started
- **Week**: 11-12
- **Why**: Alternative for entity extraction, embeddings

#### Google NotebookLM
- **Platform**: https://notebooklm.google.com/
- **Guide**: https://support.google.com/notebooklm
- **Week**: 11-12
- **Why**: Initial document exploration with citations

#### LM Studio
- **Download**: https://lmstudio.ai/
- **Docs**: https://lmstudio.ai/docs
- **Week**: 11-12
- **Why**: Local LLMs for sensitive documents

#### Haystack
- **Docs**: https://docs.haystack.deepset.ai/
- **Tutorial**: https://haystack.deepset.ai/tutorials/01_basic_qa_pipeline
- **Week**: 11-12
- **Why**: Simple RAG patterns

#### pgvector
- **Docs**: https://github.com/pgvector/pgvector
- **Tutorial**: https://github.com/pgvector/pgvector/blob/master/README.md#getting-started
- **Week**: 11-12 (if ambitious)
- **Why**: Vector similarity search in Postgres

## Prerequisites (From Week 0)

### Python & Libraries
#### pandas
- **Docs**: https://pandas.pydata.org/docs/
- **10 Minutes Tutorial**: https://pandas.pydata.org/docs/user_guide/10min.html
- **Week**: Prerequisite
- **Why**: Data manipulation (students should know this)

#### requests
- **Docs**: https://requests.readthedocs.io/
- **Quickstart**: https://requests.readthedocs.io/en/latest/user/quickstart/
- **Week**: Prerequisite
- **Why**: Web scraping (students should know this)

### Version Control
#### GitHub Desktop
- **Download**: https://desktop.github.com/
- **Docs**: https://docs.github.com/en/desktop
- **Week**: Prerequisite
- **Why**: GUI for Git (students should know this)

## Additional/Optional Tools

### PostGIS
- **Docs**: https://postgis.net/documentation/
- **Tutorial**: https://postgis.net/workshops/postgis-intro/
- **Week**: 1-2 (Nice-to-have)
- **Why**: Redistricting analysis

### JSON Processing
#### Python json module
- **Docs**: https://docs.python.org/3/library/json.html
- **Week**: 5-6
- **Why**: Configuration files mentioned

## Bonus Command-Line Tools
*Not in curriculum but useful for data journalists*

### ripgrep (rg)
- **Docs**: https://github.com/BurntSushi/ripgrep
- **User Guide**: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md
- **Why**: Fastest text search, better than grep

### xsv
- **Docs**: https://github.com/BurntSushi/xsv
- **Quick Examples**: https://github.com/BurntSushi/xsv#quick-examples
- **Why**: Lightning-fast CSV operations

### miller
- **Docs**: https://miller.readthedocs.io/
- **10 Minute Tutorial**: https://miller.readthedocs.io/en/latest/10min/
- **Why**: Like awk/sed but for structured data

### jq
- **Docs**: https://stedolan.github.io/jq/
- **Tutorial**: https://stedolan.github.io/jq/tutorial/
- **Why**: Query and transform JSON

### csvkit
- **Docs**: https://csvkit.readthedocs.io/
- **Tutorial**: https://csvkit.readthedocs.io/en/latest/tutorial.html
- **Why**: Swiss army knife for CSVs

## Notes

- **Starred items (*)** in the curriculum indicate recommended/primary tools
- This list only includes tools explicitly mentioned in curriculum.md
- Prerequisites are assumed from first semester
- Cloud service free tiers should be sufficient for coursework