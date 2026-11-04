# From First to Second Purchase

**Status:** Work in progress 

**First checkpoint:** October 23, 2026 (about 15 hours of work)

**Second checkpoint:** November 3, 2026 (approximately 30 hours of work)

**Tools used so far:** Excel, Power Query, and Pivot Tables

## Project Overview

This project analyzes transaction data from an online retail store in the United Kingdom, recorded between late 2010 and late 2011.

The goal is to identify how many customers had a second valid invoice and how much time passed between their first and second valid invoices.

**Data source:** [UCI Machine Learning Repository — Online Retail](https://archive.ics.uci.edu/dataset/352/online+retail). Prices are recorded in GBP.

## What I've Done So Far

### Data Preparation

I downloaded the original Online Retail dataset, which contained **541,909 transaction lines**. In Power Query, I created a query to keep only records with an identified `CustomerID`, since I need to track each customer's invoices over time. After filtering, **406,829 lines** remained.

### Exploratory Data Analysis

I initially explored time intervals between consecutive invoices using indexed queries, a left join in Power Query, and pivot tables.

This preliminary analysis helped me identify potential data-quality issues, including cancellations and invoices recorded within short time intervals.

These preliminary results were not used to calculate the final second valid invoice rates.

### Valid Purchase Classification

I created another query to investigate which conditions a transaction line should meet to count as evidence of a paid product purchase.

I established some conditions: the `InvoiceNo` must not start with `C`, `Quantity` and `UnitPrice` must be greater than zero, and `StockCode` must not equal special codes such as `D`, `POST`, `C2`, `DOT` or `M`.

I created a column called `Valid_Line` to classify these records as `Yes` or `No`. After grouping and counting the results, I obtained **396,352 lines classified as `Yes`** and **10,477 as `No`**. I also checked examples involving cancellations, zero-priced products, and an invoice containing both paid products and a shipping charge.

### Valid Invoices and Second Purchase Analysis

After applying my classification rules, I identified 18,413 valid invoices and 4,335 customers with at least one valid invoice.

Of these customers, 2,830 had an observed second valid invoice during the dataset period.

To calculate the second valid invoice rates, I used observation windows of 30, 60, and 90 days. Only customers with enough observation time were included in each window.

### Key Results

| Observation Window | Eligible Customers | Customers with Second Valid Invoice | Rate |
|---|---:|---:|---:|
| 30 days | 4,067 | 891 | 21.91% |
| 60 days | 3,729 | 1,390 | 37.28% |
| 90 days | 3,369 | 1,593 | 47.28% |

I selected the 60-day rate as the main KPI. The 30-day and 90-day rates provide additional context about how the results change across observation windows.
Each rate uses its own eligible customer population.

### Dashboard Progress

I created a first version of the dashboard in Excel to present the number of customers with a second valid invoice, the 60-day rate, and the distribution of time between the first and second valid invoices.

The dashboard is still being developed.

The detailed, provisional criteria and case checks are in [Business Rules v0.1](docs/business_rules_v0.1.md).

## Limitations and Next Steps

The analysis has some methodological limitations, including short intervals between valid invoices, customers without valid invoices, and differences in observation time.

These decisions and limitations are documented in [Business Rules v0.1](docs/business_rules_v0.1.md).

The project is still in progress. The remaining tasks include:

- Finalizing the Excel dashboard.
- Writing business conclusions and recommendations.
- Performing an additional Excel formula validation.
- Preparing the workbook and instructions for reproducibility.
- Completing the final project review.


## Reproducing the Analysis

The original data is available from UCI at the link above. The Power Query workbook will be added separately; when it is available, its local source-file path may need to be updated before refreshing the queries. The raw dataset is not included here.
