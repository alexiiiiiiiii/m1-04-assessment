# Assessment | Python Foundations Data Analysis

## Overview

This mini-project assesses your ability to use core Python and NumPy fundamentals to analyze a small dataset from start to finish. You will generate a reproducible airline pricing dataset from explicit formulas, validate and clean it, perform vectorized computations, and deliver a concise summary report.

## Learning Goals

By the end of the mini-project, you should be able to:

- Build reproducible synthetic data using explicit generation rules
- Validate and clean records with core Python data structures
- Use NumPy arrays for descriptive and grouped computations
- Validate outputs with consistency checks
- Communicate findings in a concise final report

## Setup

Work in a notebook named `m1-04-assessment.ipynb`.

## Tasks

### Task 1: Generate the raw dataset using fixed rules

Use the rules below exactly. Do not change formulas or thresholds.

1. Create `seed_value` from your birth date in format `DDMM` (example: 7 April -> `704`).
2. Set `n = 320`.
3. Create RNG: `rng = np.random.default_rng(seed_value)`.

Generate a list of dictionaries named `tickets` with exactly `n` records. For index `i` from 1 to `n`:

- `ticket_id`: `"T{seed_value}-{i:04d}"`
- `route`: choose from `["NYC-LAX", "LHR-JFK", "SFO-SEA", "DXB-SIN", "MAD-ROM"]` using `(i + seed_value) % 5`
- `day`: choose from `["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]` using `(i + seed_value) % 7`
- `days_to_departure`: `1 + ((i * 3 + seed_value) % 60)`
- `class`: choose from `["economy", "premium", "business"]` using `(i * 2 + seed_value) % 3`
- `price_usd`:
  - `base = 120 + (days_to_departure * -1.5)`
  - `route_adj = [140, 220, 60, 180, 80]` based on route index
  - `class_adj = [0, 80, 220]` based on class index
  - `noise = rng.normal(0, 25)`
  - `price_usd = round(base + route_adj + class_adj + noise, 2)`

Inject deterministic data issues:

- If `i % 28 == 0`, set `price_usd = ""`
- If `i % 45 == 0`, multiply `price_usd` by `-1`
- If `i % 37 == 0`, set `class` to uppercase

After generation, print total record count and show first five records.

### Task 2: Validate and clean records with core Python

Identify invalid records (missing/non-numeric `price_usd`, or negative prices). Build `cleaned_tickets` with only valid records and normalized lowercase `class`.

After cleaning, confirm cleaned count and verify no invalid prices remain. Show two cleaned records.

### Task 3: Convert to NumPy for analysis

Create NumPy arrays for `prices` and `days`. Compute mean and standard deviation of prices. Compute total revenue per day and ticket counts per day using vectorized operations (no loops). Validate daily totals sum to overall total revenue.

### Task 4: Identify high-price tickets

Define high-price tickets as above the 90th percentile of `prices`. Compute threshold and count. Verify all selected prices are `>=` threshold.

### Task 5: Produce a final report

Create a report dictionary with keys:

- `total_tickets`
- `cleaned_tickets`
- `mean_price`
- `std_price`
- `daily_totals`
- `high_price_count`

Print a readable report and include at least one explicit validation statement.

## Common Pitfalls

- Changing generation rules makes results inconsistent.
- Mixing string and numeric types in `price_usd` breaks arithmetic.
- Misaligned labels during aggregation causes wrong daily totals.

## Submission

### What to submit

- `m1-04-assessment.ipynb`

### Git workflow

1. Fork this repo to your GitHub account.
2. Clone your fork locally.
3. Complete the assessment notebook.
4. Commit and push your work:

```bash
git add .
git commit -m "Solve m1-04 assessment"
git push -u origin HEAD
```

5. Open a Pull Request from your fork to your own default branch.
6. Submit the **Pull Request URL** in the Student Portal field for this assessment.

## Evaluation Criteria

Your work is evaluated on correctness, clarity, and structure. The notebook must run top-to-bottom without errors and include all required tasks, checks, and outputs.
