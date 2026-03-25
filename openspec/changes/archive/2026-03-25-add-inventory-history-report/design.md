## Context

项目是一个 Django 4.2 库存管理系统，已有完整的报表中心（7个报表），使用 `ReportService` 提供数据、`views_report.py` 处理请求、`reports/` 下的模板渲染页面。前端用 Bootstrap 5 + Chart.js。

数据源为现有的 `InventoryTransaction` 模型，包含 `product`(FK)、`transaction_type`(IN/OUT/ADJUST)、`quantity`、`operator`(FK)、`notes`、`created_at` 字段。无需数据库变更。

## Goals / Non-Goals

**Goals:**
- 提供出入库数据的聚合分析（汇总、趋势图、按商品/操作员统计）
- 提供出入库明细的灵活查询（按日期/类型/关键字筛选、分页）
- 支持 Excel 导出明细数据
- 融入现有报表中心，保持一致的 UI 风格和权限体系

**Non-Goals:**
- 不修改 `InventoryTransaction` 模型或添加新字段
- 不新建数据库迁移
- 不实现实时数据刷新（AJAX/WebSocket）
- 不改造现有的 `/inventory/transactions/` 页面

## Decisions

### 1. 复用现有 Service/View/Template 分层模式
**选择**: 遵循项目既有的 `ReportService` + `views_report` + `reports/*.html` 模式
**原因**: 保持代码一致性，降低维护成本。其他方案如 CBV 或独立 app 会增加不必要的复杂度。

### 2. 使用已有 `DateRangeForm` 作为基类
**选择**: 继承 `DateRangeForm` 创建 `InventoryHistoryForm`
**原因**: `DateRangeForm` 已实现预设日期范围、缓存等功能，直接复用。

### 3. Tab 切换用 Bootstrap Nav-tabs（前端切换）
**选择**: 用 Bootstrap 5 的 nav-tabs 实现聚合分析/履历明细的 Tab 切换
**原因**: 数据在同一次请求中获取，前端切换即可，无需 AJAX，简单高效。
**备选**: 使用 AJAX 按需加载 Tab 数据 — 数据量不大时过度设计。

### 4. 分页使用 Django Paginator
**选择**: 对明细列表使用 `Paginator`，每页 20 条
**原因**: 与现有 `inventory_transaction_list` 保持一致。

### 5. 聚合查询使用 Django ORM annotate
**选择**: 利用 `TruncDay` + `annotate` + `Case/When` 做按日/按类型的聚合
**原因**: 避免 raw SQL，保持 ORM 风格统一，SQLite 兼容。

## Risks / Trade-offs

- **大数据量性能**: 如果 `InventoryTransaction` 记录很多，聚合查询可能变慢 → 可通过日期范围限制和缓存缓解
- **Chart.js CDN 依赖**: 图表依赖外部 CDN → 项目已有此模式，风险已知可接受
- **两个 Tab 数据一次加载**: 如果数据量大，首次加载可能较慢 → 当前阶段可接受，后续可优化为 AJAX 懒加载
