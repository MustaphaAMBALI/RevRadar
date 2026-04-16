# RevRadar | Automated Sales Performance Monitor

**Company:** Crestline Solutions (Fictional)
**Tools:** SQL Server (analysis), PostgreSQL/Supabase (cloud database), Power BI, n8n
**Type:** End-to-End Automated Analytics System

---

## Project Overview

Crestline Solutions had no early warning system for sales underperformance.
Management only reviewed rep performance at month-end, meaning problems were
identified too late to act on. Revenue targets were being missed, at-risk reps
were going undetected, and leadership was making decisions without real-time visibility.

**RevRadar** solves this by monitoring every rep's performance against their monthly
target, automatically alerting management when a rep falls below the risk threshold,
and providing an interactive dashboard for leadership to track revenue health across
all regions and reps.

---

## The Data Model

Four tables built and validated in SQL Server, then migrated to Supabase
(PostgreSQL) to enable cloud connectivity with the n8n automation workflow.

| Table | Description |
|---|---|
| Reps | 10 sales reps across 4 regions |
| Targets | Monthly revenue and deal targets per rep |
| Deals | 2,539 deal records (Won and Lost) across 12 months |
| Alerts_Log | Automated record of every alert triggered |

---

## SQL Analysis

Five analytical queries were written to extract business insights:

**1. Quota Attainment per Rep per Month**
Compares actual revenue generated against monthly targets per rep,
calculating an attainment percentage for each rep across all 12 months.

**2. Regional Revenue Breakdown**
Calculates each region's total revenue and percentage contribution
to overall company revenue using a CTE.

**3. Deal Win Rate per Rep**
Measures how efficiently each rep converts opportunities into closed deals.

**4. Month over Month Revenue Trend**
Tracks how company revenue changed each month using the LAG function
to identify seasonal patterns and risk periods.

**5. Underperformer Flag**
Classifies each rep each month as On Track, Needs Improvement, or At Risk
using CASE WHEN threshold logic. This query powers the automated alert system.

---

## Key Findings

- North region leads revenue at 32.28%, driven by 3 consistently strong performers
- West ranks third despite having the top individual performer — Sola Adesanya's
  chronic underperformance offsets Tunde Fashola's output entirely
- Win rates are consistent across all reps (68% to 72%), meaning underperformance
  is a pipeline and deal size problem, not a closing problem
- August shows a seasonal revenue dip of 17.67%, with a strong September recovery
  of 21.06% confirming it as a recurring pattern rather than a structural issue
- 3 reps (Sola Adesanya, Fatima Bello, Biodun Martins) are chronically below
  70% attainment and flagged as At Risk every month

---

## Automated Alert System (n8n)

The workflow runs automatically every day at 8am:

1. **Schedule Trigger** fires daily at 8am
2. **PostgreSQL Node** runs the underperformer query against Supabase
3. **Filter Node** isolates all At Risk records (below 70% attainment)
4. **Aggregate Node** bundles all At Risk reps into a single summary
5. **Gmail Node** sends a formatted HTML alert email to management

Management receives one daily email listing every at-risk rep with their
name, month, revenue, target, and attainment percentage.

---

## Power BI Dashboard

Single page executive dashboard featuring:

- **KPI Cards** — Total Revenue, Deals Won, Deals Lost, Overall​​​​​​​​​​​​​​​​
