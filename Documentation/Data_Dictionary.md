# Data Dictionary & Grain

The following descriptions are based on the *source CSV extracts*. The Power Query / model versions can have additional derived and renamed fields.

| Source CSV | Grain and primary key | Source columns |
|---|---|---|
| `subscribers(1).csv` | one subscriber / `subscriber_id` | `subscriber_id`, `customer_name`, `birth_date`, `governorate`, `region_and_zip`, `customer_segment`, `gender` |
| `subscriptions(1).csv` | one subscription / `subscription_id` | `subscription_id`, `subscriber_id`, `phone_number`, `line_type`, `status`, `activation_date` |
| `recharges(1).csv` | one recharge / `recharge_id` | `recharge_id`, `subscription_id`, `recharge_date`, `amount`, `payment_method` |
| `shop_sales(1).csv` | one sales record / `sales_id` | `sales_id`, `subscriber_id`, `transaction_date`, `product_name`, `quantity`, `total_sales_amount`, `store_branch` |
| `complaints(1).csv` | one complaint / `complaint_id` | `complaint_id`, `subscriber_id`, `complaint_date`, `category`, `resolution_days`, `satisfaction_score` |

## Model roles

- `DimSubscriber`: subscriber attributes; geographic and demographic slicers.
- `DimSubscription`: line type, status, activation date; subscription identity.
- `DimDate`: calendar features to aggregate events by date/month/year.
- `FactRecharges`: recharge event grain; signed amount and payment method.
- `FactShopSales`: sales record grain; derived numeric value, sale type and quantity.
- `FactComplaints`: complaint event grain; complaint category, durations and ratings.

## Date coverage

- `recharges`: **2025-01-01** to **2026-12-28**.
- `shop_sales`: **2026-01-01** to **2026-06-28**.
- `complaints`: **2026-01-01** to **2026-06-28**.

Dates in `subscriptions` are line activation timestamps, **not** a history of each line's status changes. Subscriber age is calculated with a reference-date convention and should not be read as continuously current.

## Identifiers and privacy

Customer-name fields and phone-number-like identifiers exist in the source extracts. **The raw row-level CSVs are included** in `Data/Raw/`. The project owner has stated they may be published, but any reuse or redistribution should independently check permissions and assess privacy. The original source URL and license were not established from the supplied materials.
