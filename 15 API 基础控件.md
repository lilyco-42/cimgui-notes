---
tags: [cimgui, api, widgets]
aliases: [基础控件, 按钮, 图片]
---

# API 基础控件

- 相关:[[10 API 索引]] · [[17 API 数值与颜色控件]] · [[18 API 列表与树]]

本分类 16 个函数:按钮、勾选、图片、进度条。

## 按钮

| C 函数 | 说明 |
|---|---|
| `igButton` | 常规按钮；返回点击是否发生 |
| `igSmallButton` / `igInvisibleButton` | 小按钮 / 隐形命中区 |
| `igArrowButton` | 方向箭头按钮（方向＝`ImGuiDir_*`） |
| `igRadioButton` | 单选（`_Str`/`v`/`v`+`V_v` 变体） |

## 勾选 / 标志

| C 函数 | 说明 |
|---|---|
| `igCheckbox` | 布尔勾选 |
| `igCheckboxFlags` | 对 `unsigned int*` 按位勾选（flags |=/~= 某位） |

## 图片

| C 函数 | 说明 |
|---|---|
| `igImage` | 纹理贴图（`ImTextureID` + 尺寸 + UV + tint） |
| `igImageWithBg` | 带背景色/边框颜色的版本 |
| `igImageButton` | 可点击图片按钮（3 态色调） |

> `ImTextureID` 隐式转换自各 API 的纹理由用户传（如 `(ImTextureID)(intptr_t)tex_id`）。

## 其他

| C 函数 | 说明 |
|---|---|
| `igProgressBar` | 进度条（分数 0..1；可带 overlay 文本） |
| `igBullet` | 无文本的圆点标记（文本版见 [[16 API 文本控件]] 的 `igBulletText`） |

## 小抄

```c
if (igButton("Submit", (ImVec2){120,0})) { /* 点击 */ }
igCheckbox("启用", &flag);
igCheckboxFlags("Auto", &flags, ImGuiComboFlags_HeightLarge);
igImage((ImTextureID)(intptr_t)tex, (ImVec2){64,64}, (ImVec2){0,0}, (ImVec2){1,1}, (ImVec4){1,1,1,1}, (ImVec4){0,0,0,0});
igProgressBar(0.6f, (ImVec2){0,0}, "60%");
```

## 相关

- 风格:按钮配色/间距见 [[14 API 样式与字体]]。
- 判定交互状态（hovered/active）见 [[24 API 交互与拖放]]。