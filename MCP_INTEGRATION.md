# MCP 集成说明

## 使用的 MCP Server

| 项 | 值 |
|---|---|
| Server 名称 | 麦当劳 MCP（`mcd-mcp`） |
| 接入地址 | `https://mcp.mcd.cn` |
| 传输协议 | Streamable HTTP |
| 鉴权方式 | HTTP Header `Authorization: Bearer <MCP Token>` |
| Token 申请 | 麦当劳 MCP 控制台（open.mcd.cn）申请 |

脱敏配置示例见仓库根目录 `mcp-config.example.json`（仅含环境变量占位符，不包含真实 Token）。

## 使用的 Tool

| Tool | 用途 | 关键入参 |
|---|---|---|
| `campaign-calendar` | 获取当月营销活动日历（进行中/未来） | `specifiedDate`（可选） |
| `query-lottery-info` | 查询积分抽奖活动与奖品 | 无 |
| `query-nearby-stores` | 查询到店门店，获取 `storeCode` | `beType`、`searchType`、`city`、`keyword` |
| `query-meals` | 查询菜单与餐品价格 | `storeCode`、`orderType`、`beType` |
| `query-meal-detail` | 查询套餐组成与可特调项 | `storeCode`、`orderType`、`beType`、`code` |
| `available-coupons` | 查询麦麦省可领取优惠券（放码 / 领券类活动来源） | 无 |
| `calculate-price` | 计算商品总价（含优惠） | `storeCode`、`orderType`、`beType`、`items[]` |
| `create-order` | 创建订单 | `storeCode`、`orderType`、`beType`、`items[]`、`takeWay` |

## 调用流程

```
用户问「最近有什么活动」
        │
        ▼
 campaign-calendar（可选 now-time-info 确认日期）
        │
        ├─ 非购买补全 → query-lottery-info（抽奖）+ available-coupons（放码/领券）
        │              + references/external-events.md（官方征集比赛，MCP 外）
        │
        ├─ 可购买补全 → query-nearby-stores → query-meals（价格）
        │              → query-meal-detail（套餐所含食物）
        │
        ├─ 用户想下单 → calculate-price → create-order
        │
        ▼
 按「套餐优惠 / 比赛征集」两类分类输出（活动名称 / 参与要求 / 活动入口）
```

## 业务价值

- 把麦当劳 MCP 的「活动日历 + 抽奖 + 菜单/价格」能力组合成一条端到端的活动发现链路
- 将零散的营销信息按「可购买 vs 可参与」自动归类，用户一眼看清「能省什么钱」与「能玩什么」
- 价格单位（分→元）、入口渠道（APP/小程序/到店）等细节由 skill 统一处理，避免 AI 误读字段
- 为后续下单、领券等交易行为提供前置的透明信息层
