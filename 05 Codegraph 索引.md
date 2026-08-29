---
tags: [cimgui, codegraph, overview]
aliases: [codegraph, 代码图]
---

# Codegraph 索引

- 相关:[[00 Home]] · [[02 仓库结构]] · [[50 调用链专题]]

> 本仓库已用 [codegraph](https://github.com/cstack/codegraph) 建立符号索引，CLI 版本 **1.5.0**（位于 `C:\Users\liuqi\AppData\Local\codegraph\current\bin\codegraph.cmd`）。索引位置:`D:\Code\工作区\sui\lyco_imgui\cimgui\.codegraph`。

## 索引统计（初始化结果）

| 指标 | 值 |
|---|---|
| 文件数 | 139 |
| 节点数 | 8,760 |
| 边数 | 20,050 |
| DB 大小 | 20.86 MB |

节点类型分布:function 3,393 · enum_member 2,221 · method 777 · type_alias 640 · import 625 · struct 452 · variable 221 · enum 202 · file 130 · property 55 · class 30 · constant 12。语言:cpp 60 · c 47 · objc 11 · lua 9 等。

## 常用命令（在 `D:\Code\工作区\sui\lyco_imgui\cimgui` 下执行）

```text
# 概览
codegraph status
codegraph files

# 按词探查（推荐起点）:打出相关符号源码 + 调用路径
codegraph explore "igCreateContext igGetIO igNewFrame igBegin igRender"
codegraph explore "ImGui_ImplSDL2_NewFrame igRender"

# 找某个符号被谁调用(例:igRender 的 8 处调用)
codegraph callers igRender
# 结果:main @ backend_test/example_glfw_dx11/main.c:50, .../example_glfw_opengl3/main.c:24,
#        .../example_sdl3_vulkan/main.c:379, .../example_sdl_opengl3/main.c:29,
#        .../example_sdl_renderer3/main.c:13, .../example_sdl_vulkan/main.c:383,
#        .../example_sdlgpu3/main.c:15, test/main.c:16

# 查树/正则
codegraph tree imgui
codegraph grep "igBegin" --lang c
```

> 说明:本 shell 环境没有 `ls`/`wc`/`Select-Object`，codegraph 输出直接回显即可；大批量数据用 Python 解析 JSON（见 [[04 生成器机制]]）。

## 与 Obsidian 的关系

- 本图（Obsidian 笔记）是"人类可读的分类索引"，指向 `definitions.json` 的基名。
- codegraph 是"机器可读的符号图"，可直接回答 *谁调用 igRender*、*igBegin 内部实现* 等问题。
- 两者互补：先在本图找到分类 → 再到 codegraph 按名字精确查源码与调用链（如 [[50 调用链专题]] 的实例）。