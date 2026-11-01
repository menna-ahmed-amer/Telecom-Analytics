# DAX Reference — Selected Measures

These are **reconstructed teaching/reference expressions from the verified KPI definitions**, not an exported copy of the original `.pbix` measure library. Confirm measure names/column casing before pasting into Power BI.

```dax
Total Subscribers = COUNTROWS(DimSubscriber)

Total Subscriptions = COUNTROWS(DimSubscription)

Active Lines =
CALCULATE([Total Subscriptions], DimSubscription[status] = "Active")

Active Line Rate % = DIVIDE([Active Lines], [Total Subscriptions])

Suspended Lines =
CALCULATE([Total Subscriptions], DimSubscription[status] = "Suspended")

Recharge Transactions = COUNTROWS(FactRecharges)

Net Recharge Value = SUM(FactRecharges[amount])

Gross Positive Recharge Value =
CALCULATE([Net Recharge Value], FactRecharges[amount] > 0)

Recharging Line IDs = DISTINCTCOUNT(FactRecharges[subscription_id])

Average Positive Recharge Value =
CALCULATE(AVERAGE(FactRecharges[amount]), FactRecharges[amount] > 0)

Negative Recharge Transactions =
CALCULATE(COUNTROWS(FactRecharges), FactRecharges[amount] < 0)

Negative Transaction Rate % =
DIVIDE([Negative Recharge Transactions], [Recharge Transactions])

Shop Sales Records = COUNTROWS(FactShopSales)

Shop Sales Value = SUM(FactShopSales[sales_revenue])

Shop Units = SUM(FactShopSales[quantity])

Average Paid Sales Record Value =
CALCULATE(AVERAGE(FactShopSales[sales_revenue]), FactShopSales[Sale_Type] = "Paid")

Free Sales Rate % =
DIVIDE(
  CALCULATE([Shop Sales Records], FactShopSales[Sale_Type] = "Free"),
  [Shop Sales Records]
)

Total Complaints = COUNTROWS(FactComplaints)

Average Resolution Days = AVERAGE(FactComplaints[resolution_days])

Average Satisfaction Score = AVERAGE(FactComplaints[satisfaction_score])

Low Satisfaction Rate % =
DIVIDE(
  CALCULATE([Total Complaints], FactComplaints[satisfaction_score] IN {1, 2}),
  COUNT(FactComplaints[satisfaction_score])
)

Satisfaction Response Rate % =
DIVIDE(COUNT(FactComplaints[satisfaction_score]), [Total Complaints])

Purchasing Customers =
COUNTROWS(
  INTERSECT(
    VALUES(DimSubscriber[subscriber_id]),
    VALUES(FactShopSales[subscriber_id])
  )
)

Purchase Participation Rate % = DIVIDE([Purchasing Customers], [Total Subscribers])

Customers with Complaints =
COUNTROWS(
  INTERSECT(
    VALUES(DimSubscriber[subscriber_id]),
    VALUES(FactComplaints[subscriber_id])
  )
)

Complaint Participation Rate % =
DIVIDE([Customers with Complaints], [Total Subscribers])
```

**Important:** Formatting (`Percentage`, decimal places, currency EGP), filter propagation, and date coverage must be checked in the Power BI report, not inferred from a copied expression alone.
