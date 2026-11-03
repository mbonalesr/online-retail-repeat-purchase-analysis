
# Business Rules — Version 0.1

## 1. Valid Purchase Line

We excluded lines from invoices whose numbers start with C (indicating a cancellation). We also excluded lines with zero or negative quantities or unit prices.

Finally, we excluded the following stock codes: `D`, `POST`, `C2`, `DOT`, and `M`, which were outside the scope of valid product sales for this analysis.

## 2. Valid Invoice

An invoice is considered valid if it has at least one valid purchase line, even if other lines in the transaction do not meet the criteria.

## 3. Observation Window Eligibility

A customer was considered eligible for the 60-day analysis if at least 60 days had passed between their first valid invoice and the end of the dataset.

The same eligibility logic was applied to the 30-day and 90-day observation windows.

## 4. Second Valid Invoice Classification

A customer was classified as `Repeat_60D` = `Yes` if two conditions were met. First, they had to be eligible for the 60-day analysis. Second, they had to have a second valid invoice within 60 days of their first valid invoice.

If they were eligible but did not have a second valid invoice within that period, they were classified as `No`.

If they had less than 60 days of observation, they were classified as `Not Eligible`.

The same logic was applied to the 30-day and 90-day windows.

## 5. KPI Calculation

The second valid invoice rate was calculated by dividing the number of eligible customers who had a second valid invoice within the observation window by the total number of eligible customers.

| Observation Window | Eligible Customers | Customers with Second Valid Invoice | Rate |
|---|---:|---:|---:|
| 30 days | 4,067 | 891 | 21.91% |
| 60 days | 3,729 | 1,390 | 37.28% |
| 90 days | 3,369 | 1,593 | 47.28% |

Each observation window has its own eligible customer population. Therefore, the rates should not be interpreted as results from exactly the same group of customers.

## 6. Methodological Limitations

### 6.1. Short Intervals Between Valid Invoices

We found 226 customers with less than 24 hours between their first and second valid invoices.

We kept them in our analysis because we did not have enough evidence to determine whether those invoices represented separate purchasing occasions or the same purchasing occasion.

This uncertainty could affect the interpretation of our results because some second valid invoices might not represent a separate purchase.

### 6.2. Customers Without Valid Invoices

We excluded 37 customers because they did not have any valid purchase lines. Their records included cancellations, quantities or unit prices that did not meet our criteria, and in some cases, the stock code M (Manual), whose exact meaning we could not confirm.

Excluding these customers reduced our analysis population from 4,372 identified customers to 4,335 customers with at least one valid invoice.

### 6.3. Observation Window Limitations

Some customers did not have enough observation time because their first valid invoice occurred close to the end of the dataset.

Therefore, these customers were not eligible for the 60-day analysis.

Including them as customers who did not repeat could underestimate our rate because we could not observe their full 60-day window.

End Date of the dataset: `2011-12-09 12:50:00`

Under this approach, customers without a complete observation window were excluded from that window's rate, even if their second valid invoice had already been observed.

### 6.4. Excluded Stock Codes

We decided to exclude stock codes associated with discounts, shipping, and manual transactions because we were not sure whether they represented valid product sales.

These stock codes were excluded at the transaction-line level, even when the invoice contained other valid lines.

This decision could exclude some legitimate sales because we could not confirm the exact meaning of every stock code.
