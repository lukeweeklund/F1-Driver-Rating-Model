# Formula 1 Driver Skill vs Car Performance

A portfolio analytics project that estimates Formula 1 driver performance while minimizing the influence of constructor strength.

Instead of relying on championship points or finishing positions alone, this project compares drivers directly against their teammates to isolate individual performance. Because teammates share the same car, team, race weekend, and circuit conditions, teammate comparisons provide a more defensible estimate of driver ability than raw race results.

---

## Project Goal

The central question:

> **How much of Formula 1 success comes from the driver versus the car?**

Traditional statistics such as wins, podiums, and championship points are heavily influenced by constructor performance. This project builds a teammate-normalized rating system designed to separate driver performance from car performance as much as possible.

---

## Methodology

The project follows a six-stage pipeline.

### Notebook 1 — Data Collection

Collected complete Formula 1 race and qualifying data from 2010–2025.

Output:

- Raw race results
- Qualifying results
- Race schedule
- Circuit lookup tables

---

### Notebook 2 — Driver-Race Dataset

Created one observation per driver per race.

Engineered identifiers including:

- race_id
- driver_race_id
- constructor_race_id
- teammate relationships
- finish gaps
- teammate outcome variables

---

### Notebook 3 — Constructor Features

Estimated prior constructor strength using only information available before each race.

Features include:

- rolling average finishes
- rolling qualifying pace
- rolling constructor points
- composite car performance score

Importantly, current-race information is never used when estimating car strength, preventing target leakage.

---

### Notebook 4 — Circuit Features

Added circuit metadata including:

- circuit type
- lap length
- altitude
- downforce requirements
- speed profile
- technical difficulty
- overtaking opportunities

---

### Notebook 5 — Teammate Rating Model

The final model estimates driver ability using fixed-effects regression.

Target variable:

- teammate performance score

This combines:

- teammate win/loss
- finish gap versus teammate

with weights of

- 35% teammate win
- 65% finish gap

while controlling for:

- season
- regulation era
- circuit characteristics
- circuit type
- season phase

---

### Notebook 6 — Storytelling

Creates visualizations and interprets the final ratings.

Outputs include:

- driver leaderboard
- rating distribution
- car contamination diagnostics
- reliability analysis
- robustness checks

---

# Dataset

Coverage:

- Seasons: **2010–2025**
- Races: **329**
- Drivers: **83**
- Constructors: **23**

Final teammate model:

- **6,908** teammate comparisons

---

# Results

The teammate-normalized model produces ratings that behave as intended.

Key validation metrics:

| Metric | Value |
|---------|-------|
| Rating vs teammate win rate | **0.925** |
| Rating vs average finish gap | **0.928** |
| Rating vs average car score | **0.087** |

The low relationship with average car score suggests the model is not simply reproducing constructor performance.

---

## Highest Rated Drivers

| Rank | Driver |
|------|---------|
| 1 | Max Verstappen |
| 2 | Fernando Alonso |
| 3 | Rubens Barrichello* |
| 4 | George Russell |
| 5 | Alexander Albon |
| 6 | Lando Norris |
| 7 | Charles Leclerc |
| 8 | Pascal Wehrlein* |
| 9 | Mick Schumacher* |
| 10 | Pierre Gasly |

\* Smaller sample sizes.

---

## Example Findings

Some results align closely with Formula 1 expectations:

- Max Verstappen produces the strongest teammate record in the dataset.
- Fernando Alonso performs consistently well across multiple teams and teammates.
- George Russell and Charles Leclerc rate highly despite spending portions of their careers in non-championship cars.

The model also produces surprising rankings that become understandable after inspecting teammate statistics.

For example:

- Oscar Piastri ranks lower because the available data shows him trailing Lando Norris in teammate wins, qualifying wins, finish gap, and points.
- Mark Webber ranks low largely because most of his sample comes against Sebastian Vettel during Red Bull's dominant era.
- Felipe Massa similarly spent much of the sample competing directly against Fernando Alonso.

---

# Visualizations

The project generates:

- Driver rating distribution
- Driver rating vs constructor strength
- Rating vs sample size
- Reliability tier summaries
- Sensitivity analyses

Example:

```
Driver Rating vs Average Car Score

Correlation = 0.087
```

This demonstrates that driver ratings remain largely independent of constructor strength.

---

# Limitations

This project intentionally focuses on teammate comparisons.

Current limitations include:

- Mechanical DNFs are not yet separated from driver-caused retirements.
- Drivers are only evaluated from 2010–2025.
- Small sample drivers have greater uncertainty.
- Circuit difficulty ratings include analyst-assigned values.

---

# Future Improvements

Potential next steps include:

- Bayesian hierarchical driver ratings
- Elo-based driver system
- Mechanical DNF detection
- Confidence intervals
- Interactive dashboard
- Season-by-season driver trajectories

---

# Repository Structure

```
F1 Project/

├── data/
│   ├── external/
│   ├── processed/
│   └── outputs/
│
├── notebooks/
│   ├── 01_download_raw_data.ipynb
│   ├── 02_build_driver_race_dataset.ipynb
│   ├── 03_build_constructor_features.ipynb
│   ├── 04_build_circuit_features.ipynb
│   ├── 05_teammate_driver_rating_model.ipynb
│   └── 06_driver_rating_storytelling.ipynb
│
├── src/
├── README.md
├── requirements.txt
└── .gitignore
```

---

# Technologies

- Python
- pandas
- NumPy
- statsmodels
- Matplotlib
- SciPy
- Jupyter Notebook

---

# Author

**Luke Weeklund**

Business Analytics & Information Systems  
University of Iowa

Interested in analytics, data visualization, predictive modeling, and sports analytics.
