---
tags: [cimgui, overview]
aliases: [总览, what is cimgui]
---

# cimgui 总览

- 相关:[[00 Home]] · [[02 仓库结构]] · [[03 架构分层]] · [[04 生成器机制]] · [[10 API 索引]]

## 是什么

**cimgui** 是 Dear ImGui 的 **纯 C 绑定层**，用 Lua 生成器扫描 imgui 的 C++ 头文件，自动产出:

- `cimgui.h / cimgui.cpp` —— 把 `ImGui::` 命名空间函数和结构体转成 `ig` 前缀的 C 函数与 C 结构体。
- `cimgui_impl.h / cimgui_impl.cpp` —— 把部分官方后端（GLFW/SDL2/SDL3/OpenGL/Vulkan）的函数也转成 C，用 `CIMGUI_USE_*` 宏开关。

C API 因此可用于 C、以及 Go/Rust/Python×CFFI 等各种 FFI 语言。本工作区 `lyco_imgui` 的 `xmake.lua` 即用 `add_requires("cimgui 2023.08.02")` 消费它。

## 版本事实（本克隆）

- 分支 `docking_inter`，commit `7001407`（Merge PR #326 header-test）。
- 生成的 `cimgui.h` 头注释:Built from Dear ImGui **"1.92.9b" (19291)**, `docking` 分支；包含 `imgui.h`、`imgui_internal.h`、`imgui_freetype.h` 三部分 API。
- imgui 子模块（`imgui/`）checkout 在 `b48d1afbe8ee8b238e2961dc363a949dd7304e23`。

## 规模数字（来自 `generator/output/definitions.json`，脚本统计）

| 项 | 数值 |
|---|---|
| 函数基名（含重载展开前） | 1585 |
| 重载（overloading）总数 | 1708 |
| 来自 imgui.h | 1705 |
| 来自 imgui_freetype.h | 3 |
| manual（手工补的）函数 | 2（`ImGuiTextBuffer_appendf` / `ImGuiTextBuffer_appendf0`） |
| 顶层自由函数（ig\d+ 基名） | 1011 |
| 其中 imgui 公开 API（location=imgui.h） | 391 |
| imgui_internal API（location=imgui_internal.h） | ~643 |

## 为什么文档按"分类"组织

公开 API 可以按 [[10 API 索引]] 里的 38 个主题分类浏览；内部 API（imgui_internal，未公开但被绑定成 `ig*` 的函数）归入 [[26 API 内部函数]] 概要，供进阶阅读。

## 延伸

- 详细目录与文件职责 → [[02 仓库结构]]
- 函数与后端如何拼到一起 → [[50 调用链专题]]