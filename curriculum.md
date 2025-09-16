# Data and Databases Curriculum revamp

Data and Databases is a second-semester core course in the Columbia Journalism School Data Journalism MS. After a first semester spend learning both writing/reporting skills and fundamental Python data journalism skills (scraping, pandas, etc) students move into more advanced possibilities with larger datasets.

## Context

This course is directed at dealing with larger data sets, whether sourced from official sources or reader-submitted. Concurrently students are working on a stats course, a data-driven projects class (more daily news-ish) and several reporting classes.

## Course Overview

This course teaches students to work with datasets that require more than pandas and a laptop - government databases with millions of records, document dumps from investigations, and long-term data projects that multiple journalists need to access. The focus is on the ProPublica model: acquire a significant dataset, analyze it thoroughly over time, and build public-facing tools that both tell stories and let readers explore the data themselves.

### Core Philosophy
- **Start with the story**: Every technical decision should enable better journalism
- **Build for longevity**: Many investigations take months; your data infrastructure should last
- **Assume messiness**: Government data is dirty, incomplete, and inconsistently formatted
- **Plan for collaboration**: You'll rarely work alone on big investigations
- **Think about readers**: How will the public interact with your findings?

## Week 0: Prerequisites

Students should arrive comfortable with:
- **Python basics**: pandas for data manipulation, requests for web scraping
- **Git fundamentals**: Using GitHub Desktop for version control
- **Command line basics**: Navigating directories, running scripts
- **SQL exposure**: Basic SELECT statements (we'll build from here)
- **Data formats**: CSVs, JSON, basic Excel

If you can't do these, review first-semester materials before class begins.

## Weekly Breakdown

### Week 1-2: Working with Large Government Databases
**Core Question**: How do you handle datasets too big for Excel but not worth the cloud?

**Journalistic Context**: 
You've just received HMDA mortgage data (25 million rows), PPP loans (11 million rows), or Medicare payments to doctors. These datasets are too large for pandas to handle comfortably but don't require distributed computing. You need to quickly explore them, find patterns, and identify potential stories without waiting for cloud infrastructure.

**Must-Haves**:
- **Concepts**: Understanding when a dataset is truly "big" vs just "bigger than Excel", memory vs disk trade-offs
- **Technical Skills**:
  - The progression from pandas to SQL:
    1. Start with pandas, hit memory limits on large CSV
    2. Use DuckDB to query that same CSV directly*
    3. Learn SQL patterns through DuckDB's familiar interface
    4. Move to Postgres when you need persistence/sharing
    5. Connect DuckDB to Postgres for best of both worlds
  - Quick analysis patterns:
    - Finding outliers in government spending
    - Identifying geographic patterns
    - Time series analysis for trends
  - Command-line alternatives (optional but powerful):
    - xsv for ultra-fast CSV operations
    - miller for CSV/JSON transformations
    - csvkit for quick CSV exploration

**Stories This Enables**:
- Redlining patterns in mortgage data
- PPP fraud detection through anomaly analysis
- Doctor payment outliers suggesting fraud

**Nice-to-Haves**:
- PostGIS for redistricting analysis

### Week 3-4: When and Why to Use the Cloud
**Core Question**: When is it worth moving beyond your laptop?

**Journalistic Context**:
Three scenarios force you to the cloud: (1) Your investigation team needs to query the same database simultaneously, (2) You're building something the public will access (like ProPublica's Nonprofit Explorer), or (3) You're processing data continuously (like election results). The cloud isn't about "big data" - it's about access and availability.

**Must-Haves**:
- **Concepts**: 
  - When cloud makes sense (collaboration, public access, continuous processing)
  - When it doesn't (one-off analysis, sensitive data, budget constraints)
  - Real costs using actual newsroom examples
- **Technical Skills**:
  - Shared databases
    - Managed Postgres (DigitalOcean?)*
    - Understanding connection strings and permissions
    - BigQuery for truly massive datasets (know it exists)
  - File storage for documents/data
    - Backblaze B2* (simpler and cheaper than S3)
    - Use boto3 with B2's S3-compatible API
    - Organizing investigation materials (naming conventions, versioning)
  - Quick cloud exploration
    - Datasette Cloud for instant database sharing
    - TablePlus for GUI access to Postgres

**Stories This Enables**:
- Collaborative investigations like Panama Papers (shared database)
- Public databases like ProPublica's Nonprofit Explorer
- Live election results tracking

**Nice-to-Haves**:
- Budget alerts (don't accidentally spend $1000)
- Backup strategies for critical investigations

### Week 5-6: Long-term Data Projects
**Core Question**: How do you manage datasets you'll work with for months?

**Journalistic Context**:
Most important investigations aren't built on breaking news - they're built on systematic analysis of data over time. ProPublica's Dollars for Docs took months to clean, verify, and build. The LA Times' LAPD crime statistics investigation required comparing years of reports. You need systems that let you incrementally improve data quality, track your cleaning decisions, and update analyses as new data arrives.

**Must-Haves**:
- **Concepts**: 
  - Data versioning (tracking how your dataset changes as you clean it)
  - Reproducibility (can another reporter verify your work?)
  - Documentation as you go (you won't remember why you excluded those rows)
- **Technical Skills**:
  - GitHub Actions for automation*
    - Daily scrapers that run automatically
    - Screenshot monitoring for website changes
    - Building your own git-scraping workflow
    - Error notifications when scrapers break
  - Data pipeline patterns
    - Python scripts for cleaning/transformation
    - SQL for analysis
    - JSON for configuration
  - Change detection
    - Comparing yesterday's data to today's
    - Finding new records, deleted records, modifications
    - Alerting on significant changes

**Stories This Enables**:
- Long-term tracking of police complaints
- Monitoring changes in government databases
- Building institutional knowledge about datasets

**Nice-to-Haves**:
- Data quality tests
- Automated fact-checking of new data

### Week 7-8: Collaborative Investigation Infrastructure
**Core Question**: How do you coordinate when five reporters are working on the same dataset?

**Journalistic Context**:
Big investigations require teamwork. During the Panama Papers, hundreds of journalists needed to search the same document set without stepping on each other's toes. For local investigations, you might have reporters in different cities analyzing the same police data. You need systems where reporters can explore data, claim stories, share findings, and avoid duplication.

**Must-Haves**:
- **Concepts**: 
  - Division of labor in data projects
  - Understanding different investigation platforms for different data types
  - Maintaining chain of custody for sensitive data
- **Technical Skills**:
  - Exploration and research tools
    - Datasette* for internal SQL exploration (safe, no DELETE accidents)
    - Datasette Cloud for quick sharing without infrastructure
    - Pre-populated Aleph instances for document investigations
    - DocumentCloud for smaller document sets
  - Understanding investigation platforms
    - When to use Aleph (massive leaks, entity extraction needed)
    - When to use DocumentCloud (smaller sets, US-focused)
    - When to use Datasette (structured data exploration)
  - Entity extraction basics
    - Understanding what Aleph does under the hood

**Stories This Enables**:
- Multi-newsroom collaborations
- Document dumps like the Facebook Papers
- Coordinated local-national investigations

**Nice-to-Haves**:
- Entity resolution across datasets
- Secure drop integration

### Week 9-10: Building Public-Facing Data Tools
**Core Question**: How do you let readers explore your investigation?

**Journalistic Context**:
The best data investigations don't just tell one story - they provide tools for readers to find their own stories. ProPublica's Nonprofit Explorer lets anyone research tax-exempt organizations. The Texas Tribune's salary database lets citizens see what public employees earn. These tools extend your journalism's impact and build trust through transparency.

**Must-Haves**:
- **Concepts**: 
  - Designing for non-technical users
  - Privacy and ethical considerations (doxxing, harassment)
  - Maintenance burden (who updates this in 2 years?)
- **Technical Skills**:
  - Building with Flask*
    - Simple routes that query Postgres
    - Jinja2 templates for HTML (no React needed)
    - Form handling for search/filters
    - Static file generation for high traffic
  - Database patterns for public tools
    - Read-only database users
    - Caching frequent queries
    - Pagination for large result sets
  - Search and filtering
    - SQL-based full-text search
    - Faceted search with counts
    - CSV download endpoints

**Stories This Enables**:
- Salary databases for government transparency
- School test score comparisons
- Campaign finance lookup tools
- Hospital quality comparisons

**Nice-to-Haves**:
- Embed widgets for other newsrooms
- API access for developers

### Week 11-12: Document Intelligence at Scale
**Core Question**: How do you make 100,000 documents searchable and explorable?

**Journalistic Context**:
You've received a massive FOIA dump: 100,000 PDFs from city emails, or thousands of contracts in multiple languages (like Quartz's Luanda Leaks investigation of Angolan documents in Portuguese). Traditional keyword search fails because bureaucrats use euphemisms, documents are in different languages, and what you're looking for might be described a dozen different ways. AI excels at this specific task: making unstructured documents structured and searchable.

**Must-Haves**:
- **Concepts**: 
  - Semantic search vs keyword search (finding meaning, not just words)
  - Citation and verification (every AI claim needs a source)
  - Cost/speed tradeoffs (processing 100,000 docs isn't free)
  - What AI can't do: counting, math, facts without sources
- **Technical Skills**:
  - Bulk document processing
    - Entity extraction with OpenAI/Anthropic structured outputs*
    - Batch processing strategies to manage costs
    - Language detection and translation workflows
  - Entity extraction
    - Using LLMs for simple NER tasks (or maybe move to later session)
    - Structured outputs from OpenAI/Anthropic APIs
  - Semantic search (simplified)
    - Creating embeddings with OpenAI/Anthropic APIs
    - Storing embeddings in Postgres (maybe pgvector if ambitious)
    - Finding similar documents without complex infrastructure
  - Practical tools
    - NotebookLM* for initial exploration (always verify citations)
    - Local LLMs with LMStudio for sensitive documents
    - Simple RAG patterns with Haystack or similar

**Real Investigations**:
- Quartz's Luanda Leaks (Portuguese documents made searchable in English)
- ICIJ's FinCEN Files (finding similar suspicious activity reports)
- Reuters' Guantanamo Bay documents (extracting prisoner details from PDFs)

**Stories This Enables**:
- Cross-border investigations with multilingual documents
- Finding similar patterns across thousands of police reports
- Extracting structured data from government PDFs
- Making historical archives searchable

**Nice-to-Haves**:
- OCR integration for scanned documents
- pgvector for more advanced similarity search
- Fine-tuning for specific journalism tasks

### Week 13: Sustainability and Handoffs
**Core Question**: What happens to your data project after you publish?

**Journalistic Context**:
Many data projects die after publication. The reporter moves on, the data gets stale, the tools break. But the best investigations live on - updated annually, used by other newsrooms, cited in lawsuits. Building sustainably from the start makes this possible.

**Must-Haves**:
- **Concepts**: 
  - Documentation as journalism (methodology articles)
  - Handoff planning (what if you leave?)
  - Maintenance burden vs impact
- **Technical Skills**:
  - Automation
    - GitHub Actions* for scheduled updates
    - Error alerting when things break
    - Automated testing of data assumptions
  - Documentation
    - README files that explain everything*
    - Data dictionaries for complex datasets
    - Contact information for sources
  - Archiving
    - When to sunset a project
    - How to preserve data for future use
    - Making data downloadable for others

**Stories This Enables**:
- Annual updates to salary databases
- Long-term tracking projects
- Building institutional memory

**Nice-to-Haves**:
- Usage analytics to track impact
- Automated reports to editors

## Capstone Project Prep

Students leave with a template repository containing:
- Pre-configured database setup (DuckDB + Postgres)
- Basic ETL pipeline (GitHub Actions + Python)
- Datasette publishing configuration
- Documentation templates
- Verification checklists

This foundation can power their capstone project in the following semester.