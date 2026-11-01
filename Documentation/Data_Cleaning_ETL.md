# Data Cleaning & ETL — Decision Log

**Workflow:** CSV → Power Query → typed/standardized columns → validation flags → dimensional model → DAX measures.

The table below describes the business logic observable in the supplied source datasets and in the confirmed Power BI project. It is **not a verbatim exported Power Query M script**; the source `.pbix` was not available for exporting exact Applied Steps.

## 1. Subscribers

**Observed issues:** `governorate` contains inconsistent casing, bilingual labels and city names used as governorates; `region_and_zip` combines two attributes with `|`; birth dates may be invalid/future/placeholder; postal codes can be missing (or a placeholder such as `00000`).

**Transformation approach:**

- Trim/standardize location labels, split `region_and_zip` into `Region` and `Zip_Code`, and derive/standardize governorate where the region-to-governorate mapping is trustworthy.
- Create `Region_Zip_Quality` and `DOB_Quality` flags instead of silently repairing uncertain values.
- Flag birth-date problems (missing, placeholder `01-01-1900`, or outside the defined validity horizon). Use a consistent **2026-12-31** reference date for the age-group exercise; the age result is *reference-based*, not necessarily current as of publication.
- Derive `Age_Group` only when the age is interpretable; retain `Unknown` where needed. Keep customer segment and gender categorical.

**Reasoning / trade-off:** consistent categories improve group-by reliability, but correcting ambiguous geography by region can introduce inferred values. Document the assumption and allow unknowns rather than pretending every location is certain.

## 2. Subscriptions

**Observed issues:** `line_type` has variants such as `PREPAID`, `Prepaid `, `postpaid`, `Postpaid`; `phone_number` is an identifier and can be misread as scientific notation when treated numerically.

**Transformation approach:** standardize `Prepaid`, `Postpaid`, `Hybrid`; trim text; type `activation_date` as Date; keep phone numbers as text; preserve `Active`, `Suspended`, `Terminated` statuses as recorded.

**Reasoning / trade-off:** stable labels make status-by-type comparisons valid. Snapshot status does **not** reveal historical transitions or actual churn.

## 3. Recharges

**Observed issues:** amount strings may include `EGP`, `L.E` or spaces; some are negative; payment method can carry stray spaces.

**Transformation approach:** normalize currency-text formatting; convert amount to a signed numeric field; standardize payment method; parse `recharge_date`; distinguish `Gross Positive Recharge Value` from signed `Net Recharge Value`.

**Reasoning / trade-off:** negative values remain in the model so aggregate sums reconcile with the file, but the business reason (reversal/refund/adjustment) cannot be concluded from the sign alone.

**Reporting-date validation:** The source has dates from **2025-01-01 through 2026-12-28**. As of **2026-11-01**, **9,764 rows** have a recharge date later than the current reference date. Preserve those records in the source, but confirm the reporting cutoff before claiming historical realized transactions up to 2026-11-01.

## 4. Shop sales

**Observed issues:** `total_sales_amount` contains the text `FREE`, so it cannot be directly summed as a numeric field. A `quantity` field is present, and no explicit order table is supplied.

**Transformation approach:** derive `Sale_Type` = Free/Paid; create a numeric unit-price field (`FREE` → 0, numeric strings → number); calculate `sales_revenue = unit_price × quantity`; standardize branch and product text; type `transaction_date` as Date.

**Reasoning / trade-off:** this yields a consistent **modeled sales value** but it rests on the interpretation of the source amount as a per-unit price. Without invoices/discounts/order IDs, do not call it profit, cash revenue, or order-level AOV.

## 5. Complaints

**Observed issues:** `NETWORK` and `Network Issue` represent the same category; `billing` capitalization differs; satisfaction ratings are missing in **17,358** rows; resolution time has **8,442** actual nulls plus **2,033** text `blank` tokens.

**Transformation approach:** standardize complaint labels (`Network Issue`, `Billing`, `Internet Speed`); convert text `blank` resolution values to null and type duration numeric; keep missing satisfaction values null; type complaint date as Date.

**Reasoning / trade-off:** averages naturally ignore missing durations; low-rating rate must divide by the **rated** complaints, not all complaints. Response-rate KPI separately reveals missing feedback.

## 6. Data-quality checks and unmatched references

| Source fact/child | Lookup table | Unmatched rows |
|---|---|---:|
| Subscriptions | Subscribers | 382 |
| Recharges | Subscriptions | 3 |
| Shop sales | Subscribers | 403 |
| Complaints | Subscribers | 338 |

**Model choice:** keep all original fact records for comprehensive global totals. Customer or line segmentation can naturally exclude unmatched child records because they cannot be assigned to an existing parent. Use `INTERSECT` in customer participation measures when restricting to known subscribers.

## 7. Validation principles

- Check input row counts and key uniqueness before model loading.
- Validate numeric/date conversions and derived field types.
- Verify distinct-count measures against their intended grain.
- Compare numeric recharge sign logic and free-sale counts against source totals.
- Check visual interactions with slicers; model relationships, not interaction settings alone, determine whether a filter propagates.
- State each KPI's scope and calendar coverage in the relevant report/documentation.

## Reproduction boundary

Power Query transformations have been described here from project decisions and the original source structure. **The exact M queries, parameters, source paths, and refresh settings are contained in the owner's original `.pbix`, which must be added separately** if public reproducibility is desired.
