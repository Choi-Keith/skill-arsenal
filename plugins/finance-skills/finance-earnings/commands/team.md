---
description: 财报精读团队：四大师并行解读 + 编辑润色 + 读者评审，产出可发布的公众号文章
argument-hint: "<公司名 季度>"
---

# /team -- 财报精读团队

Apply the **finance-earnings-team** skill to: $ARGUMENTS

## Invocation

```
/finance-earnings:team 腾讯 2025Q4
/finance-earnings:team PDD 2025年报
/finance-earnings:team 美团 最新
```

## Notes

- 与 `/finance-earnings:review` 的区别：四位大师并行解读 + 编辑润色 + 读者评审，产出可直接发布的公众号文章；只需一个视角快速过一遍时用 review
- 数据验证使用 skill 自带脚本 `scripts/financial_rigor.py`，报告审计使用 `scripts/report_audit.py`
