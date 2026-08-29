---
tags: [cimgui, example, sdl3, vulkan]
aliases: [SDL3 Vulkan, sdl3_vulkan]
---

# 示例 SDL3 Vulkan

- 相关:[[40 示例总览]] · [[45 示例 SDL2 Vulkan]] · [[36 后端 Vulkan]] · [[34 后端 SDL3]]
- 源码:`backend_test/example_sdl3_vulkan/main.c`（≈630 行）

> 与 [[45 示例 SDL2 Vulkan]] 同结构的 **SDL3 平台** 版:平台用 `ImGui_ImplSDL3_*`（cimgui C 包装），渲染用 `ImGui_ImplVulkan_*` + VulkanH（同样 C 包装）。适合已有 SDL3 工程的用户。

## 与 SDL2 版的逐行差异

| 步骤 | SDL2 版 | SDL3 版 |
|---|---|---|
| 初始化 | `SDL_INIT_VIDEO` | `SDL_INIT_VIDEO \| SDL_INIT_GAMEPAD` |
| 平台的 WSI | `SDL_Vulkan_GetInstanceExtensions` / `SDL_Vulkan_CreateSurface` | `SDL_Vulkan_...`（SDL3 同族 API 换签名） |
| 平台后端 | `ImGui_ImplSDL2_InitForVulkan(window)` | `ImGui_ImplSDL3_InitForVulkan(window)` |
| 事件 | `ImGui_ImplSDL2_ProcessEvent` | `ImGui_ImplSDL3_ProcessEvent` |
| 新帧 | `ImGui_ImplSDL2_NewFrame()` | `ImGui_ImplSDL3_NewFrame()` |
| 清理 | `ImGui_ImplSDL2_Shutdown()` | `ImGui_ImplSDL3_Shutdown()` |

Vulkan 初始化结构（`ImGui_ImplVulkan_InitInfo`、VulkanH Window、`CreateOrResizeWindow`）两版完全一致 —— 因为它们与平台无关。

## 帧管线（同 [[45 示例 SDL2 Vulkan]]）

```c
ImGui_ImplSDL3_NewFrame();
igNewFrame();
// UI: igBegin/igEnd ...
igRender();
vkBeginCommandBuffer(cmd, ...);
ImGui_ImplVulkan_RenderDrawData(igGetDrawData(), cmd, pipeline);
vkEndCommandBuffer(cmd);
vkQueueSubmit(...);
// resize → ImGui_ImplVulkanH_CreateOrResizeWindow(...)
```

## 相关

- Vulkan 后端机制详解 → [[36 后端 Vulkan]]。
- SDL3 平台加入多显示器/双缓存的注意点 → [[34 后端 SDL3]]。