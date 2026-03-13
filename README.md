# superbowl-halftime-sentiment-nlp

Public Sentiment Analysis of Super Bowl Halftime Shows — Before & After the NFL's Roc Nation Partnership  
Unstructured Data Analytics | MSBA, University of Texas | Fall 2025

**Team (Group 3):** Aileen · Aubrey Oh · Emma · Liyan · Nisha

---

## Overview

This project investigates how **public sentiment and engagement** toward Super Bowl Halftime Shows shifted before and after the NFL's partnership with Roc Nation (post-2019), using unstructured data from **YouTube** and **Reddit**.

We scraped ~60,000 comments across 6 halftime shows, applied NLP pipelines (VADER sentiment analysis, BERTopic topic modeling), ran statistical hypothesis tests (Welch t-test, Mann-Whitney U), and visualized the results across platforms and eras.

### Research Question
> *How does public sentiment toward Super Bowl Halftime Shows differ across Reddit and YouTube, and did the NFL's Roc Nation partnership lead to any measurable shifts in sentiment or engagement?*

---

## Shows Analyzed

| Era | Year | Artist(s) |
|-----|------|-----------|
| **Pre–Roc Nation** | 2015 | Katy Perry |
| **Pre–Roc Nation** | 2016 | Coldplay ft. Beyoncé & Bruno Mars |
| **Pre–Roc Nation** | 2017 | Lady Gaga |
| **Post–Roc Nation** | 2020 | Shakira & Jennifer Lopez |
| **Post–Roc Nation** | 2022 | Dr. Dre, Snoop Dogg, Eminem, Mary J. Blige, Kendrick Lamar |
| **Post–Roc Nation** | 2023 | Rihanna |

---

## Repository Structure

```
superbowl-halftime-sentiment-nlp/
│
├── notebooks/
│   ├── Webscrapping_YT_Comments.ipynb        # YouTube API scraping (6-month window)
│   ├── Cleaning_Preprocessing.ipynb          # Data cleaning, deduplication, language filter
│   ├── Sentiment_EDA.ipynb                   # VADER sentiment scoring + EDA
│   ├── topic_modeling_Emma.ipynb             # BERTopic topic modeling (Reddit + YouTube)
│   ├── Roc_Nisha.ipynb                       # Roc Nation era comparison + statistical tests
│   └── integrated_visualization_Julie.ipynb  # Full integrated dashboard & final visualizations
│
├── data/                                     # (not tracked — see Data section below)
│   ├── comments_initial_window_withReplies.parquet
│   ├── comments_yt_180d_clean_balanced.csv
│   ├── comments_yt_180d_clean_unbalanced.csv
│   ├── comments_reddit_180d_clean_balanced.csv
│   └── comments_reddit_180d_clean_unbalanced.csv
│
├── presentation/
│   └── Group3-Super_Bowl_Halftime_Show.pptx  # Final slide deck
│
└── README.md
```

---

## Pipeline Overview

```
YouTube API / Reddit Scraping
        ↓
Cleaning & Preprocessing
  (emoji removal, language detection, deduplication, balancing)
        ↓
    ┌───────────────────────────────────┐
    │  VADER Sentiment Analysis         │  → compound scores, pos/neu/neg labels
    │  BERTopic Topic Modeling          │  → per-show topic distributions
    │  Roc Nation Era Comparison        │  → pre/post statistical tests
    └───────────────────────────────────┘
        ↓
Integrated Visualizations & Final Insights
```

---

## Notebooks

### 1. `Webscrapping_YT_Comments.ipynb`
Collects YouTube comments using the YouTube Data API v3.

- Scrapes 6 official Super Bowl Halftime Show videos
- Captures comments up to **6 months** after each show date
- Outputs: video metadata CSV + raw comment parquet with replies

---

### 2. `Cleaning_Preprocessing.ipynb`
Prepares raw data for NLP analysis.

- Removes duplicates, bots, non-English comments (`langdetect`)
- Strips emojis, URLs, special characters (`emoji` library)
- Adds features: `show_key`, `platform`, `created_dt`, era label (`pre_roc` / `post_roc`)
- Produces both **balanced** (equal samples per show) and **unbalanced** datasets

---

### 3. `Sentiment_EDA.ipynb`
Runs VADER sentiment analysis and exploratory data analysis on both platforms.

**VADER outputs per comment:** `compound`, `pos`, `neu`, `neg`  
**Labeling rule:** positive (compound ≥ 0.05), negative (≤ −0.05), neutral otherwise

**Key findings:**
- YouTube consistently more positive than Reddit (mean compound: **0.266 vs 0.127**)
- Platform effect holds on both balanced and unbalanced datasets
- Visual spectacle shows (Shakira/JLo 2020) show the largest YouTube vs. Reddit sentiment gap

---

### 4. `topic_modeling.ipynb`
Applies **BERTopic** separately to Reddit and YouTube corpora.

**Model stack:** `all-MiniLM-L6-v2` embeddings → HDBSCAN clustering → KeyBERT representation  
**Fallback:** `gensim` LDA topic modeling for comparison

**Topics identified per show:**

| Show | Main Topics |
|------|------------|
| 2015 Katy Perry | Performance Praise, Left Shark, Halftime vs. Game |
| 2016 Coldplay/Beyoncé/Bruno | Setlist & Collaboration, Politics, Halftime vs. Game |
| 2017 Lady Gaga | Performance Praise, Politics, Dance & Choreography |
| 2020 Shakira & JLo | Dance & Choreography, Setlist, Performance Praise |
| 2022 Dr. Dre et al. | Performance Praise, Nostalgia, Production Quality |
| 2023 Rihanna | Performance Praise, Brand & Visuals, Halftime vs. Game |

---

### 5. `Roc.ipynb`
Statistical comparison of pre vs. post Roc Nation eras.

- Sentiment distributions by era and platform (boxplots)
- Engagement metrics: Reddit `score`, YouTube `like_count`
- Topic prevalence shifts: comment share per topic, pre vs. post
- **Statistical tests:** Welch t-test + Mann-Whitney U (non-parametric)
- Outputs CSVs: `sentiment_overall_by_era.csv`, `sentiment_by_era_by_platform.csv`

---

### 6. `integrated_visualization.ipynb`
Combines all team outputs into a unified visual dashboard.

4 sections: Sentiment Evolution · Topic Evolution · Show-Specific Analysis · Statistical Rigor

---

## Key Findings

### Platform Effect
- **YouTube is consistently more positive than Reddit** across all 6 shows and both eras
- Balanced dataset: YouTube mean compound **0.266** vs Reddit **0.127**
- Reflects a persistent **platform culture difference**, not a show-mix artifact

### Roc Nation Era
- **Minimal aggregate sentiment change post-Roc Nation** — both platforms held their emotional identity
- Post-Roc Nation shows introduce more **"Brand/Visuals"** and **"Identity/Culture"** topics
- **Recent shows are more emotionally consistent** (lower variance) than earlier ones

### Topic Trends
- **"halftime_vs_game"** dominates (~22–27%) in both eras — audiences debate show quality vs. game entertainment
- **"performance_praise"** is stable and strong throughout
- Post-Roc Nation: more visual/cultural framing; Pre-Roc Nation: more sharp swings (e.g., Left Shark virality, political debates)

### Aha Moment
> Despite major industry changes (the Roc Nation partnership), **platform cultures maintained their emotional identity**. YouTube stayed positive and energetic; Reddit stayed critical and community-driven.

---

## Methods & Tools

| Component | Tool / Library |
|-----------|---------------|
| Web scraping | YouTube Data API v3, `requests` |
| Data processing | `pandas`, `numpy`, `pyarrow` |
| Language detection | `langdetect` |
| Emoji handling | `emoji` |
| Sentiment analysis | `nltk` VADER |
| Topic modeling | `BERTopic`, `HDBSCAN`, `UMAP`, `SentenceTransformers` |
| Alternative topics | `gensim` LDA |
| Statistical tests | `scipy.stats` (Welch t-test, Mann-Whitney U) |
| Visualization | `matplotlib`, `seaborn` |

---

## Data

Raw and processed data files are **not tracked** due to size. To reproduce:

1. Run `Webscrapping_YT_Comments.ipynb` with a valid YouTube Data API v3 key
2. Supply Reddit comment exports (via PRAW or Pushshift)
3. Run `Cleaning_Preprocessing.ipynb` to generate all clean CSVs

> **Note:** Notebooks were developed on **Google Colab** with Google Drive mounting. Paths like `/content/drive/MyDrive/...` will need to be updated for local runs.

---

## Execution Order

```
1. Webscrapping_YT_Comments.ipynb
2. Cleaning_Preprocessing.ipynb
3. Sentiment_EDA.ipynb
4. topic_modeling_Emma.ipynb
5. Roc_Nisha.ipynb
6. integrated_visualization_Julie.ipynb
```

### Install Dependencies

```bash
pip install pandas numpy pyarrow emoji langdetect nltk vaderSentiment
pip install bertopic[all]==0.16.0 hdbscan umap-learn sentence-transformers gensim
pip install matplotlib seaborn scipy statsmodels
```

---

## Presentation

See [`presentation/Group3-Super_Bowl_Halftime_Show.pptx`](./presentation/Group3-Super_Bowl_Halftime_Show.pptx) for the final slide deck.

---

## License

Completed as coursework for an Unstructured Data Analytics course at the University of Texas MSBA program. For academic use only.
