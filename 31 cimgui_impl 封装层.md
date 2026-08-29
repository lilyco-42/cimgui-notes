---
tags: [cimgui, backend, impl]
aliases: [cimgui_impl, 封装层]
---

# cimgui_impl 封装层

- 相关:[[30 后端总览]] · [[32 后端 GLFW]] · [[33 后端 SDL2]] · [[34 后端 SDL3]] · [[35 后端 OpenGL]] · [[36 后端 Vulkan]]

`cimgui_impl.h / cimgui_impl.cpp`（235 行头)把官方后端函数转成 C。每个后端包在 `#ifdef CIMGUI_USE_*` 里，**必须显式定义对应宏**；另外整段同样受 `CIMGUI_DEFINE_ENUMS_AND_STRUCTS` 控制（末尾 4 行 VulkanH `ImVector_*` typedef 例外）。

## 启用方式

```c
#define CIMGUI_DEFINE_ENUMS_AND_STRUCTS
#define CIMGUI_USE_GLFW
#define CIMGUI_USE_OPENGL3
#include "cimgui.h"
#include "cimgui_impl.h"
```

## 各后端包装的 C 函数一览（全部名称与顺序来自 cimgui_impl.h）

### GLFW（20 个）
`ImGui_ImplGlfw_InitForOpenGL / InitForVulkan / InitForOther / NewFrame / Shutdown / InstallCallbacks / RestoreCallbacks / SetCallbacksChainForAllWindows / Sleep / GetContentScaleForWindow / GetContentScaleForMonitor / KeyCallback / MouseButtonCallback / ScrollCallback / CharCallback / CursorPosCallback / CursorEnterCallback / WindowFocusCallback / MonitorCallback`（=> [[32 后端 GLFW]]）

### OpenGL3（8 个)
`ImGui_ImplOpenGL3_Init`（传 glsl_version 字符串）/ `NewFrame` / `Shutdown` / `RenderDrawData` / `CreateDeviceObjects` / `DestroyDeviceObjects` / `GetRenderState` / `UpdateTexture` + 结构体 `ImGui_ImplOpenGL3_RenderState`（=> [[35 后端 OpenGL]]）

### OpenGL2（7 个)
`ImGui_ImplOpenGL2_Init / NewFrame / Shutdown / RenderDrawData / CreateDeviceObjects / DestroyDeviceObjects / UpdateTexture`（=> [[35 后端 OpenGL]]）

### SDL2（13 个)
`ImGui_ImplSDL2_InitForOpenGL(D3D/Metal/Vulkan/SDLRenderer/Other)` ×6 + `NewFrame / ProcessEvent / SetGamepadMode / SetMouseCaptureMode / GetContentScaleForWindow / GetContentScaleForDisplay / Shutdown` + 枚举 `ImGui_ImplSDL2_GamepadMode`、`ImGui_ImplSDL2_MouseCaptureMode`（=> [[33 后端 SDL2]]）

### SDL3（13 个)
同上族: `ImGui_ImplSDL3_InitFor*` ×6 + `NewFrame / ProcessEvent / SetGamepadMode / SetMouseCaptureMode / GetContentScaleForWindow / GetContentScaleForDisplay / Shutdown`（=> [[34 后端 SDL3]]）

### Vulkan（21 个)
`ImGui_ImplVulkan_Init`（吃大结构体 `ImGui_ImplVulkan_InitInfo`）/ `NewFrame` / `Shutdown` / `RenderDrawData` / `AddTexture` / `RemoveTexture` / `UpdateTexture` / `CreateMainPipeline` / `LoadFunctions` / `SetMinImageCount` + VulkanH 辅助 `ImGui_ImplVulkanH_CreateOrResizeWindow` / `DestroyWindow` / `GetMinImageCountFromPresentMode` / `GetWindowDataFromViewport` / `SelectPhysicalDevice` / `SelectPresentMode` / `SelectSurfaceFormat` / `SelectQueueFamilyIndex` / `Window_ImGui_ImplVulkanH_Window` / + 结构体 `ImGui_ImplVulkan_InitInfo`、`ImGui_ImplVulkan_PipelineInfo`、`ImGui_ImplVulkan_RenderState`、`ImGui_ImplVulkanH_Window`、`ImGui_ImplVulkanH_Frame`、`ImGui_ImplVulkanH_FrameSemaphores`（=> [[36 后端 Vulkan]]）

## 后端缓存/纹理更新要点

- 各后端实现 `_UpdateTexture(ImTextureData*)`（OpenGL3/2、Vulkan）——对应 Dear ImGui 新纹理数据 API（`io->Textures`）。
- OpenGL 后端不负责编译 shader 细节的场景分配（由 `CreateDeviceObjects` 做，示例在 OpenGL 上下文就绪后调用一次）。

## **重要**:DX / Metal / WGPU / SDLRenderer / SDLGPU 这些后端**不在** cimgui_impl.h 里

backend_test 的 DX11、SDLRenderer3、SDLGPU3 示例其实是**在示例自己的 CMake 里直接编译对应官方 C++ 后端 `imgui_impl_*.cpp`**（见 [[40 示例总览]] / [[44 示例 GLFW DX11]]），所以你看到示例里 `ImGui_ImplDX11_Init` 是 C++ 符号而非 `ImGui_Impl*` C 包装。<br>
想给这些后端做 C 绑定需自行生成（generator 支持用 `config_generator.lua` 加后端头）。

## 相关

- 构建时这些 C++ 后端源由库/示例 CMake 直接加入编译:`imgui_impl_{glfw|sdl2|sdl3|vulkan|opengl3|opengl2}.cpp`（[[02 仓库结构]]）。