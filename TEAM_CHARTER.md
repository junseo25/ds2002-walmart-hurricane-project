# Walmart Midterm — Team Charter

**Date:** October 20, 2026  
**Team:** Michael Tague · Mark Chernov · Jun Seo (J.S.) Lee  
**Project:** Walmart data analysis midterm

> **Purpose:** Establish shared expectations for deliverables, responsibilities, communication, and accountability throughout the project.

## 1. Shared goals

Our team aims to produce a well-organized, reproducible notebook that every member can explain and defend. Successful work means:

- Clean the provided datasets reliably and document decisions, including SKU mappings and affected row counts.
- Retrieve and integrate weather data from the **Open-Meteo API** with appropriate date/timezone alignment and join validation.
- Use clearly structured, commented code and reusable dictionaries or functions informed by prior labs.
- Answer the business questions with evidence, effective visualizations, and actionable recommendations.
- Ensure every member understands the reasoning behind the analysis and can explain our methods.

We want to avoid incomplete cleaning, exposed API credentials, weak or misleading visualizations, missed commitments, insufficient updates, and duplicated effort.

## 2. Individual learning goals

### Michael Tague
- Build confidence cleaning real-world datasets.
- Strengthen understanding of course labs, particularly API calls.
- Develop actionable recommendations from reliable, cleaned data.
- Explore how businesses analyze supply and consumer needs during disruptive events.
- Explain each analysis step and justify methodological decisions.

### Mark Chernov
- Build reusable templates from previous labs for future projects.
- Gain experience cleaning larger real-world datasets.
- Improve Open-Meteo API skills, especially date handling.
- Write maintainable code that can accommodate new records.
- Analyze results from marketing and business perspectives to answer management questions.
- Communicate code clearly through comments.

### Jun Seo (J.S.) Lee
- Create visualizations that support business decisions.
- Gain practical experience collaborating through a shared GitHub repository and notebook.
- Identify limitations in the data and code and propose improvements.
- Maintain traceability of each contributor's work.
- Produce high-quality, maintainable code with clear ownership.

## 3. Roles and deliverables

The owner coordinates delivery; backup contributors can step in as needed. **All members remain responsible for understanding the overall project.**

| Workstream | Owner | Backup / support | Completion criteria | Grade area |
|---|---|---|---|---|
| Data cleaning & decision log | Michael | Mark, J.S. | A function transforms raw files into clean tables; decision log records each change and affected row counts | 25% |
| Weather API & join | Mark | J.S., Michael | A function returns daily weather; sales join passes a row-count check; API base URL is cited | 15% |
| Analysis: Q1–Q5 | J.S. | Mark | Each question includes code, a numerical result, and a written finding; Q5 uses SQLite | 30% |
| Charts & presentation | Mark | J.S. | At least six labeled plots with message-focused titles; six-minute presentation rehearsed at least once; README completed | 15% |
| Individual reflection | Everyone | — | Each person completes their own reflection | 15% |

**Shared responsibilities:** Everyone reads all five questions and reviews at least one teammate's code. A rotating notebook owner integrates finished work into the shared notebook and runs **Restart and Run All**.

**Dependencies:** Prioritize Q1 (clean dates) and Q3 (weather API), as these depend on upstream preparation. Other questions can progress in parallel.

**Question-level ownership:** The team will assign a writer and a different reviewer for each question. Reviewers will check baselines, row counts, and whether charts support their titles. *Specific Q1–Q5 assignments remain to be recorded.*

## 4. Availability and scheduling

| Member | Constraints / availability |
|---|---|
| Michael | Architecture studio (~4 hours/day); TA office hours; Monday/Wednesday/Friday commitments; internship applications and interviews; club interviews. Nights generally work best; afternoons are difficult. |
| Mark | Weekend morning job; Thursdays after 7 p.m.; generally available at night; concurrent DS 2003 project. |
| J.S. | Job interviews; TA office hours; Saturday club commitments; classes. |

**Recurring meeting:** Tuesdays, **7:00–8:30 p.m.** If needed, another weeknight around the same time is the preferred fallback.

## 5. Communication and decisions

**Primary communication channel:** Team iMessage group chat.

| Member | Response expectation |
|---|---|
| Michael | Within 2 hours |
| J.S. | Within 1 hour |
| Mark | Within 1 hour when not in class; no later than end of day |

**Disagreements:** First identify what is failing or unclear, test alternative approaches, and evaluate efficiency and clarity. For visualizations, choose the format that best communicates the relevant finding. *A final decision-maker and documentation procedure for unresolved disagreements have not yet been formally specified.*

**Unresponsive team member:**
1. Check in after **1 day** without a response.
2. If there is still no response after **2 days**, raise the issue with the full team.
3. If the issue remains unresolved, email **Professor Williamson**.

## 6. GitHub collaboration

The team intends to collaborate in a shared repository, with members editing, pulling, and pushing code. The following conventions are **proposed for team approval**, rather than already-agreed contract terms:

- Use separate branches for substantial changes and pull requests for review.
- Describe changes and any data-cleaning decisions in pull requests or the decision log.
- Do not commit API keys, credentials, or other secrets.
- Before merging integrated notebook changes, run the notebook from the beginning and verify its outputs.
- Track open tasks and question-level assignments in GitHub Issues or a project board.

## 7. Team acknowledgment

By acknowledging this charter, team members confirm that they have reviewed the shared goals, responsibilities, and communication expectations.

| Member | Acknowledgment | Date |
|---|---|---|
| Michael Tague | Pending | — |
| Mark Chernov | Pending | — |
| Jun Seo (J.S.) Lee | Pending | — |

---

*Prepared from the team's October 20, 2026 planning document. Items labeled proposed or pending require team confirmation.*
