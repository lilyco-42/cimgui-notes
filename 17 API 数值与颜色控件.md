---
tags: [cimgui, api, numeric, color]
aliases: [数值控件, 颜色, Drag, Slider, Input, Plot]
---

# API 数值与颜色控件

- 相关:[[10 API 索引]] · [[15 API 基础控件]] · [[14 API 样式与字体]] · [[27 数据结构与枚举]]

本分类 53 个函数:Drag（12）+ Slider（14）+ Input（16）+ 颜色（9）+ Plot（2）。

## Drag 拖值

| C 函数 | 说明（Float/Int 各 4 维 + Range2 + Scalar 通用版） |
|---|---|
| `igDragFloat` / `igDragFloat2/3/4` | 浮点拖值，可 `format` 与 `ImGuiSliderFlags_*` |
| `igDragInt` / `igDragInt2/3/4` | 整型版 |
| `igDragFloatRange2` / `igDragIntRange2` | 双指针（min/max）范围拖值 |
| `igDragScalar` / `igDragScalarN` | 按 `ImGuiDataType_*` 通用版（advanced） |

## Slider 滑块

| C 函数 | 说明 |
|---|---|
| `igSliderFloat` / `igSliderFloat2/3/4` | 浮点滑块 |
| `igSliderInt` / `igSliderInt2/3/4` | 整型滑块 |
| `igSliderScalar` / `igSliderScalarN` | 通用类型版 |
| `igSliderAngle` | 角度滑块（弧度 ↔ 度数） |
| `igVSliderFloat` / `igVSliderInt` / `igVSliderScalar` | 垂直滑块 |

## Input 输入框

| C 函数 | 说明 |
|---|---|
| `igInputFloat` / `igInputFloat2/3/4` | 浮点输入（可带 step/step_fast） |
| `igInputInt` / `igInputInt2/3/4` | 整型输入 |
| `igInputDouble` | double 版 |
| `igInputScalar` / `igInputScalarN` | 通用版 |
| `igInputText` | 文本输入（回调可选） |
| `igInputTextMultiline` | 多行 |
| `igInputTextWithHint` | 带 placeholder 提示 |
| `igInputTextEx` / `igInputTextDeactivateHook` | 内部（advanced） |

## 颜色

| C 函数 | 说明 |
|---|---|
| `igColorEdit3` / `igColorEdit4` | 可调颜色编辑（RGB / RGBA） |
| `igColorPicker3` / `igColorPicker4` | 调色板选择 |
| `igColorButton` | 颜色块按钮 |
| `igColorConvertFloat4ToU32` / `igColorConvertU32ToFloat4` | float↔u32 颜色 |
| `igColorConvertHSVtoRGB` / `igColorConvertRGBtoHSV` | HSV↔RGB |

## Plot（旧版绘图）

| C 函数 | 说明 |
|---|---|
| `igPlotLines` | 线图（数组+步长+overlay 文本） |
| `igPlotHistogram` | 直方图 |

> 官方已淘汰 Plot 家族，推荐改用 ImPlot（独立库）。

## 标志速查（常见）

- 前缀 `ImGuiSliderFlags_AlwaysClamp / NoInput`。
- `ImGuiInputTextFlags_Password / CharsNoBlank / EnterReturnsTrue / CallCompletion`。
- `ImGuiColorEditFlags_NoInputs / NoAlpha / DisplayRGB` 等。

```c
igSliderFloat("亮度", &brightness, 0.0f, 1.0f, "%.2f", 0);
igDragInt("阈值", &threshold, 1, 0, 100);
igInputFloat2("缩放", (float[2]){sx, sy}, "%.2f", 0);
float c[4] = {1,0,0,1};
igColorEdit4("Main", c, ImGuiColorEditFlags_AlphaBar);
```

## 相关

- 带标签/后缀:标签文本由控件内联提供；对齐排布见 [[13 API 布局与滚动]]。
- 示例用法在 [[42 示例 SDL2 OpenGL3]]（slider/checkbox）与 [[49 示例 test烟雾测试]]。