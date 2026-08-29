---
tags: [cimgui, example, test, smoke]
aliases: [烟雾测试, test]
---

# 示例 test烟雾测试

- 相关:[[40 示例总览]] · [[11 API 上下文与主循环]] · [[01 cimgui 总览]]
- 源码:`test/main.c`（约 60 行）+ `test/CMakeLists.txt`

> **无窗口、无平台/渲染后端**的最小区块测试:只在"null 后端"上跑一帧,用来验证 **cimgui 编译产物 + 核心 API** 可用。属于单元/冒烟测试模板。

## 流程（对应 [[11 API 上下文与主循环]]）

```c
1. igDebugCheckVersionAndDataLayout(ImGui::GetVersion()-式版本串, sizeof(ImGuiIO), sizeof(ImGuiStyle),
                                    sizeof(ImVec2), sizeof(ImVec4), sizeof(ImDrawVert));
   // 校验头文件版本与所用库一致（生产工程强烈建议 main 里先调）

2. igCreateContext(NULL);                       // 建上下文
3. ImGuiIO* io = igGetIO();                     // 空上下文也行（null backend 不喂输入）
4. igStyleColorsDark(NULL);
5. igNewFrame();                                 // 一跳帧（无平台）
   igText("Hello world");
   igSliderFloat("float", &f, 0.0f, 1.0f, "%.3f", 0);  // 控件同样能用
   igShowDemoWindow(1);                          // 可选
6. igRender();
7. igDestroyContext(NULL);                       // 销毁
```

## null 后端

Dear ImGui 自带 `imgui_impl_null`（[[37 后端 其他]]）用于无窗口环境;本 test 未直接调用它,依赖 cimgui 内部 `ImGui::GetIO`/`Render` 的空实现即可完成一帧。也就是说这个 test **不验证任何绘图或输入**,只证明:绑定层（cimgui.cpp）↔ 核心（imgui.cpp）链接、版本一致、无断言崩溃。

## 它在 CI 里的角色

- 编译产物:由主 `CMakeLists.txt` 的 `CIMGUI_TEST=yes` 开关构建（[[02 仓库结构]]）。
- 每次 cimgui 发布都会跑它 → 是**新接入工程的一个最小可跑基线**。

## 相关

- 用它当最简模板往上加窗口 = [[41 示例 主循环模式]]。