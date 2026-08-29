---
tags: [cimgui, example, sdl2, opengl3]
aliases: [SDL2 OpenGL3, sdl_opengl3]
---

# 示例 SDL2 OpenGL3

- 相关:[[40 示例总览]] · [[41 示例 主循环模式]] · [[33 后端 SDL2]] · [[35 后端 OpenGL]]
- 源码:`backend_test/example_sdl_opengl3/main.c`（对应构建方式见该目录 README: `cmake path/to/example_sdl_opengl3 && make install`）

> **最推荐的入门示例**:SDL2 平台（C 包装）+ OpenGL3 渲染（C 包装），两件都有 cimgui 包装，能完整体会 C API。

## 骨架（≈200 行）

1. **GL 上下文**:`SDL_GL_CONTEXT_PROFILE_CORE` + 3.2，glsl `#version 150`（若 GL 环境缺 core 则回退 `#version 130`）。
2. **cimgui 样板**:`CIMGUI_DEFINE_ENUMS_AND_STRUCTS` + `IMGUI_HAS_IMSTR` 映射 + `igGetIO_Nil`（[[04 生成器机制]]）。
3. **初始化**:
   ```c
   igCreateContext(NULL);
   io->ConfigFlags |= ImGuiConfigFlags_NavEnableKeyboard;
   igStyleColorsDark(NULL);
   ImGui_ImplSDL2_InitForOpenGL(window, gl_context);   // SDL2 平台
   ImGui_ImplOpenGL3_Init(glsl_version);               // OpenGL3 渲染
   ```
4. **DPI**:`f = ImGui_ImplSDL2_GetContentScaleForDisplay(0)`;`io->ConfigDpiScaleFonts`

## 事件 → 帧:

```c
while (SDL_PollEvent(&e)) { ImGui_ImplSDL2_ProcessEvent(&e); }
ImGui_ImplSDL2_NewFrame();
igNewFrame();
// 演示窗口 + 自定义窗口
igShowDemoWindow(1);
if (igBegin("Hello, cimgui!", NULL, 0)) {
    igText("这是一个 SDL2+OpenGL3 窗口");
    igSliderFloat("透明度", &f, 0, 1, "%.3f", 0);
    igColorEdit3("背景色", (float[3]){r,g,b}, 0);
    if (igButton("按钮", (ImVec2){0,0})) ...
    igEnd();
}
igRender();
ImGui_ImplOpenGL3_RenderDrawData(igGetDrawData());
SDL_GL_SwapWindow(window);
```

## 值得抄的 API 组合

| 意图 | 调用 |
|---|---|
| 上下文 | `igCreateContext` / `igDestroyContext`（[[11 API 上下文与主循环]]） |
| 窗口 | `igBegin/igEnd`、`igSetNextWindowSize`（[[12 API 窗口与视图]]） |
| 控件 | `igSliderFloat`、`igCheckbox`、`igButton`、`igColorEdit3`（[[17 API 数值与颜色控件]]） |
| 样式 | `igStyleColorsDark`（[[14 API 样式与字体]]） |
| 状态 | `io->WantCaptureMouse`（[[23 API IO与输入]]） |

## 变体对照

- GLFW 同款:[[43 示例 GLFW OpenGL3]]（事件用回调而非轮询）。
- SDL3 同款:[[46 示例 SDL3 SDLRenderer3]]、[[48 示例 SDL3 SDLGPU3]]。
- 显卡无故障时 GL 3.2+;老机器跑 `#version 130` 分支。