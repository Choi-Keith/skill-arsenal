---
description: 巴菲特价值投资买入前 Checklist，支持单个或多个公司批量分析
argument-hint: "<公司名，可多个>"
---

# /checklist -- 巴菲特买入前 Checklist

Apply the **finance-investment-checklist** skill to: $ARGUMENTS

## Invocation

```
/finance-screening:checklist 腾讯
/finance-screening:checklist 腾讯, 茅台, 英伟达
/finance-screening:checklist NVDA AAPL MSFT
```

## Notes

- 这是买入前的最终决策关卡；研究底稿可来自 `/finance-research:company` 或 `/finance-research:team`
- 估值验算使用 skill 自带脚本 `scripts/financial_rigor.py`
