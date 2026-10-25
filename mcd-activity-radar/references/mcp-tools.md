# 麦当劳 MCP 工具速查（本 skill 使用）

> 工具前缀均为 `mcp__mcd-mcp__`。价格单位：`calculate-price` / `query-meals` 的「分」需 ÷100 转「元」；`campaign-calendar` 文案中的价格已是「元」。

## campaign-calendar — 活动日历（主数据源）
- 入参：`specifiedDate`（可选，`yyyy-MM-dd`）。不传则返回当月所有活动；传了则以该日为锚点，返回当天及前后各一个最近活动日。
- 出参：按日期分组的活动列表，每条含 `活动标题`、`活动内容介绍`、`活动图片介绍`（图片为 `<img src=...>`）。
- 用途：「最近有什么活动」的主力工具。

## query-lottery-info — 积分抽奖活动
- 入参：无。
- 出参：`activityName`、`activityStatusText`（未开始/进行中/已结束）、`beginTime`/`endTime`、`drawPoint`（单次积分）、`drawTypeText`（消耗规则）、`availablePoint`、`drawDecision`（本次消耗决策）、`prizes[]`（奖品 name/imageUrl/typeText）。
- 用途：抽奖 / 领券类「非购买」活动。

## query-nearby-stores — 到店门店
- 入参：`beType`（1 到店自提 / 5 得来速）、`searchType`（2 按位置）、`city`、`keyword`。
- 出参：`storeCode`、`beCode`、`storeName`、`address`、`distance`、`reservation`。
- 用途：拿 `storeCode`，供 query-meals / calculate-price / create-order 使用。

## query-meals — 菜单与价格
- 入参：`storeCode`、`orderType`（1 到店 / 2 外送）、`beType`、`beCode`（得来速/外送必传）。
- 出参：`categories[]`（分类）与 `meals{code}` 映射，每项含 `name`、`currentPrice`（现价，分）、`originalPrice`（原价，分）、`discountType`（优惠类型）、`tags`（如「上新」「限时特惠」）、`canWithOrder`。
- 用途：查某套餐 / 新品具体价格，配合「套餐优惠」类活动。

## query-meal-detail — 套餐组成
- 入参：`storeCode`、`orderType`、`beType`、`code`（餐品编码）。
- 出参：套餐内组成、可特调项（`supportModify`）。
- 用途：用户想了解套餐里有什么、能否更换搭配。

## available-coupons — 麦麦省可领优惠券（放码 / 领券）
- 入参：无。
- 出参：当前可领取的优惠券列表（券名称、图片、状态、促销标签）。
- 用途：「非购买」类领券 / 放码活动的数据源。只做展示；用户明确要领券时才调 `auto-bind-coupons`（一键领取，需先确认）。
- 相关：`query-store-coupons`（指定门店可用券，下单场景）、`query-my-coupons`（我的卡包券）。

## 官方征集 / 比赛活动（MCP 外渠道）
- 数据源：本 skill `references/external-events.md`（人工维护清单）。
- 用途：麦当劳开发者征集、比赛类活动**不在 MCP 内**。输出「比赛 / 征集」分类时，必须把清单中仍在有效期内的条目合并进结果，并给出清单中的跳转链接。

## calculate-price — 价格计算
- 入参：`storeCode`、`orderType`、`beType`、`items[{productCode, quantity}]`。
- 出参：`price`（现价，分）、`originalPrice`、`discount`、`productList[]`、`takeWayList[]`。
- 用途：下单前确认总价；展示时 ÷100 转元。

## create-order — 下单
- 入参：`storeCode`、`orderType`、`beType`、`items`、`takeWay` 等。
- 用途：用户明确要下单时使用，需先经 calculate-price 确认。
