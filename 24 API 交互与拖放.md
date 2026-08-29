---
tags: [cimgui, api, interaction]
aliases: [交互, IsItem, 拖放, MultiSelect]
---

# API 交互与拖放

- 相关:[[10 API 索引]] · [[23 API IO与输入]] · [[15 API 基础控件]]

本分类 31 个函数:交互状态（25）+ 拖放（5）+ 多选（1）。

## 上一项状态（Is* 家族,紧跟控件之后调用）

| C 函数 | 说明 |
|---|---|
| `igIsItemHovered` | 上一项是否被悬停（`ImGuiHoveredFlags_*` 可带:AllowWhenDisabled/ChildWindows/DelayNormal 等） |
| `igIsItemActive` / `igIsItemActivated` / `igIsItemDeactivated` / `igIsItemDeactivatedAfterEdit` | 激活/刚激活/刚失活/编辑后失活 |
| `igIsItemClicked` | 上一项被点击（带 MouseButton） |
| `igIsItemEdited` / `igIsItemFocused` / `igIsItemVisible` / `igIsItemToggledOpen` | 被修改/聚焦/可见/开合 |
| `igIsAnyItemHovered` / `igIsAnyItemActive` / `igIsAnyItemFocused` | 任意项级查询 |
| `igGetItemID` / `igGetItemFlags` / `igGetItemStatusFlags` / `igGetItemRectMin` / `igGetItemRectMax` / `igGetItemRectSize` | 数据查询 |
| `igKeepAliveID` / `igFocusItem` / `igSetItemKeyOwner` | 聚焦/按住控制 |
| `igSetKeyboardFocusHere` | 键盘焦点定位（[[23 API IO与输入]]） |

```c
igButton("Save", (ImVec2){0, 0});
if (igIsItemHovered())             igSetTooltip("保存当前文件");
if (igIsItemClicked(ImGuiMouseButton_Left)) doSave();
```

## 拖放（Drag & Drop）

| C 函数 | 说明 |
|---|---|
| `igBeginDragDropSource` / `igEndDragDropSource` | 拖拽源（配合 `IMGUI_PAYLOAD_TYPE_COLOR_4F` 等 payload） |
| `igSetDragDropPayload` | 在源内注册 payload（类型字符串 + 数据） |
| `igBeginDragDropTarget` / `igEndDragDropTarget` | 接收目标 |
| `igAcceptDragDropPayload` | 目标内接收 payload（返回 payload 指针） |
| `igGetDragDropPayload` | 读回数据 |
| `igIsDragDropActive` / `igIsDragDropPayloadBeingAccepted` | 状态查询 |

```c
if (igBeginDragDropSource(ImGuiDragDropFlags_None)) {
    igSetDragDropPayload("MY_ITEM", &id, sizeof(int), ImGuiCond_Once);
    igText("拖走 %d", id);
    igEndDragDropSource();
}
// 目标窗口:
if (igBeginDragDropTarget()) {
    const ImGuiPayload* p = igAcceptDragDropPayload("MY_ITEM", ImGuiDragDropFlags_None);
    if (p) { memcpy(&dropped, p->Data, sizeof(int)); }
    igEndDragDropTarget();
}
```

## 多选 MultiSelect

| C 函数 | 说明 |
|---|---|
| `igSetNextItemSelectionUserData` | 配合 `igBeginMultiSelect`（[[23 API IO与输入]] 或 [[26 API 内部函数]]）:指定下一 item 在用户数据里的 id，用于多选逻辑 |
| `igBeginMultiSelect` / `igEndMultiSelect` | 多选控件容器（internal 包装，见 [[26 API 内部函数]]） |

## 相关

- hovered 标志全集（窗口/控件/延迟）见 [[27 数据结构与枚举]] `ImGuiHoveredFlags_*`、`ImGuiDragDropFlags_*`。