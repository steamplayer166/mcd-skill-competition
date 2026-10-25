# mcd-activity-radar · 麦麦活动雷达

基于麦当劳 MCP 的「活动 / 套餐推荐」WorkBuddy Skill。用户问一句「最近麦当劳有什么活动」，智能体即可自动拉取麦当劳官方活动日历、抽奖与菜单数据，并按两大类清晰呈现：

- **套餐优惠（可购买）**：限时省钱套餐、新套餐、新品、加价购周边、会员卡等
- **比赛 / 征集（非购买）**：派对、亲子活动、积分抽奖 / 领券、开发者征集等

> 本项目为「麦当劳程序员创意开发大赛」参赛作品。

## 功能

- 聚合麦当劳当月**进行中 + 即将开始**的营销活动
- 自动分类为「可购买的套餐优惠」与「可参与的比赛 / 征集」
- 可购买项自动补全**价格与套餐所含食物**（`query-meals` / `query-meal-detail`）
- 非购买类三路补全：积分抽奖（`query-lottery-info`）、麦麦省放码 / 领券（`available-coupons`）、官方征集比赛（人工维护清单，收录 MCP 外活动）
- 每条活动统一输出：**活动名称 / 价格 / 套餐内容 / 参与要求 / 活动入口**
- 支持按口味、场景、预算、身份（学生 / 亲子）追问，串联门店查询、算价下单等能力

## 安装

1. 在麦当劳 MCP 控制台（open.mcd.cn）申请 MCP Token
2. 将仓库根目录的 `mcp-config.example.json` 复制为你的配置，并把 `Bearer ${MCD_MCP_TOKEN}` 中的占位符替换为真实 Token，写入 WorkBuddy 的 `~/.workbuddy/mcp.json`
3. 在 WorkBuddy「连接器」中启用 `mcd-mcp`
4. 将 `mcd-activity-radar/` 目录安装为 WorkBuddy Skill（放入 `~/.workbuddy/skills/` 或项目 `.workbuddy/skills/`）

## 使用示例

- 「最近麦当劳有什么活动？」
- 「这个月麦当劳有什么优惠和新品？」
- 「我是学生，有哪些能省钱的活动？」
- 「最近有什么抽奖或者领券的活动？」
- 「龙焰鸡腿堡三件套多少钱，里面有什么？」

## 目标用户

- 想第一时间了解麦当劳优惠、新品与联名活动的普通消费者
- 有预算 / 口味 / 热量诉求，需要推荐搭配的打工人与学生党
- 想参加派对、抽奖、领券等互动活动的麦当劳会员
- 基于麦当劳 MCP 做二次开发、研究营销活动结构的开发者

## 目录结构

```
.
├── README.md                 # 本项目说明
├── CONTEST_DECLARATION.md    # 参赛声明（官方原文，勿改）
├── MCP_INTEGRATION.md        # MCP 集成说明
├── mcp-config.example.json   # 脱敏 MCP 配置示例
└── mcd-activity-radar/       # Skill 源码
    ├── SKILL.md
    └── references/
        └── mcp-tools.md      # MCP 工具字段速查
```

## 免责声明

本项目为参赛作品，非麦当劳官方产品。活动、餐品信息、价格及供应状态以麦当劳官方渠道实时结果为准。
