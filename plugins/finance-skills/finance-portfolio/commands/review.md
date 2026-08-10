---
description: 组合管理：从研究公司到管理组合，执行投资组合审视与优化
argument-hint: "<持仓清单>"
---

# /review -- 投资组合审视

Apply the **finance-portfolio-review** skill to: $ARGUMENTS

## Invocation

```
/finance-portfolio:review 腾讯30%, 美团20%, 茅台20%, 英伟达15%, 现金15%
```

## Notes

- 估值校验使用 skill 自带脚本 `scripts/financial_rigor.py`；对每只持仓标注信息丰富度（A/B/C级），C级持仓结论标注低置信度
- 需要推荐新标的时，转用 `/finance-research:industry` 或 `/finance-screening:checklist`，而非在本命令内直接荐股
- 单只持仓的投后跟踪用 `/finance-portfolio:track`
