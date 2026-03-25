## Why

报表中心缺少出入库履历的查询和分析功能。目前虽有 `/inventory/transactions/` 页面展示交易流水，但位于库存管理模块内，缺乏聚合分析能力（如趋势图、按商品/操作员汇总），也不支持 Excel 导出。用户需要一个集明细查询与数据分析于一体的出入库报表。

## What Changes

- 新增「出入库履历报表」页面，包含两个 Tab：聚合分析 + 履历明细
- 聚合分析 Tab：汇总面板(总入库/出库/净变化)、按日趋势图、按商品汇总表、按操作员统计饼图
- 履历明细 Tab：可筛选(日期/类型/商品关键字)、分页的交易流水列表
- 支持 Excel 导出出入库明细数据
- 报表中心首页新增入口卡片

## Capabilities

### New Capabilities
- `inventory-history-analysis`: 出入库履历的聚合分析（汇总面板、趋势图、按商品/操作员统计）
- `inventory-history-detail`: 出入库履历明细查询（筛选、分页、Excel 导出）

### Modified Capabilities
_(无)_

## Impact

- **Service 层**: `report_service.py` 新增 `get_inventory_history` 方法
- **Form 层**: `report_forms.py` 新增 `InventoryHistoryForm`
- **View 层**: `views_report.py` 新增 `inventory_history_report` 视图
- **Template 层**: 新增 `reports/inventory_history.html`
- **URL 路由**: `urls.py` 添加 `/reports/inventory-history/`
- **导出**: `export_service.py` 新增导出方法
- **数据源**: 复用现有 `InventoryTransaction` 模型，无数据库变更
