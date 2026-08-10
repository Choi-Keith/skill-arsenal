---
description: 财务数据获取与交叉验证规范：每个关键数据必须来自两个独立来源，误差>1%须标记
argument-hint: "<公司名或市场（可选）>"
---

# /data-rules -- 财务数据交叉验证规范

Apply the **finance-financial-data** skill to the current research task: $ARGUMENTS

本规范适用于所有涉及企业财务数据的研究，其他 finance 命令在执行中会引用它。

## Notes

- 每个关键数据必须来自两个独立来源，误差>1%须标记
- 台股取数使用 skill 自带脚本 `scripts/twstock_data.py`（FinMind 数据源，零依赖）
