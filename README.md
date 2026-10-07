# cimgui-notes · cimgui 学习笔记（Obsidian 知识库）

[Dear ImGui �?C 绑定 **cimgui**](https://github.com/cimgui/cimgui)（branch `docking_inter`，Dear ImGui 1.92.9b）的学习笔记，以 **Obsidian 双向链接知识�?* 形式组织�?3 个笔记�?38 �?wikilink、零悬空引用。全部内容为逐行读源�?+ codegraph 索引后人工整理的第一手理解�?
## 如何打开

1. 安装 [Obsidian](https://obsidian.md)
2. `Open folder as vault` �?选择本仓库根目录
3. 打开 `00 Home.md`（MOC 入口），或切�?**Graph view** 看整个知识图
4. 统计与绘图引用代码索引（[codegraph](https://codegraph.dev)），索引方法�?`05 Codegraph 索引.md`

> 本仓库只含笔记，不含 cimgui/Dear ImGui 源码。你需要自备一�?`cimgui`（`git clone --recurse-submodules -b docking_inter https://github.com/cimgui/cimgui`）来复现笔记里的行号与分类�?
## 笔记地图

```
00 Home.md · MOC
├── 概览  01 cimgui 总览 · 02 仓库结构 · 03 架构分层 · 04 生成器机�?· 05 Codegraph 索引
├── API 文档�?�?  ├── 10 API 索引�?011 个顶层函数基�?�?391 公开 / 643 内部�?�?  ├── 11~26 按功能域分类（带数量�?上下�?窗口/布局/样式/控件/文本/数�?�?菜单/表格/
�?  �?  Docking/绘制/IO/交互/杂项/内部
�?  └── 27 数据结构与枚举（ImGuiIO、ImGuiStyle、ImDrawData、ImGuiWindowFlags…）
├── 后端
�?  ├── 30 后端总览 · 31 cimgui_impl 封装层（C 包装边界�?�?  ├── 32 GLFW · 33 SDL2 · 34 SDL3 · 35 OpenGL · 36 Vulkan · 37 其他
�?  └── （重�?哪些后端�?C 包装、哪些直接链官方 C++ 源）
├── 示例
�?  ├── 40 示例总览 · 41 示例 主循环模式（公共样板�?�?  ├── 42~48 七个 backend_test:SDL2/GLFW/DX11/Vulkan/SDL3/SDLRenderer3/SDLGPU3
�?  └── 49 test 烟雾测试
└── 50 调用链专题（main() �?ig* �?ImGui::* �?后端 RenderDrawData 的完整链路）
```

### 按功能分类的 API 数量（完整清单与原始类别表见 `cimgui/api_categories.txt`，本仓库不含�?
| �?| 数量 | | �?| 数量 |
|---|---|---|---|---|
| Context 上下�?| 38 | | Table 表格 | 18 |
| Window Begin/End | 75 | | TabBar | 4 |
| SetNext/Set | 15 | | Columns | 9 |
| 光标/布局 | 21 | | Docking | 17 |
| 滚动 | 13 | | IO/输入 | 26 |
| Style 样式 | 14 | | Viewport | 5 |
| 字体 | 9 | | 交互 | 25 |
| 参数�?| 10 | | 拖放 | 5 |
| ID �?| 6 | | MultiSelect | 1 |
| 基础控件 | 16 | | Ini | 6 |
| 文本 | 26 | | 日志 | 9 |
| Drag | 12 | | 剪贴�?| 2 |
| Slider | 14 | | 内存 | 7 |
| Input | 16 | | Demo/调试 | 31 |
| 颜色 | 9 | | 数学/辅助 | 34 |
| Plot | 2 | | Nav | 3 |
| Combo | 2 | | 其他(未归�? | 476 |
| Selectable/�?| 19 | | | |
| 菜单 | 3 | | 合计 | 1011 |
| Tooltip | 6 | | **公开/内部** | **391 / 643** |
| Popup | 7 | | | |

## 一个关键事实：示例目录名会误导

`example_sdl_renderer3` / `example_sdlgpu3` 听上去是 SDL2，实际平台后端是 **SDL3**（`ImGui_ImplSDL3_*`）。详�?`46 示例 SDL3 SDLRenderer3.md`、`48 示例 SDL3 SDLGPU3.md`�?
cimgui �?C 包装 GLFW / SDL2 / SDL3 / OpenGL2 / OpenGL3 / Vulkan�?VulkanH）后端；DX11、SDLRenderer3、SDLGPU3 的渲染后端在示例中是直接编译官方 C++ 源接�?`igGetDrawData()`（见 `44 示例 GLFW DX11.md` 的边界说明）�?
## 数据来源与可复现�?
- 源码:cimgui `docking_inter` @ 7001407（Dear ImGui 1.92.9b�?- API 清单与行�? `generator/output/definitions.json`、`structs_and_enums.json`、`impl_definitions.json`
- 调用关系: codegraph 索引（`callers igRender` �?8 个调用者等查询�?`50 调用链专�?md`�?- 306 个结构体/枚举、`ImGuiIO` 33 个方法包装等统计口径�?`27 数据结构与枚�?md`

## License

笔记与知识图结构 © 作者，�?[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 发布。引用的函数�?概念来自 [cimgui](https://github.com/cimgui/cimgui) �?[Dear ImGui](https://github.com/ocornut/imgui)（MIT License）；cimgui 生成器打出的行号/代码以各自仓库为准�