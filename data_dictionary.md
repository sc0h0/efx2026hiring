# Data Dictionary

## Overview

The dataset is designed to resemble a simple debt collections / receivables analytics environment.

The core analysis can be completed with:

- `accounts.csv`
- `payments.csv`

The `contacts.csv` file is included as an additional event table for candidates who want to demonstrate stronger SQL and analytical reasoning. It is useful for optional analysis of collections activity, contact timing, promise-to-pay behaviour, and operational segmentation.

## accounts.csv

One row per account placed with a collection agency.

This is the main denominator table for recovery and activation calculations.

| Column | Meaning |
|---|---|
| `account_id` | Unique account ID. |
| `customer_id` | Customer ID. Customers may have multiple accounts. |
| `placement_date` | Date the account was placed with collections. |
| `placed_balance` | Balance placed for collection. This is the recommended denominator for recovery rate, subject to data-quality checks. |
| `original_balance` | Original account balance before placement. |
| `product_type` | Product category, such as credit card, personal loan, BNPL, utility arrears, or telco arrears. |
| `risk_band` | Risk band from A to E. A is lower risk, E is higher risk. |
| `customer_age` | Customer age. May require validation. |
| `age_band` | Banded age group. |
| `income_band` | Customer income band. |
| `employment_status` | Employment category. |
| `state` | Customer state. May contain inconsistent values and may need standardisation before segmentation. |
| `region` | Metro, regional, remote, or unknown. |
| `customer_segment` | Behavioural segment assigned before or at placement. |
| `digital_consent` | 1 if digital contact is permitted, otherwise 0. |
| `hardship_flag` | 1 if a hardship marker exists. |
| `dispute_flag` | 1 if a dispute marker exists. |
| `prior_successful_payments_12m` | Count of successful payments in the 12 months before placement. |
| `credit_score_band` | Banded credit score. |
| `tenure_months` | Customer tenure in months. |
| `assigned_agency` | Collection agency or internal team assigned to the account. |
| `treatment_stream` | Treatment strategy assigned at placement. |

### Notes for analysis

Use `accounts.csv` to define:

- the placement cohort
- the fixed recovery denominator
- customer and account segments
- the population of accounts placed
- accounts with no payment activity

A common mistake is to join `accounts` to `payments` and then sum `placed_balance` at the payment-row grain. That can duplicate the denominator for accounts with multiple payments.

## payments.csv

One row per payment, refund, or reversal transaction.

This is the main transaction table for recovery and activation calculations.

| Column | Meaning |
|---|---|
| `payment_id` | Payment transaction ID. |
| `account_id` | Account ID. Should usually match an account in `accounts.csv`, but this should be checked. |
| `payment_date` | Date of payment transaction. |
| `payment_amount` | Amount paid. Negative rows may represent refunds or reversals. |
| `payment_channel` | Payment channel, such as card, bank transfer, BPAY, direct debit, or cash. |
| `payment_type` | Transaction type, such as payment, refund, or reversal. |
| `source_system` | Source system that supplied the transaction. |
| `reversal_of_payment_id` | Original payment ID if the row is a reversal and this is known. |

### Notes for analysis

Use `payments.csv` to calculate:

- payments received after placement
- first payment date
- payment activation
- period payment amount
- cumulative payment amount
- gross versus net recovery, if you choose to compare them

Be ready to explain how you treat:

- payments before placement
- duplicate payment rows
- zero-value payments
- negative payments
- payments linked to unknown accounts

A reasonable approach is to build a cleaned payment staging table before calculating the final curve.

## contacts.csv

One row per collection contact attempt.

This table is not required for the core recovery curve. It is included to allow optional analysis of operational collections activity.

| Column | Meaning |
|---|---|
| `contact_id` | Contact event ID. |
| `account_id` | Account ID. Should usually match an account in `accounts.csv`, but this should be checked. |
| `contact_date` | Date of contact attempt. |
| `contact_channel` | Contact channel, such as SMS, email, phone, letter, or field visit. |
| `contact_result` | Result of the contact attempt, such as delivered, no answer, right-party contact, promise to pay, wrong number, hardship discussed, or dispute raised. |
| `agent_id` | Agent ID. |
| `promised_payment_date` | Promise-to-pay date if relevant. |

### Notes for optional analysis

Use `contacts.csv` if you want to explore questions such as:

- contact volume before first payment
- payment activation after a promise-to-pay event
- recovery by first contact channel
- right-party contact rate by risk band
- contact intensity by customer segment
- whether contacted accounts have different recovery outcomes

Be careful when interpreting contact effects. Contacted and non-contacted accounts may not be comparable. For example, harder accounts may receive more contacts, and accounts showing early signs of payment may be contacted differently. This table is useful for exploratory analysis, but it does not by itself prove that contact caused payment.

## hand_check_accounts.csv

A tiny three-account dataset for manual validation.

Use this to show that you understand the denominator, payment timing, and cumulative recovery calculation.

## hand_check_payments.csv

Payment rows for the hand-check accounts.

A good walkthrough should be able to calculate the result manually before scaling the logic to the full dataset.