# Skill Arsenal

> **给 AI Agent 配上一整套专业技能包——价值投资、产品管理、全栈开发、自媒体写作，一站式调度。**

Skill Arsenal 是一个面向 Claude Code（兼容 Codex 等 Agent 平台）的技能插件集合。它不是传统的代码库——而是 **Agent 的"操作系统"**：把产品经理、投资人、工程师、创作者的思维方式和工作流程编译成可被 AI 调度的 skills 和 commands。

---

## 项目理念

### 问题

AI Agent 很强，但**不懂领域最佳实践**：
- 让它分析一家公司 → 不知道该看 ROE 还是毛利率
- 让它走 PM 流程 → 不知道从用户访谈到 PRD 的顺序
- 让它写公众号 → 不知道微信公众号和小红书的排版差异

### 解法

把每个领域的专家方法论写成 **Skill（技能文件）**——告诉 Agent 在这个场景下应该怎么思考、用什么框架、产出什么。Skills 之间通过 **Orchestrator（编排器）**串联，形成完整工作流。

```
用户自然语言 → Orchestrator 诊断阶段 → 推荐+调度 Skills → 产出专业交付物
```

---

## 快速开始

### 核心概念：项目级 vs 用户级

Skill Arsenal 的所有配置都在**项目目录**中（`.claude/settings.json`），不需要修改用户级配置（`~/.claude/settings.json`）。clone 之后，插件和 skill 跟随项目走，不会污染你的全局环境。

```
用户级 (~/.claude/settings.json)     ← 不动，保持干净
项目级 (<repo>/.claude/settings.json) ← 插件在这里，Git 追踪，团队共享
```

---

### 方式 A：安装整套插件（推荐团队使用）

克隆后，需要先**注册市场**，然后按需安装插件。你可以按单个插件安装，也可以按领域（如 `pm-skills`）批量安装。

#### 安装单个插件

```bash

# 注册市场（只需执行一次，Claude Code 重启后自动加载）
/plugin marketplace add Choi-Keith/skill-arsenal

# 产品经理SKILL
/plugin install pm-master@skill-arsenal
/plugin install pm-product-strategy@skill-arsenal
/plugin install pm-product-discovery@skill-arsenal
/plugin install pm-market-research@skill-arsenal
/plugin install pm-execution@skill-arsenal
/plugin install pm-data-analytics@skill-arsenal
/plugin install pm-go-to-market@skill-arsenal
/plugin install pm-marketing-growth@skill-arsenal
/plugin install pm-ai-shipping@skill-arsenal
/plugin install pm-toolkit@skill-arsenal

# 开发SKILL
/plugin install dev-backend@skill-arsenal
/plugin install dev-frontend@skill-arsenal

# 金融投资SKILL
/plugin install finance-earnings@skill-arsenal
/plugin install finance-portfolio@skill-arsenal
/plugin install finance-research@skill-arsenal
/plugin install finance-screening@skill-arsenal
/plugin install finance-writing@skill-arsenal
/plugin install finance-earnings@skill-arsenal
/plugin install finance-earnings@skill-arsenal

# 写作
/plugin install write-social@skill-arsenal
```

> 💡 可用领域：`pm-skills` | `finance-skills` | `dev-skills` | `write-skills`
>
> 每个领域包含的插件列表请参考 [marketplace.json](.claude-plugin/marketplace.json)。

**验证安装成功**：
```
/plugin list                    # 应看到 skill-arsenal 市场
/plugin uninstall xxx           # 删除plugin
/finance-research:company 腾讯  # 应能正常执行
/pm-master:master               # 应能正常执行
```

---

### 方式 B：安装单个 Skill / Command（推荐个人使用）

使用 `scripts/install.sh` 脚本，**交互式浏览** 97 个 skills 和 68 个 commands，一键安装。

```bash
# 克隆仓库
git clone https://github.com/Choi-Keith/skill-arsenal.git
cd skill-arsenal

# 交互式菜单（浏览所有 skill/command，按名称安装）
./scripts/install.sh

# 直接安装指定 skill
./scripts/install.sh --skill create-prd
./scripts/install.sh --skill finance-investment-research

# 直接安装指定 command
./scripts/install.sh --command company

# 安装整个插件的全部 skill
./scripts/install.sh --category dev-backend

# 安装整个领域的全部 skill + command（一键装完 pm-skills）
./scripts/install.sh --domain pm-skills
./scripts/install.sh --domain finance-skills

# 列出所有可安装项
./scripts/install.sh --list

# 安装全部
./scripts/install.sh --all
```

**Windows 用户**：
```cmd
scripts\install.bat --skill create-prd
scripts\install.bat --list
scripts\install.bat
```

**为什么用脚本安装**：
- ✅ 无需记路径——脚本自动扫描全部 skill/command，交互式选择
- ✅ 自动复制 `references/` 目录——skill 的参考文件一并安装
- ✅ 安装后提示相关 command——装 skill 时提醒你可以搭配哪些 slash 命令
- ✅ 跨平台——macOS/Linux 用 `install.sh`，Windows 用 `install.bat`
- ✅ 零依赖——只需 Python 3（macOS/Linux 自带，Windows 需安装）
- ✅ `--domain` 一键装完整个领域（如 `./scripts/install.sh --domain pm-skills`）

---

### 安装方式对比

| | 方式 A（整套插件） | 方式 B（install.sh 脚本） |
|---|---|---|
| **配置复杂度** | 中（需注册 market） | 低（一行命令） |
| **安装范围** | 全部 skills + commands | 按需挑选 |
| **粒度控制** | 单个插件 / 整个领域 | 单个 skill / 单个插件 / 整个领域 |
| **上下文占用** | 全部 skill 描述在 catalog 中 | 仅安装的 skill |
| **团队共享** | ✅ `.claude/settings.json` 提交 Git | ✅ `.claude/skills/` 提交 Git |
| **适合场景** | 团队统一工具链 | 个人按需取用 |

### 使用方式

**方式一：直接调用 Command（推荐入门）**

当你明确知道自己要做什么时，直接使用命令：

```
/finance-research:company 腾讯           # 深度研究腾讯
/pm-execution:write-prd SSO功能         # 写一份PRD
/dev-frontend:nextjs 实现用户登录页面     # 用Next.js开发
/write-social:publish 如何成为优秀PM     # 发布到小红书/公众号/抖音
```

**方式二：使用 Orchestrator（不知道从哪开始时）**

当你不确定该用哪个 skill 时，先找编排器诊断：

```
/pm-master:master 我想做一个新功能，从哪开始？
/pm-master:master 用户流失很严重，怎么分析？
/pm-master:master 帮我走完从需求到上线的全流程
```

**方式三：自然语言触发（最自然）**

直接描述你的需求，Agent 会根据 skill 描述自动匹配：

```
"帮我分析一下腾讯的商业模式和护城河"
"用户说onboarding太难了，帮我设计个方案"
"写一篇关于AI大模型技术解读的公众号文章"
```

### 兼容性

| Agent 平台 | 兼容性 | 说明 |
|-----------|--------|------|
| **Claude Code** | ✅ 完全支持 | 原生 plugin/skill/command 系统 |
| **Codex (OpenAI)** | ⚠️ 部分兼容 | Skills 的 SKILL.md 可作为 system prompt 使用；plugin 和 command 系统不兼容 |
| **其他 Agent** | 🔧 可适配 | 将 SKILL.md 的内容作为 custom instruction 或 knowledge base 注入 |

> 💡 **Codex 用户**：直接将对应 skill 的 `SKILL.md` 文件内容复制到 Codex 的 Knowledge 或自定义指令中即可使用。例如，把 `plugins/finance-skills/finance-research/skills/finance-investment-research/SKILL.md` 的内容作为投资研究 Agent 的系统提示。

---

## 功能全景

### 四大领域，130+ 个技能

```
┌──────────────────────────────────────────────────────────────────┐
│                    Skill Arsenal.                                │
├────────────┬───────────────┬────────────────┬───────────────────┤
│ 📊 Finance │   📋 PM       │   🔧 Dev       │    ✍️ Write       │
│  20 skills │   69 skills   │   8 skills     │    1 orchestrator │
│  5 plugins │   10 plugins  │   2 plugins    │    1 plugin       │
├────────────┴───────────────┴────────────────┴───────────────────┤
│              🧭 3 个 Orchestrator（智能调度器）                    │
│   pm-master  │  social-media-publisher  │  (finance 内置流程)    │
└──────────────────────────────────────────────────────────────────┘
```

---

### 📊 Finance Skills — 价值投资全流程

**从选股到投后管理，覆盖价值投资的完整生命周期。**

| 阶段 | 插件 | 核心能力 | 入口命令 |
|------|------|---------|---------|
| ① 选股筛选 | finance-screening | 7指标去劣、行业漏斗、Checklist、供应链扫描 | `/finance-screening:funnel` |
| ② 投资研究 | finance-research | 四大师分析、团队并行、行业/管理层/未上市 | `/finance-research:company` |
| ③ 财报分析 | finance-earnings | 单一精读、四大师团队解读+公众号产出 | `/finance-earnings:review` |
| ④ 投后管理 | finance-portfolio | 组合审视、论文追踪、漂移检测、新闻归因 | `/finance-portfolio:review` |
| ⑤ 写作输出 | finance-writing | 三Agent公众号文章、深度公司系列长文 | `/finance-writing:wechat` |

**特色能力**：
- 🧠 巴菲特-芒格-段永平-李录四大师并行分析框架
- 🔍 7 条硬指标 + 3 条豁免规则的量化去劣系统
- 🕵️ 6 Agent 拼图式未上市公司研究（信息稀缺条件下还原真实价值）
- 📰 10 分钟股价异动新闻归因系统
- ✅ 全流程财务数据交叉验证规范（双源验证，误差>1%即标记）

→ 详细文档：[plugins/finance-skills/README.md](plugins/finance-skills/README.md)

---

### 📋 PM Skills — 产品经理全流程

**从需求收集到上线验收，覆盖 PM 工作的六大阶段。**

| 阶段 | 插件 | 核心能力 | 入口 |
|------|------|---------|------|
| 🧭 总调度 | pm-master | 阶段诊断 → 技能推荐 → 多阶段编排 | `/pm-master:master` |
| 📐 策略基础 | pm-product-strategy | 愿景、战略画布、商业模型、SWOT/PESTLE/五力 | `/pm-product-strategy:strategy` |
| ① 需求收集 | pm-product-discovery + pm-market-research | 用户访谈、竞品分析、用户画像、反馈分析 | `/pm-product-discovery:discover` |
| ② 需求分析 | pm-product-discovery + pm-data-analytics | 优先级排序、OST、同期群、SQL 查询 | 由 pm-master 调度 |
| ③ 产品验证 | pm-product-discovery + pm-execution | 实验设计、A/B 分析、事前验尸、红队攻击 | 由 pm-master 调度 |
| ④ 需求设计 | pm-execution | PRD、用户故事、OKR、Sprint、干系人地图 | `/pm-execution:write-prd` |
| ⑤ 数据验证 | pm-data-analytics | SQL 查询、同期群分析、A/B 测试 | `/pm-data-analytics:analyze-cohorts` |
| ⑥ 上线验收 | pm-go-to-market + pm-execution | GTM 策略、对战卡、增长回路、回顾 | `/pm-go-to-market:plan-launch` |

**特色能力**：
- 🧭 **pm-master 编排器**：不需要记住 68 个 skill 的名字——描述你想做什么，自动诊断阶段并推荐
- 📋 3 种编排模式：单阶段诊断 / 多阶段串联 / 快速路由
- 🎯 4 个预置 Playbook：新产品从零到一、现有功能优化、竞品压力响应、数据驱动迭代
- 🤖 pm-ai-shipping：AI 构建代码的安全审计+性能审计+交付文档

→ 详细文档：[plugins/pm-skills/README.md](plugins/pm-skills/README.md)

---

### 🔧 Dev Skills — 全栈开发

**后端 + 前端，覆盖日常开发的核心技术栈。**

| 插件 | Skills | 核心能力 | 入口命令 |
|------|--------|---------|---------|
| dev-backend | 4 | 后端工程、Go 并发/微服务、MySQL 模式、代码审查 | `/dev-backend:backend` |
| dev-frontend | 4 | Next.js 全栈、Ant Design、主题工厂、代码审查 | `/dev-frontend:nextjs` |

**特色能力**：
- 🔍 双轴代码审查（Standards 规范 + Spec 规格），两个子 Agent 并行审查
- 🎨 10 套预设主题 + 即时生成新主题（幻灯片/文档/报告/HTML）
- 🐹 Go 惯用写法：goroutine+channel、pprof 性能分析、gRPC 微服务

→ 详细文档：[plugins/dev-skills/README.md](plugins/dev-skills/README.md)

---

### ✍️ Write Skills — 自媒体多平台发布

**微信公众号、小红书、抖音——一站式内容创作+封面生成。**

| 插件 | Skills | 核心能力 | 入口命令 |
|------|--------|---------|---------|
| write-social | 1 | 平台选择→格式选择→内容创作→封面生成 | `/write-social:publish` |

**四步工作流**：
1. **选平台** → 微信公众号（长文）/ 小红书（图文）/ 抖音（脚本）
2. **选格式** → 基础 MD / 富媒体 MD / HTML 幻灯片
3. **创内容** → 调度 research skill + 去 AI 感 + 平台改写
4. **生成封面** → 按平台尺寸自动生成（900×383 / 1080×1440 / 1080×1920）

**特色能力**：
- 🔄 同一份研究素材 → 自动适配三大平台的风格和字数要求
- 🤝 可调度 finance-research 等 skill 作为内容来源
- ✨ 集成 write-humanizer-zh（去 AI 感）、write-frontend-slides（HTML 幻灯片）、write-huashu-design（封面设计）

→ 详细文档：[plugins/write-skills/README.md](plugins/write-skills/README.md)

---

## 项目结构

```
skill-arsenal/
├── README.md                         # ← 你在这里
├── .claude-plugin/
│   └── marketplace.json              # 插件市场注册
├── plugins/
│   ├── dev-skills/                   # 开发者技能包
│   │   ├── README.md
│   │   ├── dev-backend/              #   后端（Go, MySQL, API, 审查）
│   │   └── dev-frontend/             #   前端（Next.js, Antd, 主题）
│   ├── finance-skills/               # 投资研究技能包
│   │   ├── README.md
│   │   ├── finance-screening/        #   选股筛选
│   │   ├── finance-research/         #   投资研究
│   │   ├── finance-earnings/         #   财报分析
│   │   ├── finance-portfolio/        #   投后管理
│   │   └── finance-writing/          #   写作输出
│   ├── pm-skills/                    # 产品经理技能包
│   │   ├── README.md
│   │   ├── pm-master/                #   🧭 总调度器
│   │   ├── pm-product-strategy/      #   战略与基础
│   │   ├── pm-product-discovery/     #   产品发现
│   │   ├── pm-market-research/       #   市场研究
│   │   ├── pm-data-analytics/        #   数据分析
│   │   ├── pm-execution/             #   执行交付
│   │   ├── pm-go-to-market/          #   GTM与发布
│   │   ├── pm-marketing-growth/      #   营销增长
│   │   ├── pm-ai-shipping/           #   AI交付审计
│   │   └── pm-toolkit/               #   实用工具
│   └── write-skills/                 # 自媒体写作技能包
│       ├── README.md
│       └── write-social/             #   多平台发布编排器
├── skills/                           # 独立 skills（32个）
├── commands/                         # 独立 commands（20个）
├── tools/                            # 辅助脚本
└── openspec/                         # 变更管理
```

---

## 设计原则

### 1. 编排器模式（Orchestrator Pattern）

用户不应该需要记住 100+ 个 skill 的名字。编排器负责：
- **诊断**：通过几个问题定位用户所处阶段
- **推荐**：给出最合适的 2-3 个 skill
- **串联**：按依赖关系编排多 skill 流程
- **衔接**：一个阶段完成后自然引导到下一阶段

### 2. 渐进式加载（Progressive Loading）

为防止 Agent 上下文窗口爆炸：
- 每个 skill 的 description 含 `触发词`（何时用）和 `不适用于`（何时不用）
- 超长 skill（>500 行）拆分为 SKILL.md + references/，按需加载
- 重量级 skill 标记 `context_weight: heavy`，让 Agent 选择性加载

### 3. 平台适配（Platform Adaptation）

同一份内容，不同平台要求不同：
- PM skills 适配 B2B/B2C 两种产品语境
- Write skills 适配微信公众号/小红书/抖音三种平台规则
- Finance skills 适配上市公司/未上市公司两套研究方法

### 4. 质量红线（Quality Gates）

关键步骤不可跳过：
- 财务数据：双源验证，误差>1%标记
- 文章写作：去 AI 感 + 字数底线（≥1500字）
- 代码审查：规范+规格双轴并行
- 封面图：按平台尺寸适配

---

## 贡献指南

### 添加新 Skill

```bash
# 使用 skill-creator
/skill-creator

# 或手动创建
mkdir -p plugins/{domain}/my-plugin/skills/my-skill/
# 编写 SKILL.md（参考现有 skill 的格式）
```

### Skill 编写规范

每个 SKILL.md 应包含：
1. **Frontmatter**：`name`, `description`（含触发词+不适用场景）
2. **核心流程**：清晰的步骤化指令
3. **输出规格**：产出物的格式和标准
4. **质量红线**：不可跳过的检查点

### 目录规范

```
plugins/{domain}/
├── README.md
└── {plugin}/
    ├── .claude-plugin/
    │   └── plugin.json
    ├── commands/           # 可选
    │   └── {cmd}.md
    └── skills/
        └── {skill}/
            ├── SKILL.md
            └── references/ # 可选，超过500行时使用
```

---

## License

MIT
