# Power BI Report (`.pbix`)

**The final Power BI Desktop file is included in this folder** under this exact name:

`Dashboard/Vodafone_Telecom_Analytics.pbix`

The editable Power BI Desktop report is included in this repository. Download the file and open it in Power BI Desktop. The main README links directly to the same path.

## Steps

1. Download and open `Vodafone_Telecom_Analytics.pbix` using Power BI Desktop.
2. Check that all **five analytical pages + landing page** are present.
3. Update **Data source settings** to point to the five included CSV files under `../Data/Raw/`, then refresh.
5. Check the complete relationship model, slicer interactions and navigation buttons.
6. Validate presentation formatting: source-level `Rating Coverage` is **83.47%**; a visual that shows `0.83` should use a percentage format. Recharge value units should be labeled **EGP**, not `$`.
7. Refresh and save. If screenshots differ, re-export the six report pages in `Screenshots/`.

The README and technical docs summarize the work. The `.pbix` remains the source of truth for the final DAX measures and applied Power Query steps.
