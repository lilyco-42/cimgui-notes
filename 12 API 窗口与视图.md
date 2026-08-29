---
tags: [cimgui, api, window]
aliases: [窗口, Begin, 视图]
---

# API 窗口与视图

- 相关:[[10 API 索引]] · [[11 API 上下文与主循环]] · [[13 API 布局与滚动]] · [[21 API Docking]] · [[23 API IO与输入]]

本分类 90 个函数:窗口 Begin/End 全家族（75）+ 下一帧/设窗口属性（15）。

## Begin/End 家族（核心）

```c
igBegin(name, p_open, ImGuiWindowFlags_*) == 返回 bool（窗口是否可见）
igEnd();
```

- 配对律:Beg…End 必须严格配对；`igBegin` 返回 false 时**仍要调 `igEnd`**。
- `igBeginChild`（`_Str`/`_ID` 两变体）→ `igEndChild`:内嵌子窗口。
- `igBeginGroup` → `igEndGroup`:逻辑分组（自动边框、整体为一项）。
- `igBeginDisabled` → `igEndDisabled`:整段禁用交互（内部 `igPushItemFlag` 实现）。
- `igBeginMenuBar` / `igBeginMainMenuBar` → 对应 End:窗口内菜单栏 / 全局主菜单栏。
- `igBeginTooltip` / `igBeginItemTooltip` / `igBeginErrorTooltip` → End 系列（见 [[19 API 菜单与弹出]]）。
- `igBeginListBox` → `igEndListBox`:固定高度列表（见 [[18 API 列表与树]]）。
- `igBeginCombo` → `igEndCombo`（见 [[18 API 列表与树]]）。
- `igBeginPopup*` → `igEndPopup`（见 [[19 API 菜单与弹出]]）。
- `igBeginTabBar/TabItem/Table/MultiSelect` → 见 [[20 API 表格与TabBar]] / [[24 API 交互与拖放]]。
- `igBeginDragDropSource/Target` → [[24 API 交互与拖放]]。

## 窗口查询

| C 函数 | 返回 |
|---|---|
| `igGetWindowPos` / `igGetWindowSize` / `igGetWindowWidth` / `igGetWindowHeight` | 当前窗口几何 |
| `igGetWindowDrawList` / `igGetWindowViewport` / `igGetWindowDpiScale` | 当前窗口的绘制层 / viewport / DPI |
| `igIsWindowHovered` / `igIsWindowFocused` | 标志位参数（HoveredFlags 等） |
| `igIsWindowAppearing` / `igIsWindowCollapsed` / `igIsWindowDocked` | 状态查询 |
| `igGetWindowDockID` | 当前窗口 dock 节点 ID（[[21 API Docking]]） |

## SetNextWindow 一族（下一帧生效）

| C 函数 | 说明 |
|---|---|
| `igSetNextWindowPos` / `igSetNextWindowSize` | 位置/大小（可带 Cond） |
| `igSetNextWindowSizeConstraints` | 大小范围（副回调可选） |
| `igSetNextWindowCollapsed` / `igSetNextWindowFocus` / `igSetNextWindowBgAlpha` | 折叠/焦点/背景透明度 |
| `igSetNextWindowContentSize` | 内容建议尺寸 |
| `igSetNextWindowScroll` | 滚动偏移 |
| `igSetNextWindowViewport` | 目标 viewport（多显示器） |
| `igSetNextWindowClass` | 窗口类（进阶） |
| `igSetNextWindowDockID` | 指定 dock 目标（[[21 API Docking]]） |

## SetWindow 一族（立即/按可见名）

| C 函数 | 说明 |
|---|---|
| `igSetWindowPos` / `igSetWindowSize` / `igSetWindowCollapsed` / `igSetWindowFocus` | 作用于"当 前 窗 口"或给定 name 的窗口 |
| `igSetWindowViewport` | 归入某 viewport |

## 常用标志（见 [[27 数据结构与枚举]]）

- `ImGuiWindowFlags_NoTitleBar / NoResize / NoMove / NoCollapse / NoDocking / MenuBar / HorizontalScrollbar / UnsavedDocument / ChildWindow / Tooltip / Popup` 等。
- Begin 函数常组合 `.NoSavedSettings | .MenuBar`、`.AlwaysAutoResize`、`.NoScrollbar` 等。

## 示例内用法

每个示例几乎都有同样的开头:

```c
igSetNextWindowSize((ImVec2){400, 300}, ImGuiCond_Once);
igBegin("Demo Window", NULL, 0);
igText("...");
igEnd();
```

见 [[41 示例 主循环模式]] 与 [[42 示例 SDL2 OpenGL3]]。