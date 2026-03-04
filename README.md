# 🎬 Netflix Data Cleaning & Business Intelligence Project

A complete data cleaning, transformation, and visualisation pipeline applied to the Netflix Movies and TV Shows dataset from Kaggle. This project covers end-to-end data preparation through to relational table design and interactive Tableau dashboards.

  [Netflix Data Cleaning – Kaggle Notebook](https://www.kaggle.com/code/dylaancodes/cleaning-netflix-data/notebook)

---

## 📦 Dataset

The raw dataset was sourced from Kaggle:
**[Netflix Movies and TV Shows – Kaggle](https://www.kaggle.com/datasets/shivamb/netflix-shows)**

---

## 🗂️ Project Overview

| Stage | Description |
|---|---|
| 🧹 Data Cleaning | Null handling, column drops, formatting |
| ✂️ Column Splitting | Decomposing multi-value fields |
| 🗃️ Relational Tables | Normalising into structured schema |
| 📊 BI Visuals | Tableau dashboards for insight |

---

## 🧹 Data Cleaning

The raw dataset contained the following 12 columns:

`show_id`, `type`, `title`, `director`, `cast`, `country`, `date_added`, `release_year`, `rating`, `duration`, `listed_in`, `description`

### Treating Nulls
- Audited all columns for missing values — `director` (2,634 nulls), `cast` (825 nulls), and `country` (831 nulls) were the most affected
- Null values in `director`, `cast`, and `country` were replaced with `'Unknown'` to retain those rows for analysis rather than losing a significant portion of the dataset
- `date_added` had a small number of nulls (10 rows) which were excluded as the date context was needed for time-based visuals
- `rating` had 4 nulls which were also excluded

### Dropping Columns
- `cast` and `description` were dropped as they held no value for the intended analysis or Tableau visuals
- `date_added` was dropped after the relevant information was extracted into two new derived columns (`month_added` and `year_added`)
- `duration` was dropped after being split into `tv_show_seasons` and `movie_duration` to separate the two distinct content types
- `listed_in` was removed from the main table after being normalised into a dedicated `genre` table
- `country` was removed from the main table after being normalised into a dedicated `countries` table

### Formatting
- Whitespace was trimmed from text fields to avoid inconsistent matching and grouping
- Text casing was normalised across name and category fields for consistency
- `release_year` was validated as an integer field throughout

---

## ✂️ Splitting Columns

Two columns were split to create more analytically useful fields:

- **`date_added`** → Split into `month_added` and `year_added`, allowing content trends to be analysed separately by month and by year
- **`duration`** → Split into `tv_show_seasons` (numeric season count) and `movie_duration` (runtime in minutes), as the two content types required different measures and combining them in one column made analysis impractical

Two columns were extracted into their own relational tables:

- **`listed_in`** → Each genre tag was separated out and stored in the `genre` table, linked back to the main table via `show_id`
- **`country`** → Each country value was separated out and stored in the `countries` table, also linked via `show_id`

---

## 🗃️ Relational Tables

The cleaned dataset was normalised into three tables:

```
netflix_titles (main table)
├── show_id (PK)
├── type
├── title
├── director
├── release_year
├── rating
├── month_added
├── tv_show_seasons
├── movie_duration
└── year_added

genre
├── g_id (PK)
├── show_id (FK → netflix_titles)
└── genre

countries
├── c_id (PK)
├── show_id (FK → netflix_titles)
└── country
```

`genre` and `countries` both relate back to `netflix_titles` via `show_id`, supporting one-to-many relationships for multi-genre and multi-country titles. This structure keeps the main table clean while allowing flexible filtering and aggregation in SQL and Tableau.

---

## 📊 Business Intelligence — Tableau

Tableau dashboards were built on top of the cleaned relational tables to surface key insights:

### Visuals Included

- **Heat Map** — Cross-tabulation view highlighting content volume patterns across categories
- **Content Pie** — Breakdown of the catalogue by content type (Movies vs TV Shows)
- **Top 10 Netflix Countries** — Ranked view of the countries producing the most Netflix content
- **Split of Content in Top 10 Countries** — Movies vs TV Shows breakdown within each of the top 10 producing countries
- **Top 10 Genres** — The most common genre categories across the full catalogue
- **Content Added vs Year** — Trend of how much content was added to Netflix each year
- **Average Delay in Years for Movie Added** — The average gap between a movie's release year and when it was added to Netflix
- **Ratings** — Distribution of content across audience rating categories

> 📌 Tableau dashboards and workbook files can be found in the `/tableau` directory of this repository.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| SQL | Data cleaning, transformation & relational table creation |
| Tableau | Business intelligence dashboards |
| Kaggle | Version control & project hosting |

---

## 📁 Repository Structure

```
netflix-cleaning-project/
│
├── data/
│   ├── raw/                  # Original Kaggle dataset
│   └── cleaned/              # Processed output files
│
├── sql/
│   └── create_tables.sql     # Schema definitions & cleaning queries
│
├── tableau/
│   └── netflix_dashboard.twbx
│
└── README.md
```

---

## 🚀 Getting Started

Visit:
[Netflix Data Cleaning – Kaggle Notebook](https://www.kaggle.com/code/dylaancodes/cleaning-netflix-data/notebook)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
