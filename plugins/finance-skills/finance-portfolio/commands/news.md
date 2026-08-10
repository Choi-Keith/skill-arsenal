---
description: 公司新闻脉搏：股价异动时快速归因，4 个并行 Agent 侦察事件/政策/对手/情绪
argument-hint: "<公司名>"
---

# /news -- 公司新闻脉搏

Apply the **finance-news-pulse** skill to: $ARGUMENTS

## Invocation

```
/finance-portfolio:news 腾讯
/finance-portfolio:news 拼多多 大跌
```

## Notes

- 产出「事件时间线 + 异动主因判断 + 是否触发论文重审」
- 不适用场景：完整投研（用 `/finance-research:team`）、财报深读（用 `/finance-earnings:review`）、长期论文跟踪（用 `/finance-portfolio:track`)
- 大V观点抓取使用 skill 自带脚本 `scripts/xueqiu_scraper.py`
