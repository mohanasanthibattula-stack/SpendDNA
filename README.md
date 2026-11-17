# SpendDNA — Your Wallet's Year-End Story
> **"Spotify Wrapped for your money: decoding six months of UPI & bank transactions with Python and Pandas."**

---

## 📌 Project Overview
**SpendDNA** is a publication-grade personal finance analytics tool built with Python, Pandas, and NumPy. It analyzes 6 months (Jan to Jun 2024) of raw, messy bank/UPI transaction exports for Rahul Sharma, a Bengaluru-based software engineer. 

The tool ingests 1,328 transaction lines, resolves multi-format dates and currencies, performs entity normalisation across 283 raw merchant descriptions into canonical vendors, categorises expenses into 12 distinct financial buckets, detects intra-category spending anomalies ($z > 2$), evaluates behavioral spending archetypes, and renders a terminal-based financial recap.

---

## 📸 Output Report Showcase
*(Insert your terminal report screenshot below before sharing on LinkedIn / GitHub)*

```text
================================================================
 SpendDNA REPORT - RAHUL SHARMA
 6 months - 1,310 transactions - Jan to Jun 2024
================================================================
 EXECUTIVE SUMMARY
 Total credits  : Rs.    509,774
 Total debits   : Rs.  1,678,901
 Net change     : Rs. -1,169,127  (overspending)
 Savings rate   :    -229.3%  (BURNING SAVINGS)
 Transactions   :      1,310
 Unique vendors :         42

 TOP CATEGORIES (% of debit total)
 E-commerce       ##############     35.4%  Rs.   593,959
 Investments      ######             14.8%  Rs.   248,160
 Food Delivery    ###                 7.7%  Rs.   129,054
 Restaurants      ###                 7.0%  Rs.   117,737
 Quick Commerce   ###                 7.0%  Rs.   117,611

 TOP VENDORS
 Amazon           Rs.   318,612  ( 77 orders)
 Zerodha          Rs.   210,000  ( 14 orders)
 Flipkart         Rs.   177,510  ( 47 orders)
 Rent             Rs.   108,000  (  6 orders)
 Swiggy           Rs.    73,738  (176 orders)

 TIME-OF-DAY PATTERNS
 Food Delivery peaks: 18:00 - 02:00  (53.2% of orders)
 Cafe peaks:          08:00 - 11:00  (morning coffee runs)
 Quick Commerce:      evenly distributed across day

 MONTHLY TREND (Food Delivery)
 Jan  Rs.  20,890  ###########
 Feb  Rs.  21,452  ###########
 Mar  Rs.  20,850  ###########
 Apr  Rs.  23,054  ############
 May  Rs.  22,167  ############
 Jun  Rs.  20,641  ###########

 TOP ANOMALIES (2+ stddev from category mean)
 12 Mar - BigBasket      Rs.    1,865  (z=4.3)
 20 May - BigBasket      Rs.    1,856  (z=4.3)
 23 Jan - BigBasket      Rs.    1,797  (z=4.1)
 26 Jun - Amazon         Rs.   22,008  (z=4.0)
 07 Feb - Amazon         Rs.   21,986  (z=4.0)

 RAHUL'S SPENDING ARCHETYPES
 -> THE FOODIE                       (36.0% of debit orders / Rs. 278,236 on food)
 -> THE QUICK COMMERCE JUNKIE        (212 orders (16.3% of debit txns, Rs. 117,611))
 -> THE SHOPAHOLIC                   (35.4% of debits (Rs. 593,959))
 -> THE INVESTOR                     (14.8% of debits (Rs. 248,160))
 -> THE LATE-NIGHT SNACKER           (53.2% of food delivery orders placed in evening/night (188 orders))
 -> THE SUBSCRIPTION LOVER           (4 active streaming subscriptions (Netflix, Spotify, Hotstar, Prime))
 -> THE YOLO SPENDER                 (Savings rate -229.3% (burning savings))
 -> THE BENGALURU CAFFEINE CONNOISSEUR (BONUS) (99 cafe visits across 3 brands (Starbucks, Third Wave, CCD))

================================================================
 KEY INSIGHTS
 1. Rahul is burning savings at Rs. 194,854 per month
    (-229.3% savings rate). Unsustainable past Q3 2024.
 2. 53.2% of food delivery orders happen after 6 PM,
    highlighting heavy dinner reliance and late-night snacking.
 3. Discretionary spending (E-commerce 35.4% + Food + Q-commerce)
    dominates his wallet, with multiple anomalous spikes > Rs. 20,000 on Amazon.
================================================================
```

---

## ⚖️ Technical Constraints & Rules
To reflect real-world fintech engineering discipline, this project was developed under strict architectural boundaries without relying on black-box profiling packages:

| Allowed Tools & Libraries | Strictly Forbidden Tools |
| :--- | :--- |
| Core Python (loops, functions, dicts, comprehensions) | `pandas-profiling`, `ydata-profiling`, `sweetviz`, `autoviz` |
| `pandas` (`read_csv`, `pivot_table`, `transform`, `.dt`, `.str`) | `matplotlib`, `seaborn`, `plotly` (all charts are ASCII) |
| `numpy` (`zeros`, `where`, array manipulation) | `scikit-learn`, `scipy.stats`, `statsmodels` (z-scores by formula) |
| `datetime` (`strptime` for fallback date formatting) | `re` (regex strictly forbidden; string methods used) |
| Standard string methods (`.str.replace`, `.str.contains`) | `collections.Counter`, `collections.defaultdict` |
| Text-based formatting with ASCII characters (`#`) | Any third-party transaction parsing library (`plaid`, etc.) |

---

## 🗓️ 7-Day Build Log

- **Day 1: Parser & Sanitization (`parse_transactions`)**
  - Resolved 4 mixed date formats (`DD/MM/YY`, `ISO`, `DD-Mon-YY`, `DD Mon YYYY`) preserving Indian day-first conventions.
  - Sanitized currency amounts containing `₹`, `Rs.`, commas, and whitespace into numeric floats. Standardised transaction types to `debit`/`credit` and dropped 18 exact duplicates.
- **Day 2: Merchant Normalisation (`extract_vendor`)**
  - Explored all 283 unique raw descriptions. Constructed an ordered dictionary mapping keyword variants (including `BUNDL Tech P L` -> Swiggy, `KIRANAKART` -> Zepto, and friend UPI handles -> P2P Transfer).
  - Achieved 100% resolution with 0 uncategorised transactions across 42 canonical vendors.
- **Day 3: Category Tagger & Spending Overview (`tag_categories`, `compute_spending_overview`)**
  - Mapped canonical vendors into 12 core consumption categories + special non-consumption flows (`Personal Transfer`, `Cash Withdrawal`, `Rent`, `Salary`).
  - Documented that non-consumption categories are isolated from consumption spend calculations.
- **Day 4: Monthly Trends & Time-of-Day Dynamics (`analyze_monthly_trends`, `analyze_time_patterns`)**
  - Generated Category x Month pivot table and computed growth percentages via NumPy.
  - Constructed a 2D Category x 24-Hour matrix to expose late-night food ordering and morning cafe commute habits. Added weekend vs weekday spend comparison.
- **Day 5: Intra-Category Anomaly Detection (`detect_anomalies`)**
  - Implemented category-specific z-score normalization: $z = (amount - \mu_{cat}) / \sigma_{cat}$.
  - Flagged 32 statistically significant outlier transactions ($z > 2$), surfacing massive shopping spikes on Amazon and Flipkart.
- **Day 6: Financial Archetype System (`evaluate_archetypes`)**
  - Programmed modular functions evaluating quantitative criteria for 8 standard fintech personas.
  - Rahul matched: **The Foodie**, **The Quick Commerce Junkie**, **The Shopaholic**, **The Investor**, **The Late-Night Snacker**, **The Subscription Lover**, and **The YOLO Spender** (and failed **The Cab Commuter** and **The Disciplined Saver**).
  - Designed bonus Bengaluru archetype: **The Bengaluru Caffeine Connoisseur** (99 cafe visits across 3 specialty brands).
- **Day 7: Final Polish & Publication**
  - Assembled the 64-character publication-ready terminal dashboard.
  - Executed notebook end-to-end with zero errors and documented student reflections.

---

## 🚀 How to Run

### Option 1: Locally
1. Clone the repository:
   ```bash
   git clone https://github.com/mohanasanthibattula-stack/SpendDNA.git
   cd SpendDNA
   ```
2. Ensure Python 3.10+ and requirements are installed:
   ```bash
   pip install pandas numpy
   ```
3. Open and run the notebook:
   ```bash
   jupyter notebook SpendDNA_YOURNAME.ipynb
   ```

### Option 2: In Google Colab
1. Upload `SpendDNA_YOURNAME.ipynb` to [Google Colab](https://colab.research.google.com/).
2. In the left file browser pane, upload `rahul_transactions.csv`.
3. Click **Runtime** > **Run all** (`Ctrl + F9`). The notebook runs top-to-bottom seamlessly.

---

## 👤 Author
- **Student:** [YOURNAME]
- **Batch:** Data Science Program October 2026 Batch
- **Project Repository:** [https://github.com/mohanasanthibattula-stack/SpendDNA](https://github.com/mohanasanthibattula-stack/SpendDNA)
