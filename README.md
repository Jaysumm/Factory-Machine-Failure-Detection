# Factory Machine Failure Detection — Daikibo Telemetry Analysis

Tableau dashboard analysing machine downtime across Daikibo Manufacturing's global
factories, built for the **Deloitte Australia Data Analytics Job Simulation** (Forage).

📄 **[Full write-up (PDF)](./Factory_Machine_Failure_Detection_Report.pdf)** — methodology, findings, and recommendations.

---

## Context

Daikibo Manufacturing runs factories in Germany, Japan, and China. Every machine on
the floor reports a `healthy` / `unhealthy` status roughly every 10 minutes. The brief
was to turn that raw telemetry log into a dashboard that shows **where downtime is
concentrated** — by factory and by machine type — so operations teams know where to
focus first.

## What's in this repo

| File | Description |
|---|---|
| `Factory_Machine_Failure_Detection_Report.pdf` | Full write-up: dataset summary, methodology, findings, and recommendations |
| `downtime_output.png` | Dashboard screenshot (Tableau Public) |
| `daikibo-telemetry-data.json` | *Not included — see note below* |

## Approach

1. Imported the raw JSON telemetry export into Tableau Public.
2. Created a calculated field so an `unhealthy` status maps to an operational
   quantity rather than a raw count:
   ```
   Unhealthy = IF [status] = "unhealthy" THEN 10 ELSE 0 END
   ```
   (10 = minutes since the last reading, matching the ~10-minute reporting interval.)
3. Built two views on `SUM(Unhealthy)`:
   - **Down Time per Factory**
   - **Down Time per Device Type**
4. Combined both into a single dashboard for a side-by-side read.

## Key findings

- Fleet-wide estimated downtime over the ~9-day sample: **1,030 minutes (~17.2 hrs)**
  across 160,704 readings.
- **daikibo-factory-seiko** (Japan) and **daikibo-shenzhen** (China) account for
  **87%** of all downtime, despite all four factories running an identical
  9-machine fleet.
- **LaserWelder** and **LaserCutter** account for **91%** of unhealthy readings —
  and in both cases it traces back to a *single device* at a *single factory*,
  not the device type being unreliable across the board.

Full breakdown and recommendations are in the [PDF report](./Factory_Machine_Failure_Detection_Report.pdf).

## Tools

- Tableau Public
- Source data: JSON telemetry export (36 devices × 9 device types × 4 factories)

---

## About the certification

This project was completed as part of the **Deloitte Australia Data Analytics
Job Simulation** on Forage (issued Jul 2026). Daikibo Manufacturing and its
telemetry data are fictional and provided as part of the simulation exercise.
