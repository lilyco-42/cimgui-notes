---
tags: [cimgui, backend, glfw]
aliases: [GLFW 后端, imgui_impl_glfw]
---

# 后端 GLFW

- 相关:[[31 cimgui_impl 封装层]] · [[43 示例 GLFW OpenGL3]] · [[44 示例 GLFW DX11]] · [[30 后端总览]]

cimgui 对官方 `imgui_impl_glfw` 的 C 包装，共 **20 个函数**（名同 C++ 版，符号在 `#ifdef CIMGUI_USE_GLFW` 内）。

## 初始化（按渲染后端选一个）

```c
#define CIMGUI_USE_GLFW
#include "cimgui_impl.h"

ImGui_ImplGlfw_InitForOpenGL(window, true);   // OpenGL 渲染
ImGui_ImplGlfw_InitForVulkan(window, true);   // Vulkan 渲染
ImGui_ImplGlfw_InitForOther(window, true);    // DX / SDLRenderer 等其它渲染
```

## 事件回调（两种模式）

1. **安装回调**——`InitFor*` 第二个参数 `install_callbacks = true` 时自动注册内部回调，无需用户处理事件。
2. **手动转发**——`install_callbacks=false`，由用户把回调接到自己的 GLFW 回调:
   `ImGui_ImplGlfw_KeyCallback / MouseButtonCallback / ScrollCallback / CharCallback / CursorPosCallback / CursorEnterCallback / WindowFocusCallback / MonitorCallback`
   之后用:
   - `ImGui_ImplGlfw_RestoreCallbacks(window)` 还原用户回调
   - `ImGui_ImplGlfw_SetCallbacksChainForAllWindows(bool)` 控制是否对所有窗口链式转发
   - `ImGui_ImplGlfw_Sleep(milliseconds)`（主循环限帧的小工具）

> GLFW 系示例（[[43 示例 GLFW OpenGL3]]、[[44 示例 GLFW DX11]]）都开 `install_callbacks=true`。

## 周期函数

| 函数 | 时机 |
|---|---|
| `ImGui_ImplGlfw_NewFrame()` | 每帧 `igNewFrame()` **之前** |
| `ImGui_ImplGlfw_Shutdown()` | 窗口销毁后、进程结束前 |

## 缩放 / 多显示器

- `ImGui_ImplGlfw_GetContentScaleForWindow(window)` → 窗口内容缩放（DPI）。
- `ImGui_ImplGlfw_GetContentScaleForMonitor(monitor)` → 缩放直接按显示器算。

## GLFW 特性映射

| 平台能力 | 实现路径 |
|---|---|
| 鼠标光标样式 | 后端读 `igSetMouseCursor`（[[23 API IO与输入]]）/ `io->MouseDrawCursor` |
| 游戏手柄 | GLFW 游戏手柄 API（若有则走 `io->AddGamepadEvent`） |
| 多显示器/多窗口 | 需 `io->ConfigFlags |= ImGuiConfigFlags_ViewportsEnable` + `igUpdatePlatformWindows`（[[23 API IO与输入]]） |

## 示例对照

- `backend_test/example_glfw_opengl3/main.c`（[[43 示例 GLFW OpenGL3]]）
- `backend_test/example_glfw_dx11/main.c`（[[44 示例 GLFW DX11]]，用 `InitForOther` + DX11 C++ 渲染器）