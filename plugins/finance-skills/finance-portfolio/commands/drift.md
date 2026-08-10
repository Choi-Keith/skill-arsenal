---
description: 投资论文漂移检测：对比两份报告快照，分清事实变化与措辞变化
argument-hint: "<公司名 旧报告路径 新报告路径>"
---

# /drift -- 投资论文漂移检测

Apply the **finance-thesis-drift** skill to: $ARGUMENTS

## Invocation

```
/finance-portfolio:drift 腾讯 reports/腾讯-2025Q3.md reports/腾讯-2025Q4.md
```

## Notes

- 依赖 `/finance-portfolio:track` 输出的结构化维度（核心假设清单、红线清单、估值锚点）；没有基线时先补齐再检测
- 所有数值变化必须使用 skill 自带脚本 `scripts/financial_rigor.py` 精确计算，禁止心算
