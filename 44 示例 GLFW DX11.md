---
tags: [cimgui, example, glfw, dx11]
aliases: [GLFW DX11, glfw_dx11]
---

# 示例 GLFW DX11

- 相关:[[40 示例总览]] · [[32 后端 GLFW]] · [[37 后端 其他]] · [[50 调用链专题]]
- 源码:`backend_test/example_glfw_dx11/main.c`（249 行）

> Win32 上最常见组合——GLFW 窗口（平台用 **cimgui C 包装** `ImGui_ImplGlfw_*`），渲染用 **Direct3D 11**。注意:**DX11 渲染后端没有 C 包装**，所以示例直接用官方 C++ `imgui_impl_dx11`（示例 CMake 把它和 imgui_impl_glfw.cpp 一起编译进可执行文件）。`ImGui_ImplDX11_Init/RenderDrawData` 是 C++ 符号，但 `igGetDrawData()` 返回的 `ImDrawData*` 直接传给它们。

## C 的边界在哪

| 组件 | 符号族 | 语言 |
|---|---|---|
| ImGui 核心 | `ig*` | C（cimgui） |
| 平台后端 GLFW | `ImGui_ImplGlfw_*` | C（cimgui_impl.h） |
| 渲染后端 DX11 | `ImGui_ImplDX11_*` | C++（imgui_impl_dx11） |
| DX11 运行时 | `ID3D11Device/Context/SwapChain` | C API（d3d11.h） |

因此示例里 `igRender()` 之后是:

```cpp
ImGui_ImplDX11_RenderDrawData(igGetDrawData());   // C++ 调用点
```

## 初始化要点（前 80 行）

```c
// 头部
#include "cimgui.h"
#include "cimgui_impl.h"
#define D3D11_NO_HELPERS   // 用纯 C 宏 CINTERFACE/COBJMACROS 访问 COM
#define CINTERFACE
#define COBJMACROS
#include <d3d11.h>
#include <GLFW/glfw3.h>
#include <GLFW/glfw3native.h>   // glfwGetWin32Window

ImGui_ImplGlfw_InitForOther(window, true);   // GLFW 平台:非 GL/Vulkan 用 Other
```

- 窗口创建前 `glfwWindowHint(GLFW_CLIENT_API, GLFW_NO_API)`（不建 GL 上下文）。
- `window_size_callback` 里 `CleanupRenderTarget(); IDXGISwapChain_ResizeBuffers(...); CreateRenderTarget();` 应对缩放（这是手写 DX11 的标准三连）。
- 交换链用 `CreateDXGIFactory` + `IDXGIFactory_CreateSwapChainForHwnd`,绑定到 `glfwGetWin32Window(window)`。

## 为什么值得看

1. **展示"pure-C 消费者用 C++ 后端"** 的边界楔合方法(`igGetDrawData()` 双端通用)。
2. DX11 后端函数极少:Init / RenderDrawData / NewFrame / Shutdown（[imgui_impl_dx11](imgui/backends/imgui_impl_dx11.cpp)），上手快。
3. 与 OpenGL 版最大差别在:**渲染发生在命令提交前，随手清屏**(`ID3D11DeviceContext_ClearRenderTargetView`)。

## 相关

- GLFW 后端细节 → [[32 后端 GLFW]];DX11 C++ 后端说明 → [[37 后端 其他]]。
- COM/交换链样板是 C 语言，适合在纯 C 工程里抄。