# Data and Databases: Assignments

Progressive exercises designed to build from basic competency to advanced investigation skills. Each assignment has Foundation (required), Extension (recommended), and Innovation (optional) tiers.

## Week 1-2: Large Local Databases
[See curriculum](curriculum.md#week-1-2-working-with-large-government-databases) | [Tech stack](tech-stack.md#databases)

### Foundation: Find the Story
**Dataset**: One month of PPP loans from your state (download from [SBA](https://data.sba.gov/dataset/ppp-foia))

**Tasks**:
1. Load the data into DuckDB without importing to memory
2. Answer these questions with SQL:
   - Which city received the most loan money?
   - What industry had the highest average loan amount?
   - Find loans that saved zero jobs but received >$100k
3. Compare performance: time the same query in pandas vs DuckDB

**Deliverable**: SQL queries, timing comparison, and one paragraph about most surprising finding

### Extension: Pattern Detection
**Additional analysis on same dataset**:
- Use window functions to rank businesses by loan amount within each city
- Calculate loan per employee ratios and identify statistical outliers
- Demonstrate the workflow difference: 
  - Option A: Load everything into pandas (watch it struggle/crash)
  - Option B: Query in SQL, then load subset into pandas for visualization

**Deliverable**: Jupyter notebook showing both workflows and when to use each

### Innovation: Build a Checker
Create a reusable Python script that:
- Accepts any state's PPP data
- Automatically flags suspicious patterns based on your analysis
- Outputs both a summary report and detailed CSV for investigation
- Includes proper indexes for common queries (demonstrating performance improvement)

**Deliverable**: GitHub repo with tool and documentation

## Week 3-4: Cloud Infrastructure
[See curriculum](curriculum.md#week-3-4-when-and-why-to-use-the-cloud) | [Tech stack](tech-stack.md#cloud-infrastructure)

### Foundation: Share Your Data
1. Convert your PPP analysis to SQLite
2. Upload to [Datasette Cloud](https://datasette.cloud/)
3. Create three saved queries that reporters could use
4. Share link with classmate - they must find one insight you missed

**Deliverable**: Datasette instance URL with documented queries

### Extension: Build Investigation Infrastructure
**Scenario**: Your newsroom is investigating PPP fraud across multiple states

Students receive a Flask application template with:
- Forms for data entry
- Document upload interface  
- Annotation system

**Your task**: Build the backend
1. Design Postgres database schema for investigations
2. Implement Flask routes to handle form submissions
3. Connect document uploads to Backblaze B2
4. Set up shared Postgres instance on DigitalOcean
5. Create read-only credentials for other reporters

**Deliverable**: Working backend that multiple students can use simultaneously

### Innovation: Performance Optimization
Using increasingly large datasets (1GB → 10GB → 100GB):
1. Measure query performance before and after adding indexes
2. Document when to use: 
   - DuckDB on local files
   - Postgres with good indexes
   - BigQuery for truly massive data
3. Create decision tree for other journalists

**Deliverable**: Performance analysis with recommendations

## Week 5-6: Automation & Change Tracking
[See curriculum](curriculum.md#week-5-6-long-term-data-projects) | [Tech stack](tech-stack.md#automation--workflows)

### Foundation: Three Ways to Track
Build three different GitHub Actions that demonstrate core patterns:

**Pattern 1: Change Detection**
- Daily download of government webpage
- Commit only if changed
- Descriptive commit messages about what changed

**Pattern 2: Running List** 
- Daily scrape of new items (e.g., press releases)
- Append to existing CSV
- Never lose historical data

**Pattern 3: Full Replacement**
- Daily download of complete dataset
- Replace entire file
- Git tracks the differences

Run all three for one week without breaking.

**Deliverable**: GitHub repo with three working Actions and week of history

### Extension: Smart Alerting
Enhance one of your trackers:
- Detect significant changes (define what "significant" means for your data)
- Create GitHub issue when threshold exceeded
- Generate human-readable summary of changes
- Include visualization of trend

**Deliverable**: Enhanced tracker with alert history

### Innovation: Story Discovery Engine
Combine multiple tracking approaches:
- Monitor 5+ related government sources
- Identify correlations between changes
- Statistical anomaly detection
- Generate weekly "story leads" report

**Deliverable**: Multi-source monitoring system

## Week 7-8: Collaborative Investigation
[See curriculum](curriculum.md#week-7-8-collaborative-investigation-infrastructure) | [Tech stack](tech-stack.md#document-processing--investigation)

### Foundation: Manual to Automated
**Phase 1: Manual Review** (Week 7)
- Each student receives 50 government documents (PDFs)
- Manually classify as: "Relevant", "Maybe", "Not Relevant" 
- Extract key information into spreadsheet
- Document your decision criteria

**Phase 2: Automation** (Week 8)
- Write rules based on your manual criteria
- Test on new set of 50 documents
- Compare automated results to manual review
- Calculate accuracy

**Deliverable**: Classification rules and accuracy report

### Extension: Collaborative Platform
Using the Flask template from Week 3-4:
1. Add document classification interface
2. Store classifications in shared Postgres
3. Build consensus system (what if two people disagree?)
4. Create training data export for machine learning

**Deliverable**: Working classification platform used by entire class

### Innovation: Custom Classifier
Using class's combined manual classifications:
1. Train simple classifier (connect to stats/ML class)
2. Test on held-out documents
3. Build confidence scoring
4. Create human-in-the-loop workflow for low-confidence predictions

**Deliverable**: Trained classifier with documentation

## Week 9-10: Public-Facing Tools
[See curriculum](curriculum.md#week-9-10-building-public-facing-data-tools) | [Tech stack](tech-stack.md#data-tools--publishing)

### Foundation: Simple Search Tool
Build basic public interface with Flask:
1. Search box that queries your PPP database
2. Results page with pagination
3. Individual record view
4. Mobile-responsive design (use template)
5. Deploy to [Render](https://render.com/)

**Deliverable**: Public URL of working search tool

### Extension: Journalist-Friendly Features
Enhance your tool:
- Advanced filters (date ranges, loan sizes, industries)
- SQL query builder for non-programmers
- CSV download of search results
- Embed widget for other newsrooms
- Add "story ideas" based on data patterns in results

**Deliverable**: Production-ready tool with documentation

### Innovation: Investigation Platform
Build tool that enables investigation:
- User accounts for saving searches
- Collaboration features (share interesting finds)
- Automated alerts for new data matching saved searches
- API for programmatic access
- Public tips submission form

**Deliverable**: Platform enabling citizen journalism

## Week 11-12: Document Intelligence at Scale
[See curriculum](curriculum.md#week-11-12-document-intelligence-at-scale) | [Tech stack](tech-stack.md#ai--machine-learning)

### Foundation: Entity Extraction
Process 50 government PDFs:
1. Use OpenAI/Anthropic API for entity extraction
2. Extract: people, organizations, dates, money amounts
3. Manually verify 10 documents (20%) for accuracy
4. Create spreadsheet of all entities found
5. Flag documents with low confidence for human review

**Deliverable**: Structured dataset from unstructured documents

### Extension: Multilingual Investigation
**Scenario**: You received documents in Spanish, Portuguese, and English
1. Detect language of each document
2. Translate non-English documents (or key sections)
3. Extract entities consistently across languages
4. Build unified search that works in any language
5. Create bilingual output for publication

**Deliverable**: Cross-language investigation tool and findings

### Innovation: Beat-Specific Intelligence
Build specialized tool for your beat:
1. Develop custom prompts that understand your beat's terminology
2. Create verification checklist specific to your topic
3. Build confidence scoring based on your beat's requirements
4. Design workflow mixing AI and human verification
5. Test on real documents from your beat

**Deliverable**: Beat-specific AI tool with accuracy metrics

## Week 13: Sustainability
[See curriculum](curriculum.md#week-13-sustainability-and-handoffs)

### Foundation: Handoff Package
Choose one previous project and create:
1. README with: purpose, data sources, update schedule
2. Requirements.txt with all dependencies
3. GitHub Action for automated updates
4. Documentation of common errors and fixes
5. Video walkthrough (5 minutes max)

**Deliverable**: Complete handoff package

### Extension: Resilient Systems
Make your project bulletproof:
- Add error handling for common failures
- Set up monitoring (uptime, data freshness)
- Create fallback options (what if API dies?)
- Add data quality checks
- Build admin interface for non-technical users

**Deliverable**: Production-hardened system

### Innovation: Newsroom Toolkit
Package your best tools for adoption:
1. Create template repository others can fork
2. Write tutorial for your specific use case
3. Add configuration options for different newsrooms
4. Create example investigations using your tool
5. Present to class (10 minutes)

**Deliverable**: Shareable toolkit with presentation

## Capstone Integration

Throughout the semester, students should be thinking about their capstone project. Each assignment can contribute:

- **Weeks 1-2**: Identify interesting dataset for long-term investigation
- **Weeks 3-4**: Set up infrastructure you'll need
- **Weeks 5-6**: Build monitoring for your beat
- **Weeks 7-8**: Collaborate with potential partners
- **Weeks 9-10**: Design public interface for findings
- **Weeks 11-12**: Process documents related to investigation
- **Week 13**: Document everything for portfolio

## Grading Approach

### Foundation (70% of grade)
Must work and answer the journalistic question

### Extension (20% of grade)
Demonstrates deeper understanding and technical growth

### Innovation (10% of grade)
Shows creativity and real-world applicability

### Alternative: Investigation Portfolio
Students can propose replacing standard assignments with a semester-long investigation that demonstrates all skills. Requires instructor approval and regular check-ins.

## Support Structures

### Weekly Lab Sessions
- First hour: Review and Q&A
- Second hour: Hands-on work with TA support
- Third hour: Peer debugging and sharing

### Office Hours
- Technical debugging (TA-led)
- Investigation consulting (Instructor-led)
- Peer mentoring (advanced students help others)

### Resources
- [Speed-run guide](speed-run.md) for catching up
- [Tech stack reference](tech-stack.md) with all tools
- [Readings](readings.md) for context and examples
- Class Slack for questions and collaboration