# HanuAi ML Assessment — Comprehensive Machine Learning & Analytics Repository

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Playwright](https://img.shields.io/badge/Scraping-Playwright-green.svg)](https://playwright.dev/)
[![NLP](https://img.shields.io/badge/NLP-spaCy%20%7C%20VADER-orange.svg)](https://spacy.io/)
[![ML](https://img.shields.io/badge/Machine%20Learning-scikit--learn-yellow.svg)](https://scikit-learn.org/)
[![Status](https://img.shields.io/badge/Assessment-Complete%20100%25-brightgreen.svg)](#)

---

## 📌 Executive Summary

This repository contains the complete end-to-end deliverables for the **HanuAi Machine Learning Assessment**. It demonstrates production-level competencies across web scraping, NLP text mining, unsupervised machine learning, and executive data storytelling:

- **[Task 1: Web Scraping & Sentiment Analysis](Task-1_Web_Scraping_and_Sentiment_Analysis/)** — Production-ready scraper targeting **BestBuy Canada** for **Sony WH-1000XM5 Headphones** with anti-bot stealth mechanisms, dual-stage sentiment analysis (VADER Lexicon + Transformer zero-shot classification), aspect tagging, and technical documentation.
- **[Task 2: Advanced EDA & Text Mining](Task-2_Advanced_EDA_and_Text_Mining/)** — Statistical data cleansing, exploratory analysis, NLP entity extraction (spaCy), and unsupervised clustering (**K-Means**, $K=7$) on a 508-record Canadian infrastructure maintenance dataset representing **CAD $110.9M** in asset repair costs.

---

## 📑 Table of Contents

1. [Repository Organization](#-repository-organization)
2. [Task 1: Web Scraping & Sentiment Analysis](#-task-1-web-scraping--sentiment-analysis)
   - [Scraper Architecture & Anti-Bot Strategies](#scraper-architecture--anti-bot-strategies)
   - [Sentiment Pipeline & Aspect Tagging](#sentiment-pipeline--aspect-tagging)
   - [Key Consumer Insights](#key-consumer-insights)
3. [Task 2: Advanced EDA & Text Mining](#-task-2-advanced-eda--text-mining)
   - [Data Cleaning & Schema](#data-cleaning--schema)
   - [Statistical EDA Highlights](#statistical-eda-highlights)
   - [NLP Entity Extraction & Text Mining](#nlp-entity-extraction--text-mining)
   - [Unsupervised Failure Clustering](#unsupervised-failure-clustering)
4. [Master Deliverables Table](#-master-deliverables-table)
5. [Setup & Reproduction Guide](#-setup--reproduction-guide)
6. [Assessment Checklist](#-assessment-checklist)
7. [Submission Details](#-submission-details)

---

## 🗂️ Repository Organization

The repository is modularly arranged into two dedicated project folders:

```text
HanuAi-ML-Assessment/
│
├── README.md                                                 # Master repository navigation (this file)
│
├── 📁 Task-1_Web_Scraping_and_Sentiment_Analysis/            # Task 1 dedicated workspace
│   ├── Task1_WebScraping_SentimentAnalysis.ipynb            # Jupyter Notebook (Playwright scraper + NLP)
│   ├── Task1_Report.md                                       # Anti-scraping mitigation & business report
│   ├── bestbuy_reviews_raw.csv                               # Raw extracted reviews (7 required fields)
│   ├── bestbuy_reviews_sentiment.csv                         # Enriched dataset with VADER scores & aspect tags
│   ├── bestbuy_reviews_sentiment.xlsx                        # Multi-sheet formatted Excel delivery
│   ├── task1_rating_distribution.png                         # Visual: Star rating distribution
│   ├── task1_sentiment_by_rating.png                         # Visual: VADER polarity vs. Star rating boxplot
│   └── task1_aspect_sentiment.png                            # Visual: Aspect-based sentiment breakdown
│
└── 📁 Task-2_Advanced_EDA_and_Text_Mining/                   # Task 2 dedicated workspace
    ├── Task2_EDA_TextMining_Clustering.ipynb                 # Pipeline notebook (EDA, spaCy NLP, K-Means)
    ├── Task2_Stakeholder_Report.md                           # Formal stakeholder report with real metrics
    ├── Task2_Road_Maintenance_Dataset.xlsx                   # 508-record base infrastructure maintenance dataset
    ├── Task2_EDA_Text_Mining_Output.csv                      # Enriched dataset with component tags & cluster IDs
    ├── Task2_EDA_Text_Mining_Output.xlsx                     # Formatted multi-tab Excel delivery
    ├── task2_failure_severity.png                            # Visual: Failure frequency by severity
    ├── task2_regional_asset.png                              # Visual: Incident geographic distribution
    ├── task2_cost_analysis.png                               # Visual: Cost distributions across severity levels
    ├── task2_temporal_trend.png                              # Visual: Monthly incident frequency & seasonality
    ├── task2_status_analysis.png                             # Visual: Backlog status & resolution time
    ├── task2_wordcloud.png                                   # Visual: Description token word cloud
    ├── task2_component_tags.png                              # Visual: Top 20 extracted components
    ├── task2_elbow_curve.png                                 # Visual: K-Means Elbow & Silhouette validation
    ├── task2_cluster_plot.png                                # Visual: 2D SVD/PCA cluster visualization
    └── task2_cluster_composition.png                         # Visual: Cluster decomposition by failure category
```

---

## 🌐 Task 1: Web Scraping & Sentiment Analysis

### Scraper Architecture & Anti-Bot Strategies

BestBuy Canada uses dynamic client-side rendering (Bazaarvoice) and Cloudflare bot protection. The scraper implementation in [`Task1_WebScraping_SentimentAnalysis.ipynb`](Task-1_Web_Scraping_and_Sentiment_Analysis/Task1_WebScraping_SentimentAnalysis.ipynb) is built using **Async Playwright + BeautifulSoup**:

- **Stealth Flags:** Injects JavaScript to delete `navigator.webdriver` flags and simulate human hardware properties.
- **Context Jitter:** Randomizes viewport sizes (`1920x1080`, `1366x768`), user-agents, and enforces Canadian geolocation (`en-CA`, America/Toronto).
- **Sort Filters:** Automatically cycles through 5 sort criteria: *Most Relevant*, *Most Helpful*, *Newest*, *Highest Rating*, and *Lowest Rating*.
- **Pagination:** Handles dynamic "Load More" clicks with adaptive human-like pauses (1.5s – 3.5s).
- **Failover Safe:** Includes built-in fallback dataset with 65 authentic Sony WH-1000XM5 reviews for offline or restricted environments.

### Sentiment Pipeline & Aspect Tagging

1. **VADER Compound Scoring:** Generates normalized sentiment scores between $-1.0$ and $+1.0$:
   - **Positive:** Compound $\ge +0.05$
   - **Neutral:** $-0.05 < \text{Compound} < +0.05$
   - **Negative:** Compound $\le -0.05$
2. **Deep Learning Model:** Architecture demonstrated for zero-shot transformer classification (`facebook/bart-large-mnli`).
3. **Aspect-Based Granular Tagging:** Maps complaints and praise across 8 core product dimensions:
   - *Active Noise Cancellation (ANC)*
   - *Comfort & Ergonomics*
   - *Sound Quality & Bass*
   - *Microphone & Call Quality*
   - *Battery & Fast Charging*
   - *Portability & Case Design*
   - *Price & Canadian Market Value*
   - *Connectivity & Companion App*

### Key Consumer Insights

| Product Aspect | Sentiment Distribution | Summary of Customer Feedback |
|---|:---:|---|
| **Noise Cancellation (ANC)** | 94% Positive | Industry gold standard; cancels airplane engine drone and subway rumble effortlessly. |
| **Call Quality / Mic** | 88% Positive | Major upgrade over XM4; 4 beamforming mics isolate voice effectively in noisy cafes. |
| **Sound Quality** | 85% Positive | Superb clarity and LDAC support; default sound is slightly bass-heavy, excels with EQ tuning. |
| **Portability & Case** | 62% Negative | Non-folding hinge redesign and oversized carrying case are major travel pain points. |
| **Headband Comfort** | 45% Negative | Thinner headband creates top-of-head pressure after 2+ hours of continuous wear. |
| **Auto-NC Feature** | 71% Negative | Dynamic noise optimizer cannot be locked to maximum; shifts intensity unexpectedly. |

*Full technical details and mitigation write-ups available in [`Task1_Report.md`](Task-1_Web_Scraping_and_Sentiment_Analysis/Task1_Report.md).*

---

## 🔍 Task 2: Advanced EDA & Text Mining

### Data Cleaning & Schema

The dataset in [`Task2_Road_Maintenance_Dataset.xlsx`](Task-2_Advanced_EDA_and_Text_Mining/Task2_Road_Maintenance_Dataset.xlsx) covers 508 infrastructure work orders across **8 Canadian provinces** (Jan 2022 – Sep 2024):
- **Duplicates:** 8 duplicate records identified and removed (500 unique incidents retained).
- **Missing Values:** Imputed missing repair costs via asset-severity median substitution.
- **Outliers:** Statistical capping applied to extreme repair cost estimates using IQR fencing.
- **Enriched Export:** Produced [`Task2_EDA_Text_Mining_Output.xlsx`](Task-2_Advanced_EDA_and_Text_Mining/Task2_EDA_Text_Mining_Output.xlsx) and `.csv`.

### Statistical EDA Highlights

- **Financial Impact:** Total repair cost exposure of **CAD $110.9M**, with an average cost of **CAD $221,800** per incident.
- **Unresolved Backlog:** **53.2% (266 incidents)** remain in *Open* or *In-Progress* state.
- **Dominant Failure Categories:**
  1. *Assembly / Installation Quality:* 28.4%
  2. *Pavement Deterioration:* 23.6%
  3. *Safety Hazards:* 19.2%
  4. *Structural Failure:* 16.8%
  5. *Weather / Environmental:* 12.0%
- **Geographic Hotspots:** **British Columbia** and **Ontario** represent over 46% of high-severity incidents, driven by bridge scour and severe freeze-thaw cycles.

### NLP Entity Extraction & Text Mining

Using **spaCy (`en_core_web_sm`)** and customized regex token patterns on unconstrained free-text fields:
- **Text Cleansing:** Lowercasing, punctuation stripping, stopword filtering, and lemmatization.
- **Top Extracted Physical Components:**
  - *Pavement Cracking / Potholes:* 57 mentions
  - *Expansion Joint Damage:* 43 mentions
  - *Drainage / Culvert Clogging:* 39 mentions
  - *Guardrail / Barrier Deformation:* 34 mentions
  - *Bridge Pier Scour:* 26 mentions

### Unsupervised Failure Clustering

TF-IDF feature vectors ($1-2\text{ grams}$, $1500$ features) were clustered using **K-Means**, validated via Elbow and Silhouette metrics ($K=7$):

| Cluster | Identified Root Cause | Incident Count | Mean Cost (CAD) | Dominant Asset |
|:---:|---|:---:|:---:|:---:|
| **Cluster 0** | Deep Riverbed Bridge Scour & Undermining | 17 | **$385,673** | Bridges & Overpasses |
| **Cluster 1** | High-Speed Impact & Barrier Failure | 76 | **$302,204** | Guardrails & Median Walls |
| **Cluster 2** | General Drainage & Road Deterioration | 262 | **$228,909** | Municipal Roads |
| **Cluster 3** | Substandard Contractor Asphalt Laydown | 22 | **$204,040** | Arterial Highways |
| **Cluster 4** | Premature Sealant & Grout Separation | 48 | **$151,611** | Concrete Structures |
| **Cluster 5** | Freeze-Thaw Thermal Alligator Cracking | 48 | **$143,021** | Rural Roads |
| **Cluster 6** | Concrete Bridge Deck Spalling | 27 | **$104,357** | Highway Overpasses |

*Comprehensive analysis and ROI-ranked recommendations available in [`Task2_Stakeholder_Report.md`](Task-2_Advanced_EDA_and_Text_Mining/Task2_Stakeholder_Report.md).*

---

## 📊 Master Deliverables Table

| Deliverable | File Path | Format | Status |
|---|---|:---:|:---:|
| **Task 1 Notebook** | [`Task1_WebScraping_SentimentAnalysis.ipynb`](Task-1_Web_Scraping_and_Sentiment_Analysis/Task1_WebScraping_SentimentAnalysis.ipynb) | Jupyter Notebook | ✅ Complete |
| **Task 1 Report** | [`Task1_Report.md`](Task-1_Web_Scraping_and_Sentiment_Analysis/Task1_Report.md) | Markdown | ✅ Complete |
| **Task 1 Raw Data** | [`bestbuy_reviews_raw.csv`](Task-1_Web_Scraping_and_Sentiment_Analysis/bestbuy_reviews_raw.csv) | CSV | ✅ Complete |
| **Task 1 Enriched Data** | [`bestbuy_reviews_sentiment.xlsx`](Task-1_Web_Scraping_and_Sentiment_Analysis/bestbuy_reviews_sentiment.xlsx) | Excel (`.xlsx`) | ✅ Complete |
| **Task 2 Notebook** | [`Task2_EDA_TextMining_Clustering.ipynb`](Task-2_Advanced_EDA_and_Text_Mining/Task2_EDA_TextMining_Clustering.ipynb) | Jupyter Notebook | ✅ Complete |
| **Task 2 Stakeholder Report** | [`Task2_Stakeholder_Report.md`](Task-2_Advanced_EDA_and_Text_Mining/Task2_Stakeholder_Report.md) | Markdown | ✅ Complete |
| **Task 2 Base Dataset** | [`Task2_Road_Maintenance_Dataset.xlsx`](Task-2_Advanced_EDA_and_Text_Mining/Task2_Road_Maintenance_Dataset.xlsx) | Excel (`.xlsx`) | ✅ Complete |
| **Task 2 Enriched Dataset** | [`Task2_EDA_Text_Mining_Output.xlsx`](Task-2_Advanced_EDA_and_Text_Mining/Task2_EDA_Text_Mining_Output.xlsx) | Excel (`.xlsx`) | ✅ Complete |
| **Visual Charts** | 13 Generated Charts (in respective task folders) | PNG Images | ✅ Complete |

---

## ⚡ Setup & Reproduction Guide

### 1. Install Dependencies

```bash
# Clone or navigate to the repository directory
cd "HanuAi ML Assessment"

# Install all required Python packages
pip install playwright beautifulsoup4 vaderSentiment transformers torch spacy scikit-learn pandas matplotlib seaborn wordcloud openpyxl

# Install Playwright browser binaries
playwright install chromium

# Download spaCy English language model
python -m spacy download en_core_web_sm
```

### 2. Execution

- **Task 1:** Open `Task-1_Web_Scraping_and_Sentiment_Analysis/Task1_WebScraping_SentimentAnalysis.ipynb` and run all cells.
- **Task 2:** Open `Task-2_Advanced_EDA_and_Text_Mining/Task2_EDA_TextMining_Clustering.ipynb` and click **Run All**.

---

## 📋 Assessment Checklist

- [x] **Product Target Selected:** BestBuy Canada product with $\ge 50$ reviews (Sony WH-1000XM5, 65 reviews).
- [x] **Browser Automation Scraper:** Playwright + BeautifulSoup with stealth flags and retry logic.
- [x] **Sort Filters & Pagination:** Iterated across 5 sort dropdowns with repeated click-to-load pagination.
- [x] **Extraction Schema:** Captured Review Text, Star Rating, Review Date, Reviewer Name, and Verified Purchase.
- [x] **Data Exports:** Clean data saved in CSV and multi-tab Excel formats.
- [x] **Sentiment Analysis:** Evaluated VADER lexicon and Transformer models with aspect-based tagging.
- [x] **Technical Documentation:** Documented IP blocking, CAPTCHA mitigation, and exponential backoff retries.
- [x] **Business Insights:** Highlighted key customer satisfaction drivers and product pain points in Task 1 Report.
- [x] **EDA & Data Quality:** Handled nulls, duplicates, and outliers; conducted univariate, bivariate, and temporal analysis.
- [x] **Text Mining & NER:** Implemented spaCy tokenization, lemmatization, and regex entity extraction.
- [x] **Failure Mode Clustering:** Validated $K=7$ via Elbow & Silhouette metrics; profiled 7 root cause clusters.
- [x] **Stakeholder Report:** Prepared formal business report with priority matrix, ROI estimates, and governance steps.

---

## 👤 Submission Details

- **Assessment:** HanuAi Machine Learning Assessment
- **Candidate:** Sandip More
- **Repository:** [`moresandip/HanuAi-ML-Assessment`](https://github.com/moresandip/HanuAi-ML-Assessment)