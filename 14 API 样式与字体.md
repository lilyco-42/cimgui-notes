---
tags: [cimgui, api, style, font]
aliases: [样式, Style, 字体, 参数栈]
---

# API 样式与字体

- 相关:[[10 API 索引]] · [[13 API 布局与滚动]] · [[27 数据结构与枚举]] · [[22 API 绘制与DrawList]]

本分类 33 个函数:Style 样式（14）+ 字体（9）+ 参数栈（10）。

## Style 样式

| C 函数 | 说明 |
|---|---|
| `igGetStyle` | 取全局 `ImGuiStyle*`（直接在字段上改，如 `.WindowPadding`、`.Colors[ImGuiCol_Button]`） |
| `igStyleColorsDark` / `igStyleColorsLight` / `igStyleColorsClassic` | 三套预设主题 |
| `igPushStyleColor` / `igPopStyleColor` | 压栈颜色（`ImGuiCol_*`），可批量 |
| `igPushStyleVar` / `igPopStyleVar` | 压栈变量（`ImGuiStyleVar_*`），可批量 |
| `igPushStyleVarX` / `igPushStyleVarY` | 只改尺寸的 X / Y 分量 |
| `igGetStyleColorName` / `igGetStyleColorVec4` | 枚举名 / 颜色值查询 |
| `igGetColorU32` | 任意颜色源 → uint32（用于绘制/纹理） |
| `igGetStyleVarInfo` | 变量类型/结构信息（internal 辅助） |

常用 StyleVar:`ImGuiStyleVar_ItemSpacing`、`FramePadding`、`ItemInnerSpacing`、`WindowRounding`、`FrameRounding`、`WindowPadding`、`ScrollbarSize`、`GrabMinSize`、`WindowBorderSize` 等（清单见 [[27 数据结构与枚举]]）。

```c
igPushStyleVar_Vec2(ImGuiStyleVar_FramePadding, (ImVec2){8,4});
igPushStyleColor_U32(ImGuiCol_Button, igGetColorU32(ImGuiCol_ButtonHovered, 1.0f));
igButton("OK", (ImVec2){0,0});
igPopStyleColor(1); igPopStyleVar(1);
```

## 字体

| C 函数 | 说明 |
|---|---|
| `igPushFont` / `igPopFont` | 用 `ImFont*` 切换渲染字体（需 atlas 烘焙后） |
| `igGetFont` / `igGetFontSize` | 当前字体 / 当前字体大小 |
| `igGetFontTexUvWhitePixel` | 字体图集"白点"UV（白色 1×1 像素） |
| `igCalcTextSize` | 测文本尺寸（多行/含 wrap） |
| `igGetDefaultFont` | 默认字体（返回 `ImFont*`） |
| `igGetFontBaked` | 烘焙后的内置字体（advanced） |
| `igGetFontRasterizerDensity` | 光栅化密度 |

> 字体配置经 `io->Fonts`（`ImFontAtlas*`，见 [[27 数据结构与枚举]]）+ `igGetFontAtlas`（internal）完成，例见 [[22 API 绘制与DrawList]]。`ImGuiFreeType_*` 相关函数见 [[25 API 杂项与调试]]。

```c
ImFont* font = io->Fonts->Fonts[0];       // 第一个字体
igPushFont(font);
igText("自定义字体文本");
igPopFont();
```

## 参数栈

| C 函数 | 说明 |
|---|---|
| `igPushItemWidth` / `igPopItemWidth` | 下一个控件建议宽度 |
| `igSetNextItemWidth` | 下一个控件宽度（单次） |
| `igCalcItemWidth` | 计算实际 item 宽度 |
| `igPushItemFlag` / `igPopItemFlag` | 如 `ImGuiItemFlags_Disabled`、`SelectableDontClosePopup` |
| `igPushClipRect` / `igPopClipRect` | 裁剪区域（绘制层用） |
| `igPushTextWrapPos` / `igPopTextWrapPos` | 自动换行宽度 |

## 相关

- 颜色枚举与 StyleVar/StyleColor 全表 → [[27 数据结构与枚举]]。
- 主题切换按钮示例（`ShowStyleSelector`）见 [[25 API 杂项与调试]] 的 Demo 工具。