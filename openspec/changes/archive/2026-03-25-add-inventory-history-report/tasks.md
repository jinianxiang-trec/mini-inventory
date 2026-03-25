## 1. Service 层

- [x] 1.1 在 `report_service.py` 中添加 `get_inventory_history` 静态方法，实现汇总统计（总入库/出库/调整/净变化/交易笔数）
- [x] 1.2 在 `get_inventory_history` 中实现按日聚合趋势数据（使用 TruncDay + Case/When 分别统计入库和出库数量）
- [x] 1.3 在 `get_inventory_history` 中实现按商品汇总统计
- [x] 1.4 在 `get_inventory_history` 中实现按操作员汇总统计
- [x] 1.5 在 `get_inventory_history` 中实现明细查询（支持 transaction_type、search 筛选）

## 2. Form 层

- [x] 2.1 在 `report_forms.py` 中创建 `InventoryHistoryForm(DateRangeForm)`，添加 `transaction_type` 下拉和 `search` 文本框
- [x] 2.2 在 `forms.py` 桥接文件中添加 `InventoryHistoryForm` 的 import

## 3. View 层

- [x] 3.1 在 `views_report.py` 中创建 `inventory_history_report` 视图函数，处理 GET/POST 请求
- [x] 3.2 在视图中集成 Excel 导出功能（检测 `export_excel` in POST）

## 4. Template 层

- [x] 4.1 创建 `reports/inventory_history.html` 基础框架（继承 base.html，筛选表单）
- [x] 4.2 实现聚合分析 Tab（汇总卡片 + Chart.js 趋势图 + 商品汇总表 + 操作员饼图）
- [x] 4.3 实现履历明细 Tab（交易流水表 + 分页）

## 5. 路由与入口

- [x] 5.1 在 `urls.py` 中添加 `reports/inventory-history/` 路由
- [x] 5.2 在 `reports/index.html` 中添加出入库履历报表入口卡片

## 6. Excel 导出

- [x] 6.1 在 `export_service.py` 中添加 `export_inventory_history` 方法

## 7. 验证

- [x] 7.1 浏览器访问报表中心确认新入口卡片显示
- [x] 7.2 进入报表页面验证 Tab 切换、筛选、分页、图表渲染
- [x] 7.3 测试 Excel 导出功能
