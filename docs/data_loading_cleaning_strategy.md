# Data Loading and Cleaning Strategy

## Overview

This document outlines the data loading, cleaning, and quality monitoring approach for the Sheffield United FC ticket sales analysis project.

---

## 1. Data Sources

| Dataset | File | Records | Description |
|---------|------|---------|-------------|
| **Ticket Sales** | `ticket_sales.csv` | 43,289 | Transaction-level ticket data with purchaser, owner, event, and seat information |
| **Customers** | `customers.csv` | 277 | Customer demographics including location, age, gender, and marketing preferences |

---

## 2. Data Loading Process

### Steps
1. Load CSV files using pandas
2. Convert date columns (`transaction_date`, `event_date`) to datetime format
3. Validate data types for each column
4. Perform initial missing value assessment

### Key Relationships
- `ticket_sales.purchaser_id` → `customers.customer_id`
- `ticket_sales.owner_id` → `customers.customer_id`

---

## 3. Data Quality Issues Identified

### 3.1 Missing Values

| Column | Missing Count | % Missing | Root Cause |
|--------|---------------|-----------|------------|
| `country` | 14 | 5.1% | Incomplete CRM data entry |
| `district` | 28 | 10.1% | Non-England customers (UK nations + international) |
| `ward` | 28 | 10.1% | Non-England customers |
| `distance_to_bramall_lane_km` | 28 | 10.1% | Derived from district/ward |
| `age` | 109 | 39.4% | Accounts (71) + Customers with missing data (21) |

### 3.2 Key Findings

- **Age is not applicable for Accounts**: 71 of 92 missing age values are business accounts, not individuals
- **Geographical data is England-specific**: District and ward are English administrative divisions, unavailable for Scotland, Wales, NI, and international customers
- **No duplicate IDs**: Both datasets have unique primary keys
- **All customers retained**: Non-England customers flagged rather than excluded to avoid bias

---

## 4. Cleaning Strategy

### 4.1 Cleaning Steps Applied

| Step | Action | Result |
|------|--------|--------|
| 1 | Convert date columns to datetime format | Enabled date validation |
| 2 | Verify no duplicate customer IDs | 0 duplicates found ✓ |
| 3 | Verify no duplicate transaction IDs | 0 duplicates found ✓ |
| 4 | Retain non-England customers with flag | Created `is_england_customer` flag for 28 customers |
| 5 | Retain all tickets | No customers excluded, no orphan tickets |
| 6 | Validate date logic (transaction_date ≤ event_date) | Invalid records flagged |
| 7 | Validate age range (0-120) | All ages valid ✓ |
| 8 | Validate distance is positive | All distances valid ✓ |

### 4.2 Non-England Customer Handling

**Decision: Retain with Flag (NOT Exclude)**

| Category | Count | Flag Value |
|----------|-------|------------|
| England customers | 249 | `is_england_customer = True` |
| Non-England customers | 28 | `is_england_customer = False` |

**Why retention is better than exclusion:**
- **International hospitality customers** are potentially high-value (corporate buyers, overseas fans)
- **Corporate buyers** may not have ward/district data but represent significant revenue
- **Excluding non-England customers biases the dataset** toward local fans
- **Scottish, Welsh, NI customers** are still UK-based and relevant for analysis

**Usage guidance:**
> Geographical variables (`district`, `ward`, `distance_to_bramall_lane_km`) are England-specific. Non-England customers are retained and analysed separately where geographical features are required. Filter on `is_england_customer == True` for distance-based analysis.

### 4.3 Handling Missing Ages

| Customer Type | Missing Age | Decision |
|---------------|-------------|----------|
| **Account** (business entity) | 71 | Retain — age is not applicable for organisations |
| **Customer** (individual) | 21 | Retain — affects only 1.86% of tickets; exclude only for age-specific analysis |

### 4.4 Cleaning Results

| Dataset | Original | Cleaned | Retained |
|---------|----------|---------|----------|
| Customers | 277 | 277 | 100% |
| Tickets | 43,289 | 43,289 | 100% |

---

## 5. Data Quality Checks

### 5.1 Checks Performed

| Category | Check | Result |
|----------|-------|--------|
| **Completeness** | No missing values in tickets | ✓ PASS |
| **Uniqueness** | No duplicate transaction IDs | ✓ PASS |
| **Uniqueness** | No duplicate customer IDs | ✓ PASS |
| **Validity** | transaction_date ≤ event_date | ✓ PASS |
| **Validity** | Age in range 0-120 | ✓ PASS |
| **Validity** | Distance is positive | ✓ PASS |
| **Consistency** | All purchaser_id in customers | ✓ PASS |
| **Consistency** | All owner_id in customers | ✓ PASS |
| **Accuracy** | No future transaction dates | ✓ PASS |

---

## 6. Continuous Data Quality Monitoring

### 6.1 Recommended Framework


```python
# Example validation rules
expect_column_values_to_be_unique("customer_id")
expect_column_values_to_not_be_null("transaction_id")
expect_column_values_to_be_between("age", 0, 120)
expect_column_pair_values_A_to_be_less_than_B("transaction_date", "event_date")
```

### 6.2 Monitoring Components

| Component | Description | Frequency |
|-----------|-------------|-----------|
| **Automated DQ Reports** | Generate quality reports after each data load | Daily/Weekly |
| **Threshold Alerts** | Notify when missing values exceed limits | Real-time |
| **Referential Integrity Checks** | Validate ticket-customer links | On data load |
| **Mandatory CRM Fields** | Enforce required fields at entry | Always |

### 6.3 Dashboard Metrics

| Metric | Target | Purpose |
|--------|--------|---------|
| % Missing Age (Customers) | <5% | Track data completeness |
| % Marketing Opt-outs | Monitor | Understand reach limitations |
| % Unmatched Transaction IDs | 0% | Ensure referential integrity |
| % Non-England Customers | Monitor | Track data coverage |
| % Duplicate Records | 0% | Data quality baseline |

### 6.4 Benefits

1. **Early Detection** — Identify issues before they impact analysis
2. **Accountability** — Clear ownership of data quality
3. **Trend Tracking** — Monitor improvement over time
4. **Compliance** — Meet regulatory and business requirements

---

## 7. Output Datasets

After cleaning, the following datasets are available for analysis:

| Dataset | Variable Name | Records | Use Case |
|---------|---------------|---------|----------|
| Cleaned Tickets | `ticket_sales_df_clean` | 43,289 | All ticket analysis |
| Cleaned Customers | `customers_df_clean` | 277 | Customer segmentation, demographics |

### New Column Added

| Column | Type | Description |
|--------|------|-------------|
| `is_england_customer` | Boolean | `True` for England customers with complete geographical data; `False` for non-England customers |

### Usage Notes

- **All analysis**: Use full datasets — no records excluded
- **Geographical analysis**: Filter on `customers_df_clean[customers_df_clean['is_england_customer'] == True]`
- **Distance-based segmentation**: Only available for England customers

---

## 8. Recommendations for CRM Improvement

1. **Make postcode mandatory** — Enables accurate distance calculation
2. **Validate age at entry** — Enforce range 0-120
3. **Separate Account vs Customer profiles** — Different data requirements
4. **Add data source tracking** — Know where each record originated
5. **Implement regular data audits** — Monthly review of data quality metrics

---

