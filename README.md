# From First to Second Purchase

**Status:** Work in progress  
**First checkpoint:** October 23, 2026 (about 15 hours of work)  
**Tools used so far:** Excel and Power Query

## Project Overview

This project analyzes transaction data from an online retail store in the United Kingdom, recorded between late 2010 and late 2011.

The goal is to identify what proportion of identified customers made a second purchase during this period, and how much time passed between their first and second recorded purchases.

I also want to explore which purchasing patterns or characteristics these returning customers have in common, using the information available in the dataset.

**Data source:** [UCI Machine Learning Repository — Online Retail](https://archive.ics.uci.edu/dataset/352/online+retail). Prices are recorded in GBP.

## What I've Done So Far

### Data Preparation

I downloaded the original Online Retail dataset, which contained **541,909 transaction lines**. In Power Query, I created a query to keep only records with an identified `CustomerID`, since I need to track each customer's invoices over time. After filtering, **406,829 lines** remained.

### Exploratory Data Analysis

I used indexed queries and a left join in Power Query to link consecutive invoices for each customer. Then, I calculated the time difference between invoice dates and grouped the intervals into time ranges, from zero minutes to 90 days or more.

I created a pivot table to explore their distribution. Among the **17,818 invoice intervals**, I found that the **7–30-day range** had the highest individual frequency. However, when I combined the intervals shorter than seven days, they accounted for more observations.

This analysis is **preliminary** because the time ranges have different widths and the data still includes cancellations and other documents that may not represent paid purchases. These are intervals between **consecutive invoices**, not yet between a customer's first and second valid purchases.

### Valid Purchase Classification

I created another query to investigate which conditions a transaction line should meet to count as evidence of a paid product purchase.

I established some conditions: the `InvoiceNo` must not start with `C`, `Quantity` and `UnitPrice` must be greater than zero, and `StockCode` must not equal special codes such as `D`, `POST`, `C2`, `DOT` or `M`.

I created a column called `Valid_Line` to classify these records as `Yes` or `No`. After grouping and counting the results, I obtained **396,352 lines classified as `Yes`** and **10,477 as `No`**. I also checked examples involving cancellations, zero-priced products, and an invoice containing both paid products and a shipping charge.

The next step is to classify valid invoices, since a single invoice can contain both `Yes` and `No` lines. Under our current definition, a valid purchase invoice must belong to an identified customer and contain **at least one** line classified as `Yes`.

The detailed, provisional criteria and case checks are in [Business Rules v0.1](docs/business_rules_v0.1.md).

## Limitations and Next Steps

- **Not finished yet:** `Valid_Invoice` has been defined but has not been implemented or validated across all invoices.
- I still need to review other special `StockCode` values and decide how to handle returns and invoices recorded only minutes apart.
- After identifying valid invoices, I will rebuild each customer's purchase sequence and calculate first-to-second purchase metrics, accounting for the limited observation period.
- The final dashboard should help answer customer-retention questions, not just display charts.

This repository documents an ongoing analysis. **The line counts and preliminary invoice intervals should not be interpreted as a customer retention rate.**

## Reproducing the Analysis

The original data is available from UCI at the link above. The Power Query workbook will be added separately; when it is available, its local source-file path may need to be updated before refreshing the queries. The raw dataset is not included here.
