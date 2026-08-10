---
description: 去劣筛选：7条指标快速排除非一流公司
argument-hint: "<公司名，可多个>"
---

# /quality -- 去劣指标筛选

Apply the **finance-quality-screen** skill to: $ARGUMENTS

## Invocation

```
/finance-screening:quality 腾讯
/finance-screening:quality 腾讯, 茅台, 英伟达
```

## Notes

- 目的是快速排除而非精选：任何一条核心指标不达标即出局
- 通过筛选的标的可接 `/finance-research:company` 做深度研究
