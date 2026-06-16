# Tableau Build Guide — Project Output (IITG7003)
### Dashboard theme: "Superstore Profitability & Discount Analysis"

This guide tells you exactly what to click in Tableau to build the interactive dashboard and the
data story the assignment requires. It maps 1-to-1 to the report I drafted, so once you build this
and take the screenshots listed at the end, the report is complete.

**Time needed:** ~60–90 minutes. **Tool:** Tableau Desktop (or free **Tableau Public**).
**Data file:** `data/Sample - Superstore.csv` (in this folder).

---

## STEP 0 — Connect the data
1. Open Tableau → **Connect → Text file** → choose `Sample - Superstore.csv`.
   (Tableau Public: **Connect → Text file** as well.)
2. On the Data Source tab, check the data previewed correctly (9,994 rows). Tableau will read
   `Order Date` and `Ship Date` as dates automatically — if not, click the column's data-type icon and set **Date**.
3. Click **Sheet 1** to start.

> Tip: the brief asks you to "document preprocessing." There's almost none needed — the data is clean
> (no missing values/duplicates). The report already says this; you only create the calculated fields below.

---

## STEP 1 — Create calculated fields
For each one: **Analysis → Create Calculated Field…**, paste the name and formula, click OK.

| Name | Formula |
|------|---------|
| **Profit Ratio** | `SUM([Profit]) / SUM([Sales])` |
| **Delivery Days** | `DATEDIFF('day',[Order Date],[Ship Date])` |
| **Profit or Loss** | `IF [Profit] < 0 THEN "Loss-making" ELSE "Profitable" END` |
| **Loss Flag** | `IF [Profit] < 0 THEN 1 ELSE 0 END` |
| **% Loss-Making Orders** | `SUM([Loss Flag]) / COUNT([Profit])` |
| **Discount Band** | see below |

**Discount Band** formula:
```
IF [Discount] = 0 THEN "0% (None)"
ELSEIF [Discount] <= 0.10 THEN "1–10%"
ELSEIF [Discount] <= 0.20 THEN "11–20%"
ELSEIF [Discount] <= 0.30 THEN "21–30%"
ELSEIF [Discount] <= 0.50 THEN "31–50%"
ELSE "50%+"
END
```
- Right-click **Profit Ratio** → Default Properties → Number format → **Percentage (1 dp)**. Do the same for **% Loss-Making Orders**.

---

## STEP 2 — Create parameters (interactivity)
**Right-click in the Data pane → Create Parameter…**

1. **Top N Sub-Categories** — Data type *Integer*, Current value `10`, Range `3` to `17`, step `1`.
2. **Discount Threshold** — Data type *Float*, Current value `0.20`, Range `0` to `0.80`, step `0.05`.

After creating each, right-click it → **Show Parameter** so it appears as a control.

---

## STEP 3 — Build the worksheets
Create each as a new sheet (tab at the bottom). Rename each tab as shown in **bold**.

### Sheet "KPIs" (4 big numbers — viz type: text/BAN)
Quickest version (one sheet):
1. Double-click **Sales**, **Profit** — they go to Rows as a measure. Instead, do this: drag **Measure Names** to **Rows**, drag **Measure Values** to **Text**.
2. In the **Measure Values** card keep only: `SUM(Sales)`, `SUM(Profit)`, `Profit Ratio`, `% Loss-Making Orders` (remove the rest).
3. Marks type = **Text**; enlarge font (Format → Font ~20pt bold). This gives a clean KPI strip.
   *(Alternative: make four separate sheets, each with one measure on Text, for individual tiles.)*

### Sheet "Profit by Sub-Category" (viz type: bar)
1. **Rows:** `Sub-Category`  **Columns:** `SUM(Profit)`.
2. Drag **Profit or Loss** to **Color** (red = Loss-making, green = Profitable — set via the color legend).
3. Sort the bars descending: click the sort icon on the Sub-Category axis.
4. Drag `SUM(Profit)` to **Label**. Add a title.
5. Apply the **Top N** parameter: drag `Sub-Category` to **Filters → Top tab → By field → Top → [Top N Sub-Categories] → by Sum of Sales** (lets the viewer change how many show).

### Sheet "Profit by State" (viz type: filled map)
1. Double-click **State** → Tableau draws a map.
2. Drag **Profit** to **Color**. Click Color → Edit Colors → diverging **Red-Green**, center at 0.
3. Marks type = **Map** (filled). Add title "Profit by State".

### Sheet "Monthly Sales & Profit" (viz type: line, dual-axis)
1. **Columns:** `Order Date` → set to **Month (continuous)** (right-click the pill → choose the lower "Month May 2015" green option).
2. **Rows:** drag `SUM(Sales)`, then drag `SUM(Profit)` next to it (two row pills).
3. Right-click the second axis → **Dual Axis**. Right-click an axis → leave un-synchronised (different scales). Marks = **Line**.
4. Colour Sales and Profit differently; add title "Monthly Sales & Profit (2014–2017)".

### Sheet "Profit by Category & Region" (viz type: highlight table)
1. **Rows:** `Category`  **Columns:** `Region`.
2. Marks type = **Square**. Drag **Profit** to **Color** (diverging) and to **Label**.
3. Add title "Profit by Category and Region".

### Sheet "Avg Profit by Discount Band" (viz type: bar — the key insight)
1. **Columns:** `Discount Band`  **Rows:** `AVG(Profit)`.
2. Drag **Profit or Loss** (or `AVG(Profit)`) to **Color** so negative bands show red.
3. Right-click the Discount Band axis → **Sort** manually into the order: 0% (None), 1–10%, 11–20%, 21–30%, 31–50%, 50%+.
4. **Analytics pane → Reference Line → Constant 0** on the AVG(Profit) axis (shows where profit crosses zero).
5. Title: "Average Profit by Discount Band".

---

## STEP 4 — Build the dashboard
1. Bottom toolbar → **New Dashboard**. Set **Size → Fixed → 1200 × 850** (or "Automatic").
2. Drag a **Text** object to the top: title = **"Superstore Profitability & Discount Analysis"**.
3. Drag sheets in: **KPIs** across the top; below it a row with **Profit by State** and **Profit by Sub-Category**; below that **Avg Profit by Discount Band** and **Monthly Sales & Profit**; put **Profit by Category & Region** at the bottom (use layout containers to arrange).
4. **Filters (interactivity):** on one sheet, click the dropdown on these fields → **Filters**, then for each filter card → dropdown → **Apply to Worksheets → All Using This Data Source**:
   - `Region`, `Category`, and `YEAR(Order Date)`.
5. **Parameter controls:** dropdown on the dashboard → show **Top N Sub-Categories** and **Discount Threshold**.
6. **Dashboard actions** (Dashboard → Actions → Add Action):
   - **Filter action:** "On select" → source = Profit by Sub-Category → target = all other sheets (click a bar to filter the dashboard).
   - **Highlight action:** "On hover" → source = Profit by State (hovering a state highlights it across views).
   - *(Optional)* a **URL action** linking to your GitHub repo.
7. Tidy formatting: consistent fonts, clear titles, currency number formats (right-click a measure → Format → Currency), remove clutter (Format → Lines → none where appropriate). Keep one coherent colour scheme.

---

## STEP 5 — Build the data story
1. Bottom toolbar → **New Story**. Size: 1200 × 900.
2. Add **5 story points** (drag the Dashboard or a sheet onto each, then type the caption):
   1. **"Profitable overall — but margins are thin."** Drag the **Dashboard**. Caption the KPIs: $2.30M sales, $286K profit (12.5% margin), but **18.7% of orders lose money**.
   2. **"Discounts above ~20% destroy profit."** Drag **Avg Profit by Discount Band**. Caption: average profit is positive up to 20%, then turns sharply negative (−$311 at 50%).
   3. **"Furniture is the problem — Tables & Bookcases lose money."** Drag **Profit by Sub-Category**. Caption: a few sub-categories erase the gains made elsewhere.
   4. **"Profit concentrates in the West/East and in Technology."** Drag **Profit by State** (or Category & Region). Caption: where to focus growth.
   5. **"Sales and profit peak in Q4 — plan for it."** Drag **Monthly Sales & Profit**. Caption: strong seasonality; align inventory and staffing.
3. Give the story a title: **"From Data to Decisions: Protecting Superstore Profit."**

---

## STEP 6 — Save & export (what to send me)
1. **File → Save As → Tableau Packaged Workbook (.twbx)** (this embeds the data).
   Name it `Q1122568_JuanOvalle_Superstore.twbx` and put it in this folder.
2. **Screenshots** — save these into the `dashboard_screenshots/` folder (PNG). Use the exact filenames so they map straight into the report:

| Filename | What to capture |
|----------|-----------------|
| `dashboard_full.png` | the whole finished dashboard |
| `sheet_subcategory.png` | the Profit by Sub-Category sheet |
| `sheet_state_map.png` | the Profit by State map |
| `sheet_discount_band.png` | the Avg Profit by Discount Band sheet |
| `sheet_monthly_trend.png` | the Monthly Sales & Profit sheet |
| `sheet_category_region.png` | the Category × Region highlight table |
| `story_overview.png` | the Story view (showing the point navigator) |

> To export a clean image of a sheet/dashboard: **Worksheet/Dashboard → Export → Image…** (or Dashboard → Export → Image).

3. **Send me the screenshots** (or drop them in `dashboard_screenshots/`) and I'll insert them into the report and finalise it. The report is already written around these exact views.

---

## What the report already covers (so you don't have to write it)
Introduction, Literature review (visual analytics & storytelling), Methodology (data, calculated fields,
dashboard & story design), Results & Discussion (each view + the 5 insights and recommendations), and
Conclusion — all drafted in the same BSBI format as your first assignment, with placeholders where the
seven screenshots above will go.
