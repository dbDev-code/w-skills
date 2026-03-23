# API Reference

## 目录

1. [服务地址](#服务地址)
2. [参数详细说明](#参数详细说明)
3. [数据类型约束](#数据类型约束)
4. [响应码说明](#响应码说明)
5. [测试场景](#测试场景)
6. [错误处理](#错误处理)

---

## 服务地址

| 配置项 | 值 |
|--------|-----|
| Base URL | `http://192.168.31.114:8105` |
| 接口路径 | `/erp/test/post_TestOrder` |
| 完整地址 | `http://192.168.31.114:8105/erp/test/post_TestOrder` |
| 请求方式 | POST |
| Content-Type | application/json |

### curl 调用示例

```bash
curl -X POST http://192.168.31.114:8105/erp/test/post_TestOrder \
  -H "Content-Type: application/json" \
  -d '{"order_no": "ORD001"}'
```

---

## 参数详细说明

### order_no (订单号)

- **类型**: string
- **必填**: 是
- **说明**: 唯一标识订单的编号
- **示例**: `ORD20260317001`

### customer_name (客户名称)

- **类型**: string
- **必填**: 否
- **说明**: 下单客户的公司或个人名称
- **示例**: `上海科技有限公司`

### product_code (产品编码)

- **类型**: string
- **必填**: 否
- **说明**: 产品的唯一编码，用于库存和销售管理
- **示例**: `PRD-001`, `SKU-2026-12345`

### quantity (数量)

- **类型**: int
- **必填**: 否
- **说明**: 订购数量，通常为正整数
- **约束**: 建议范围 1 - 999999
- **示例**: `100`, `500`

### unit_price (单价)

- **类型**: float
- **必填**: 否
- **说明**: 单个产品的价格
- **精度**: 支持两位小数
- **示例**: `25.50`, `199.99`

### total_amount (总金额)

- **类型**: float
- **必填**: 否
- **说明**: 订单总金额
- **自动计算**: 若未传且 `quantity > 0` 且 `unit_price > 0`，自动计算为 `quantity * unit_price`
- **示例**: `2550.00`

### order_date (订单日期)

- **类型**: string
- **必填**: 否
- **格式**: `YYYY-MM-DD` 或 `YYYY-MM-DD HH:mm:ss`
- **示例**: `2026-03-17`, `2026-03-17 14:30:00`

### warehouse (仓库)

- **类型**: string
- **必填**: 否
- **说明**: 发货仓库标识
- **示例**: `上海仓`, `WH-SH-001`

### operator (操作员)

- **类型**: string
- **必填**: 否
- **说明**: 创建订单的操作人员姓名或工号
- **示例**: `张三`, `EMP001`

### remark (备注)

- **类型**: string
- **必填**: 否
- **说明**: 订单附加说明
- **示例**: `加急处理`, `客户要求发票`

---

## 数据类型约束

| 字段 | 类型 | 最小值 | 最大值 | 精度 |
|------|------|--------|--------|------|
| quantity | int | - | - | 整数 |
| unit_price | float | 0 | - | 2位小数 |
| total_amount | float | 0 | - | 2位小数 |
| order_no | string | 1字符 | 无限制 | - |

---

## 响应码说明

### 成功响应

```json
{
  "code": 0,
  "message": "ERP测试接口调用成功",
  "data": {
    "order": {
      "order_no": "ORD001",
      "customer_name": "客户A",
      ...
    },
    "processed_at": "2026-03-17 10:30:00",
    "status": "received",
    "note": "数据已校验通过，未持久化存储"
  },
  "time": 1710655800
}
```

### 失败响应

```json
{
  "code": 1,
  "message": "参数错误",
  "data": "Key: 'ErpTestOrder.OrderNo' Error:Field validation for 'OrderNo' failed on the 'required' tag",
  "time": 1710655800
}
```

| code | 含义 |
|------|------|
| 0 | 成功 |
| 1 | 失败（参数错误等） |

---

## 测试场景

### 场景1: 最小有效请求

测试仅提供必填参数的情况：

```json
{
  "order_no": "TEST001"
}
```

**预期**: 返回 code: 0，order 中仅 order_no 有值

### 场景2: 自动计算总金额

测试自动计算功能：

```json
{
  "order_no": "TEST002",
  "quantity": 10,
  "unit_price": 100
}
```

**预期**: 返回的 order.total_amount = 1000

### 场景3: 完整订单数据

测试所有字段：

```json
{
  "order_no": "TEST003",
  "customer_name": "测试客户",
  "product_code": "PRD-001",
  "quantity": 50,
  "unit_price": 29.99,
  "total_amount": 1499.50,
  "order_date": "2026-03-17",
  "warehouse": "上海仓",
  "operator": "系统测试",
  "remark": "完整字段测试"
}
```

### 场景4: 缺少必填参数

测试错误处理：

```json
{
  "customer_name": "测试客户"
}
```

**预期**: 返回 code: 1，提示 order_no 必填

### 场景5: 边界值测试

```json
{
  "order_no": "TEST005",
  "quantity": 0,
  "unit_price": 0
}
```

**预期**: 返回 code: 0，total_amount 保持为 0（不触发自动计算）

---

## 错误处理

常见错误及解决方法：

| 错误信息 | 原因 | 解决方法 |
|----------|------|----------|
| `Field validation for 'OrderNo' failed on the 'required' tag` | 未提供 order_no | 添加 order_no 字段 |
| `invalid character` | JSON 格式错误 | 检查 JSON 语法 |
| `parameter error` | 参数绑定失败 | 检查字段类型是否正确 |
