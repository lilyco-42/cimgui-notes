---
tags: [cimgui, api, menu, tooltip, popup]
aliases: [菜单, 提示, 弹出]
---

# API 菜单与弹出

- 相关:[[10 API 索引]] · [[12 API 窗口与视图]] · [[24 API 交互与拖放]]

本分类 16 个函数:菜单（3）+ Tooltip（6）+ Popup（7）。

## 菜单

| C 函数 | 说明 |
|---|---|
| `igBeginMenuBar` / `igEndMenuBar` | 窗口内菜单栏（需 `ImGuiWindowFlags_MenuBar`） |
| `igBeginMainMenuBar` / `igEndMainMenuBar` | 全局主菜单栏 |
| `igBeginMenu` / `igEndMenu` | 子菜单 |
| `igMenuItem` | 菜单项（可勾选/快捷键）最快方式 |
| `igShortcut` | 显式快捷键提示 |
| `igMenuItemEx` | 内部细节版 |

```c
if (igBeginMainMenuBar()) {
    if (igBeginMenu("文件", true)) {
        if (igMenuItem("退出", "Ctrl+Q", false, true)) { running = false; }
        igEndMenu();
    }
    igEndMainMenuBar();
}
```

## Tooltip

| C 函数 | 说明 |
|---|---|
| `igSetTooltip` | 悬停上一项时立即显示 |
| `igSetItemTooltip` | 同 `igSetTooltip`（新推荐命名） |
| `igBeginTooltip` / `igEndTooltip` | 多行/富文本提示块 |
| `igBeginItemTooltip` / `igEndItemTooltip` | 上一项的多行提示（新推荐） |

```c
igButton("Hover", (ImVec2){0,0});
igSetItemTooltip("这是提示内容");
igBeginTooltip();
igText("第一行"); igSeparator(); igText("第二行");
igEndTooltip();
```

## Popup

| C 函数 | 说明 |
|---|---|
| `igOpenPopup` | 按标识名打开（可带 id） |
| `igOpenPopupOnItemClick` | 上一项点击弹出（右键/左键可选） |
| `igCloseCurrentPopup` | 在当前 popup 内关闭 |
| `igBeginPopup` / `igEndPopup` | 手动管理 popup 内容块 |
| `igBeginPopupContextItem` / `igBeginPopupContextWindow` / `igBeginPopupContextVoid` | 带触发机制:右键菜单（最常用） |
| `igBeginPopupModal` | 模态对话框（带遮罩、Esc/点击外部不关） |
| `igIsPopupOpen` | 查询 popup 状态 |

```c
if (igBeginPopupContextItem("ctx", ImGuiPopupFlags_MouseButtonRight)) {
    igMenuItem("剪切"); igMenuItem("复制"); igMenuItem("粘贴");
    igEndPopup();
}
```

> 注意:Begin/End 严格配对（与 [[12 API 窗口与视图]] 同规则）；popup 也可经 `igOpenPopup` + `igBeginPopup` 分离触发与内容。

## 相关

- 菜单/Popup 内 widget 交互状态见 [[24 API 交互与拖放]]；隐藏窗口可用 `igSetNextWindowViewport`（[[12 API 窗口与视图]]）。