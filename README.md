# SQL Summative Lab

Two-part SQL + Python analysis project: guided querying and visualization of a
wholesale model-car customer database, followed by an open-ended exploratory
analysis of an IMDB movie dataset, delivered as a short slide presentation.

## Contents

| File | Description |
|---|---|
| `SQLSummativeLab.ipynb` | Completed Jupyter notebook for both Part 1 and Part 2 |
| `movie_data_exploration.pptx` | 4-slide deck summarizing the Part 2 findings |
| `data.sqlite` | Customer/orders database used in Part 1 (*not included — see Setup*) |
| `im.db.zip` | Zipped IMDB movie database used in Part 2 (*not included — see Setup*) |
| `images/` | `toycars.jpg`, `ERD.png`, `movie_data_erd.jpeg` referenced by the notebook's markdown cells (*not included — see Setup*) |

## Setup

The notebook expects the following files in the **same folder** as
`SQLSummativeLab.ipynb`:

```
.
├── SQLSummativeLab.ipynb
├── data.sqlite
├── im.db.zip
└── images/
    ├── toycars.jpg
    ├── ERD.png
    └── movie_data_erd.jpeg
```

1. Place `data.sqlite` (the customers/orders/products database) in the project root.
2. Place `im.db.zip` (the zipped IMDB database) in the project root — the notebook
   unzips it to `im.db` automatically in Part 2.
3. Create an `images/` folder containing the three referenced images if you want
   the markdown cells to render the pictures.
4. Install dependencies: `pandas`, `matplotlib`, `sqlite3` (standard library).

Then open and run the notebook top to bottom.

## Part 1: Querying a Customer Database

Business questions answered via SQL, each assigned to its own DataFrame variable:

1. **Connect** to `data.sqlite` with `sqlite3` + `pandas`.
2. **California high-credit customers** — CA customers with `creditLimit > 25000`
   (credit limit is stored as text, so it's cast to `FLOAT` before comparing).
3. **International "Collectable" campaign** — non-US customers with "Collect" in
   their company name.
4. **Average credit limit by US state** — visualized as a bar chart.
5. **Top 10 customers by total payments** — visualized as a horizontal bar chart.
6. **Products purchased in quantities ≥ 10, per customer** — grouped, filtered
   with `HAVING`, and sorted ascending by quantity.
7. **Product line analysis** — total distinct products vs. total quantity ordered
   per product line (using `COUNT(DISTINCT productCode)` to avoid inflating the
   product count via the join with order details), visualized as a scatter plot.
8. **Remote office staffing** — employees, job titles, and supervisors for any
   office with fewer than 5 employees, found via a `GROUP BY … HAVING` subquery.
9. **Close the connection.**

Each visualization-required step includes a matplotlib chart with a verbose
title and labeled axes. Two reflection questions are answered in markdown cells
explaining the `WHERE` clause logic and the subquery reasoning in plain language.

## Part 2: Exploratory Analysis of the IMDB Movie Data

Open-ended exploration of movies released 2010–2019, joined with their ratings,
splitting the multi-value `genres` field to compare genres by average rating,
average audience size (`numvotes`), and number of titles.

**Key finding:** Documentary has the highest average rating (~7.3) but by far
the smallest audience; Action and Sci-Fi have some of the largest audiences but
only middling average ratings — suggesting a popularity/rating tradeoff.

**Business question:** Does a genre's popularity trade off against its average
rating, and should that inform marketing vs. content-investment decisions?

**Data cleaning issues identified** (not corrected, per assignment
instructions):
- `start_year` values far outside the documented 2010–2019 range (e.g. 2115)
- Implausible `runtime_minutes` outliers (tens of thousands of minutes)
- Missing `runtime_minutes` values
- The multi-value, comma-separated `genres` field, which requires splitting
  before genre-level analysis
- Missing `genres` values

See `movie_data_exploration.pptx` for the slide summary of these findings,
including the genre popularity-vs-rating scatter plot.
