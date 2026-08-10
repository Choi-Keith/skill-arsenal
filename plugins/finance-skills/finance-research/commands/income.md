---
description: Income investment: analyze whether a company can produce durable, attractive distributable income to justify a portfolio role
argument-hint: "<company or ticker>"
---

# /income -- Income Investment Analysis

Apply the **finance-income-investment** skill to: $ARGUMENTS

## Invocation

```
/finance-research:income "Realty Income"
/finance-research:income "O" mode=new role=core-income
/finance-research:income "TROW" role=opportunistic-income quantity=100 cost_basis=95.5
```

Optional parameters: `mode=new|existing`, `role=core-income|opportunistic-income`, `quantity`, `cost_basis`, `portfolio_weight`, `target_yield`, `tax_residence`, `horizon`.

## Notes

- Exact payout/yield/valuation arithmetic must use the skill's `scripts/financial_rigor.py`; never rely on mental arithmetic for decision-sensitive numbers
- After saving the report, run the skill's `scripts/report_audit.py` extract + verdict workflow — a report that fails audit is a draft, not publishable research
- For full fundamental research use `/finance-research:company`; for pre-purchase decision use `/finance-screening:checklist`; for post-decision monitoring use `/finance-portfolio:track`
