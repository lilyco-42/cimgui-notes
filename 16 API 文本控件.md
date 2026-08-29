---
tags: [cimgui, api, text]
aliases: [文本, 标签]
---

# API 文本控件

- 相关:[[10 API 索引]] · [[15 API 基础控件]] · [[14 API 样式与字体]]

本分类 26 个函数:纯文本、彩色、标签+控件、成对文本、链接。

## 纯文本

| C 函数 | 说明 |
|---|---|
| `igText` | 文本（变参 `...`，`_Str` 分组见 [[04 生成器机制]]） |
| `igTextV` | va_list 版 |
| `igText0` | 无格式化版本 |
| `igTextUnformatted` | 原始文本（含 `\n` 手动换行，不做 % 转义） |
| `igTextWrapped` | 自动换行文本 |
| `igTextColored` | 带颜色 |
| `igTextDisabled` | 使用 `ImGuiCol_TextDisabled` 颜色 |
| `igTextLink` / `igTextLinkOpenURL` | 可点击的链接样式文本（OpenURL 变体自动用系统浏览器打开 URL） |
| `igTextAligned` | 对齐版本（advanced） |
| `igTextEx` | 内部通用绘制（advanced） |

## 标签 + 值 / 控件

| C 函数 | 说明 |
|---|---|
| `igLabelText` | "标签:值"（值在右侧，可 `%s` 变参） |
| `igBulletText` | 圆点 + 文本 |
| `igValue` | 纯数值显示（`bool`/`int`/`unsigned int`/`float` 变体，不占 item 宽度） |

## 变参约定（重复出现的后缀）

| 后缀 | 含义 |
|---|---|
| （无） | 变参 `const char* fmt, ...` |
| `V` | `va_list` 传入 |
| `0` | 无参（不带格式化） |

```c
igText("Hello %s!", name);
igTextColored((ImVec4){1,0.4f,0.4f,1}, "Warning");
igLabelText("FPS", "%.1f", fps);
igBulletText("共 %d 项", n);
igTextUnformatted("原始\n多行");
if (igTextLink("点击我")) { system("start https://..."); }
```

## 相关

- 文本颜色取色:[[14 API 样式与字体]] 的 `igGetColorU32(ImGuiCol_Text)`。
- 对齐/定位:[[13 API 布局与滚动]]。