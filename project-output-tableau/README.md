# Assignment 2 — Project Output: Tableau Dashboard & Data Story

**MSc Information Technology Management — Berlin School of Business and Innovation (BSBI)**

**Module:** Machine Learning and Visualization for Data (IITG7003)

**Author:** Juan Osvaldo Ovalle Perez · **Student ID:** Q1122568

This folder holds the second assessment (Project Output). It is the companion to the machine-learning
assignment in the repository root and uses the **same Sample Superstore dataset**, so the analysis
carries across both pieces of work.

## Contents

```
Q1122568_JuanOvalle_ProjectOutput.docx   # the written report (submitted on Canvas)
Q1122568_JuanOvalle_Superstore.twbx      # packaged Tableau workbook (dashboard + data story, data embedded)
TABLEAU_BUILD_GUIDE.md                   # step-by-step build guide
dashboard_screenshots/                   # PNG exports: dashboard, the 5 views, and the story
```

The dataset itself lives in the repository root at `../data/Sample - Superstore.csv`
(and the cleaned version at `../outputs/superstore_clean_for_tableau.csv`).

## The dashboard

A single interactive dashboard, *"Superstore Profitability & Discount Analysis"*, combining views
across five visualisation types: KPI text, bar charts, a filled map, a dual-axis line chart and a
highlight table. Interactivity: a Region filter, a "select a sub-category to filter the dashboard"
action, calculated fields (Profit Ratio, Discount Band, Profit/Loss) and a Top-N parameter.

## Key insights (data story)

1. Profitable overall (~$2.30M sales, 12.5% margin) but **18.7% of orders lose money**.
2. **Discounts above ~20% turn average profit negative** — the decisive controllable lever.
3. **Tables and Bookcases** are structurally loss-making; Copiers/Phones/Accessories lead.
4. Profit concentrates in the **West and East** regions.
5. Strong **Q4 seasonality** in sales and profit.

## How to open

Open `Q1122568_JuanOvalle_Superstore.twbx` in Tableau Desktop (or the free Tableau Public / Reader).
The workbook embeds the data, so no separate connection is needed.
