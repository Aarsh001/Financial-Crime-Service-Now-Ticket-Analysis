# Financial-Crime-Service-Now-Ticket-Analysis
Power BI analytics dashboard for FinCrime ServiceNow ticket operations — SLA compliance, backlog triage, team performance, and root-cause analysis across 850+ tickets.

FinCrime Ticket Operations
A Data-Driven Case Study on Volume, SLA Performance & Root Cause Trends
Prepared from the FinCrime ServiceNow Ticket Analytics Power BI Report
Reporting Period: January – December  |  Total Tickets Analyzed: 850
Prepared by Aarsh Sapra  |  FinCrime Operations Leadership
July 15, 2026
 
Executive Summary
Between January and December, FinCrime Operations processed 850 ServiceNow tickets spanning transaction monitoring, sanctions screening, KYC/CDD, fraud referrals, case management, access & entitlements, and regulatory reporting. The program closed 806 tickets at an average 35.4-hour turnaround, held a 66.4% SLA compliance rate, and saw 7.6% of resolved tickets reopened. As of the reporting date, 107 tickets (12.6% of total intake) remain open, of which 43 are already in SLA breach.

The analysis surfaces three actionable patterns. First, SLA performance is systematically weaker for Medium- and Low-priority tickets, which absorb the largest share of breaches and the longest queue times — evidence of a triage and queue-management gap rather than a target-setting problem. Second, SLA compliance and reopen rates vary widely by team, with an 11-point compliance spread and a greater than 3x reopen-rate spread between the strongest and weakest groups, pointing to transferable best practices. Third, a small number of root causes — capacity constraints, data quality issues, and third-party delays — account for a disproportionate share of breaches, a classic Pareto pattern that should anchor the next process-improvement roadmap.

This document walks through the underlying data, the methodology behind the analysis, section-by-section findings drawn directly from the live dashboard, a set of data-quality observations uncovered during the review, and a concise set of recommendations.

1. Business Context & Objectives

FinCrime Operations triages and resolves ServiceNow tickets raised across the transaction-monitoring and compliance estate — from automated alerts (e.g., sanctions screening hits, KYC/CDD reviews) to manually reported issues (e.g., fraud referrals, access requests, regulatory reporting exceptions). Because many of these tickets carry regulatory SLA obligations, timely and consistent resolution is not just an operational efficiency question but a compliance risk-management one.
The FinCrime ServiceNow Ticket Analytics report was built to give operations leadership a single, continuously refreshed view of: intake volume and mix; the health of the open backlog; SLA compliance and turnaround performance; team-level ownership and workload balance; and the systemic root causes behind service breaches. This case study distills that report into a narrative form suitable for a business review, using the exact figures produced by the underlying data model at the time of writing.

2. Data & Methodology

The analysis is built on a single fact table of 850 ticket records (Fact_Tickets), enriched with created/resolved date dimensions, and a library of DAX measures 
covering backlog, SLA compliance, turnaround, reopen rate, and breach counts. The report is organized into five pages, each answering a distinct operational question:
•	Executive Overview — headline volume, compliance, turnaround and reopen KPIs, plus mix by region, business unit, category and ticket type.
•	Triage & Backlog — the current open backlog, its SLA risk status, and the oldest at-risk tickets by owner.
•	SLA & Turnaround — compliance trend, breach counts by priority and category, and response/resolution speed by priority.
•	Ownership & Team Performance — a team-by-team scorecard of backlog share, compliance, turnaround and reopen rate.
•	Root Cause & Trend — a Pareto view of why SLA breaches occur, root-cause mix by category, and the monthly volume trend.

All figures cited below were read directly from the live report measures and drill-through tables; none are estimated or rounded beyond what the dashboard itself displays.

3. Key Findings

3.1  Ticket Volume & Mix

Monthly intake ranged from 54 tickets (April) to 89 tickets (August). The August spike coincides with the onboarding of a new sanctions list; volume returned to the seasonal baseline the following month, suggesting the team absorbed the surge without a lasting backlog build-up.

 <img width="875" height="416" alt="image" src="https://github.com/user-attachments/assets/ac544bdd-5ae7-49e3-9ca4-1299eb7001e0" />

Figure 1 — Monthly ticket volume (Created), with the August sanctions-list onboarding event annotated.

Regionally, EMEA generated the largest share of tickets (354, 41.7%), followed by AMER (309, 36.4%) and APAC (187, 22.0%). By business unit, Retail Banking (302, 35.5%) and Corporate Banking (240, 28.2%) together account for almost two-thirds of volume, ahead of Payments (158, 18.6%) and Wealth Management (150, 17.6%).

 <img width="875" height="350" alt="image" src="https://github.com/user-attachments/assets/84a9889c-feb1-449e-b579-d746f108a5ac" />

Figure 2 — Ticket volume by region and by business unit.

By ticket type, Incidents dominate at 500 (58.8%), ahead of Requests (269, 31.6%) and Problems (81, 9.5%). By category, Transaction Monitoring (169) and Sanctions Screening (132) are the two largest sources of intake — together roughly 35% of all tickets — which tracks with the transaction-heavy nature of FinCrime monitoring, and marks them as the categories to watch most closely for capacity planning.

3.2  Backlog & Triage Health

As of the reporting date, 107 tickets sit in the open backlog (12.6% of total intake). Of these, 43 (40%) are already in SLA breach, and only 1 is flagged as safely on track. The remaining tickets fall into an “At Risk” category that currently always reports zero — a measurement gap discussed in Section 4, not a literal absence of at-risk work.

The oldest open tickets cluster in the Compliance Advisory, Data Quality Team, MLRO Office, and Platform Engineering assignment groups, with the single oldest ticket exceeding 39 days of age. Of the subset of open tickets with a computable age bucket, nearly two-thirds (29 of 44) have already aged past 10 days — a strong signal that the tail of the backlog, not the median ticket, is where the real SLA risk concentrates.

3.3  SLA Performance & Turnaround

Org-wide SLA compliance stands at 66.4%, against 314 recorded historical breaches. Breach volume is heavily skewed toward Medium priority:

 <img width="813" height="417" alt="image" src="https://github.com/user-attachments/assets/1d9f032e-77ee-45a2-9d52-fe5fe8ea765b" />

Figure 3 — SLA breach count by priority (n = 314 total breaches).

P3-Medium alone accounts for 156 breaches (49.7% of all breaches) — nearly seven times the Critical-tier breach count of 21 — despite carrying a materially longer SLA target window than P1/P2. This points to a triage and queue-management gap rather than a target-setting issue: Medium-priority work is likely being deprioritized behind visibly urgent Critical/High tickets until it, too, breaches.

 <img width="844" height="428" alt="image" src="https://github.com/user-attachments/assets/1710f2ce-b3ee-4d82-be4f-b26f035b15ad" />

Figure 4 — Average first-response and resolution turnaround time by priority.

Response and turnaround times scale with priority as intended: P1 tickets average 2.5h first response and 7.6h turnaround; P2, 3.8h / 10.6h; P3, 9.8h / 28.3h; and P4, 26.3h / 75.6h. The roughly 30x gap in turnaround between P1 and P4 confirms the triage model is directionally sound — but P4 tickets sitting over three calendar days on average could themselves become a compliance concern if low-priority queues include regulatory-adjacent work.

Breach share by category broadly tracks ticket volume, but breach rate (breaches ÷ category volume) tells a sharper story: Case Management System breaches at roughly 42% of its own volume and Access & Entitlements at roughly 41%, both above the organization's overall breach rate of about 37%, while Regulatory Reporting sits lowest at roughly 31%.

3.4  Team Performance & Ownership

The team scorecard below is the single most complete view of where operational risk and best practice both concentrate:

<img width="700" height="277" alt="image" src="https://github.com/user-attachments/assets/8087366c-6a5e-4eb6-97cf-32cdf0e21bb3" />

Table 1 — Team Performance Summary (source: Ownership & Team Performance page).

 <img width="875" height="369" alt="image" src="https://github.com/user-attachments/assets/e3db7a31-899c-4be3-bbe5-bfa16ed10cc8" />

Figure 5 — SLA compliance % (bars) and reopen rate % (line) by team, against the org average.

Tier 1 – FinCrime Ops carries the single largest live backlog (33 open tickets, 31% of the total open backlog) and an above-average reopen rate (9.4%), alongside the second-highest overall caseload (255 total tickets). By contrast, Compliance Advisory (70.3%) and Data Quality Team (71.4%) post the highest SLA compliance and the lowest reopen rates (3.0% / 3.4%) — three times better than Access Management's 10.0% reopen rate. MLRO Office posts the lowest SLA compliance (60.0%) despite a comparatively small caseload of 60 tickets, suggesting a specialization or staffing bottleneck rather than a volume problem.

3.5  Root Cause & Trend

Root cause is captured at closure, so this view reflects 271 closed, SLA-breached tickets with a recorded root cause. The distribution is a textbook Pareto pattern:

 <img width="875" height="369" alt="image" src="https://github.com/user-attachments/assets/d07dfa1e-7042-47ce-9e9f-0515ad638acc" />

Figure 6 — Root cause Pareto analysis of closed, SLA-breached tickets (n = 271).

The top three causes — Capacity Constraint (44), Data Quality Issue (40), and Third-Party Delay (37) — together account for 44.6% of all root-caused breaches, and the top five causes account for roughly 74.5%. Notably, “Not a True Issue” (34) and “Duplicate/False Positive” (27) together represent 61 tickets (22.5%) of the root-caused population — meaning close to a quarter of “breaches” may reflect noisy alerting or triage overhead rather than genuine service failures. Alert-tuning investment could shrink the apparent breach rate without any change in operational capacity.

4. Data Quality Observations

Two limitations in the current measure logic surfaced during this review and are worth flagging before the figures above are used for target-setting:
•	“At Risk” always reports zero. The underlying SLA flag currently only distinguishes Green (on track) and Red (breached), with no intermediate Amber warning state. As a result, the dashboard cannot proactively flag tickets approaching breach before they breach — recommend adding an amber threshold (e.g., 80% of the SLA window elapsed).
•	The open-backlog aging view undercounts the true backlog. It is filtered on “Resolved Date is blank,” which correctly excludes normally closed tickets but also excludes Reopened tickets that retain a historical resolved-date stamp — producing an aging total of 44 versus the authoritative Open Backlog measure of 107. Recommend re-basing this view on ticket Status rather than Resolved Date.
•	Root cause is retrospective only. Because it is assigned at closure, root-cause analysis by definition excludes the current 107-ticket open backlog. A lightweight “predicted root cause” tag at triage time would let the same Pareto lens apply to active work, not just historical closures.

5. Recommendations

•	Rebalance Medium-priority triage capacity. P3 tickets, despite generous SLA windows, produce the most breaches by a wide margin. Consider a dedicated queue or automated aging alerts for P3 tickets nearing 70–80% of their SLA window.

•	Close the “at risk” visibility gap. Implement an Amber SLA state and route amber tickets into a proactive daily huddle before they breach, rather than discovering breaches after the fact.

•	Codify and transfer top-team practices. Compliance Advisory and Data Quality Team combine above-average SLA compliance with the lowest reopen rates in the organization. A lightweight practice-sharing session with Access Management and MLRO Office — the two lowest-compliance teams — is a low-cost, high-leverage intervention.

•	Target the top three root causes. Capacity Constraint, Data Quality Issue, and Third-Party Delay together drive roughly 45% of breaches. A focused sprint on staffing elasticity, upstream data quality, and third-party SLAs would have an outsized effect on overall compliance.

•	Tune alerting to reduce false positives. With about 22% of breach-tagged closures marked “Not a True Issue” or “Duplicate/False Positive,” a scenario and threshold tuning review could shrink both ticket volume and the apparent breach rate simultaneously.

•	Re-baseline the aging/backlog view. Fix the Resolved-Date filter logic described in Section 4 so leadership sees a true, reopened-inclusive view of backlog aging.

6. Conclusion

The FinCrime ticket operation is fundamentally healthy — priority-based triage works directionally as designed, and the team absorbed a significant mid-year volume spike without lasting backlog impact. At the same time, the data points to concrete, addressable gaps: Medium-priority tickets are the primary source of SLA risk, performance varies meaningfully by team in ways that suggest transferable best practice, and a small number of root causes drive a disproportionate share of breaches. Addressing the data-quality gaps identified here — particularly the missing at-risk tier and the backlog aging filter — will sharpen the dashboard's ability to catch problems before they become breaches, turning this report from a retrospective scorecard into a forward-looking early-warning tool.
