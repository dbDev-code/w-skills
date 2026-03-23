---
name: openclaw-erp-tester
description: ERP订单测试接口调用技能。用于调用 DiBinAdmin 的 ERP 测试订单接口，验证参数格式和测试订单数据。当需要测试 ERP 订单接口、提交订单测试数据、验证 API 响应时使用此技能。
---

# OpenClaw ERP Tester

调用 DiBinAdmin ERP 测试订单接口的技能。

## 服务地址

- **Base URL**: `http://192.168.31.114:8105`
- **完整接口地址**: `http://192.168.31.114:8105/erp/test/post_TestOrder`

## 接口信息

- **Method**: POST
- **Endpoint**: `/erp/test/post_TestOrder`
- **Content-Type**: `application/json`

## 快速调用

### 最小请求（仅必填参数）

```json
{
  "order_no": "ORD001"
}
```

### 完整请求示例

```json
{
  "order_no": "ORD20260317001",
  "customer_name": "测试客户A",
  "product_code": "PRD-001",
  "quantity": 100,
  "unit_price": 25.50,
  "total_amount": 2550.00,
  "order_date": "2026-03-17",
  "warehouse": "上海仓",
  "operator": "张三",
  "remark": "测试订单"
}
```

## 响应格式

```json
{
  "code": 0,
  "message": "ERP接口调用成功",
  "data": {
    "order": { ... },
    "processed_at": "2026-03-17 10:30:00",
    "status": "received",
    "note": "数据已校验通过"
  }
}
```

- `code: 0` 表示成功
- `code: 1` 表示失败（参数错误等）

## 参数说明

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| order_no | string | ✓ | 订单号 |
| customer_name | string | | 客户名称 |
| product_code | string | 产品编码 |
| quantity | int | 数量 |
| unit_price | float | 单价 |
| total_amount | float | 总金额（可自动计算） |
| order_date | string | 订单日期 |
| warehouse | string | 仓库 |
| operator | string | 操作员 |
| remark | string | 备注 |

**自动计算**: 若未传 `total_amount` 且 `quantity > 0` 且 `unit_price > 0`，则自动计算 `total_amount = quantity * unit_price`

## 详细文档

完整参数说明和测试场景见 [references/api-reference.md](references/api-reference.md)
