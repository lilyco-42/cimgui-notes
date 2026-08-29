---
tags: [cimgui, example, sdl3, sdlgpu3]
aliases: [SDL3 SDLGPU3, sdlgpu3]
---

# 示例 SDL3 SDLGPU3

- 相关:[[40 示例总览]] · [[41 示例 主循环模式]] · [[34 后端 SDL3]] · [[37 后端 其他]]
- 源码:`backend_test/example_sdlgpu3/main.c`（216 行）

> SDL3 + **SDL GPU**（`imgui_impl_sdlgpu3`,官方 C++ 渲染后端）——最"modern & 短"的 GPU 示例。窗口、事件、GPU 设备、交换链全由 SDL3 一手包办，不碰 GL/Vulkan 原生 API。

## 初始化（GPU 三步）

```c
SDL_GPUDevice* gpu = SDL_CreateGPUDevice(
    SDL_GPU_SHADERFORMAT_SPIRV | SDL_GPU_SHADERFORMAT_DXIL | SDL_GPU_SHADERFORMAT_METALLIB,
    true, NULL);
SDL_ClaimWindowForGPUDevice(gpu, window);
SDL_SetGPUSwapchainParameters(gpu, window, SDL_GPU_SWAPCHAINCOMPOSITION_SDR,
                             SDL_GPU_PRESENTMODE_MAILBOX);

igCreateContext(NULL);
io->ConfigFlags |= ImGuiConfigFlags_NavEnableKeyboard | NavEnableGamepad;
igStyleColorsDark(NULL);

ImGui_ImplSDL3_InitForSDLGPU(window);        // 平台（C 包装）
ImGui_ImplSDLRenderer3? 否 → ImGui_ImplSDLGPU3_Init(gpu);   // 渲染（C++）
```

## 主循环提交（GPUCommandBuffer）

```c
SDL_GPUCommandBuffer* cmd = SDL_AcquireGPUCommandBuffer(gpu);
...（SDL 排队 clear/自绘后）...
ImGui_ImplSDLGPU3_NewFrame();                 // 若内部驱动用（并节流）
ImGui_ImplSDL3_NewFrame();
igNewFrame();
// UI ...
igRender();

SDL_BindGPUVertexBuffers(...);               // SDL 内部处理
ImGui_ImplSDLGPU3_RenderDrawData(cmd, igGetDrawData());
SDL_SubmitGPUCommandBuffer(cmd);
```

## 为什么它是"新默认推荐"

| 优势 | 说明 |
|---|---|
| 单依赖 | 只要 SDL3，不要 GL/Vulkan/DX 初始化样板 |
| 三平台 shader | SPIRV（Vulkan）/ DXIL（D3D12）/ METALLIB（Metal）自动选 |
| 短 | 216 行，比 Vulkan 版（627 行）短 2/3 |
| 事件统一 | SDL_Event → `ImGui_ImplSDL3_ProcessEvent`（同平台其它示例） |

## 相关

- 平台/事件仍走 `ImGui_ImplSDL3_*`（[[34 后端 SDL3]]）;渲染后端说明见 [[37 后端 其他]] 的 sdlgpu3 行。
- 若想要更长的真实渲染照抄,可再对比 [[45 示例 SDL2 Vulkan]]（手写 WSI）。