# KPI Dictionary — 24 Core Measures

This dictionary explains the **business definition, grain, and purpose** of each KPI. Measure names in the Power BI user interface may be shortened; confirm exact names and DAX in the final `.pbix`.

| # | Area | KPI | Definition | Business objective | Scope / caveat |
|---:|---|---|---|---|---|
| 1 | Customers | `Total Subscribers` | COUNTROWS(DimSubscriber) | Size of registered subscriber master | Subscriber snapshot |
| 2 | Customers | `Purchasing Customers` | Distinct known subscribers appearing in FactShopSales | Extent of shop engagement | Shop transactions: Jan–Jun 2026; known subscribers only |
| 3 | Customers | `Purchase Participation Rate %` | Purchasing Customers / Total Subscribers | Share of master subscribers who bought | Customer snapshot vs Jan–Jun 2026 events |
| 4 | Customers | `Customers with Complaints` | Distinct known subscribers appearing in FactComplaints | Reach of complaint activity | Complaint transactions: Jan–Jun 2026; known subscribers only |
| 5 | Customers | `Complaint Participation Rate %` | Customers with Complaints / Total Subscribers | Share of master subscribers who submitted complaints | Customer snapshot vs Jan–Jun 2026 events |
| 6 | Subscriptions | `Total Subscriptions` | COUNTROWS(DimSubscription) | Number of registered lines | Current status snapshot |
| 7 | Subscriptions | `Active Lines` | Total Subscriptions where status = Active | Number of active lines | Current snapshot |
| 8 | Subscriptions | `Active Line Rate %` | Active Lines / Total Subscriptions | Share of registered lines that are active | Current snapshot |
| 9 | Subscriptions | `Suspended Lines` | Total Subscriptions where status = Suspended | Potential reactivation segment | Current snapshot, not a churn measurement |
| 10 | Recharges | `Recharge Transactions` | COUNTROWS(FactRecharges) | Frequency of recorded recharge events | 2025–2026 |
| 11 | Recharges | `Net Recharge Value` | SUM(FactRecharges[amount]) | Signed recharge value | Includes negative entries |
| 12 | Recharges | `Gross Positive Recharge Value` | SUM(amount) where amount > 0 | Positive recharge value separate from adjustments | Positive entries only |
| 13 | Recharges | `Recharging Line IDs` | DISTINCTCOUNT(FactRecharges[subscription_id]) | Breadth of line IDs with recharge activity | Includes IDs not matched to DimSubscription |
| 14 | Recharges | `Average Positive Recharge Value` | AVERAGE(amount) where amount > 0 | Average positive transaction size | Positive entries only |
| 15 | Shop Sales | `Shop Sales Records` | COUNTROWS(FactShopSales) | Sales-record volume | Jan–Jun 2026; not orders |
| 16 | Shop Sales | `Shop Sales Value` | SUM(FactShopSales[sales_revenue]) | Modeled retail value | unit_price × quantity; not profit |
| 17 | Shop Sales | `Shop Units` | SUM(FactShopSales[quantity]) | Units recorded | Jan–Jun 2026 |
| 18 | Shop Sales | `Average Paid Sales Record Value` | AVERAGE(sales_revenue) where Sale_Type = Paid | Typical paid record value | Not order-level AOV |
| 19 | Shop Sales | `Free Sales Rate %` | Free sales records / Shop Sales Records | Promotional/free record share | Free distinguished from paid |
| 20 | Customer Experience | `Total Complaints` | COUNTROWS(FactComplaints) | Support demand | Jan–Jun 2026 |
| 21 | Customer Experience | `Average Resolution Days` | AVERAGE(FactComplaints[resolution_days]) | Time reported for complaint handling | Missing durations excluded; no completion status |
| 22 | Customer Experience | `Average Satisfaction Score` | AVERAGE(FactComplaints[satisfaction_score]) | Average recorded feedback | Rated complaints only; 1–5 |
| 23 | Customer Experience | `Low Satisfaction Rate %` | Rated complaints scored 1 or 2 / all rated complaints | Unfavorable feedback share | Missing ratings excluded |
| 24 | Customer Experience | `Satisfaction Response Rate %` | Complaints with rating / Total Complaints | Feedback completeness | Also displayed as Rating Coverage |

## Additional diagnostic measures

- **Negative Recharge Transactions** = count of recorded recharge amounts < 0; **17,043** rows.
- **Negative Transaction Rate %** = negative recharge rows / all recharge rows; **14.44%**.
- **Rating Coverage** = alias for Satisfaction Response Rate %; **83.47%**. Display as a **percentage**, not 0.83.
- **Suspended rate** can be derived from Suspended Lines / Total Subscriptions; this is not a historical churn rate.

## Key source-level figures (unfiltered)

| Metric | Value |
|---|---:|
| Subscribers | 114,618 |
| Subscriptions | 114,994 |
| Active Lines | 101,095 |
| Suspended Lines | 10,527 |
| Recharge Transactions | 118,025 |
| Net Recharge Value | 15,297,520 EGP |
| Gross Positive Recharge Value | 16,149,670 EGP |
| Shop Sales Records | 113,904 |
| Shop Units | 132,294 |
| Modeled Shop Sales Value | 2,000,609,900 EGP |
| Total Complaints | 105,000 |
| Average Resolution Days (known duration) | ~2.00 |
| Low Satisfaction Rate (rated only) | 39.75% |
| Satisfaction Response Rate | 83.47% |

## Filters, date scope, and interpretation

- Engagement KPIs compare a **subscriber snapshot denominator** with events from **Jan–Jun 2026**; interpret as within available records, not as a year-to-date active-customer retention rate.
- Date filters affect event facts. They do **not** turn snapshot status measures into status-as-of-date measures.
- Revenue/cash labels should not imply realized income when the dataset only supports signed transaction or derived sales values.
- `Customers with Complaints` and `Purchasing Customers` use known subscriber IDs to avoid counting orphan fact IDs as members of the subscriber master.
