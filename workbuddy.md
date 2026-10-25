# workbuddy.md — WorkBuddy 开发对话上下文

> 本文件记录本项目「麦当劳程序员创意开发大赛」参赛 Skill 使用腾讯 WorkBuddy 智能体完成开发的全过程，用于核验 WorkBuddy 专项奖励联动条件。

## 项目概述

- 项目名称：mcd-activity-radar（麦麦活动雷达）
- 项目类型：基于麦当劳 MCP 的 WorkBuddy Skill（活动 / 套餐推荐）
- 开发工具：腾讯 WorkBuddy 智能体（Agent 模式，全程对话式完成）
- 仓库：https://github.com/steamplayer166/mcd-skill-competition

## 开发全过程（WorkBuddy 对话记录）

### 第 1 阶段：接入麦当劳 MCP
- 用户提供麦当劳 MCP 地址（https://mcp.mcd.cn）与 Token，要求接入 WorkBuddy
- WorkBuddy 将 `mcd-mcp` 写入 `~/.workbuddy/mcp.json`（streamableHttp + Bearer 鉴权），并指导用户在「连接器」中启用
- 实测连通：`query-nearby-stores` / `query-meals` / `calculate-price` 均返回成功

### 第 2 阶段：调研比赛规则
- 用户提供比赛仓库 https://github.com/M-China/mcd-developer-innovation-challenge
- WorkBuddy 克隆仓库并解析 `README.md` / `activityGuidelines.md` / `CONTEST_DECLARATION.md`，整理出：
  - 必需文件清单与命名规范（README.md / CONTEST_DECLARATION.md / MCP_INTEGRATION.md / mcp-config.example.json / 源代码 / workbuddy.md）
  - 参赛资格、提交方式（Issue）、评审规则（公开 Star 排名）、奖励与合规红线

### 第 3 阶段：建仓
- 用户要求创建仓库 `mcd-skill-competition`
- WorkBuddy 通过本机 `gh` CLI 创建 Public 仓库（GitHub 账号 steamplayer166）

### 第 4 阶段：设计并生成 Skill
- 用户确定方向：套餐 / 活动推荐，按「口味、场景、随单购优惠」推荐
- WorkBuddy 实际调用 `campaign-calendar` / `query-lottery-info` 摸清字段结构后，确定：
  - 分类规则：套餐优惠（可购买）vs 比赛 / 征集（非购买）
  - 输出格式：活动名称 / 参与要求 / 活动入口
- 生成 `mcd-activity-radar/SKILL.md` 及 `references/mcp-tools.md`

### 第 5 阶段：测试与迭代
- 用户实测「非购买类」后反馈两点：
  1. 可购买套餐要标注价格 + 套餐所含食物
  2. 非购买类不全（缺放码领券、缺征集比赛）
- WorkBuddy 迭代 v2：
  - 可购买类输出增加「价格」「套餐内容」，流程接入 query-meals + query-meal-detail
  - 非购买类三路补全：query-lottery-info + available-coupons + references/external-events.md（人工维护官方征集清单，收录 1024 skill 大赛）
- 实测中发现并修复 bug：`query-meals` 价格单位为「元」而非「分」（仅 `calculate-price` 为分）

## 关键决策记录

- Skill 数据源以 `campaign-calendar` 为主，抽奖 / 领券 / 征集为补充
- 官方征集类活动不在 MCP 内，用人工维护清单 `external-events.md` 补齐
- 遵守「不虚构 URL」原则：MCP 无跳转链接字段时写渠道而非硬造链接
- 价格单位分/元严格区分，避免误读

## 交付物清单

- README.md、CONTEST_DECLARATION.md（官方原文）、MCP_INTEGRATION.md、mcp-config.example.json（脱敏占位符）、DEMO.md
- mcd-activity-radar/（SKILL.md + references/mcp-tools.md + references/external-events.md）
- mcd-activity-radar.zip（官方校验打包产物）

## 声明

本项目由参赛者借助腾讯 WorkBuddy 智能体完成开发，符合「真实使用 WorkBuddy 智能体」的联动奖励条件。以上对话上下文为真实开发过程记录。
