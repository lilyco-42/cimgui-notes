---
tags: [cimgui, api, moc]
aliases: [API索引, API 索引]
---

# API 索引

- 相关:[[00 Home]] · [[04 生成器机制]] · [[27 数据结构与枚举]] · [[05 Codegraph 索引]]

> 顶层自由函数共 **1011** 个基名；其中 imgui 公开 API **391** 个（location=imgui.h），imgui_internal 约 **643** 个（归类至 [[26 API 内部函数]]）。下表按主题分类（合计数 535 个已归类成员；其余内部/辅助函数见 [[26 API 内部函数]]）。

## 分类总表（点击进入对应笔记）

| 分类 | 函数数 | 笔记 |
|---|---|---|
| Context 与主循环（上下文、帧循环、渲染） | 38 | [[11 API 上下文与主循环]] |
| 窗口 Begin/End（含 Popup/Combo/Tooltip 的 Begin 系列） | 75 | [[12 API 窗口与视图]] |
| 窗口 SetNext/Set | 15 | [[12 API 窗口与视图]] |
| 光标/布局（Cursor/SameLine/Separator） | 21 | [[13 API 布局与滚动]] |
| 滚动（Get/SetScroll） | 13 | [[13 API 布局与滚动]] |
| Style 样式 | 14 | [[14 API 样式与字体]] |
| 字体（Font push/calc/get） | 9 | [[14 API 样式与字体]] |
| 参数栈（ItemWidth/ItemFlag/ClipRect/TextWrap） | 10 | [[14 API 样式与字体]] |
| ID 栈 | 6 | [[25 API 杂项与调试]] |
| 基础控件（Button/Checkbox/Image/Progress） | 16 | [[15 API 基础控件]] |
| 文本输出（Text*/Label*/Value） | 26 | [[16 API 文本控件]] |
| 拖拽 Drag | 12 | [[17 API 数值与颜色控件]] |
| 滑块 Slider（含 VSlider） | 14 | [[17 API 数值与颜色控件]] |
| 输入 Input（Int/Float/Scalar/Text） | 16 | [[17 API 数值与颜色控件]] |
| 颜色（Edit/Picker/Convert） | 9 | [[17 API 数值与颜色控件]] |
| Plot 绘图（旧） | 2 | [[17 API 数值与颜色控件]] |
| Combo/列表 | 2 | [[18 API 列表与树]] |
| Selectable/树（TreeNode*/CollapsingHeader） | 19 | [[18 API 列表与树]] |
| 菜单（MenuItem/Shortcut） | 3 | [[19 API 菜单与弹出]] |
| Tooltip（Set/SetItem） | 6 | [[19 API 菜单与弹出]] |
| Popup | 7 | [[19 API 菜单与弹出]] |
| 表格 Table | 18 | [[20 API 表格与TabBar]] |
| TabBar | 4 | [[20 API 表格与TabBar]] |
| 旧 Columns | 9 | [[20 API 表格与TabBar]] |
| Docking（DockSpace+DockBuilder） | 17 | [[21 API Docking]] |
| 绘制 DrawList（igRender* 系列 + DrawList 方法） | — | [[22 API 绘制与DrawList]] |
| IO/输入（Key/Mouse 查询） | 26 | [[23 API IO与输入]] |
| Viewport/平台 | 5 | [[23 API IO与输入]] |
| 交互状态 IsItem/IsAny | 25 | [[24 API 交互与拖放]] |
| 拖放 Drag&Drop | 5 | [[24 API 交互与拖放]] |
| 多选 MultiSelect | 1 | [[24 API 交互与拖放]] |
| Ini 设置 | 6 | [[25 API 杂项与调试]] |
| 日志/堆栈（LogButtons/LogTo*） | 9 | [[25 API 杂项与调试]] |
| 剪贴板 | 2 | [[25 API 杂项与调试]] |
| 内存/工具（MemAlloc/SetAllocator） | 7 | [[25 API 杂项与调试]] |
| Demo/调试工具（Show*/Debug*） | 31 | [[25 API 杂项与调试]] |
| 数学/辅助（Im*） | 34 | [[25 API 杂项与调试]] |
| 导航 Nav | 3 | [[26 API 内部函数]] |
| 内部/未归类 | 476 | [[26 API 内部函数]] |

> 行号来源:`generator/output/definitions.json` 的 `location`（如 `imgui:3153` 即 `imgui.cpp:3153`）；符号级检索用 `codegraph explore`（[[05 Codegraph 索引]]）。

## 命名速查

- 前缀 `ig` = ImGui 自由函数;`ImGui_Impl*` = 后端(见 [[31 cimgui_impl 封装层]])。
- 后缀 `_Str` / `_Nil` / `V` / `0` / `Ex`，含义见 [[04 生成器机制]]。
- 结构体/枚举清单见 [[27 数据结构与枚举]]。