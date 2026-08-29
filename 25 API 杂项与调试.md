---
tags: [cimgui, api, misc, debug]
aliases: [杂项, 调试, Ini, 日志, 内存]
---

# API 杂项与调试

- 相关:[[10 API 索引]] · [[12 API 窗口与视图]] · [[21 API Docking]] · [[27 数据结构与枚举]]

本分类 9 个子主题共 67 个函数:ID 栈（6）+ Ini（6）+ 日志（9）+ 剪贴板（2）+ 内存（7）+ Demo/调试（31）+ 数学/辅助（34 归此节说明）。

## ID 栈

| C 函数 | 说明 |
|---|---|
| `igPushID` / `igPopID` | 避免同名控件冲突（`_Str/Int/Float/Ptr` 变体） |
| `igGetID` | 取/派生稳定 ID（`_Str` 或 `_Int`） |
| `igGetIDWithSeed`/`igPushOverrideID` | 覆写派生（advanced） |
| `igPushMultiItemsWidths` | 批量控件等宽辅助（advanced） |

## Ini 设置（布局持久化,含 Docking 布局）

| C 函数 | 说明 |
|---|---|
| `igSaveIniSettingsToDisk` / `igSaveIniSettingsToMemory` | 强迫保存（内存版返回缓冲） |
| `igLoadIniSettingsFromDisk` / `igLoadIniSettingsFromMemory` | 预加载 |
| `igClearIniSettings` / `igMarkIniSettingsDirty` | 清空/标记脏 |

> Docking 布局、窗口 Pos/Size/Open 都在 [Window] 与 [Docking] 节;自动读写由 `io->IniFilename` 控制。

## 日志/堆栈

| C 函数 | 说明 |
|---|---|
| `igLogToClipboard` / `igLogToFile` / `igLogToTTY` / `igLogToBuffer` | 切换输出目标 |
| `igLogButtons` | 把日志开关按到"上一次调用"的那个窗口 |
| `igLogFinish` / `igLogText` | 结束 / 写入一条 |

## 剪贴板

| C 函数 | 说明 |
|---|---|
| `igGetClipboardText` / `igSetClipboardText` | 读写剪贴板（需后端实现 IO.ClipboardUserData 回调） |

## 内存 / 分配器

| C 函数 | 说明 |
|---|---|
| `igMemAlloc` / `igMemFree` | imgui 内部堆（对应用层一般无关） |
| `igGetAllocatorFunctions` / `igSetAllocatorFunctions` | 自定义分配器挂接 |
| `igGetStateStorage` / `igSetStateStorage` | 控件状态存储库（跨帧保存控件状态） |
| `igSetContextName` | 设置 context 名字（调试用） |

## Demo / 调试工具（Show* 家族,常做成"一键调试面板"）

| C 函数 | 说明 |
|---|---|
| `igShowDemoWindow` | 官方完整示例窗口 |
| `igShowAboutWindow` / `igShowUserGuide` | 关于/使用指南 |
| `igShowMetricsWindow` / `igShowDebugLogWindow` / `igShowIDStackToolWindow` | 性能/日志/ID 检查器 |
| `igShowStyleEditor` / `igShowStyleSelector` / `igShowFontSelector` / `igShowFontAtlas` | 样式/字体调试窗口 |
| `igDebugCheckVersionAndDataLayout` | 版本/结构体布局自检（**建议 main 开头调用**） |
| `igDebugStartItemPicker` | 启动控件拾取器 |
| `igDebugTextEncoding` / `igDebugLog` | 编码检测 / 打印日志 |
| `igDebugFlashStyleColor` / `igDebugHookIdInfo` / `igDebugDrawCursorPos` / `igDebugDrawItemRect` | 高级调试着色/钩子 |
| `igDebugNodeWindow` / `igDebugNodeWindowsList` / `igDebugNodeTable` / `igDebugNodeFontGlyph` / `igDebugNodeDrawList` / `igDebugNodeStorage` / `igDebugNodeColumns` / `igDebugNodeTabBar` / `igDebugNodeDockNode` / `igDebugNodeViewport` / `igDebugNodeTexture` / `igDebugNodeMultiSelectState` … | Metrics 窗口用的节点调试器 |

## 数学 / 辅助（Im* 前缀;imgui 内部集合,也可直接用）

| C 函数 | 说明 |
|---|---|
| `igImMin` / `igImMax` / `igImClamp` / `igImSaturate` / `igImAbs` / `igImSign` / `igImFloor` / `igImCeil`(注:见 `igImTrunc`)`igImTrunc` / `igImTrunc64` | 标量数学 |
| `igImLerp` / `igImInvLength` / `igImLengthSqr` / `igImDot` / `igImMul` / `igImRotate` | 向量/点积 |
| `igImPow` / `igImLog` / `igImRsqrt` / `igImIsPowerOfTwo` / `igImBezierCubicCalc` / `igImBezierQuadraticCalc` | 数值 |
| `igImTriangleContainsPoint` / `igImTriangleClosestPoint` | 几何 |
| `igImFormatString*` / `igImStrdup` / `igImStricmp` / `igImHashStr` / `igImHashData` | 字符串/哈希 |

> Freetype 相关公开函数（`ImGuiFreeType_SetAllocatorFunctions` / `GetFontLoader` / `DebugEditFontLoaderFlags`）也在本仓库公开列表，但需 IMGUI_ENABLE_FREETYPE 编译（[[02 仓库结构]]）。

## 相关

- 锁帧/节流:`io->Framerate` 与 `igGetTime`（[[11 API 上下文与主循环]]）。
- 布局持久化与 Docking 布局联动见 [[21 API Docking]]。