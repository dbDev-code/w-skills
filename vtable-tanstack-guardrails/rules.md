---
name: vtable-tanstack-guardrails
description: VTable + TanStack 表格开发规范与性能护栏，适用于 tableDemo/js/vtable.js 及相关高性能表格开发。涉及表格排序/过滤/选择/虚拟滚动时自动触发。
alwaysApply: true
---

# VTable TanStack Guardrails 规则

本规则为 vtable-tanstack-guardrails 技能的配套项目规则，确保所有表格开发遵循架构边界和性能约束。

## 架构边界（强制）

| 职责 | 必须归属 |
|------|----------|
| 排序、过滤、行选择、列固定、列宽、列序、列可见性 | `@tanstack/table-core` |
| 行/列虚拟化、范围计算、偏移量、overscan、scroll-to | `@tanstack/virtual-core` |
| DOM 池化、节点复用、事件代理、渲染适配器 | `VTable` |

**禁止在 VTable 层重新实现上述库已有功能。**

## 性能红线（禁止）

- ❌ **scroll 热路径禁止创建新数组/Map/闭包**
- ❌ **scroll 时禁止重建 header 或 body HTML**
- ❌ **禁止全量 rerender**（选择、hover、小状态变更必须定向 patch）
- ❌ **禁止手动实现排序/过滤逻辑**（使用 TanStack 扩展点）
- ❌ **禁止手动计算虚拟范围/偏移量**（使用 virtual-core API）
- ❌ **禁止 scroll handler 中手动计算虚拟范围**
- ❌ **禁止 DOM 缓存字段成为业务状态第二数据源**
- ❌ **禁止 renderRowCells 中对每一可见行遍历所有列**

## 渲染性能要求

- ✅ **渲染成本公式**：`visibleRows × visibleColumns`，禁止 `visibleRows × allColumns`
- ✅ **DOM 读写分离**：读取和写入分开操作
- ✅ **帧边界批处理**：使用 `requestAnimationFrame` 批量写入
- ✅ **滚动路径零分配**：在热路径中复用稳定的数据结构
- ✅ **定向更新**：选择/排序/过滤只 patch 受影响的行/窗口

## 大型数据集强制要求

- ✅ **双向虚拟化**：行虚拟化 + 列虚拟化必须同时实现
- ✅ **渲染范围**：`visibleRows × visibleColumns + overscan`
- ✅ **固定列对齐**：固定列必须与 TanStack 状态和虚拟范围保持同步
- ✅ **基准测试场景**：10000+ 行 ERP 数据集作为验收标准
- ✅ **宽表支持**：电子组件等宽 Schema 场景必须可用的列虚拟化

## 中文编码保护

- ✅ 编辑含中文文件前检测并保留当前编码策略和换行符风格
- ✅ 编辑后验证所有中文注释、文档块、UI 字符串显示正常
- ✅ 如发现 `???`、乱码或 mojibake，立即回滚并使用更安全的编辑方式
- ✅ 不主动将中文注释翻译为英文，除非用户明确要求

## 代码质量检查清单

### 每次提交前自检

1. [ ] 表格语义是否全部映射到 `@tanstack/table-core` 或其扩展点？
2. [ ] 虚拟化和范围计算是否全部映射到 `@tanstack/virtual-core`？
3. [ ] VTable 层是否保持了 thin adapter 角色，未成为第二表格引擎？
4. [ ] scroll 热路径是否实现了零分配？
5. [ ] 渲染成本是否基于可见区域而非数据集总量？
6. [ ] 是否实现了双向虚拟化？
7. [ ] scroll 时是否避免了全量 header/body 重建？
8. [ ] 选择/排序/过滤变更是否只 patch 了受影响区域？
9. [ ] 是否避免了循环中的不必要分配？
10. [ ] DOM 读写是否分离或批处理？
11. [ ] 如涉及中文内容，编辑后是否验证了可读性？

### 触发词

以下关键词自动触发本规则检查：
- `vtable`、`tableDemo`、`js/vtable.js`
- 表格排序、表格过滤、表格选择、表格固定、列虚拟化
- `@tanstack/table-core`、`@tanstack/virtual-core` 配置
- `renderBody`、`renderRowCells`、`initVirtualScroll`
