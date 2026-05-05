# Data and Databases: Course Readings

Per-week readings, verified May 2026. Methodology articles, real investigations, and technical resources organized to match the 12-week curriculum. The general resources section at the end covers communities and ongoing references.

If a URL has rotted by the time you're assigning a reading, check `readings-sweep.md` for the verification history and the Wayback Machine for archives.

## Week 1: SQL via Question Families + DuckDB

### Investigations as Question-Family Exemplars
- [ProPublica's Dollars for Docs](https://projects.propublica.org/docdollars/) — historical snapshot through Dec 2018; current data lives at CMS Open Payments. Great example of a tail-analysis (top-N) investigation.
- [Washington Post Police Shootings Database](https://github.com/washingtonpost/data-police-shootings) — long-running rates/proportions and reconciliation work.
- [BuzzFeed/The Trace: Cities Failing to Arrest Shooters](https://www.hillmanfoundation.org/sidney-awards/trace-and-buzzfeed-news-win-february-sidney-exposing-cities-failure-arrest-shooters) — Sarah Ryley, Jeremy Singer-Vine, Sean Campbell. Comparison-across-peers + concentration analysis.

### Methodologies
- [Tips for Building a Database for Investigations (GIJN)](https://gijn.org/resource/tips-for-building-a-database-for-investigations/)
- [Introducing agate (Source)](https://source.opennews.org/articles/introducing-agate/) — Christopher Groskopf, 2015. Python library designed for journalists.
- [What We've Learned About Sharing Our Data Analysis (Source)](https://source.opennews.org/articles/what-weve-learned-about-sharing-our-data-analysis/) — Jeremy Singer-Vine, 2015.
- [investigate.ai](https://investigate.ai/) — practical machine learning examples for journalism.
- [IRE Tipsheets Database](https://www.ire.org/resource-center/tipsheets/) — search "large datasets," "SQL," "PostgreSQL" (login required).

### Technical Resources
- [DuckDB guides](https://duckdb.org/docs/current/guides/overview)
- [DuckDB Postgres extension](https://duckdb.org/docs/current/core_extensions/postgres)

## Week 2: JOINs, Dirty Data, and Domain Checking

### Anchor Investigation
- **Reveal "Kept Out"** — use the [Pulitzer finalists page](https://www.pulitzer.org/finalists/aaron-glantz-and-emmanuel-martinez-reveal-center-investigative-reporting-emeryville-calif) as the stable archival anchor. The Center for Investigative Reporting / Reveal post-merger has reorganized URLs; the Pulitzer page is the durable reference. Aaron Glantz and Emmanuel Martinez identified modern-day redlining in 61 metros using 31M HMDA records.
- [USC Annenberg's Selden Ring Award writeup](https://annenberg.usc.edu/news/spotlight/center-investigative-reportings-reveal-wins-2019-selden-ring-award-investigative) — additional context on the methodology.

### Domain Knowledge Resources
- [CFPB Reg C interpretations §1003.4 — action-taken definitions](https://www.consumerfinance.gov/rules-policy/regulations/1003/interp-4/)
- [FFIEC/CFPB Public HMDA LAR Data Fields](https://ffiec.cfpb.gov/documentation/publications/loan-level-datasets/lar-data-fields/)
- [CFPB Beginner's Guide to HMDA Data (2022)](https://files.consumerfinance.gov/f/documents/cfpb_beginners-guide-accessing-using-hmda-data_guide_2022-06.pdf)
- [NCRC: HMDA 2018 race/ethnicity calculation methodology](https://ncrc.org/ncrcs-hmda-2018-methodology-how-to-calculate-race-and-ethnicity/)
- [Minneapolis Fed (2024): Lender-reported denial reasons don't explain racial disparities](https://www.minneapolisfed.org/article/2024/lender-reported-reasons-for-mortgage-denials-dont-explain-racial-disparities)

### Consequences-of-Journalism Reading
- [MLK50: "CFPB moves to limit home loan data"](https://mlk50.com/2019/05/23/consumer-financial-protection-bureau-moves-to-limit-home-loan-data/) — what happens when journalism actually publishes HMDA findings.

### Domain-Checking Exemplar (2026)
- [THE CITY: "Drive-by Detention: 800 New Yorkers Swept Up in 'Collateral' ICE Arrests"](https://www.thecity.nyc/2026/04/02/ice-immigration-arrests-collateral-operation-trump/) — Haidee Chu and Gwynne Hogan, April 2, 2026. A textbook example of the W2 lesson: ICE's *own* "targeted vs. collateral" categorical field shows 24% of NYC arrests were collateral and 85% of those had no criminal record — directly contradicting agency framing. Read this when you want to argue that domain-checking is the journalism, not a side activity.

## Week 3: Cloud as Access

### Methodology Anchor
- [ProPublica: "Heart of Nerd Darkness — Why the Dollars for Docs Data Was So Difficult"](https://www.propublica.org/nerds/heart-of-nerd-darkness-why-the-dollars-for-docs-data-is-so-difficult) — custom OCR tool ("Farrago") to parse 50+ disclosure PDFs. Pair with the Dollars for Docs entry from Week 1.

### Case Studies
- [How BuzzFeed News Used Machine Learning to Find Spy Planes (CJR)](https://www.cjr.org/watchdog/how-buzzfeed-news-revealed-hidden-spy-planes-in-us-airspace.php) — Peter Aldhous, August 2017.
- [The Markup's Blacklight: How We Built a Real-Time Privacy Inspector](https://themarkup.org/blacklight/2020/09/22/how-we-built-a-real-time-privacy-inspector) — scaling website scanning.
- [Simon Willison: How Datasette Helps With Investigative Reporting](https://www.newsroomrobots.com/p/how-datasette-helps-with-investigative) — Newsroom Robots podcast, Dec 2023.

### Technical Guides
- [Digital Ocean Tutorials](https://www.digitalocean.com/community/tutorials)

## Week 4: Long-term Tracking — Build the Bill Scraper

### Anchor
- [Simon Willison: "Git scraping: Track Changes Over Time"](https://simonwillison.net/2021/Mar/5/git-scraping/)
- [Simon Willison's PG&E Outages tracker](https://github.com/simonw/pge-outages) — tens of thousands of commits tracking power outages over time.

### Methodologies & Examples
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [ProPublica's Nerd Guides](https://github.com/propublica/guides) — Design and Structure of a News Application; Data Style Guide; Data Bulletproofing Guide.
- [How coding can change the very journalism we do (Source)](https://source.opennews.org/articles/how-coding-can-change-very-journalism-we-do/) — Anastasia Valeeva, Jan 2023.
- [Data Is Plural](https://www.data-is-plural.com/) — Jeremy Singer-Vine's weekly newsletter of useful datasets.

### Long-term Tracking in 2026
- [Marshall Project: "ICE Has Detained 6,200+ Kids in Trump's Second Term"](https://www.themarshallproject.org/2026/04/06/ice-kids-detention-over-6200-trump) — Anna Flagg and Shannon Heffernan, April 6, 2026. Combines FOIA-obtained raw tables (via the Deportation Data Project) with longitudinal analysis showing daily child-detention populations rose tenfold under Trump's second term. Textbook recurring-records-pipeline reporting.

## Week 5: How Data Gets Acquired and Grows Over Time

### Long-term Tracking Anchors
- [Washington Post Fatal Force methodology](https://www.washingtonpost.com/investigations/2022/12/05/washington-post-fatal-police-shootings-methodology/) — explicitly long-term, FOIA + scraping + ongoing local sourcing. Documents ~2× what FBI captures.
- [WaPo Fatal Force GitHub](https://github.com/washingtonpost/data-police-shootings/blob/master/README.md) — open backup if the methodology page blocks.
- Simon Willison's PG&E tracker (revisit from Week 4 as a longitudinal example).
- [COVID Tracking Project methodology](https://covidtracking.com/about-data) — historical reference; project ended 2021. Strong example of volunteer-built long-term infrastructure.

### FOIA Readings
- [Marshall Project: "Journalists: Requesting Public Records for Criminal Justice Stories"](https://www.themarshallproject.org/2025/07/18/journalism-foia-public-records-law) — Alysia Santo, July 2025. Practical workflow from a beat reporter who lives inside FOIA: paper-trail mapping, learning the exact names of forms/databases before filing, scoping requests (recommends ~10 years of data — directly relevant to long-term tracking), avoiding over-redactions, and building custom databases when government data is unreliable.
- [MuckRock: "Keep an eye on your favorite agencies with DocumentCloud's automated scraping and alerts"](https://www.muckrock.com/news/archives/2022/may/24/release-notes-keep-an-eye-on-your-favorite-agencie/) — May 2022. The bridge between one-off FOIA and persistent accountability infrastructure: monitoring agency document drops over time. Pairs directly with the week's "scrapers running for months" framing.
- [MuckRock FOIA Log Explorer](https://www.muckrock.com/foi/logs/) — pre-reporting exercise: see what others have already requested before drafting your own.

### FOIA as Infrastructure (2026 case study)
- [Deportation Data Project: ICE Data Release Oct. 2022 – Mar. 2026](https://deportationdata.org/news/2026-03-30-ICE-release.html) — March 30, 2026. The methodology behind a litigated FOIA workflow that now produces individual-level data on every ICE encounter, detainer, arrest, book-in, and removal — with documented caveats on reliability, redaction, and variable-name drift across releases. Read this *before* reading anything that cites it; it's the upstream source most 2026 ICE reporting depends on. Companion [processing docs](https://deportationdata.org/docs/ice/processing.html) walk through cleaning steps.

### Guest Speaker Prep
- A working journalist whose work spans long-term tracking and FOIA. Students prep questions in advance.

## Week 6: OpenAleph — Collaborative Investigation Infrastructure

### The Anchor
- [ICIJ Cyprus Confidential methodology](https://www.icij.org/investigations/cyprus-confidential/leaked-data-journalism-methodology/) — literally the workflow students are about to replicate. 3.6M documents, cross-referenced against OpenSanctions oligarch list, Forbes 2023 billionaires, and Dow Jones risk database. 272 journalists in 54 countries.

### The Tools
- [OpenAleph](https://openaleph.org/) — the going-forward fork (OCCRP sunsetting active Aleph maintenance after Dec 2025).
- [OCCRP Public Aleph instance](https://aleph.occrp.org/) — for hands-on exploration.
- [Aleph documentation](https://docs.aleph.occrp.org/) — heritage OCCRP docs; functionally apply to OpenAleph.
- [OCCRP Investigative Dashboard](https://id.occrp.org/) — concrete collaborative-investigation infrastructure.
- [FollowTheMoney (FtM) homepage](https://followthemoney.tech/)
- [FtM on GitHub](https://github.com/alephdata/followthemoney)

### Companion Methodologies
- [How We Built the Data Team Behind the Panama Papers (Source)](https://source.opennews.org/articles/how-we-built-data-team-behind-panama-papers/) — Mar Cabra, Nov 2017. How a 2-person data unit scaled to support ~400 reporters.
- [Pandora Papers: Technology Behind the Investigation (Linkurious)](https://linkurious.com/blog/technology-pandora-papers-investigation/) — full tech stack: Python extraction, Fonduer + scikit-learn, Datashare, Neo4j, Linkurious.
- [What is Datashare? (ICIJ)](https://www.icij.org/inside-icij/2019/11/what-is-datashare-frequently-asked-questions-about-our-document-analysis-software/)

## Week 7: Self-hosted Aleph + Ingest Your Own Data

### The Anchor
- [How ICIJ Deals with Massive Data Leaks (Data Journalism Handbook chapter)](https://datajournalism.com/read/handbook/two/working-with-data/how-icij-deals-with-huge-data-dumps-like-the-panama-and-paradise-papers) — cross-investigation reflection on Datashare, I-Hub, Neo4j, and the social/legal protocols of running a 100-newsroom collaboration.

### NYC Building Ownership Resources
- [THE CITY: "How to Search Property Building Records Like a Reporter"](https://www.thecity.nyc/2023/10/13/how-to-search-property-building-records/) — the reader-facing how-to as the model.
- [JustFix's Who Owns What](https://whoownswhat.justfix.org/) — open-source LLC-piercing tool; the working precedent students are aspiring to.
- [Documented / NYCity News Service: When Your New York Landlord is a Corporation](https://www.nycitynewsservice.com/2024/07/08/when-your-new-york-landlord-is-a-corporation/) — series on LLC ownership.

### Companion Reading
- [Linkurious and the Panama Papers (April 2016)](https://linkurious.com/blog/panama-papers-how-linkurious-enables-icij-to-investigate-the-massive-mossack-fonseca-leaks/) — what the entity graph looks like at scale.

## Week 8: Networks for Investigations (Cypher)

### Investigations Using Graphs
- [GIJN: How They Did It — Paradise Papers](https://gijn.org/stories/paradise-papers/) — methodology and tools used.
- [ICIJ's Use of Graph Databases (Neo4j case study)](https://neo4j.com/customer-stories/icij/)
- [Graph Databases in the Paradise Papers (Neo4j blog)](https://neo4j.com/blog/analyzing-paradise-papers-neo4j/)

### Technical
- [Neo4j Cypher Manual (current)](https://neo4j.com/docs/cypher-manual/current/)
- [Neo4j Sandbox](https://sandbox.neo4j.com/) — free, browser-based, no install.

### Network Investigation in 2026
- [ICE Tracking Database — corporate network view](https://icetracking.org/) — Evident Media, continuously updated. Aggregates a $22B private-contractor network linking ICE to vendors and subcontractors. Useful both as an exemplar of contractor-network reporting and as a critique target — how the entity graph was assembled, what's missing, where the deduplication might fail.
- [Documented: "These Companies Work With ICE. New York City Pays Them Millions"](https://documentedny.com/2026/02/05/new-york-city-ice-contracts/) — Feb 5, 2026. Local angle on the federal contractor network — same kind of vendor-graph reporting at city scale.

## Week 9: Build a Baby Aleph — Extraction + Fuzzy Matching + Linking

### Tools
- [rapidfuzz](https://rapidfuzz.github.io/RapidFuzz/) — fast fuzzy string matching.
- [dedupe](https://docs.dedupe.io/en/latest/) — active-learning record linkage.
- [Anthropic API: Structured Outputs](https://docs.anthropic.com/) — for AI entity extraction.
- [OpenAI Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)

### Context
- [Pandora Papers: Technology Behind the Investigation (Linkurious)](https://linkurious.com/blog/technology-pandora-papers-investigation/) — what the big tools do; helps frame the "build a baby version" exercise.

### Entity Resolution in Practice (2026)
- [ProPublica/FRONTLINE: "Caught in the Crackdown: As Arrests at Anti-ICE Protests Piled Up, Prosecutions Crumbled"](https://www.propublica.org/article/caught-in-crackdown-ice-cbp-doj-trump-arrests-convictions) — A.C. Thompson and Gabrielle Schonder, April 14, 2026. Reporters identified 300+ people across social media, court records, federal defender reports, lawsuits, and DHS press releases — sources with no shared key. The entity-resolution problem is the methodology. Most prosecutions collapsed once video footage contradicted officers' statements.
- [Documented: "NY Sees Sevenfold Surge in ICE Arrests of Asian Immigrants"](https://documentedny.com/2026/04/30/new-york-ice-arrests-surge-target-asian-immigrants/) — April 30, 2026. Filtering by citizenship-of-origin field across millions of rows from the DDP FOIA pipeline.

## Week 10: Public-Facing Tools + Security + Privacy + Methodology

### Methodology Anchor
- [ProPublica: "How We Analyzed the COMPAS Recidivism Algorithm"](https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm) — gold-standard methodology piece on auditing an algorithm. The model students are writing toward.

### Reference Implementations
- [News Apps at ProPublica (Data Journalism Handbook 1)](https://datajournalism.com/read/handbook/one/delivering-data/news-apps-at-pro-publica)
- [ProPublica's News App Guides (Nerd Guides)](https://www.propublica.org/nerds/propublicas-news-app-guides) — Scott Klein, design and structure.
- [ProPublica's Nonprofit Explorer](https://projects.propublica.org/nonprofits/) — example of a public-facing investigation database.
- [Texas Tribune Government Salaries Database](https://salaries.texastribune.org/) — another canonical example.
- [NPR Visuals Team Blog](https://blog.apps.npr.org/) — news app development practices.

### Methodology-as-Journalism
- [The Markup's "Show Your Work" series](https://themarkup.org/series/show-your-work) — recurring methodology documentation, the strongest single source for "how to write up methodology."
- [Datasette Documentation](https://datasette.io/) — the simpler path to a public database than Flask, useful as a counterpoint.

### NYC Reference (if students built from W7 NYC data)
- [THE CITY: How to Search Property Building Records Like a Reporter](https://www.thecity.nyc/2023/10/13/how-to-search-property-building-records/)

### Public-Facing Tools to Critique (2026)
- [ICE Tracking Database](https://icetracking.org/) — Evident Media. Interactive map of 140+ detention facilities, 60+ raids, 40+ in-custody deaths, 170+ wrongfully detained citizens, plus the corporate-contractor network. Read this as a design study: how the public-facing tool handles identified individuals, where it discloses sourcing, what privacy tradeoffs it makes. Useful both as a model and as a target for critique.

## Week 11: AI Extraction at Depth + Multilingual

### Anchor
- [Quartz: How Quartz used AI to sort through the Luanda Leaks](https://qz.com/1786896/ai-for-investigations-sorting-through-the-luanda-leaks) — Jeremy B. Merrill. Sentence-level vector embeddings + Annoy nearest-neighbor + Apertium Portuguese MT. The model for the W11 multilingual + verification work.
- [ICIJ: How we mined more than 715,000 Luanda Leaks records](https://www.icij.org/investigations/luanda-leaks/how-we-mined-more-than-715000-luanda-leaks-records/) — the ICIJ companion piece.

### Investigations Using AI
- [Suisse Secrets methodology (OCCRP)](https://www.occrp.org/en/project/suisse-secrets/what-is-suisse-secrets-everything-you-need-to-know-about-the-swiss-banking-leak)
- [FinCEN Files investigation hub (ICIJ)](https://www.icij.org/investigations/fincen-files/)
- [How BuzzFeed News Analyzed the FinCEN Files](https://www.buzzfeednews.com/article/jsvine/fincen-files-explainer-data-money-transactions) — Jeremy Singer-Vine et al., Sep 2020. Custom software to extract structured tabular data from SAR forms; manual review of ~3M words of narrative.

### AI for Journalism
- [AI for Data Journalism (Simon Willison)](https://simonw.substack.com/p/ai-for-data-journalism-demonstrating) — April 2024.
- [OpenAI Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)
- [Bellingcat's Online Investigation Toolkit](https://bellingcat.gitbook.io/toolkit) — including AI-assisted search tools.

### Surveillance Infrastructure (2026)
- NPR: "ICE has spun a massive surveillance web" — March 4, 2026. Reporting on ICE's surveillance stack (phone tracking, license plate readers, facial recognition, social-media monitoring, AI targeting). *URL flagged for instructor verification before assigning — verification agent hit a timeout, but the piece is real and indexed; confirm in browser.*

## Week 12: Methodology Article + Bill Tracker Harvest + Sustainability

### Methodology Models (read three before writing yours)
- [Washington Post Fatal Force methodology](https://www.washingtonpost.com/investigations/2022/12/05/washington-post-fatal-police-shootings-methodology/) — canonical "how we collected this."
- [ProPublica: How We Analyzed the COMPAS Recidivism Algorithm](https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm) — algorithm/data audit methodology.
- [The Markup's "Show Your Work" series](https://themarkup.org/series/show-your-work) — sustained methodology practice.
- [What We've Learned About Sharing Our Data Analysis (Source)](https://source.opennews.org/articles/what-weve-learned-about-sharing-our-data-analysis/) — Jeremy Singer-Vine.

### Sustainability
- Stalph & Borges-Rey, "Data Journalism Sustainability," *Digital Journalism* 6(8), 2018 — paywalled; available via Columbia Libraries.
- [AP Datakit](https://github.com/associatedpress/datakit-core) — datakit-core CLI plus plugins (`-project`, `-github`, `-data`).
- [Deportation Data Project: "One Year of Immigration Enforcement Under the Second Trump Administration"](https://deportationdata.org/analysis/immigration-enforcement-first-year.html) — Blair & Hausman, April 7, 2026. Academic-tone analysis but the methodology section is an excellent model for documenting reproducible cleaning code and longitudinal scope decisions.

### Long-term Examples
- [ProPublica's Nerd Blog](https://www.propublica.org/nerds/) — methodology posts (archive runs through 2022; older posts still useful).

## General Resources

### Books & Handbooks
- [The Data Journalism Handbook 2 (free online)](https://datajournalism.com/read/handbook/two)
- [Practical R for Mass Communication and Journalism](https://www.routledge.com/Practical-R-for-Mass-Communication-and-Journalism/Machlis/p/book/9781138726918) — Sharon Machlis. ISBN 9781138726918.

### Communities & Learning
- [News Nerdery Slack](https://newsnerdery.org/)
- [OpenNews Source](https://source.opennews.org/)
- [GIJN Resources](https://gijn.org/resources/)
- [IRE/NICAR](https://www.ire.org/) — verify current conference page before assigning.
- [NICAR-L Mailing List](https://www.ire.org/nicar-l-mailing-list/)

### GitHub Resources
- [Awesome Data Journalism Lists (infoculture)](https://github.com/infoculture/awesome-datajournalism)
- [BuzzFeed News Analysis Scripts](https://github.com/BuzzFeedNews) — open source investigations.
- [Awesome OSINT (jivoi)](https://github.com/jivoi/awesome-osint)

### Newsletters & Ongoing Sources
- [Data Is Plural](https://www.data-is-plural.com/) — Jeremy Singer-Vine's weekly dataset newsletter.
- [Simon Willison's Blog](https://simonwillison.net/) — Datasette and data journalism updates.
- [The Markup](https://themarkup.org/) — technical investigations and methodologies.
- [Bellingcat Resources](https://www.bellingcat.com/category/resources/) — OSINT guides and tools.
- [GIJN Weekly Newsletter](https://gijn.org/newsletter/) — global investigative journalism updates.
- [ICIJ Investigations](https://www.icij.org/investigations/) — methodology posts for new investigations.

## Notes for Instructor

### Verified May 2026
URLs in this file were verified by a structured sweep against the previous version. See `readings-sweep.md` for the full audit (KEEP / FIX / CUT / ADD).

### Re-verify before each assignment
URLs rot. Re-verify methodology URLs the week before they're assigned. If a URL has 404'd, check the Wayback Machine (https://web.archive.org/) and update.

### Things removed from prior versions
- Reuters Guantanamo methodology (hallucinated; doesn't exist)
- LA Times 2015 LAPD crime stats URL (URL rotted post-restructure)
- Guardian's Data Blog (project wound down)
- FiveThirtyEight Data Downloads (FiveThirtyEight shut down March 2025)
- Guardian's Swarmize platform (deprecated August 2022)
- Several duplicates across week sections
