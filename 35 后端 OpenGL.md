---
tags: [cimgui, backend, opengl]
aliases: [OpenGL 后端, imgui_impl_opengl3, imgui_impl_opengl2]
---

# 后端 OpenGL

- 相关:[[31 cimgui_impl 封装层]] · [[42 示例 SDL2 OpenGL3]] · [[43 示例 GLFW OpenGL3]] · [[30 后端总览]]

cimgui 同时包装了 OpenGL3（**8 个函数**）与 OpenGL2（**7 个函数**）两个渲染后端。

## OpenGL3（`#ifdef CIMGUI_USE_OPENGL3`）

```c
ImGui_ImplOpenGL3_Init("#version 150");   // 或 "#version 330 core" 等 glsl 版本串
ImGui_ImplOpenGL3_NewFrame();             // 每帧 igNewFrame 之前
...
ImGui_ImplOpenGL3_RenderDrawData(igGetDrawData());   // igRender 之后
ImGui_ImplOpenGL3_Shutdown();
```

| 函数 | 说明 |
|---|---|
| `ImGui_ImplOpenGL3_Init(glsl_version)` | 必须在 GL 上下文创建后调;传 `#version` 字符串控制管线 | 
| `ImGui_ImplOpenGL3_CreateDeviceObjects` / `DestroyDeviceObjects` | 着色器/VAO 单例创建与释放（多数情况内部自动处理） |
| `ImGui_ImplOpenGL3_GetRenderState` | 取内部 `ImGui_ImplOpenGL3_RenderState*`（`UseBindSampler`/`UseTexParameterFilter`/缓存句柄） |
| `ImGui_ImplOpenGL3_UpdateTexture(ImTextureData*)` | 新纹理系统上传/更新 |
| `ImGui_ImplOpenGL3_NewFrame` / `RenderDrawData` / `Shutdown` | 生命周期三件套 |

GLSL 版本与功能:核心 3.2+ 用 `#version 150`（desktop）、`#version 330 core`;OpenGL ES 用 `#version 300 es`;GLSL <130 自动 fallback 到固定管线 compat。

## OpenGL2（`#ifdef CIMGUI_USE_OPENGL2`）

```c
ImGui_ImplOpenGL2_Init();                 // 无参数（用 legacy 固定管线/immediate）
ImGui_ImplOpenGL2_NewFrame();
ImGui_ImplOpenGL2_RenderDrawData(igGetDrawData());
ImGui_ImplOpenGL2_Shutdown();
```

适合老适配/最小依赖环境;与 OpenGL3 完全不同:顶点用 `glBegin/glEnd`（或 DL）路径。

## 示例对照

- [[43 示例 GLFW OpenGL3]]:GLFW 平台 + OpenGL3 渲染（cimgui C 包装两件套）。
- [[42 示例 SDL2 OpenGL3]]:SDL2 平台 + OpenGL3 渲染。

## 相关

- OpenGL 需要窗口编程中正确管理 GL 上下文（`glfwMakeContextCurrent`）;纹理上传/缩放 y 轴翻转:OpenGL 后端在 `RenderDrawData` 内处理 `FramebufferScale` 与绑定。
- 渲染数据 `ImDrawData` 见 [[22 API 绘制与DrawList]]。