# Data Loading and Cleaning Strategy

## Overview

Strategy doc for how we loaded, cleaned, and validated the two datasets for the SUFC hospitality analysis.

## Data sources

Two CSVs:
- `ticket_sales.csv`  - 43,289 transaction-level records (purchaser, owner, event, seat info)
- `customers.csv`  - 277 customer records with demographics, location, and marketing preferences

They link on `ticket_sales.purchaser_id` / `owner_id` → `customers.customer_id`.

## Loading process

Pretty standard: load with pandas, convert `transaction_date` and `event_date` to datetime, check types, and do an initial missing value pass.

## Data quality issues

### Missing values

The main gaps:
- `country`  - 14 missing (5.1%), just incomplete CRM entries
- `district`, `ward`, `distance_to_bramall_lane_km`  - 28 missing each (10.1%), all non-England customers where these fields don't apply
- `age`  - 109 missing (39.4%), but 71 of those are business accounts where age isn't meaningful anyway. Only 21 individual customers actually have missing age.

### Other findings

No duplicate IDs in either dataset. All primary keys are unique.

The geographical fields (district, ward) are England-specific administrative divisions, so they're naturally empty for Scotland/Wales/NI/international customers. This isn't dirty data  - it's a structural limitation.

## Cleaning strategy

### What we did

1. Converted date columns to datetime
2. Verified no duplicate customer or transaction IDs (both clean)
3. Created an `is_england_customer` flag for the 28 non-England customers rather than dropping them
4. Kept all 277 customers and all 43,289 tickets  - nothing excluded
5. Flagged records where transaction_date > event_date
6. Validated age range (0-120) and distance positivity  - all passed

### Non-England customers: retain, don't exclude

This was probably the most important decision. 28 customers don't have district/ward/distance data because they're outside England. The temptation is to just drop them, but that would:
- Lose potentially high-value international hospitality buyers
- Bias the analysis toward local fans
- Remove corporate accounts that might be based elsewhere in the UK

Instead we flag them with `is_england_customer = False` and filter only when we need distance-based features.

### Missing ages

| Customer type | Missing | What we did |
|---|---|---|
| Account (business) | 71 | Keep as-is  - age doesn't apply to organisations |
| Customer (individual) | 21 | Keep  - only 1.86% of tickets affected, exclude from age-specific analysis only |

### Cleaning results

100% retention on both datasets. Customers: 277 → 277. Tickets: 43,289 → 43,289.

## Data quality checks

All passed:

- No missing values in ticket data
- No duplicate transaction or customer IDs
- transaction_date ≤ event_date for all records
- All ages between 0-120
- All distances positive
- Every purchaser_id and owner_id maps to a valid customer
- No future transaction dates

## Monitoring recommendations

If this were a production pipeline, you'd want:

- Automated DQ reports after each data load (missing value counts, referential integrity)
- Threshold alerts when key metrics drift (e.g. missing age above 5%)
- Mandatory CRM fields enforced at entry  - especially postcode and customer type
- Regular audits, probably monthly

Example validation rules (Great Expectations style):

```python
expect_column_values_to_be_unique("customer_id")
expect_column_values_to_not_be_null("transaction_id")
expect_column_values_to_be_between("age", 0, 120)
expect_column_pair_values_A_to_be_less_than_B("transaction_date", "event_date")
```

## Output datasets

After cleaning:
- `ticket_sales_df_clean` (43,289 rows)  - used for all ticket analysis
- `customers_df_clean` (277 rows)  - used for segmentation and demographics

New column: `is_england_customer` (boolean). True for the 249 England customers with full geographical data, False for the 28 without.

For geographical analysis, filter to `is_england_customer == True`. For everything else, use the full dataset.

## CRM improvement suggestions

A few things that would make future analysis easier:

1. Make postcode mandatory at booking  - enables proper distance calculation
2. Validate age at entry (enforce 0-120 range)
3. Separate account vs individual profiles in the CRM  - they have fundamentally different data needs
4. Track data source per record so you know where quality issues originate
5. Run monthly data quality audits against the metrics above

