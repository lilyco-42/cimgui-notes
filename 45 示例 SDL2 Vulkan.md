---
tags: [cimgui, example, sdl2, vulkan]
aliases: [SDL2 Vulkan, sdl_vulkan]
---

# 示例 SDL2 Vulkan

- 相关:[[40 示例总览]] · [[36 后端 Vulkan]] · [[33 后端 SDL2]] · [[41 示例 主循环模式]]
- 源码:`backend_test/example_sdl_vulkan/main.c`（627 行,S个示例里最长）

> 平台 SDL2（cimgui C 包装 `ImGui_ImplSDL2_*`）+ Vulkan 渲染（**cimgui C 包装 `ImGui_ImplVulkan_*` + VulkanH helper**）。示例自带 VkInstance/Device/交换链样板，是与 [[44 示例 GLFW DX11]] 同类但更完整的 "手写原生 API" 教程。

## 独特点

| 项 | 说明 |
|---|---|
| Volk | 可选 `IMGUI_IMPL_VULKAN_USE_VOLK` 动态装载（有它就是 `#include <volk.h>`，作为 C） |
| 调试报告 | `APP_USE_VULKAN_DEBUG_REPORT` 开 `VkDebugReportCallbackEXT` |
| DPI | `ImGui_ImplSDL2_GetContentScaleForDisplay(0)`（SDL2 也有缩放接口） |
| 交换链缓存 | `g_SwapChainRebuild` 标志 + 每帧检查 Framebuffer 尺寸 |

## 初始化（Vulkan 段）

```c
// 用 SDL 辅助创建实例:
SDL_Vulkan_GetInstanceExtensions(&count, NULL);
SDL_Vulkan_CreateSurface(window, g_Instance, &surface);

ImGui_ImplVulkan_InitInfo info = {};
info.Instance = g_Instance; info.PhysicalDevice = g_PhysicalDevice;
info.Device = g_Device;     info.QueueFamily = g_QueueFamily;
info.Queue = g_Queue;       info.DescriptorPool = g_DescriptorPool;
info.MinImageCount = 2;     info.ImageCount = g_MainWindowData.ImageCount;
info.CheckVkResultFn = check_vk_result;
ImGui_ImplVulkan_Init(&info);
```

## 每帧提交（与 OpenGL 本质不同：嵌入自建 CommandBuffer）

```c
ImGui_ImplSDL2_NewFrame();
igNewFrame();
// ...UI...
igRender();

// 手动拥有的一次性 CommandBuffer:
vkBeginCommandBuffer(cmd, &beginInfo);
ImGui_ImplVulkan_RenderDrawData(igGetDrawData(), cmd, pipeline);
vkEndCommandBuffer(cmd);
vkQueueSubmit(queue, 1, &submit, fence);   // 或直接等待
```

## 窗口重置

- `resize` 事件→ `g_SwapChainRebuild=true`;帧头检查尺寸→
  `ImGui_ImplVulkanH_CreateOrResizeWindow(g_Instance, g_PhysicalDevice, g_Device, &g_MainWindowData, g_QueueFamily, g_Allocator, w, h, g_MinImageCount,...)`。

## 学习顺序建议

Vulkan 复杂度最高，别用它入门;建议 GL 版（[[42 示例 SDL2 OpenGL3]]）→ SDL GPU 版（[[48 示例 SDL3 SDLGPU3]]）→ 本示例（理解 WSI 细节）。同款 SDL3 版见 [[47 示例 SDL3 Vulkan]]。