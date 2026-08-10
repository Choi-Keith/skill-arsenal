---
description: 财报精读：一手资料深度解读
argument-hint: "<公司名 季度>"
---

# /review -- 财报精读

Apply the **finance-earnings-review** skill to: $ARGUMENTS

## Invocation

```
/finance-earnings:review 腾讯 2025Q4
/finance-earnings:review PDD 2025年报
/finance-earnings:review 美团 最新        # 默认读取最近一期
```

## Notes

- 坚持一手资料（财报原文/业绩会纪要）；无法获取原文时按 **finance-financial-data** 规范用标准数据源拼凑并标注
- 数据验证使用 skill 自带脚本 `scripts/financial_rigor.py`，报告审计使用 `scripts/report_audit.py`
- 需要多视角解读或产出可发布文章时，改用 `/finance-earnings:team`
