---
description: 投资研究：巴菲特-芒格-段永平-李录四大师综合分析框架，对公司进行系统化投资研究
argument-hint: "<公司名或代码>"
---

# /company -- 四大师综合投资研究

Apply the **finance-investment-research** skill to: $ARGUMENTS

## Invocation

```
/finance-research:company 腾讯
/finance-research:company NVDA
```

## Notes

- 财务数据必须遵循 **finance-financial-data** 规范：两个独立来源，误差>1%须标记
- 数据验算使用 skill 自带脚本 `scripts/financial_rigor.py`，报告审计使用 `scripts/report_audit.py`
- 管理层评分不确定时，可接着用 `/finance-research:management` 做纵深研究
