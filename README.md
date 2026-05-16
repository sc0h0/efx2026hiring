# Candidate Brief: Collections Recovery Curve SQL Exercise

## Context

You are working as a junior analyst in a collections analytics team.

When an account becomes overdue, it may be placed with a debt collection agency. The business wants to understand how quickly money is recovered after placement and whether recovery differs across customer or account segments.

The main goal of this exercise is to show how you reason from raw tables to a reliable business metric using SQL.

## Data provided

You receive two core CSV files:

- `accounts.csv`: one row per account placed with a collection agency
- `payments.csv`: one row per payment transaction, refund, or reversal

You also receive one additional event file:

- `contacts.csv`: one row per collection contact attempt

The core recovery and activation tasks can be completed using `accounts.csv` and `payments.csv`.

The `contacts.csv` file is included because real collections analysis often requires linking operational activity to customer outcomes. You do not need to use it to complete the core task. However, if you have time, it can be used to demonstrate stronger SQL and analytical thinking by exploring whether contact timing, contact channel, or promise-to-pay events are associated with payment activation.

You also receive two tiny hand-check files:

- `hand_check_accounts.csv`
- `hand_check_payments.csv`

Use the hand-check files to prove you understand the calculation manually.

## Core task

Use SQL to build a recovery curve.

A recovery curve shows cumulative payments received over time since the account was placed into collections.

A simple recovery rate is:

`cumulative valid payments / placed balance`

The important part is not producing a polished chart. The important part is showing that you understand the grain of the data, the denominator, the date logic, and the cumulative calculation.

## Required outputs

### 1. Account summary

Produce a summary with:

- account count
- total placed balance
- average placed balance
- placement date range
- accounts with at least one payment
- total payments received

### 2. Recovery curve

Produce a weekly recovery curve with columns like:

- `placement_month`
- `week_since_placement`
- `accounts_placed`
- `placed_balance`
- `payments_in_period`
- `cumulative_payments`
- `cumulative_recovery_rate`

### 3. Segmented recovery curve

Build at least two segmented recovery curves.

Useful segment fields include:

- `risk_band`
- `product_type`
- `customer_age` or `age_band`
- `income_band`
- `employment_status`
- `state`
- `region`
- `customer_segment`
- `digital_consent`
- `hardship_flag`
- `dispute_flag`
- `assigned_agency`
- `treatment_stream`
- a balance band you create from `placed_balance`

When comparing segments, be ready to explain whether the difference appears meaningful and what other mix effects or data-quality issues might affect the conclusion.

### 4. Activation curve

Build an activation curve.

Activation means the account has made its first valid positive payment.

Suggested columns:

- `segment_name`
- `segment_value`
- `week_since_placement`
- `accounts_placed`
- `accounts_with_first_payment_by_week`
- `cumulative_activation_rate`

### 5. Data-quality checks

Before calculating the final metric, inspect the data and note any quality issues or assumptions.

Be ready to show checks for things like:

- missing placement dates
- missing or invalid placed balances
- payment rows with unknown account IDs
- payments before placement
- duplicate payment rows
- negative or zero payment amounts
- inconsistent segment values
- very recent cohorts with incomplete observation time

## Optional extension: contact-event analysis

If you finish the core task and want to demonstrate additional analytical depth, use `contacts.csv` to explore one operational question.

Examples:

- Do accounts with at least one contact before first payment activate faster?
- Are accounts with a `promise_to_pay` contact more likely to pay within 14 days?
- Which contact channel is associated with the highest first-payment rate?
- How many contacts happen before first payment by risk band?
- Are higher-risk or hardship accounts contacted more frequently?
- Are contact outcomes associated with recovery, or could the relationship be affected by selection bias?

This is optional. If you use the contacts table, be careful with causality. Contacted accounts may differ from non-contacted accounts. A higher payment rate after contact does not automatically prove that the contact caused the payment.

## Interview walkthrough

Prepare for a 50-minute walkthrough.

You should be ready to:

- connect your laptop to the meeting-room screen
- open your SQL environment
- show sample rows
- explain table grain
- explain your joins
- explain your denominator
- show intermediate CTEs or temp tables
- show the final recovery curve
- show at least one segmented view
- explain assumptions and limitations
- complete one small live modification

You do not need to present every line of SQL in detail. Focus on the decisions that affect correctness:

- which records are in the denominator
- which payment records are included or excluded
- how you calculate elapsed time since placement
- how you avoid double-counting balances
- how you calculate cumulative values
- how you validate the result

## Tools

Use any SQL engine you are comfortable with:

- SQLiteOnline
- local SQLite
- DuckDB
- PostgreSQL
- MySQL
- SQL Server
- BigQuery
- Snowflake

You may use CTEs, temporary tables, permanent staging tables, or step-by-step queries. The goal is clear, testable SQL, not one clever query.

## AI use

You may use online resources or AI tools during preparation, but you must understand every line of SQL you present.

In the interview, you may be asked to modify, debug, or manually validate your query. You should be able to explain your assumptions, joins, denominators, date logic, and cumulative calculations without relying on generated commentary.
