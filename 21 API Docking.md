---
tags: [cimgui, api, docking]
aliases: [Docking, 停靠]
---

# API Docking

- 相关:[[10 API 索引]] · [[12 API 窗口与视图]] · [[23 API IO与输入]] · [[50 调用链专题]]

本分类 17 个函数:运行时 DockSpace（5）+ DockBuilder（12）。**仅对 docking 分支生效**（本克隆正是 `docking_inter`）。

> 需要 `ImGuiConfigFlags_DockingEnable`（见 [[23 API IO与输入]] 的 ConfigFlags）。

## 运行时 DockSpace

| C 函数 | 说明 |
|---|---|
| `igDockSpace` | 在窗口内铺开一个停靠区（dockspace id 常用窗口 id） |
| `igDockSpaceOverViewport` | 覆盖整个主 viewport 的全局停靠区（常见入口） |
| `igIsWindowDocked` | 当前窗口是否已停靠 |
| `igGetWindowDockID` | 当前窗口所在 dock 节点 ID |
| `igSetNextWindowDockID` | 下单帧窗口停到指定节点 |

```c
igDockSpaceOverViewport(0, 0, NULL);  // 主循环里每帧调用一次
igSetNextWindowDockID(dock_id, ImGuiCond_Appearing);   // 拆分后归位到某节点
igBegin("工具", NULL, 0); ... igEnd();
```

## DockBuilder（一次性布局）

| C 函数 | 说明 |
|---|---|
| `igDockBuilderGetCentralNode` | 取根节点的中央子节点（常用于"全屏垫底窗口"） |
| `igDockBuilderSplitNode` | 把节点按方向拆两半（返回新生成的节点 ID） |
| `igDockBuilderDockWindow` | 把窗口停到指定节点 |
| `igDockBuilderGetNode` | 查节点（内部） |
| `igDockBuilderAddNode` / `igDockBuilderRemoveNode` | 增删节点 |
| `igDockBuilderRemoveNodeChildNodes` / `igDockBuilderRemoveNodeDockedWindows` | 递归清理子节点/停靠窗口 |
| `igDockBuilderSetNodePos` / `igDockBuilderSetNodeSize` | 节点位置/尺寸 |
| `igDockBuilderFinish` | 结束布局（生成 Layout 存于 DockSpace state） |
| `igDockBuilderCopyNode` / `igDockBuilderCopyDockSpace` / `igDockBuilderCopyWindowSettings` | 复制布局/窗口设置（预设切换） |

典型"三分屏"初始化（在第一次显示主窗口时调用一次）:

```c
ImGuiID root = igGetID("MyDockSpace");
igDockBuilderRemoveNode(root);
igDockBuilderAddNode(root, ImGuiDockNodeFlags_None);
igDockBuilderSetNodeSize(root, (ImVec2){1280, 800});
ImGuiID left  = igDockBuilderSplitNode(root, ImGuiDir_Left,  0.25f, NULL, &right);
ImGuiID right = igDockBuilderSplitNode(right, ImGuiDir_Right, 0.30f, NULL, &right_bottom);
igDockBuilderDockWindow("场景", right);
igDockBuilderDockWindow("资源", left);
igDockBuilderDockWindow("属性", right_bottom);
igDockBuilderFinish(root);
```

## 标志/枚举

- `ImGuiDockNodeFlags_None / DockSpace / SplitterResize / PassthruCentralNode / NoDockingSplit / AutoHideTabBar` 等。
- `ImGuiDir_*`（Left/Right/Up/Down）用于 SplitNode。
- `ImGuiCond_Appearing` 让 SetNextWindowDockID 只生效一次。

## 相关

- 每窗口是否可停靠由 `ImGuiWindowFlags_NoDocking` 等控制（[[12 API 窗口与视图]]）。
- 窗口状态保存（Docking 布局持久化）由 [[25 API 杂项与调试]] 的 Ini 系统完成。
- 后台标签栏显示见 [[20 API 表格与TabBar]]。