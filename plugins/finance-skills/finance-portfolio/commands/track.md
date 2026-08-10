---
description: 投资论文追踪：买入后的纪律系统，建立并追踪投资论文
argument-hint: "<公司名>"
---

# /track -- 投资论文追踪

Apply the **finance-thesis-tracker** skill to: $ARGUMENTS

## Invocation

```
/finance-portfolio:track 腾讯              # 首次使用：建立投资论文
/finance-portfolio:track 腾讯              # 后续使用：追踪检查
```

## Notes

- 首次使用时建立投资论文（核心假设清单、红线清单、估值锚点、追踪记录表），后续使用时对照追踪
- 已有 `/finance-research:company` 或 `/finance-research:team` 报告时优先从中读取
- 估值校验使用 skill 自带脚本 `scripts/financial_rigor.py`
- 对比两份报告快照检测论文漂移时用 `/finance-portfolio:drift`
