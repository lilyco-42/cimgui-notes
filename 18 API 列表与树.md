---
tags: [cimgui, api, combo, selectable, tree]
aliases: [下拉, 可选中, 树]
---

# API 列表与树

- 相关:[[10 API 索引]] · [[16 API 文本控件]] · [[20 API 表格与TabBar]] · [[24 API 交互与拖放]]

本分类 21 个函数:Combo/ListBox（2）+ Selectable/TreeNode（19）。

## Combo / ListBox（下拉与列表）

| C 函数 | 说明 |
|---|---|
| `igCombo` | 简单下拉（`const char* const* items, int items_count`） |
| `igListBox` | 简单列表选择 |

> 复杂组合（自绘 label、多列、tag）用 **Begin/End** 结构（下两行配合 `igBeginCombo`/`igBeginListBox`，见 [[12 API 窗口与视图]]）:

```c
if (igBeginCombo("主题", current, ImGuiComboFlags_None)) {
    for (int i = 0; i < 3; i++)
        igSelectable("选项", &selected[i], 0, (ImVec2){0,0});
    igEndCombo();
}
```

## Selectable

| C 函数 | 说明 |
|---|---|
| `igSelectable` | 可选中行（返回值 + `p_selected` 勾选态） |
| `igSetNextItemOpen` | 下一项默认展开/折叠（树用） |

## TreeNode 树

| C 函数 | 说明 |
|---|---|
| `igTreeNode` / `igTreeNodeEx` | 树节点（Ex 可带 `ImGuiTreeNodeFlags_*`） |
| `igTreePush` / `igTreePop` | 手动压栈树缩进 |
| `igCollapsingHeader` | 可折叠标题（= TreeNode 样式） |
| `igTreeNodeGetOpen` / `igTreeNodeSetOpen` | 查询/强制开合 |
| `igGetTreeNodeToLabelSpacing` | 节点标签与缩进的间距 |

常用 `ImGuiTreeNodeFlags_Framed / DefaultOpen / OpenOnArrow / OpenOnDoubleClick / Leaf / Bullet` 等（见 [[27 数据结构与枚举]]）。

```c
if (igTreeNodeEx("Node", ImGuiTreeNodeFlags_DefaultOpen)) {
    igText("子内容");
    igTreePop(); // 与 TreeNode 返回 true 时配对
}
```

## 相关

- 树内拖拽节点可使用 [[24 API 交互与拖放]] 的 BeginDragDropSource。
- 多功能列表可用 [[20 API 表格与TabBar]] 的 Table 实现网格布局。