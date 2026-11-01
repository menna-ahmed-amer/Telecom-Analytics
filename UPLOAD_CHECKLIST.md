# GitHub Upload Checklist — Vodafone Telecom Analytics

## Required before uploading

- [x] **Final `.pbix` is included:** `Dashboard/Vodafone_Telecom_Analytics.pbix` (15,893,204 bytes); the package structure was inspected.
- [ ] **Check the raw CSV files:** all five source files are included in `Data/Raw/` and their row counts/checksums match the manifest. Confirm publication permission covers **row-level customer names and phone numbers**, not only aggregated dashboard statistics.
- [ ] **Review screenshots:** the repository now includes the **original dashboard screenshots** you shared. Replace any file only if you make later visual updates.
- [ ] **Review final formatting:** the supplied `Screenshots/Customer_Experience.png` currently shows `Rating Coverage` as `0.83`. Check the final measure format (expected display **83.47%** from the source-level ratio), save the `.pbix`, then update this screenshot to match.
- [ ] **Confirm Power Query paths:** if Power BI asks for file locations, reconnect the sources to `Data/Raw/` and refresh.
- [ ] **Verify relationships:** compare your final model with `Data_Model/Power_BI_Model_View.png`.
- [ ] **Review documentation:** make sure KPI names, DAX logic, and ETL explanations still match your final Power BI file.
- [ ] **Validate `.pbix` in Power BI Desktop:** the original ZIP was missing this file, but the final separately supplied report has now been included. Programmatic inspection confirms six pages and report visuals, but actual refresh, slicer behavior, model relationships and DAX results still require a desktop test. The file is under 25 MiB for GitHub browser upload.
- [ ] **Review the reporting date:** `Data/Raw/recharges(1).csv` has **9,764 transactions dated after 2026-11-01** (through 2026-12-28). Confirm the intended reporting period and filter/date interpretation before presenting the values as historic to that date.
- [x] **Check local links:** all internal README links (including the `.pbix`) resolve inside this completed ZIP. Recheck them after publishing on GitHub.

## Upload steps

1. Create a new GitHub repository named `Vodafone-Telecom-Analytics`.
2. Extract the ZIP package.
3. Confirm `Dashboard/Vodafone_Telecom_Analytics.pbix` is included.
4. Upload **the contents of the project folder**, not the ZIP file itself.
5. Confirm that:
   - the `README.md` renders correctly,
   - screenshots display properly,
   - CSV files are visible under `Data/Raw/`,
   - and documentation files open without broken links.

### Suggested repository description
`Power BI telecom analytics project | Power Query ETL, DAX KPIs, dimensional modeling, and business insights`

**Note:** this package contains the unmodified `.pbix` file provided by the project owner. Its report metadata was inspected, but successful refresh and interactive behavior must be checked in Power BI Desktop.

## Additional report polish (optional)

- [ ] Correct the display names in Power BI Desktop: `Excutive Overview` → `Executive Overview`, `Customers & Subscribtions` → `Customers & Subscriptions`; save the report after editing.
