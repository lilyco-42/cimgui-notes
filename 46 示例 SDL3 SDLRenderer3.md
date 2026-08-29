---
tags: [cimgui, example, sdl3, sdlrenderer3]
aliases: [SDL3 SDLRenderer3, sdl_renderer3]
---

# 示例 SDL3 SDLRenderer3

- 相关:[[40 示例总览]] · [[41 示例 主循环模式]] · [[34 后端 SDL3]] · [[37 后端 其他]]
- 源码:`backend_test/example_sdl_renderer3/main.c`（158 行,S个示例里最短）

> **目录名叫 `sdl_renderer3`,实际平台后端是 `ImGui_ImplSDL3_*`（SDL3）**，渲染用官方 **C++** `imgui_impl_sdlrenderer3`（无 C 包装）。无 GPU 依赖,单窗口 158 行,数据编号示例。

## 初始化

```c
SDL_Init(SDL_INIT_VIDEO | SDL_INIT_GAMEPAD);
SDL_CreateWindowAndRenderer("...", 1280*scale, 720*scale, flags, &window, &renderer);

igCreateContext(NULL);
io->ConfigFlags |= ImGuiConfigFlags_NavEnableKeyboard | NavEnableGamepad;
igStyleColorsDark(NULL);

ImGui_ImplSDL3_InitForSDLRenderer(window, renderer);   // 平台（C 包装）
ImGui_ImplSDLRenderer3_Init(renderer);                 // 渲染（C++）
```

## DPI 缩放（本示例的教学重点）

```c
float main_scale = SDL_GetDisplayContentScale(SDL_GetPrimaryDisplay());
ImGuiStyle* style = igGetStyle();
ImGuiStyle_ScaleAllSizes(style, main_scale);      // structural form
style->FontScaleDpi = main_scale;
io->ConfigDpiScaleFonts = true;                    // 自动覆盖字体缩放
io->ConfigDpiScaleViewports = true;
```

> `ImGuiStyle_ScaleAllSizes` 是 `ImGuiStyle` 的**结构体方法包装**（[[27 数据结构与枚举]]），直接把 `ItemSpacing`/`FramePadding` 等整体按系数放大。

## 主循环

```c
while (!done) {
    while (SDL_PollEvent(&e)) { ImGui_ImplSDL3_ProcessEvent(&e); }
    ImGui_ImplSDL3_NewFrame();
    igNewFrame();
    igShowDemoWindow(1);
    // 自定义窗口 igBegin/igEnd...
    igRender();
    SDL_RenderSetScale(renderer, main_scale, main_scale);
    ImGui_ImplSDLRenderer3_RenderDrawData(igGetDrawData());  // C++
    SDL_RenderPresent(renderer);
}
```

## 适合谁

- 想要"纯 SDL 生态、无 GL/Vulkan 负担"的快速 UI 工具。
- 比拟对 SDL3 API 学习（`SDL_CreateWindowAndRenderer`、`SDL_GetDisplayContentScale`）。
- 对比纯 C++ 后端如何与 C 核心 API 混用（同 [[44 示例 GLFW DX11]] 的边界楔合）。

## 相关

- SDL3 平台后端细节 → [[34 后端 SDL3]]。
- SDLRenderer3 渲染后端（C++）→ [[37 后端 其他]]。