---
tags: [cimgui, api, layout]
aliases: [布局, 光标, 滚动]
---

# API 布局与滚动

- 相关:[[10 API 索引]] · [[12 API 窗口与视图]] · [[14 API 样式与字体]]

本分类 34 个函数:光标定位（21）+ 滚动（13）。

## 光标/布局

| C 函数 | 说明 |
|---|---|
| `igSameLine` | 上一个控件后同行继续（可带偏移/间距） |
| `igNewLine` / `igSpacing` / `igDummy` | 垂直推进：固定行距 / 半个行距 / 空占位 |
| `igIndent` / `igUnindent` | 增加/减少缩进 |
| `igSeparator` / `igSeparatorText` | 分隔线（`igSeparatorText` 带居中文本） |
| `igAlignTextToFramePadding` | 文本与帧垂直对齐 |
| `igGetCursorPos` / `igGetCursorPosX/Y` | 相对窗口内容的游标 |
| `igSetCursorPos` / `igSetCursorPosX/Y` | 设置相对游标 |
| `igGetCursorScreenPos` | 屏幕坐标游标 |
| `igSetCursorScreenPos` | 设置屏幕坐标游标 |
| `igGetCursorStartPos` | 窗口内容起点（展开前） |
| `igGetContentRegionAvail` | 当前行剩余可用区域 |

> `GetCursorScreenPos` 常用于 `IGG::ImGuiCursorScreenPos` 配合 `PushClipRect`/绘制。

## 滚动

| C 函数 | 说明 |
|---|---|
| `igGetScrollX` / `igGetScrollY` | 当前滚动量 |
| `igGetScrollMaxX/Y` | 可滚动上限 |
| `igSetScrollX` / `igSetScrollY` | 直接设置（可用 `ImGuiScrollFlags_KeepVisibleEdgeAligned` 类修饰） |
| `igSetScrollHereX/Y` | 把当前光标项滚到可见（旧版用，新用 Item 自动滚动） |
| `igSetScrollFromPosX/Y` | 把指定绝对位置滚到可见 |
| `igScrollToItem`（`_Ex` 变体）`igScrollToRect` / `igScrollToRectEx` | 主动滚到某项/某矩形（internal，见 [[26 API 内部函数]]） |

## 典型小抄

```c
igSetNextItemWidth(150.0f);          // 下面一个控件限定宽度
igSameLine(0.0f, 8.0f);              // 与上一控件并排
igSeparatorText("区间");             // 带标题分隔线
igDummy((ImVec2){0, 4});             // 空白间隔
```

## 相关

- 宽度/间距由 [[14 API 样式与字体]] 的 `ItemSpacing`、`FramePadding` 等 StyleVar 控制。
- 布局示例在 [[42 示例 SDL2 OpenGL3]] 的窗口内容区。