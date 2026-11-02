# Data Sources

This folder contains the **five original CSV extracts** used for the Power BI analysis. File names are preserved, including `(1)` suffixes, to make comparison with the original extracts straightforward. Source files were copied **byte-for-byte** into `Data/Raw/`.

| File | Records | Grain | Role |
|---|---:|---|---|
| `Raw/subscribers(1).csv` | 114,618 | Subscriber | Customer attributes |
| `Raw/subscriptions(1).csv` | 114,994 | Subscription | Line attributes and status |
| `Raw/recharges(1).csv` | 118,025 | Recharge | Payment and recharge events |
| `Raw/shop_sales(1).csv` | 113,904 | Sales record | Retail sales |
| `Raw/complaints(1).csv` | 105,000 | Complaint | Service and feedback |
| **Total** | **566,541** | Mixed | Sum of rows, **not** distinct customers |

## Working with these files

1. Open the included report at [`../Dashboard/Vodafone_Telecom_Analytics.pbix`](../Dashboard/Vodafone_Telecom_Analytics.pbix) in Power BI Desktop.
2. In Power BI Desktop, open **Transform data → Data source settings**.
3. Update the paths to the CSVs in `Data/Raw/` (the CSVs are included here; relative paths depend on the local environment).
4. Apply changes and refresh; check query errors, relationship filters and KPI results.

**Keep the raw CSVs unchanged.** Power Query is responsible for cleaning, type conversion and derived fields. Read the [Data Cleaning & ETL notes](../Documentation/Data_Cleaning_ETL.md) for the transformation rationale. No exported Power Query M script is claimed in this repository.

**Data provenance:** The five CSVs were provided as project inputs. The original publisher/source URL and license are **not established in the supplied files**; no claim about their origin or whether the records are simulated is made here.

**Publication and privacy:** Source tables include customer-name fields, phone-number-like identifiers and customer IDs. The project owner has stated these files may be shared, but that statement is not independent verification of source licensing or authorization for downstream redistribution. Anyone reusing the files should independently check the applicable permissions and handle identifiers appropriately.
