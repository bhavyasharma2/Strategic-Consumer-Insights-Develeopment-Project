# Consumer Insights Analytics: Beats by Dre

An end-to-end consumer intelligence pipeline analyzing Amazon reviews for Beats by Dre and 10 competitor headphone brands, combining web scraping, EDA, sentiment analysis, and LLM-powered insight extraction to deliver strategic brand positioning recommendations.

---

## Overview

This project was completed as part of a Data Analytics Externship with Beats by Dre (via Extern). The goal was to understand consumer sentiment towards Beats products relative to competitors, identify recurring pain points and strengths, and translate quantitative findings into actionable business recommendations.

The pipeline covers the full data lifecycle: scraping raw review data, cleaning and preprocessing, exploratory analysis, sentiment classification, LLM-powered competitive benchmarking, and final strategic reporting.

---

## Problem Statement

Given the highly competitive premium headphone market, Beats by Dre needed a data-driven understanding of how its products are perceived relative to competitors like Bose, Sony, JBL, Anker/Soundcore, and TOZO. Specifically:

- How does consumer sentiment towards Beats compare to competitors?
- What product attributes drive positive and negative reviews?
- Where does Beats have a competitive advantage or gap?
- What strategic actions can improve brand positioning?

---

## Pipeline Architecture

```
Amazon Product Pages
        |
        v
Oxylabs API (Web Scraping)
        |
        v
Raw Review Dataset (CSV)
        |
        v
Data Cleaning & Preprocessing (Pandas)
        |
        v
Exploratory Data Analysis (Pandas, Seaborn, Matplotlib)
        |
        v
Sentiment Analysis (TextBlob)
        |
        v
Correlation Analysis (SciPy - Point Biserial)
        |
        v
LLM-Powered Insight Extraction (Gemini 1.5 Flash API)
        |
        v
SWOT Analysis + Strategic Recommendations Report
```

---

## Methodology

### Data Collection
- Scraped 1,200 Amazon reviews across 12 headphone products (2 Beats, 10 competitors) using the Oxylabs API
- Each review record includes: review ID, product ID, title, author, star rating, review content, timestamp, verified purchase status, helpful vote count, and product attributes
- Managed API rate limits and data quotas through scheduled batch collection

### Data Cleaning
- Identified and handled 103 null values across the author, content, and product attributes columns
- Replaced null values with placeholder to preserve dataset integrity rather than dropping rows
- Validated data types, checked for duplicates, and computed IQR-based outlier detection on helpful vote counts
- Retained outliers to preserve full variability in the dataset

### Exploratory Data Analysis
- Computed descriptive statistics (mean, median, mode, variance, standard deviation, quantiles) for rating and helpful count columns
- Visualized rating distributions using histograms and box plots, segmented by Beats vs. competitors
- Analyzed review volume and average ratings by product attribute (color, style, model variant)
- Plotted scatter plots of rating vs. helpful count to examine engagement patterns

### Sentiment Analysis
- Applied TextBlob polarity scoring to classify each review as positive, neutral, or negative
- Aggregated sentiment proportions separately for Beats and competitor products
- Compared sentiment distributions with grouped bar charts

### Correlation Analysis
- Computed point-biserial correlation between star rating and helpful vote count for the full dataset, Beats products, and competitor products separately
- Found a statistically significant weak negative correlation overall (r = -0.18, p < 0.001), with a stronger negative correlation for Beats specifically (r = -0.36, p < 0.001)

### LLM-Powered Insight Extraction
- Integrated Gemini 1.5 Flash API with structured prompt engineering to conduct aspect-level analysis at scale
- Designed 10 targeted prompts covering: sound quality comparison, customer satisfaction, price-performance ratio, build quality and durability, noise cancellation effectiveness, customer service and warranty experiences, and identification of common complaints
- Used the top 300 most helpful reviews as a high-signal subset for one prompt to surface insights that the broader user base found most credible

### Strategic Deliverables
- Compiled findings into a structured SWOT analysis covering Beats strengths, weaknesses, opportunities, and threats relative to the competitive landscape
- Produced a strategic recommendations report with specific product improvement and marketing strategy guidance
- Outlined areas for future research including demographic segmentation, longitudinal analysis, and usability studies

---

## Key Findings

**Beats Strengths:**
- Consistently praised for stylish design and aesthetic appeal
- Strong bass signature well-received by casual listeners
- Seamless Apple device integration via W1 chip is a significant differentiator

**Beats Weaknesses:**
- Active Noise Cancellation rated significantly below Bose and Sony across reviews
- Recurring build quality complaints, particularly hinge mechanism failures and earcup deterioration
- Micro USB charging perceived as outdated relative to competitors using USB-C
- Premium pricing not consistently justified by feature set compared to value brands

**Competitive Landscape:**
- Bose and Sony lead on ANC and balanced audio quality
- Anker/Soundcore and TOZO outperform on price-performance ratio
- Beats occupies a style and brand identity niche that competitors have not replicated

**Sentiment Summary:**
- Beats: 86.5% positive, 9.0% neutral, 4.5% negative
- Competitors: 93.9% positive, 3.5% neutral, 2.6% negative

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Oxylabs API | Web scraping Amazon product reviews |
| Pandas | Data manipulation, cleaning, grouping |
| Matplotlib / Seaborn | EDA visualizations |
| TextBlob | Sentiment polarity classification |
| SciPy | Point-biserial correlation analysis |
| Gemini 1.5 Flash API | LLM-powered aspect-level insight extraction |
| Google Colab | Development environment |

---

## Project Structure

```
├── eda_notebook.ipynb              # Data cleaning, EDA, sentiment analysis, correlation
├── gemini_insights.ipynb           # Gemini API prompt engineering and insight extraction
├── capstone_report.ipynb           # Final findings, SWOT, and recommendations
├── requirements.txt
└── README.md
```

---

## Dataset

Reviews were collected from Amazon using the Oxylabs API. The dataset is not included in this repository. To replicate the data collection, you will need an active Oxylabs API subscription and credentials.

The 12 products analyzed include:

- Beats Studio3 Wireless (B085296FLT)
- Beats Solo3 Wireless (B0CCBKGDJD)
- 10 competitor products from Bose, Sony, JBL, Panasonic, Anker/Soundcore, TOZO, Jabra, and others

---

## How to Run

1. Clone the repository
2. Install dependencies: `pip install -r requirements.txt`
3. Add your Gemini API key as an environment variable: `export GEMINI_API_KEY=your_key_here`
4. Run notebooks in order: EDA first, then Gemini insights, then the capstone report

**Requirements:**
```
pandas
numpy
matplotlib
seaborn
textblob
scipy
google-generativeai
```

---

## Key Takeaways

- Web-scraped data requires significantly more preprocessing effort than curated datasets; null handling decisions have real downstream impact on analysis quality
- TextBlob sentiment classification works well for high-volume directional analysis but lacks the nuance needed for aspect-level insights, which is where the Gemini API added the most value
- Structuring LLM prompts around specific business questions (price-performance, durability, customer service) produced far more actionable outputs than open-ended summarization
- The negative correlation between rating and helpful vote count for Beats specifically (r = -0.36) suggests that lower-rated, critical reviews of Beats products attract disproportionate engagement, which is a meaningful signal for brand perception
