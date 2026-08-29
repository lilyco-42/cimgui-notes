---
tags: [cimgui, api, io, input]
aliases: [IO, 输入, 键盘, 鼠标]
---

# API IO与输入

- 相关:[[10 API 索引]] · [[24 API 交互与拖放]] · [[27 数据结构与枚举]] · [[30 后端总览]]

本分类 31 个函数:IO/输入查询（26）+ Viewport/平台（5）。IO 值与键盘/鼠标/游戏手柄的**真值来自后端**（`ImGui_Impl*_NewFrame` 灌入）。

## ImGuiIO（igGetIO）

```c
ImGuiIO* io = igGetIO();   // 生产代码推荐用 igGetIO_Nil 宏
```

常用字段（全部可见可改，见 [[27 数据结构与枚举]]）:

| 字段 | 用途 |
|---|---|
| `io->ConfigFlags` | `ImGuiConfigFlags_NavEnableKeyboard / NavEnableGamepad / DockingEnable / ViewportsEnable / NoMouse` 等 |
| `io->Fonts` | `ImFontAtlas*`，见 [[14 API 样式与字体]] |
| `io->DisplaySize` / `DisplayFramebufferScale` | 每帧由应用更新 |
| `io->DeltaTime` | 帧间隔（后端填） |
| `io->MousePos` / `MouseDown` / `MouseWheel` | 鼠标状态 |
| `io->KeyMap` / `KeysDown` | 键状态（把平台 scancode 映射到 `ImGuiKey_*`） |
| `io->AddKeyEvent / AddMouseButtonEvent / AddMousePosEvent` | 事件式输入（新模式；示例用 SDL 事件调用） |
| `io->IniFilename` | Ini 自动保存文件名 |

## 键鼠查询

| C 函数 | 说明 |
|---|---|
| `igIsKeyDown` / `igIsKeyPressed` / `igIsKeyReleased` | 按态/按下沿/释放沿（可 `ImGuiKeyOwner_None`） |
| `igIsKeyChordPressed` | 组合键（如 Ctrl+O） |
| `igGetKeyName` | 键名文本 |
| `igGetKeyPressedAmount` / `igGetKeyData` | 重复计数 / 低级键数据 |
| `igIsMouseDown` / `igIsMouseClicked` / `igIsMouseDoubleClicked` / `igIsMouseReleased` | 鼠标按钮状态（`ImGuiMouseButton_Left/Right/Middle`） |
| `igIsMouseReleasedWithDelay` / `igIsMouseDragging` / `igIsMouseDragPastThreshold` | 拖拽检测 |
| `igGetMousePos` / `igGetMousePosOnOpeningCurrentPopup` / `igGetMouseDragDelta` | 位置/差值 |
| `igIsMouseHoveringRect` / `igIsMousePosValid` | 区域判断 |
| `igIsAnyMouseDown` | 是否有任意键按下 |
| `igResetMouseDragDelta` | 重置拖拽累计 |
| `igSetMouseCursor` | 设置鼠标光标样式（`ImGuiMouseCursor_*`） |
| `igSetNextFrameWantCaptureKeyboard` / `igSetNextFrameWantCaptureMouse` | 临时接管输入焦点 |

> 快捷键系统:Dear ImGui 1.86+ 全局快捷键推荐 `igShortcut(ImGuiKeyChord)`（见 [[19 API 菜单与弹出]]），比轮询 `IsKeyPressed` 更省事件。

## Focus/Nav

| C 函数 | 说明 |
|---|---|
| `igSetKeyboardFocusHere` | 下一项获得键盘焦点 |
| `igSetItemDefaultFocus` | 聚焦"列表第一项"等默认项 |

> 导航内部态查询（如 `igSetNavID`、`io->NavActive`）归 [[26 API 内部函数]]。

## Viewport / 平台

| C 函数 | 说明 |
|---|---|
| `igGetMainViewport` | 主窗口 viewport（`ImGuiViewport*`） |
| `igFindViewportByID` / `igFindViewportByPlatformHandle` | 按 ID / 平台句柄查找 |
| `igGetPlatformIO` | `ImGuiPlatformIO*`（平台回调表） |
| `igUpdatePlatformWindows` | 同步平台窗口（多窗口模式，需 `ViewportsEnable`） |
| `igDestroyPlatformWindows` | 销毁所有平台窗口 |

## ConfigFlags 速查

```c
io->ConfigFlags |= ImGuiConfigFlags_NavEnableKeyboard;   // 键盘导航
io->ConfigFlags |= ImGuiConfigFlags_NavEnableGamepad;    // 手柄导航
io->ConfigFlags |= ImGuiConfigFlags_DockingEnable;       // 停靠（[[21 API Docking]]）
io->ConfigFlags |= ImGuiConfigFlags_ViewportsEnable;     // 多窗口/多显示器
```

## 示例里的输入接线

- SDL2 版用 `ImGui_ImplSDL2_ProcessEvent(&event)`（[[33 后端 SDL2]]），GLFW 版用安装回调（[[32 后端 GLFW]]）。
- sdlgpu3 示例还把手柄当作输入源（`InitForSDLGPU`）。

## 相关

- item 级交互判断（hovered/clicked/active）见 [[24 API 交互与拖放]]。
- 输入法/剪贴板:[[25 API 杂项与调试]] 的 Clipboard/IM 部分。