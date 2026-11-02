# Business Insights & Recommended Follow-up

All figures below are descriptive results on the supplied datasets, not evidence of causal impact.

## Executive overview

| Observation | Business interpretation | Next step |
|---|---|---|
| 114,618 master subscribers, 114,994 subscription records | Subscribers and lines have distinct grains | Track both customer and line metrics |
| 101,095 active subscriptions (87.91%) | Most recorded lines are active in the snapshot | Review suspended and terminated cohorts separately |
| 15,297,520 EGP signed recharge value | Negative amounts materially affect net vs positive-only recharge totals | Reconcile negative-entry business meaning |

## Customers & subscriptions

Cairo, Giza and Alexandria dominate the standardized subscriber geography. Compare engagement and suspension rates across regions, controlling for customer-base size before prioritizing campaigns.

- **Suspended lines:** 10,527 (9.15%). Investigate reactivation opportunities; do not assume all are lost customers.
- **Shop buyer rate:** 62.86% of known subscribers appear in sales records in the available Jan–Jun 2026 sales extract.
- **Complainant rate:** 59.96% of known subscribers appear in complaints in the available Jan–Jun 2026 extract.

## Recharge performance

- **17,043** recharge entries (14.44%) have negative signed amounts, totaling **–852,150 EGP**. Investigate the event types rather than calling all entries refunds.
- Positive-only recharge sum **16,149,670 EGP** vs signed net recharge **15,297,520 EGP**.
- Payment channels have fairly similar volume/value contributions in the dashboard, so evaluate costs, reliability and customer experience in addition to total value.

## Shop sales performance

- **86.83%** of derived shop sales value is attributable to iPhone 15 Pro. Review concentration, inventory exposure, and contribution margin once margin data are available.
- **4.98%** of shop sales *records* have `FREE` source amounts. Evaluate promotion purpose; free records are not automatically profitable/unprofitable.
- Branch sales values are similar across four branches; compare profit, traffic, and cost-to-serve before reallocating resources.

## Customer experience

- **50.07%** of complaints fall in the normalized `Network Issue` category. Investigate network issues and prioritize diagnostic analysis by location.
- **39.75%** of **rated** complaints have a score of 1 or 2; this is not 39.75% of all customers or all complaints.
- **83.47%** of complaints have ratings; **16.53%** do not. The **average recorded satisfaction is 3.01/5** across rated complaints. Improve response coverage to support more representative analysis.
- Average recorded `resolution_days` is ~2.00 among records with known numeric values; unknown durations are excluded and there is no confirmed closed-status field.

## Prioritization

The strongest immediate follow-ups are (1) network-issue investigation; (2) negative recharge classification and reconciliation; (3) high product concentration assessment. These priorities are **hypotheses for investigation**, not guaranteed improvements.
