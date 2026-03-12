# Journal Observatory Dashboard

An interactive dashboard for exploring how data visualisation practices in scientific publishing have changed over time. It focuses on a simple but important question: **are researchers increasingly choosing more informative ways to display their data?**

---

## What This Dashboard Shows

For each Research Field tracked, you can follow two metrics year by year:

- 🟣 **% Bars** — the share of papers that include bar charts, a common but often criticised visualisation type that can obscure the underlying distribution of data.
- 🩵 **% Informative Graphs** — the share of papers using more informative alternatives — such as box plots, violin plots, or dot plots — that better represent individual data points and variability.

Chart titles include sample size information: aggregated plots show the number of journals and total articles analysed (e.g. _"Cardiology (n = 8 journals, 12,450 articles)"_), and per-journal plots show the total article count (e.g. _"Circulation (n = 1,847 articles)"_). Tooltips additionally show the number of articles analysed in each individual year.

### Policy Year Markers

- On **per-journal plots**, a solid vertical line marks the year that journal adopted an editorial recommendation on figure types.
- On **aggregated plots**, the policy marker is replaced by a set of semi-transparent vertical bands — one per year in which at least one journal in the field adopted a policy. Each band's **width is proportional to the percentage of journals** that adopted that year, so thicker bands indicate years of widespread adoption and thin bands mark isolated early or late movers.

---

## Research Fields Covered

- Cardiac & Cardiovascular Systems
- Clinical Neurology
- Endocrinology & Metabolism
- Genetics & Heredity
- Immunology
- Neurosciences
- Oncology
- Orthopedics
- Pharmacology & Pharmacy
- Physiology
- Rheumatology
- Urology & Nephrology

---

## How to Navigate

The dashboard opens on the **About** tab, which shows the Global Aggregated Trend chart and a description of the dashboard. Use the sidebar to switch between views:

- **GLOBAL** — global aggregated trend across all journals, plus individual aggregated trend charts for each Research Field side by side.
- **Per-field tabs** — one tab per Research Field, each showing an aggregated trend for that field followed by individual per-journal charts.

Within any per-field view you can:

- Adjust the **column slider** to display more or fewer journal charts side by side.
- Use the **journal search bar** to filter visible cards or jump directly to a specific journal.

### Showing and Hiding Lines

Lines can be toggled in three ways:

1. **Click a legend entry** inside any chart to hide or show that line within that chart only. Double-clicking isolates a single line; double-clicking again restores all lines.
2. **Use the global toggle bar** (the row of buttons at the top of the page) to show or hide a line type — _% Bars_, _% Informative Graphs_, or _Policy adoption_ — **across every chart on the page simultaneously**.
3. The policy adoption layer on aggregated plots shares the same toggle as the policy line on per-journal plots, so a single button click controls both.

Legend boxes are semi-transparent so that data points beneath them remain visible.

---

## Generating the Dashboard

The dashboard is produced as a single self-contained HTML file by running the Python script:

```bash
python generate_index_html_search_bar_about_cols1_v16.py
```

### Requirements

- Python 3.10+
- `pandas`, `plotly`, `numpy`

### Input Data

The script expects the following CSV files in the configured `local_folder`:

| File                                     | Description                                              |
| ---------------------------------------- | -------------------------------------------------------- |
| `journals_selected_<timestamp>.csv`      | Selected journals and metadata                           |
| `field_selected_<timestamp>.csv`         | Field list                                               |
| `simulated_journal_data_<timestamp>.csv` | Per-journal yearly results, including `EligibleArticles` |
| `policy_df_<timestamp>.csv`              | Policy year assignments per journal                      |

The results file must include an `EligibleArticles` column with the number of articles analysed per journal per year. Update the `latest_timestamp`, `latest_timestamp_results`, and `local_folder` variables at the top of the script to point to your data.

### Output

The script writes a timestamped HTML file to the `new_figures_<date>_v16/` output directory:

```
new_figures_20260312_v16/index_20260312_121648.html
```

---

## Technical Notes

- All Plotly figures are embedded inline — no external figure files are needed.
- Plotly is loaded once from CDN (`plotly-2.35.2.min.js`) and shared across all charts.
- The Global Aggregated Trend figure is rendered under two distinct div IDs (one for the About tab, one for the Global tab) from the same serialised JSON, so both tabs display it correctly.
- The global legend toggle uses `Plotly.restyle()` to update trace visibility across all plot elements on the page, matching traces by both `name` and `legendgroup`.
- The layout is fully responsive, with a CSS grid that the user can resize in-browser.
