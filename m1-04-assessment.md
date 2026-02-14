<!-- # Day 5 Assessment: Python Foundations Data Analysis -->

## Overview

This mini-project assesses your ability to use core Python and NumPy fundamentals to analyze a small dataset from start to finish. You will generate a reproducible airline pricing dataset from explicit formulas, validate and clean it, perform vectorized computations, and deliver a concise summary report. The work reflects real workflows where raw inputs are imperfect and analysts must build reliable, readable code to transform data into insight.

## Learning Goals

By the end of the mini-project, you should be able to build a reproducible data generation process from explicit formulas, clean and validate inputs using core Python structures, and apply NumPy arrays to compute accurate summaries. You should also be able to explain your reasoning with clear checks and a final report.

## Setup and Context

You will work with synthetic data representing airline ticket prices for a week of departures. Each record includes `ticket_id`, `route`, `day`, `days_to_departure`, `class`, and `price_usd`. The dataset must be generated using the exact rules below. Use a notebook named `m1-04-assessment.ipynb`.

## Tasks

### Task 1: Generate the raw dataset using fixed rules

Use the rules below exactly. Do not change the formulas or thresholds.

1. Create a seed called `seed_value` from your birth date in format `DDMM`. Example: if your birthday is 7 April, `seed_value = 0704` as an integer `704`.
2. Set the number of records to `n = 320`.
3. Create a NumPy random generator with `rng = np.random.default_rng(seed_value)`.

Now generate a list of dictionaries called `tickets` with exactly `n` records, where each record is built as follows for index `i` from 1 to `n`:

- `ticket_id`: `"T{seed_value}-{i:04d}"`
- `route`: choose from `["NYC-LAX", "LHR-JFK", "SFO-SEA", "DXB-SIN", "MAD-ROM"]` using index `(i + seed_value) % 5`
- `day`: choose from `["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]` using index `(i + seed_value) % 7`
- `days_to_departure`: `1 + ((i * 3 + seed_value) % 60)` (integer 1–60)
- `class`: choose from `["economy", "premium", "business"]` using index `(i * 2 + seed_value) % 3`
- `price_usd`: compute:
  - `base = 120 + (days_to_departure * -1.5)`
  - `route_adj = [140, 220, 60, 180, 80]` based on the chosen route index
  - `class_adj = [0, 80, 220]` based on class index
  - `noise = rng.normal(0, 25)`
  - `price_usd = round(base + route_adj + class_adj + noise, 2)`

Inject data issues deterministically by modifying records as you generate them:

- If `i % 28 == 0`, set `price_usd` to an empty string `""`
- If `i % 45 == 0`, set `price_usd` to a negative number by multiplying it by `-1`
- If `i % 37 == 0`, set `class` to uppercase

After generation, print the total number of records and show the first five entries to confirm structure.

### Task 2: Validate and clean records with core Python

Write validation logic that identifies invalid records. Treat missing or non-numeric `price_usd` values and negative prices as invalid. Build a new list `cleaned_tickets` that contains only valid records and normalizes the `class` values to lowercase.

After cleaning, confirm the number of cleaned records and verify that no invalid prices remain. Show two cleaned records to demonstrate normalization.

### Task 3: Convert to NumPy for analysis

Create NumPy arrays for numeric analysis. Build an array `prices` from the cleaned ticket prices and an array `days` from the cleaned day labels. Use NumPy to compute the overall mean and standard deviation of `prices`. Compute total revenue per day and the number of tickets per day using vectorized operations, not loops. Validate that the sum of daily totals matches the overall total revenue from `prices`.

### Task 4: Identify high-price tickets

Define high-price tickets as those above the 90th percentile of `prices`. Use NumPy to compute the percentile threshold and select the corresponding tickets. Report the threshold and the count of high-price tickets, and verify that all selected prices are greater than or equal to the threshold.

### Task 5: Produce a final report

Create a report dictionary with keys `total_tickets`, `cleaned_tickets`, `mean_price`, `std_price`, `daily_totals`, and `high_price_count`. Convert the report into a readable string and print it in the notebook. Include at least one explicit validation statement, such as confirming that `cleaned_tickets` is less than or equal to `total_tickets`.

## Common Pitfalls and Debugging Notes

A common mistake is changing the generation rules, which makes results inconsistent. Follow the formulas exactly, especially the seed and the data issue injection rules. Another frequent issue is mixing string and numeric types in the `price_usd` field. If arithmetic fails, inspect the types and convert values explicitly before building arrays.

When using NumPy, double-check array shapes and ensure that your day labels align with the prices you aggregate. If daily totals seem incorrect, verify that your grouping logic matches the day values in `cleaned_tickets`.

## Submission Requirements

Submit the following:
- The notebook file `m1-04-assessment.ipynb`
- A Pull Request URL with your final assessment solution

## Evaluation Criteria

Your work will be evaluated on correctness, clarity, and structure. Correctness means your dataset follows the generation rules, your cleaning logic removes invalid records, and your NumPy computations match validation checks. Clarity means your code is readable and variable names are descriptive. Structure means the notebook runs cleanly from top to bottom and each task is completed in order with visible outputs. The overall expectation is a professional, reproducible analysis workflow using Week 1 skills.
