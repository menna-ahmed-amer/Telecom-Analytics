# Technical Decisions, Trade-offs & Limitations

## Why Power Query for ETL instead of SQL or Python?

The original inputs are five local CSV extracts. The project goal was to build a refreshable **Power BI semantic model** and stakeholder-facing dashboard. Power Query provides explicit Applied Steps, field typing, joins, quality flags, and a direct refresh path into Power BI.

**What this gains:** a single integrated toolchain and transformations that are easy to inspect in Power BI Desktop.

**What this gives up:** SQL is often more scalable for relational warehouse queries and shared governance; Python is more flexible for complex scripting, testing, statistical feature engineering or predictive modeling. **Neither SQL nor Python was used for the core BI ETL**, so they are not claimed as implementation tools.

## Why dimensional modeling rather than a single flat table?

The five sources have different grains: subscriber, subscription, recharge, sale and complaint. A giant flattening join would replicate customer values across events and multiply rows where customers have multiple subscriptions, purchases or complaints.

**Decision:** use three dimensions (`DimSubscriber`, `DimSubscription`, `DimDate`) and three fact tables (`FactRecharges`, `FactShopSales`, `FactComplaints`).

**Trade-off:** relationship-aware DAX measures and filter behavior need more care than a single flat table. The `DimSubscriber → DimSubscription → FactRecharges` chain is snowflake-like, not a pure star schema.

## Why one-way filtering?

One-to-many relationships with single-direction propagation reduce ambiguous paths and surprising cross-filter behavior. Slicers on fields in a fact table do not necessarily filter unrelated snapshot KPIs. Report interactions should follow the semantic model.

## Why keep unmatched IDs?

Dropping orphan records would undercount transaction facts; joining them to invented customers would misstate subscriber counts. Preserve them, track them, and explicitly restrict known-customer measures to the subscriber dimension when appropriate.

| Child source | Unmatched parent IDs (rows) |
|---|---:|
| Subscriptions → Subscribers | 382 (0.33% of subscription rows) |
| Recharges → Subscriptions | 3 (0.003% of recharge rows) |
| Shop sales → Subscribers | 403 (0.35% of sales rows) |
| Complaints → Subscribers | 338 (0.32% of complaint rows) |

## Why retain missing ratings?

Missing satisfaction is **not** neutral satisfaction. Using only rated complaints for the average and low-score % avoids artificial scores, but induces potential non-response bias. The separate Rating Coverage KPI (83.47%) makes that limitation visible.

## Why keep different reporting windows?

Recharge transactions span Jan 2025–Dec 2026, whereas sales and complaints span Jan–Jun 2026. An executive summary can show each area's entire available history, but cross-area time comparisons should be made on a clearly labeled overlapping period.

The supplied recharge file includes dates later than the README preparation date; it should be read as case-study source coverage, not a real-time or audited time series.

## Why no actual customer churn prediction?

`DimSubscription[status]` is a *snapshot*, not dated status transitions. Without historical churn labels or cancellation timestamps, neither a historical churn-rate KPI nor a valid churn prediction target is supported.

## Business assumptions

- Shop source field `total_sales_amount` is treated as a per-unit amount. The calculated `sales_revenue = unit_price × quantity` is a modeled sales value, **not** profit.
- Negative recharge entries may represent adjustments; their business meaning is unknown.
- A reported `resolution_days` value does not prove a status of "resolved" without a completion-status field.
- Geographic correction by region is an assumption; retain quality flags where mappings are uncertain.
- Current subscription status and age-group snapshot cannot be used to recreate historical state accurately.

## Documentation integrity

The repository distinguishes **observed patterns** from **proven effects**. The final `.pbix` is included in `Dashboard/` and raw CSVs are included in `Data/Raw/`; local Power BI refresh, measure behavior under filters, and source permission for any redistribution require their own checks. The original dataset publisher and license are not documented here.
