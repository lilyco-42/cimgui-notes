---
tags: [cimgui, api, table, tabbar]
aliases: [表格, TabBar, Columns]
---

# API 表格与TabBar

- 相关:[[10 API 索引]] · [[18 API 列表与树]] · [[27 数据结构与枚举]]

本分类 31 个函数:Table（18）+ TabBar（4）+ 旧 Columns（9）。

## Table（现代表格,1.89+ 推荐）

流程:BeginTable → SetupColumn×N（或 SetupScrollFreeze）→ TableHeadersRow → TableNextRow/TableNextColumn … → EndTable。

| C 函数 | 说明 |
|---|---|
| `igBeginTable` / `igEndTable` | 表生命周期（外层标志如 `ImGuiTableFlags_Borders/Resizable/RowBg`） |
| `igTableSetupColumn` | 列定义（列名 + 宽度 + `ImGuiTableColumnFlags_*`） |
| `igTableSetupScrollFreeze` | 冻结前 N 列/M 行 |
| `igTableHeadersRow` / `igTableHeader` | 表头行（自动取列名或手动） |
| `igTableNextRow` / `igTableNextColumn` | 换行 / 移下一列 |
| `igTableSetColumnIndex` / `igTableSetColumnEnabled` | 跳到指定列 / 启停列 |
| `igTableGetColumnCount` / `igTableGetColumnIndex` / `igTableGetColumnName` | 查询 |
| `igTableGetColumnFlags` | 列标志（如当前列是否 hovered） |
| `igTableGetHoveredColumn` / `igTableGetRowIndex` | 悬停列 / 当前行 |
| `igTableSetBgColor` | 设置行/单元格背景 |
| `igTableGetSortSpecs` | 获取 `ImGuiTableSortSpecs*`（点表头排序回调） |
| `igTableAngledHeadersRow` | 斜角表头（AngledHeaders 标志配合） |

```c
if (igBeginTable("t", 3, ImGuiTableFlags_Borders | ImGuiTableFlags_RowBg, (ImVec2){0,0}, 0)) {
    igTableSetupColumn("编号"); igTableSetupColumn("名称"); igTableSetupColumn("状态");
    igTableHeadersRow();
    igTableNextRow();
    igTableSetColumnIndex(0); igText("1");
    igTableSetColumnIndex(1); igText("Alpha");
    igTableSetColumnIndex(2); igCheckbox("启用", &enabled);
    igEndTable();
}
```

## TabBar

| C 函数 | 说明 |
|---|---|
| `igBeginTabBar` / `igEndTabBar` | 页签容器（`ImGuiTabBarFlags_None/AutoSelectNewTabs`） |
| `igBeginTabItem` / `igEndTabItem` | 页签项（可 `p_open` 出关闭按钮） |
| `igTabItemButton` | 页签式按钮（非 TabItem） |
| `igSetTabItemClosed` | 手动关掉某 tab |
| `igTabItemCalcSize` / `igTabItemBackground` | 内部细节（advanced） |

```c
if (igBeginTabBar("Main")) {
    if (igBeginTabItem("一页", NULL, 0)) { igText("内容一"); igEndTabItem(); }
    if (igBeginTabItem("二页", NULL, 0)) { igText("内容二"); igEndTabItem(); }
    igEndTabBar();
}
```

## 旧 Columns（已被 Table 取代）

| C 函数 | 说明 |
|---|---|
| `igColumns` / `igNextColumn` | 旧式多列布局 |
| `igGetColumnIndex` / `igGetColumnOffset` / `igGetColumnWidth` / `igGetColumnsCount` | 查询 |
| `igSetColumnOffset` / `igSetColumnWidth` | 设置 |
| `igGetColumnOffsetFromNorm` | 规范化换算（advanced） |

> 新代码一律用 Table;Columns 仅兼容老项目。

## 相关

- 表格单元格内照样放控件/图片/菜单（[[15 API 基础控件]]、[[19 API 菜单与弹出]]、[[22 API 绘制与DrawList]]）。
- 排序需结合 `ImGuiTableSortSpecs` 与 `ImGuiTableSortSpecsSingle::SortUserData`（[[27 数据结构与枚举]]）。