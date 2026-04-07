# trade-analytics--prompt-library_v1.0

> **Domain:** trade-analytics
> **Function:** prompt-library
> **Output:** mixed (HTML dashboards, PPTX, text analysis)
> **Version:** 1.0
> **Created:** 2026-02-24
> **Author:** Rich Baskin

---

## Changelog

| Version | Date | Change |
|---------|------|--------|
| 1.0 | 2026-02-24 | Initial version. 10 prompts covering program/discretionary threshold, lifecycle dashboard, source of capital, annual summary, cross-category analysis, position age curve, distribution chart, trade explorer, full suite dashboard, and nudge response presentation. Includes Appendix A column specification. |

---

## Prompt Library

**Portfolio:** [Portfolio Name] (Pxxxxxx)
**Purpose:** Standard prompts for recurring trade analytics workflows
**Note:** Replace bracketed placeholders with portfolio-specific values before use

---

### 1. Determine the Threshold for Classifying Program vs Discretionary Trades

**Prompt:**

> Analyse the trade analytics file to determine the threshold for classifying program vs discretionary trades. Examine the distribution of daily trade counts, identify natural gaps or clustering in the data, and recommend a threshold with supporting evidence. Show the distribution visually.

**What it does:** Examines how many trades occur per day, finds the natural break point in the distribution (bimodal gap), and recommends a threshold.

**Inputs:** Trade analytics XLSX file (Pxxxxxx_trade_analytics)

**Outputs:** Distribution histogram, recommended threshold, trade counts for each segment

---

### 2. Trade Analytics Lifecycle Dashboard

**Prompt:**

> Generate the Trade Analytics Lifecycle Dashboard. Use the current trade analytics file. Discretionary trades only (apply program trade threshold from Prompt 1), [start year] onwards, relative performance, [performance horizon]. Show the heatmap with PW/PL rows per activity, paired bar cards, and actionable takeaways.

**What it does:** The flagship visualisation. Shows position age (0–3m through 36m+) crossed with activity type (Entry, Scaling In, Add, Trim, Scaling Out, Exit) and prevailing winner/loser status. Includes a lifecycle phase diagram, impact heatmap, paired-bar activity cards, and actionable Stop/Do More/Watch takeaways.

**Inputs:** Trade analytics XLSX file, program trade threshold

**Outputs:** Interactive HTML dashboard

**Parameters to specify:**
- Start year (e.g. 2022)
- Performance horizon (e.g. 6-month, 12-month)
- Annualised or not annualised

**Drill-down capability:**
- Clicking any heatmap cell opens a slide-out panel showing every underlying trade for that activity × age bucket (or activity × age × prevailing status for PW/PL rows)
- The panel displays: symbol, date, entry date, prevailing status, trade weight, trade value, relative impact, absolute impact, sector, and dividend yield
- Summary strip at the top shows trade count, winners, losers, hit rate, payoff, and total impact for the filtered set
- All columns in the panel are sortable (click header to toggle ascending/descending)

---

### 3. Source of Capital Analysis

**Prompt:**

> Generate Source of Capital views for [date(s)]. Use the current trade analytics file. Show decreasing positions (source) and increasing positions (deployment) with hit rate, payoff, impact and turnover — both absolute and relative. [Performance horizon], not annualised.

**What it does:** For a given trade date, shows what was sold (decreasing) to fund what was bought (increasing), with full performance metrics for each side. Designed to accompany nudge response analysis.

**Inputs:** Trade analytics XLSX file, target date(s)

**Outputs:** HTML view per date with sidebar layout, summary rows, and individual trade detail

**Calculation rules:**
- Winner/loser determined by sign of the trade's impact (not the Prevailing Impact W/L column)
- Payoff = average(winner impact) / abs(average(loser impact))
- No rounding before calculations; display at 2 decimal places for impact
- Increasing activities: entry, add, scaling in
- Decreasing activities: trim, scaling out, exit

---

### 4. Annual Activity Summary

**Prompt:**

> Generate the annual trading activity summary broken down by activity type. Use the current trade analytics file, discretionary trades only. Show each activity (Entry, Scaling In, Add, Trim, Scaling Out, Exit) as its own section with yearly rows and a total. Relative and absolute impact, [performance horizon].

**What it does:** Year-over-year performance for each activity type. Shows whether entries, adds, trims, etc. are getting better or worse over time. Each section has yearly rows plus a total.

**Inputs:** Trade analytics XLSX file, program trade threshold

**Outputs:** HTML view with seven sections (All Activities + six activity types)

---

### 5. Cross-Category Performance Analysis

**Prompt:**

> Analyse relative performance across all category dimensions in the trade analytics file (holding period, portfolio weight, dividend yield, sector, prevailing W/L) and their cross-combinations. Discretionary trades only, [start year] onwards. Identify which combinations are value-additive and which are destructive.

**What it does:** Systematic exploration of every categoriser in the data and their intersections. Finds which specific combinations of holding period × dividend yield, portfolio weight × direction, prevailing status × activity, etc. are driving positive or negative alpha.

**Inputs:** Trade analytics XLSX file, program trade threshold

**Outputs:** Text-based analysis with tabular results for each cross-combination

**Notes:**
- Minimum sample size of 5 trades per cell
- Hit rate, payoff, and impact calculated separately for each cross
- Categories available depend on what is present in the input file

---

### 6. Position Age Performance Curve

**Prompt:**

> Calculate position age at time of each trade using entry dates. Analyse relative performance by age bucket (0–3m, 3–6m, 6–12m, 12–18m, 18–24m, 24–36m, 36m+). Discretionary trades only, [start year] onwards, [performance horizon]. Identify the sweet spot and decay point.

**What it does:** Uses the actual entry date from each episode to calculate how old a position was at the time of each trade, then analyses performance by age bucket. More precise than the pre-calculated holding period categories in the data.

**Inputs:** Trade analytics XLSX file, program trade threshold

**Outputs:** Performance curve by age bucket, breakdown by activity, direction, and prevailing status

**Notes:**
- Age = trade date minus Entry Date (Episodes)
- Buckets: 0–3m, 3–6m, 6–12m, 12–18m, 18–24m, 24–36m, 36m+
- This feeds into the Lifecycle Dashboard as the underlying data layer

---

### 7. Trade Impact Distribution Chart

**Prompt:**

> Generate the Trade Impact Distribution chart (strip + violin) for each activity type by position age bucket. Use the current trade analytics file. Discretionary trades only, [start year] onwards, relative performance, [performance horizon]. Show individual trade dots (green for winners, red for losers) overlaid on violin shapes showing the distribution density. Include median line, mean diamond, and hit rate per row. Add a legend and a "How to Read This Chart" appendix.

**What it does:** Visualises the full distribution of individual trade impacts for each activity × position age combination. Unlike summary metrics (hit rate, payoff, impact), this shows the actual shape of outcomes — skew, clustering, fat tails, and outliers — so that patterns invisible in averages become visible.

**Inputs:** Trade analytics XLSX file, program trade threshold

**Outputs:** Interactive HTML with strip + violin chart per activity, legend, and interpretation appendix

**Parameters to specify:**
- Start year (e.g. 2022)
- Performance horizon (e.g. 6-month, 12-month)
- Minimum n per row (default: 2)

**Design notes:**
- Violin shape uses Gaussian-smoothed histogram (3-bin spread) for readable contours
- Dots are jittered vertically with a seeded random for consistent layout
- Rows are coloured by age bucket (warm → cool gradient) for visual continuity with the Lifecycle Dashboard
- Median = thick vertical line through violin; Mean = open diamond; Zero = dashed vertical

**Drill-down capability:**
- Clicking any individual trade dot opens a slide-out panel showing all trades for that activity × age bucket
- The clicked trade is highlighted in the panel and auto-scrolled into view
- Panel shows the same columns and sortable headers as the Lifecycle Dashboard drill-down
- Summary strip updates to show aggregate stats for the visible trade set

---

### 8. Trade Detail Explorer

**Prompt:**

> Generate the Trade Detail Explorer. Use the current trade analytics file. Discretionary trades only, [start year] onwards, [performance horizon]. Show a tabular view of all underlying trades with tabs for each activity type (Entry, Scaling In, Add, Trim, Scaling Out, Exit). Include filters for position age bucket, prevailing winner/loser, symbol search, and winner/loser impact. All columns sortable. Summary strip showing trade count, winners, losers, hit rate, payoff, and total impact — updating live as filters change.

**What it does:** A standalone data explorer for the full trade list. Each activity has its own tab showing every trade with complete detail: symbol, date, entry date, position age, prevailing status, trade weight, trade value, relative impact, absolute impact, relative return, sector, and dividend yield. Filters and sort let users drill into specific subsets.

**Inputs:** Trade analytics XLSX file, program trade threshold

**Outputs:** Interactive HTML with activity tabs, sortable table, filter bar, and live summary

**Parameters to specify:**
- Start year (e.g. 2022)
- Performance horizon (e.g. 6-month, 12-month)

**Table columns:**
- Symbol, Date, Entry Date, Age Bucket, Age (Days), Prevailing W/L, Weight %, Trade Value, Impact Relative %, Impact Absolute %, Return Relative %, Sector, Dividend Yield

**Filter options:**
- Age bucket (dropdown: All, 0–3m, 3–6m, 6–12m, 12–18m, 18–24m, 24–36m, 36m+)
- Prevailing status (dropdown: All, Winner, Loser, No Status)
- Symbol (text search, case-insensitive)
- Impact (dropdown: All, Winners only, Losers only)
- Clear all button resets all filters

**Notes:**
- This is the standalone version of the drill-down panel available in Prompts 2 and 7
- Useful for client meetings where the question is "which trades are behind that number?"
- Can be run independently or used as a reference alongside the dashboard views

---

### 9. Trade Analytics Full Suite Dashboard

**Prompt:**

> Generate the Trade Analytics Full Suite Dashboard as a single HTML file. Use the current trade analytics file. Discretionary trades only (apply program trade threshold from Prompt 1), [start year] onwards, relative performance, [performance horizon]. Combine all three views into one page with tab navigation: (1) Lifecycle — phase timeline, PW/PL heatmap with clickable cells, paired bar activity cards with insights, and actionable takeaways; (2) Distribution — strip + violin chart per activity with clickable dots, legend, and interpretation guidance; (3) Explorer — activity tabs, filter bar (age bucket, prevailing W/L, symbol search, impact), sortable columns, live summary strip. All three views share a common slide-out drill-down panel with sortable trade detail.

**What it does:** The integrated dashboard combining Prompts 2, 7, and 8 into a single file. Three tabs give different views of the same data — summary patterns (Lifecycle), distribution shapes (Distribution), and individual trades (Explorer). Drill-down is available from both the Lifecycle heatmap and the Distribution dots, opening a shared slide-out panel.

**Inputs:** Trade analytics XLSX file, program trade threshold

**Outputs:** Single interactive HTML file with all three views, shared drill-down panel, and embedded data

**Parameters:** Same as Prompts 2, 7, and 8 (start year, performance horizon, annualised flag)

**Notes:**
- This is the recommended production deliverable — it supersedes running Prompts 2, 7, and 8 separately
- Tab specs: Lifecycle = Prompt 2, Distribution = Prompt 7, Explorer = Prompt 8. All share a common slide-out drill-down panel
- Single file, no external dependencies beyond the Google Fonts CDN. Data embedded as JSON

---

### 10. Nudge Response Presentation

**Prompt:**

> Generate the Nudge Response Presentation as a PPTX. Use the nudge response with outcome file (Pxxxxxx_nudge_reponse_with_outcome). For each nudge type (Entry, Add, Trim, Exit), identify each unique question asked (from the "Formatted Question Asked" column). For each question, count distinct responses per answer value — a response is a unique (Symbol, Submitted) pair where Trade Date is null. Present each question as a horizontal bar chart slide: answer options sorted by count descending, bars scaled to the maximum count, with count and percentage labels. Multi-select questions (where answer counts sum to more than total responses) should be flagged. Exclude non-question fields (ticker, free text, forecast range of outcome). Use Essentia Analytics branding.

**What it does:** Produces a client-ready presentation showing how the portfolio manager answered each nudge question across all four activity types. One slide per question, with horizontal bar charts showing the distribution of responses.

**Inputs:** Nudge response with outcome XLSX file (Pxxxxxx_nudge_reponse_with_outcome)

**Outputs:** PPTX presentation with title slide + one bar chart slide per question per activity

**Data extraction logic:**
- Filter to rows where Trade Date is null (these are the response-only rows, one per answer selection)
- Group by Formatted Nudge Type → Formatted Question Asked → Answer Value
- Count = number of distinct (Symbol, Submitted) pairs per answer value
- Total responses per question = number of distinct (Symbol, Submitted) pairs for that nudge type × question combination
- Percentage = count / total × 100
- Multi-select detection: if sum of counts across all answers > total responses, flag as multi-select
- Exclude questions: "What is the ticker?", "What is the forecast Range of Outcome Ratio?", "Anything else to add?", "What type of trade did you make in this symbol?"

**Questions to include (by activity):**
- Entry: "The primary investment consideration is the enhancement of:", "Which fundamental quality characteristic(s) makes the stock compelling?"
- Add: "The primary investment consideration is the enhancement of:"
- Trim: "The primary investment consideration is:"
- Exit: "Has the stock achieved its targeted return?", "Is there a more attractive alternative?", "Is there an anticipated dividend policy change?", "Is there an investment thesis change?"

**Slide format:**
- Title: "[Activity]: [Short question description]"
- Subtitle: "[ACTIVITY] — [n] responses [(multi-select)]"
- Question text in bold below subtitle
- Horizontal bars: answer label (right-aligned) | bar track (light background) | filled bar (scaled to max) | count + (percentage) label
- Multi-select note at bottom where applicable
- Essentia branding: Indigo (2A293E) top/bottom bars, Royal Blue (1D4288) bar fill, Montserrat headings, CONFIDENTIAL footer

**Design specifications:**
- Bar track background: ECEAF2
- Bar fill: 1D4288 (Royal Blue)
- Dynamic bar height: adjusts to number of answer options (max 0.38", min gap 0.12")
- Labels: 10pt Arial, right-aligned
- Count labels: 9pt, positioned at end of bar with count + (percentage%)
- Slide chrome: thin Indigo top bar (0.06"), thin Indigo bottom bar (0.04"), CONFIDENTIAL in bottom right

**Notes:**
- This is Level 1 (response counts only). Level 2 adds outcome metrics (trade count, hit rate, payoff, impact) from the matched trades in the same file
- The outcome file contains one row per response × trade match. Null Trade Date rows are the response records; non-null Trade Date rows are the matched trades
- The "Response Count for Table" and "Trade Count for Table" columns are pre-aggregated at a broader scope — do not use them for per-answer counts; compute from the raw rows instead

---

### Dependencies Between Prompts

```
[1] Program/Discretionary Threshold
        │
        ├──► [2] Trade Analytics Lifecycle Dashboard ←── drill-down
        │
        ├──► [3] Source of Capital Analysis
        │
        ├──► [4] Annual Activity Summary
        │
        ├──► [5] Cross-Category Performance Analysis
        │
        ├──► [6] Position Age Performance Curve ──► [2]
        │
        ├──► [7] Trade Impact Distribution Chart ←── dot drill-down
        │
        ├──► [8] Trade Detail Explorer (standalone)
        │
        ├──► [9] Full Suite Dashboard ══ combines [2] + [7] + [8]
        │
        └──► [10] Nudge Response Presentation (separate input file)
```

Prompt 1 must be run first for Prompts 2–9. Prompt 10 uses a different input file (nudge response with outcome) and is independent of Prompts 1–9. Prompt 6 provides the data that feeds into Prompt 2. Prompt 9 is the recommended production deliverable for trade analytics — it combines Prompts 2, 7, and 8 into a single integrated view with shared drill-down. Prompts 2, 7, and 8 remain available individually for cases where only one view is needed.

---

### Appendix A: Input File Format

The trade analytics file is an XLSX export from the Essentia Analytics platform, named `Pxxxxxx_trade_analytics_YYYYMMDD.xlsx`. See the file header row for the full column specification. Key notes: all return/impact values are in decimal form (multiply by 100 for %), Trade Value Net requires string parsing, date formats differ between columns (`DD-Mon-YYYY` vs `M/D/YYYY`), activity values are lowercase, and category band boundaries are portfolio-specific.

