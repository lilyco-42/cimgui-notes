---
tags: [cimgui, example, glfw, opengl3]
aliases: [GLFW OpenGL3, glfw_opengl3]
---

# 示例 GLFW OpenGL3

- 相关:[[40 示例总览]] · [[41 示例 主循环模式]] · [[32 后端 GLFW]] · [[35 后端 OpenGL]]
- 源码:`backend_test/example_glfw_opengl3/main.c`

> GLFW 平台（cimgui C 包装）+ OpenGL3 渲染（cimgui C 包装）。与 [[42 示例 SDL2 OpenGL3]] 几乎等长，最大差异是**事件走 GLFW 回调**而非轮询。

## 与 SDL2 版的关键差异

| 维度 | GLFW 版 | SDL2 版 |
|---|---|---|
| GL 上下文 | `glfwCreateWindow` + `glfwMakeContextCurrent` | SDL_GL_CreateContext |
| 事件 | **自动安装回调**（`install_callbacks=true`）交给 `ImGui_ImplGlfw_*Callback` | 主循环里 `ImGui_ImplSDL2_ProcessEvent` |
| 鼠标移到窗口外 | GLFW 自动 `CursorPosCallback` 通知 | SDL 事件驱动 |
| 交换缓冲 | `glfwSwapBuffers(window)` + `glfwPollEvents()` | `SDL_GL_SwapWindow` |

## 初始化

```c
igCreateContext(NULL);
io->ConfigFlags |= ImGuiConfigFlags_NavEnableKeyboard | ImGuiConfigFlags_NavEnableGamepad;
igStyleColorsDark(NULL);

const char* glsl_version = "#version 150";           // 或 #version 130（老 GL）
ImGui_ImplGlfw_InitForOpenGL(window, true);          // true = 自动装回调
ImGui_ImplOpenGL3_Init(glsl_version);
```

## 主循环

```c
while (!glfwWindowShouldClose(window)) {
    glfwPollEvents();                                 // 回调内自动喂 ImGui
    ImGui_ImplGlfw_NewFrame();
    igNewFrame();
    igShowDemoWindow(1);
    // 自定义窗口: igBegin("...",NULL,0){ ... } igEnd();
    igRender();
    int w,h; glfwGetFramebufferSize(window,&w,&h);
    glViewport(0,0,w,h);
    ImGui_ImplOpenGL3_RenderDrawData(igGetDrawData());
    glfwSwapBuffers(window);
}
```

## 清理

```c
ImGui_ImplOpenGL3_Shutdown();
ImGui_ImplGlfw_Shutdown();   // 内部自动 RestoreCallbacks
igDestroyContext(NULL);
glfwDestroyWindow(window); glfwTerminate();
```

## 相关

- GLFW 回调细节（`InstallCallbacks/RestoreCallbacks`）见 [[32 后端 GLFW]]。
- 若 `install_callbacks=false`，需自己接线 `ImGui_ImplGlfw_KeyCallback` 等 → 见 [[32 后端 GLFW]]。