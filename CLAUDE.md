# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a curriculum repository for the Data and Databases course at Columbia Journalism School's Data Journalism MS program (second semester). It contains course materials, not executable code. The course teaches journalists to work with large datasets using the ProPublica model: acquire significant datasets, analyze thoroughly over time, and build public-facing tools.

## Document Architecture

The curriculum uses a 14-week structure with extensive cross-linking between documents:

### Core Documents
- **curriculum.md**: Master document with weekly topics, concepts, and technical skills. Contains "Quick Win" motivational checkpoints.
- **assignments.md**: Three-tier exercises (Foundation/Extension/Innovation) with instructor provisions (low-prep/high-prep options) and 300-500 word writing prompts.
- **tech-stack.md**: All technologies with documentation links and week numbers. Includes "Bonus Command-Line Tools" section for optional tools.
- **readings.md**: Curated investigations and technical resources organized by week.
- **speed-run.md**: Self-study guide for independent learners.
- **instructor-guide.md**: Minimal guidance focusing on common issues and assessment approach.
- **weekly-checklist.md**: Simple navigation links to each week's materials.

### Week Structure (Must Maintain Consistency)
- Weeks 1-2: Large Government Databases
- Weeks 3-4: Cloud Infrastructure
- Weeks 5-6: Automation & Scraping  
- Weeks 7-8: Collaborative Investigation Infrastructure
- Week 9: Graph Databases and Network Analysis
- Weeks 10-11: Public-Facing Tools
- Weeks 12-13: AI Document Processing
- Week 14: Sustainability and Handoffs

## Key Principles

### Technology Philosophy
- Practical newsroom tools over cutting-edge tech
- DuckDB for local large datasets (not pandas when it hits memory limits)
- PostgreSQL for persistence and multi-user access
- Datasette/Datasette Cloud for exploration and instant sharing
- Backblaze B2 over AWS S3 (simpler and cheaper)
- Flask with Jinja2 templates (no React)
- Render for deployment (not Heroku)
- Neo4j for graph databases (week 9)
- Command-line tools (xsv, miller, csvkit) mentioned as optional in weeks 1-2

### Pedagogical Approach
- Focus on journalistic applications, not abstract technology
- Writing prompts should pose specific newsroom scenarios
- Respect instructor autonomy (avoid prescriptive instructions)
- Common issues should be specific and actionable
- AI document verification must be complete (verify all documents, not samples)

## Maintenance Guidelines

### When Updating Content
- Maintain cross-references between all documents using anchor links
- Keep week numbers consistent across all files
- Include both low-prep and high-prep options in assignments
- Add real journalism examples to readings
- Ensure "Quick Wins" remain in curriculum.md for motivation

### Writing Style
- Concise and practical
- Use real investigations as examples (Panama Papers, ProPublica's work)
- Avoid condescending language or time estimates
- Be specific about requirements (e.g., "10-20 documents" not "SHORT PDFs")

### Course Philosophy
Every technical skill should enable better journalism. The course follows the ProPublica model but adapts it for students who are concurrently taking stats, data-driven projects, and reporting classes.