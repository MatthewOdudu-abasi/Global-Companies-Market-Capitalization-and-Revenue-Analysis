# Global Companies' Market Capitalization and Revenue Analysis  
**By Matthew Odudu-Abasi**  


![Capstone Project Dashboard new  1_page-0001 (1)](https://github.com/user-attachments/assets/a0306489-63c1-4219-aa22-9b65d6b04ec9)

![Capstone Project Dashboard new  1_page-0002](https://github.com/user-attachments/assets/e2d9e513-6878-45a1-a0dc-23d5a3044bf0)




**Tools Used:** Python · Jupyter Notebook · Visual Studio Code · Microsoft Excel · Power BI  

---

## Project Description  
**Global Companies' Market Capitalization and Revenue Analysis** is a capstone project built to demonstrate a complete analytics workflow from **web scraping → data cleaning → exploratory analysis → interactive dashboard creation in Power BI**.

The project intentionally starts at the source (a **live public web table**) instead of using a ready-made dataset. That matters because in real analytics work, the toughest part is often not visualization — it’s collecting data at scale, standardizing messy formats (like **"T"** and **"B"**), and ensuring the dataset behaves correctly when you calculate totals and averages.

This project answers practical questions such as:  
- Which companies dominate global market capitalization and revenue?  
- How concentrated is market value among the top-ranked companies?  
- Do the highest market cap firms also generate the highest revenue?  
- Which companies look "expensive" relative to their revenue (market cap-to-revenue ratio)?  
- Why share price should not be treated as a size metric  

---

## Story of the Data  

### Data Source  
The dataset was scraped from **Stock Analysis**, specifically the **"Biggest Companies"** listing page:  
https://stockanalysis.com/list/biggest-companies/  

This page contains a structured ranking table of global public companies with key indicators such as **Market Cap, Revenue, Stock Price, and Percentage Change**.

### Data Collection Process  
A major requirement for this capstone was collecting at least **1,000 data points**. The project initially attempted scraping a Wikipedia financial table, but it did not provide enough rows to meet the requirement. That constraint influenced the final choice of data source.

Stock Analysis was selected because:  
- The table is large and structured for consistent extraction  
- Page 1 already contains hundreds of records  
- Pagination makes it possible to scale up reliably  
- Combining Page 1 and Page 2 provides 1,000+ records without mixing unrelated tables  

Collection steps followed:  
1. Scrape Page 1 and validate extraction success  
2. Scrape Page 2 using the site's pagination pattern  
3. Merge both pages into one dataset and validate row count  
4. Export for cleaning (Excel/CSV) and load into Power BI for modelling and dashboarding  

### Data Structure  
Each row represents one company from the global ranking list.  

Core columns include:  
- Rank  
- Symbol (ticker)  
- Company name  
- Market cap (as displayed: often in T/B units)  
- Stock price  
- Percentage change (as displayed: percent text)  
- Revenue (as displayed: often in T/B units)  

To support accurate analysis, helper numeric columns were created during preprocessing:  
- **MarketCap_USD:** market cap converted from text units into numeric USD  
- **Revenue_USD:** revenue converted from text units into numeric USD  
- **Percentage Change (numeric):** percentage converted into a usable numeric field  

### What the Data Represents (Business Meaning)  
This dataset provides a **snapshot** view of global public-company financial scale and market valuation. It supports:  
- Market dominance and concentration analysis through market cap totals  
- Scale comparisons through revenue totals  
- Valuation intensity comparisons through market cap-to-revenue ratios  
- Short-term movement signals through percentage change  
- Price-level comparisons through stock price  

### Data Limitations and Biases  
- **Snapshot effect:** the dataset reflects values at the time of scraping. Market cap, price, and percentage change can change daily or intraday.  
- **Unit formatting:** "T" and "B" values are human-readable but must be converted to numeric USD for correct totals, sorting, and measures.  
- **Coverage bias:** the dataset reflects the companies included in Stock Analysis’ list, not all companies globally.  
- **Context limitation:** the dataset is cross-sectional and lacks sector/region/time-series context unless additional data is collected.  

---

## Data Preparation & Processing  

### Python (Web Scraping + Cleaning + Export)  
Python was the core tool for data extraction and transformation using:  
- **Requests:** to retrieve webpage HTML  
- **BeautifulSoup (bs4):** to inspect and parse the page structure when needed  
- **Pandas:** to extract tables, merge pages, clean values, convert formats, and export the final dataset  

Key preprocessing work:  
- Merged Page 1 and Page 2 to meet the 1,000+ record requirement  
- Confirmed rank/row integrity after combining pages  
- Converted Market Cap and Revenue from text units to numeric USD  
- Cleaned percentage change to numeric  
- Enforced stock price as numeric  
- Exported the cleaned dataset for Power BI  

### Excel (Validation and Light Checks)  
Excel was used as a validation layer to:  
- Confirm column structure  
- Spot-check conversions (T/B conversions into full numeric values)  
- Ensure dataset consistency prior to loading into Power BI  

### Power BI (Modelling + Measures + Dashboard)  
Power BI was used to:  
- Set correct data types (decimal/whole number)  
- Build measures and KPI cards  
- Create visuals and interactive slicers  
- Enable rank and company-level drill-down  

### Handling Missing Values  
The dataset was relatively clean from the source table and the scraping script targeted structured rows consistently. Where empty cells could appear after import, they were controlled by:  
- Checking critical numeric fields before conversion  
- Excluding non-convertible records from measures to avoid distorting totals  

In practice, the dashboard measures were designed to operate only on valid numeric rows.

### Duplicate Handling  
Duplicates can occur during multi-page merges or repeated pulls. Duplicates were checked and removed using stable identifiers (**Symbol + Company name**), ensuring one row per company record in the final dataset used for the dashboard.

---

## Key KPIs (Executive Summary)  
From the dashboard KPI cards (in the current filter context):  
- **Total Market Cap:** 149T  
- **Total Revenue:** 42T  
- **Company Count:** 499  
- **Revenue (Billions Display):** 42K (≈ 42 trillion)  
- **Average Share Price:** 231  

**Important dashboard note:** If *Average Share Price* appears abnormally large at total level, it typically means the visual is summing instead of averaging, which creates misleading results.

---

## Detailed Chart-by-Chart Analysis & Observations  

### Market Capitalization by Company (USD Trillions)  
This visual ranks companies by market cap and shows a clear **top-heavy distribution**. A small number of firms dominate global market value, and the drop-off after the top leaders is steep.

**What this tells us:**  
- Market value is highly concentrated  
- The total market cap is heavily influenced by a small set of top companies  
- “Top 10 vs the rest” is a meaningful segmentation for interpretation  

### Revenue by Company (USD Billions)  
This visual ranks companies by revenue and shows that revenue leadership does not perfectly match market-cap leadership. Companies like Walmart and Amazon appear as major revenue generators, but they are not always identical to the top market cap leaders.

**What this tells us:**  
- Revenue reflects operating scale  
- Market cap reflects investor valuation, expectations, and profitability outlook  
- Value and scale are related but not proportional  

### Valuation Efficiency (Market Cap-to-Revenue Ratio)  
This ratio highlights which companies appear highly valued relative to their revenue base. The distribution is skewed, with extreme outliers (for example, AST Space Mobile showing a ratio in the “K” range).

**What this tells us:**  
- Some companies are priced heavily on future expectations rather than current revenue  
- Outliers strongly shape interpretation, so ratio analysis is one of the clearest differentiators in the dataset  
- Mature, high-revenue firms typically show smaller ratios  

### Average Share Price by Company (Table/Matrix)  
This table ranks companies by average share price. It is useful for price-level comparison, but share price should not be treated as a company size metric because it is affected by share counts and stock splits.

**What this tells us:**  
- A high share price does not automatically mean the “biggest company”  
- Market cap remains the correct size indicator for cross-company comparison  

### Company Financial Summary Table (Second Page)  
This table consolidates:  
- Company  
- Total Market Capitalization (USD)  
- Total Revenue (USD)  
- Average Share Price (USD)  
- Average Percentage Change (%)  

It supports drill-down and validation: it’s the most direct view for comparing firms and confirming that dashboard totals reflect the dataset accurately.

---

## Key Patterns & Prioritized Insights  
From the dashboard and computed measures:  
- **Market capitalization is highly concentrated**  
  A small group drives a disproportionate share of market value.  

- **Revenue leadership differs from market-cap leadership**  
  Revenue winners and valuation winners are not always identical, showing that market value is driven by more than sales volume.  

- **Valuation efficiency varies widely and creates outliers**  
  The market cap-to-revenue ratio is strongly skewed, revealing firms that appear “expensive” relative to revenue.  

- **Share price is not a size metric**  
  Share price works best as a market snapshot metric, not a scale metric.  

- **Filtering changes the story quickly**  
  Rank/company slicers reshape KPIs and visuals immediately, proving that insights depend heavily on which segment of the market you focus on (Top 10 vs Top 100 vs full list).  

---

## Detailed Recommendations (Full, Expanded, and Actionable)

### A) Investor / Decision-Making Recommendations  

#### 1) Treat high Market Cap-to-Revenue ratio companies as “expectation-driven” holdings  
A very high market cap-to-revenue ratio means the market is placing a large valuation on a company relative to what it currently generates in revenue. In practical terms, this often happens when investors believe the company has strong future growth potential, strong margins ahead, or a powerful narrative that hasn’t fully shown in current revenue.

**How to apply this insight responsibly:**  
- **Separate these companies into a “growth/expectation” category** inside your investment watchlist instead of mixing them with stable, mature firms.  
- **Size exposure carefully** because companies priced on expectation tend to react sharply to missed earnings, guidance changes, or macro shocks.  
- Use them for **high-upside positioning** rather than treating them as “safe” large-cap holdings just because they appear on a “biggest companies” list.  
- When a firm has a ratio that is dramatically higher than peers, interpret it as a **signal to investigate further**: growth rate, margins, profitability roadmap, competitive advantage, and investor sentiment.

**Why it matters in the dashboard:**  
The valuation efficiency chart shows the distribution is not balanced; a few firms sit extremely high, meaning the market is valuing them far above what current revenue alone would justify. This is a core risk/return signal.

#### 2) Use revenue leaders as stability benchmarks  
Revenue-heavy firms represent strong operating scale and usually have established demand. Even if their market cap is not the highest, their revenue strength makes them a useful reference point.

**How to apply this insight:**  
- Use revenue leaders as a **baseline comparison set** when evaluating high-ratio firms.  
- A practical approach is to compare:  
  - A high market cap company with moderate revenue (expectation-driven)  
  - vs.  
  - A revenue leader with steadier valuation intensity (scale-driven)  
- This improves decision-making because you don’t get trapped in one metric (market cap alone) and ignore operating strength.

**Why it matters:**  
The dashboard shows revenue leadership does not perfectly match market cap leadership. That mismatch is exactly where investor mistakes happen—assuming “biggest market cap” automatically means “strongest business scale.”

---

### B) Corporate Strategy / Business Leader Recommendations  

#### 1) Benchmark valuation efficiency against peers, not the entire market  
Market cap-to-revenue ratios behave differently across business models. Comparing a high-growth tech firm directly with a mature retail firm is not a fair comparison, because margins, growth expectations, and market narratives differ.

**How to use the dashboard insight strategically:**  
- Use the ratio as a **peer-based benchmarking tool**, not a global ranking.  
- The ratio can guide strategic leadership questions such as:  
  - **Are we undervalued relative to comparable companies?**  
  - **Is the market discounting our growth story?**  
  - **Do we need clearer investor communication or better narrative clarity?**  
- If a company’s ratio is weak compared to peers, it could signal a perceived weakness in margins, growth potential, or strategic positioning.

#### 2) Revenue growth alone may not lift valuation without a profitability narrative  
Some companies generate massive revenues but do not lead market cap rankings. This is not automatically “bad”; it often reflects market realities: thin margins, high competition, or limited growth expectations.

**Strategic action points:**  
- Pair revenue growth with a **profitability and margin improvement narrative**, not revenue alone.  
- Focus on improvements such as:  
  - Cost efficiency  
  - Recurring revenue models  
  - Product differentiation  
  - Stronger unit economics  
  - Clear expansion strategy  
- Improve investor messaging so the market understands what drives future profit, not just sales volume.

**Why this matters in your dashboard:**  
The revenue chart highlights firms with huge sales scale, while market cap dominance highlights different firms. That gap is where strategy and market perception matter most.

---

### C) Dashboard / Analytics Quality Recommendations (Credibility + Reporting Stability)

#### 1) Standardize KPI definitions so visuals don’t “accidentally sum”  
This is a critical reporting quality issue. A dashboard becomes unreliable when averages appear abnormally high because they are being summed in totals.

**Correct KPI logic (and why):**  
- **Total Market Cap** must be a **SUM** because it represents total value.  
- **Total Revenue** must be a **SUM** for the same reason.  
- **Company Count** must be **DISTINCTCOUNT** to avoid double counting.  
- **Average Share Price** must be an **AVERAGE**—never a SUM—because a summed share price has no business meaning.  

**Why this matters:**  
Averages often look “wrong” in Power BI when totals are incorrectly aggregated. If Average Share Price looks abnormally large at the bottom row, it signals incorrect aggregation and can reduce trust in the entire report.

#### 2) Use tiered segmentation for clearer storytelling  
Instead of treating all companies as one big group, structure interpretation by tier.

**Suggested tiers:**  
- Top 10  
- 11–50  
- 51–100  
- 101–500  

**Why it improves understanding:**  
- It reduces the dominance effect of top companies on totals.  
- It makes concentration patterns easier to communicate to non-technical viewers.  
- It helps reveal whether patterns hold beyond the very top of the list.

#### 3) Add a Top-N switch for readability  
Many visuals become difficult to interpret when too many companies are shown.

---

## Unexpected Outcomes and How to Interpret Them  

### 1) High revenue but moderate market cap is not a contradiction  
This often reflects mature industries, lower growth expectations, or thinner margins. The dashboard supports this by showing revenue winners and market cap winners are not always identical.

**How to interpret it correctly:**  
- Revenue shows operating scale, not investor optimism.  
- Market cap reflects valuation based on expected growth, margins, and sentiment.  
- A company can be huge in revenue but valued less aggressively due to slow growth or margin constraints.

### 2) A very high share price does not automatically mean “highest company”  
Stock prices can be structurally high due to share count, stock splits, and corporate structure.

**Correct interpretation:**  
- Use share price as a company-level indicator, not a size ranking metric.  
- Use market cap for size comparison because it accounts for share count.

---

## Conclusion  
This project delivered a complete pipeline — from web scraping to cleaning, modelling, analysis, and dashboard reporting — using a real-world financial dataset sourced from Stock Analysis. The final dataset enabled a structured comparison of global companies across market capitalization, revenue, share price, and valuation efficiency.

Key learnings:  
- Market capitalization is concentrated at the top and headline totals are influenced heavily by a small set of firms  
- Revenue leadership and valuation leadership do not always align  
- Market cap-to-revenue ratio is a strong lens for spotting valuation intensity and outliers  
- Share price must not be treated as a direct proxy for company size  
- Interactive filtering improves insight quality by revealing how segment-focused analysis changes conclusions  

Limitations:  
- The dataset is web-sourced and changes over time, so findings represent a snapshot at the time of extraction  
- Sector/industry conclusions are constrained because the base listing lacks a dedicated sector field  
- Deeper finance insight would improve with additional variables (net income, margins) or time-series history  

---

## References  

### Primary Data Source  
- Stock Analysis — Biggest Companies listing  
  https://stockanalysis.com/list/biggest-companies/  

### Tools / Technologies Used  
- Python (Requests, BeautifulSoup, Pandas)  
- Visual Studio Code  
- Jupyter Notebook  
- Microsoft Excel  
- Power BI  

---

## Appendices  

### Appendix A: Dataset Snapshot and Core Field Dictionary  
- **Rank:** company position in the listing (integer, used for ordering/filtering)  
- **Symbol:** stock ticker symbol (text, identifier)  
- **Company Name:** official company name (text)  
- **Market Cap:** extracted as T/B text, converted to numeric USD  
- **Share Price:** numeric (decimal)  
- **Percentage Change:** extracted as percent text, converted to numeric  
- **Revenue:** extracted as B/T text, converted to numeric USD  

### Appendix B: Cleaning and Transformation Summary  
- Page consolidation: merged Page 1 + Page 2 to meet 1,000+ data points requirement  
- Duplicate handling: checked and removed duplicates using stable identifiers  
- Unit standardization: converted T/B formats into full numeric USD using multipliers  
- Type enforcement: enforced numeric typing for stock price and percentage change  
- Derived metrics: built measures including market cap-to-revenue ratio for KPI reporting and valuation analysis  

### Appendix C: Key Measures Used in Power BI (Typical DAX Definitions)  
```DAX
Total Market Cap (USD) =
SUM( MarketCap_USD )

Total Revenue (USD) =
SUM( Revenue_USD )

Company Count =
DISTINCTCOUNT( Company_name )

Average Share Price =
AVERAGE( Stock_price )

Average Percentage Change =
AVERAGE( Percentage_Change )

Market Cap-to-Revenue Ratio =
DIVIDE( [Total Market Cap (USD)], [Total Revenue (USD)] )
