# Data and Databases: Assignments

14-week assignment structure with Foundation (required), Extension (recommended), and Innovation (optional) tiers. Each week includes technical work and written reflection.

## For Instructors

Each assignment includes:
- **Instructor Provides**: Materials/templates you supply (with low-prep and high-prep options)
- **Time Estimate**: How long students typically need
- **Common Issues**: Problems to anticipate
- **Writing Prompt**: Reflection focusing on journalistic applications

Choose the instructor prep level that fits your schedule.

## Weeks 1-2: Large Government Databases
[Curriculum](curriculum.md#weeks-1-2-working-with-large-government-databases) | [Tech stack](tech-stack.md#databases)

### Instructor Provides
**Low-prep option**: Links to PPP data, sample queries
**High-prep option**: Pre-downloaded state data, DuckDB tutorial notebook, pandas comparison code

### Foundation: Query Without Crashing
**Dataset**: One month of PPP loans from your state

**Technical**:
1. Load data into DuckDB (not memory)
2. Answer three questions:
   - Which city received the most loan money?
   - What's the average loan by industry?
   - Find suspicious patterns (you define "suspicious")

**Writing Prompt** (300 words): "What story would you pursue based on your findings? What additional data would you need?"

### Extension: Pandas vs SQL Workflow
Demonstrate when to use each tool:
- Try loading full dataset in pandas (document what happens)
- Use SQL to filter, then pandas for visualization
- Time both approaches

### Innovation: Build a Flag System
Create reusable code that flags suspicious loans based on patterns you identified.

**Common Issues**: Memory errors with pandas, SQL syntax confusion, file path problems

## Weeks 3-4: Cloud Infrastructure  
[Curriculum](curriculum.md#weeks-3-4-when-and-why-to-use-the-cloud) | [Tech stack](tech-stack.md#cloud-infrastructure)

### Instructor Provides
**Low-prep**: Datasette Cloud tutorial, sample investigation documents
**High-prep**: Pre-configured Postgres with real data, B2 bucket with document collection

### Foundation: Investigation Infrastructure
1. Upload analysis database to Datasette Cloud
2. Create saved queries your editor would actually use
3. Store 20+ investigation documents in B2 (PDFs, images, spreadsheets)
4. Share both with a partner who acts as your editor

**Writing Prompt** (300 words): "Your editor is in DC, you're in NYC, and a source in Miami just sent documents. How does cloud infrastructure change this investigation? What couldn't you do before?"

### Extension: Multi-User Database
- Set up shared Postgres for team investigation
- Create read-only access for fact-checkers
- Build simple intake form for new data

### Innovation: Cost-Benefit Analysis
Calculate actual costs for a 6-month investigation with 5TB of documents and 3 reporters.

**Common Issues**: Not understanding read vs write permissions, B2 bucket naming restrictions

## Weeks 5-6: Automation & Scraping
[Curriculum](curriculum.md#weeks-5-6-long-term-data-projects) | [Tech stack](tech-stack.md#automation--workflows)

### Instructor Provides
**Low-prep**: GitHub Actions template, government sites that update daily
**High-prep**: Working scraper example, change detection code, alert system

### Foundation: Build a News Scraper
Create GitHub Action that scrapes a government website daily.

**Technical**: 
- Scrape agency press releases or data updates
- Store each day's version
- After one week, identify what changed and when

**Writing Prompt** (300 words): "A source claims the government quietly changed policy last week. How would your scraper help verify this? What other sites would you start tracking?"

### Extension: Smart Change Detection
- Detect meaningful changes (not just timestamps)
- Create alerts for specific keywords appearing
- Build changelog showing what was added/removed

### Innovation: Investigation Memory
Build system tracking multiple related sites to spot coordinated changes.

**Common Issues**: Sites blocking GitHub IPs, dynamic content that changes every load

## Weeks 7-8: Collaborative Investigation Infrastructure
[Curriculum](curriculum.md#weeks-7-8-collaborative-investigation-infrastructure) | [Readings](readings.md#weeks-7-8-collaborative-investigation-infrastructure) | [Tech](tech-stack.md#document-processing--investigation)

### Instructor Provides
**Low-prep**: Sample documents, shared spreadsheet, Datasette instance
**High-prep**: Pre-configured Aleph/DocumentCloud, shared database, coordination platform

### Foundation: Document Processing and Team Coordination
**Week 7 Focus - Document Triage**:
- Process 50 documents into structured format
- Classify by relevance and story potential
- Extract key entities and dates
- Build searchable index

**Week 8 Focus - Team Coordination**:
- Divide document set among team
- Claim specific topics/entities to investigate
- Document findings in shared system
- Identify patterns across documents

**Writing Prompt** (500 words): "Your team has 10,000 documents to review in 2 weeks. Design the complete workflow: How do you divide work, avoid duplication, surface important findings, and ensure nothing is missed?"

### Extension: Investigation Infrastructure
- Set up Aleph or DocumentCloud instance
- Build entity extraction pipeline
- Create automated classification rules
- Design tip intake system

### Innovation: Cross-Dataset Analysis
Connect document findings with structured databases to find patterns.

**Common Issues**: PDF extraction failures, inconsistent classification, coordination breakdowns

## Week 9: Graph Databases and Network Analysis
[Curriculum](curriculum.md#week-9-graph-databases) | [Readings](readings.md#week-9-graph-databases) | [Tech](tech-stack.md#databases)

### Instructor Provides
**Low-prep**: Neo4j Sandbox account, sample network data
**High-prep**: Pre-populated graph database, investigation data, Cypher query library

### Foundation: Map the Network
**Technical**:
- Load entity data into Neo4j (people, companies, transactions)
- Find shortest paths between entities
- Identify most connected nodes
- Query for circular money flows
- Visualize key relationships

**Writing Prompt** (300 words): "You discovered a company connected to 5 politicians through 3 shell companies. How do you verify these connections are real? What additional data would strengthen the story?"

### Extension: Advanced Network Analysis
- Export from Aleph to Neo4j
- Calculate network centrality metrics
- Find hidden clusters
- Build automated alert for new connections

### Innovation: Investigation Graph Tool
Build interface for non-technical reporters to explore network connections.

**Common Issues**: Cypher syntax confusion, relationship directionality, scale visualization

## Weeks 10-11: Public-Facing Tools
[Curriculum](curriculum.md#weeks-10-11-building-public-facing-data-tools) | [Readings](readings.md#weeks-10-11-building-public-facing-data-tools) | [Tech stack](tech-stack.md#data-tools--publishing)

### Instructor Provides
**Low-prep**: Flask template with HTML/CSS, deployment guide
**High-prep**: Complete frontend, database connection examples, Render account

### Foundation: Searchable Database
**Technical**:
- Flask app searching your investigation data
- Results show context (why each result matters)
- Deploy to Render
- Test with non-technical user

**Writing Prompt** (300 words): "A reader used your tool to find their employer taking questionable payments. They want the raw data. What do you provide? What are the ethical considerations?"

### Extension: Power User Features
- Advanced filters actual users would need
- Bulk download with documentation
- API endpoint for other newsrooms

### Innovation: Story Leads
Add features that surface potential stories automatically.

**Common Issues**: Production database credentials exposed, site crashing under load

## Weeks 12-13: AI Document Processing
[Curriculum](curriculum.md#weeks-12-13-document-intelligence-at-scale) | [Readings](readings.md#weeks-12-13-document-intelligence-at-scale) | [Tech stack](tech-stack.md#ai--machine-learning)

### Instructor Provides
**Low-prep**: OpenAI API key with credits, links to government press releases
**High-prep**: Curated document set (mix of formats/topics), entity extraction template, multilingual examples

### Foundation: Extract Entities
**Technical**:
- Process 10-20 documents with AI (press releases, brief reports, or memos work well)
- Extract people, organizations, money amounts
- Verify every extraction against the original document
- Create error log documenting AI mistakes

**Writing Prompt** (300 words): "What types of errors did the AI make? Were there patterns in what it missed or misidentified?"

### Extension: Multilingual Documents
**Scenario**: Documents in multiple languages
- Detect languages
- Translate key sections
- Extract entities consistently

### Innovation: Beat-Specific Tool
Customize prompts and verification for your beat.

**Common Issues**: API rate limits, cost management, accuracy vs speed

## Week 14: Sustainability & Handoffs
[Curriculum](curriculum.md#week-14-sustainability-and-handoffs) | [Readings](readings.md#week-14-sustainability-and-handoffs)

### Instructor Provides
**Low-prep**: README template, documentation examples
**High-prep**: Automated testing framework, monitoring setup

### Foundation: Documentation
**Technical**:
- Write README for one project
- Include: purpose, setup, common issues
- Add automation for updates

**Writing Prompt** (500 words): "Reflect on the semester: Which tools will you actually use? What's still missing?"

### Extension: Production-Ready
- Add error handling
- Create monitoring
- Build simple admin interface

### Innovation: Newsroom Toolkit
Package your best work as reusable template.

**Common Issues**: Incomplete documentation, hardcoded credentials

## Assessment Structure

### Technical Work
- Foundation: Must work and address the core question
- Extension: Shows deeper understanding
- Innovation: Creative application

### Written Reflections
- Weekly 300-500 word reflections
- Focus on journalistic applications, not technical description
- Critical thinking about limitations and possibilities

## Tips for Instructors

### Managing Different Skill Levels
- Extension work can replace Foundation for advanced students or students with additional time

## Resources
- [Speed-run guide](speed-run.md) - For catching up
- [Tech stack](tech-stack.md) - All tools documented
- [Readings](readings.md) - Context and examples
- [Curriculum](curriculum.md) - Week-by-week overview