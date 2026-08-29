---
tags: [cimgui, api, internal, advanced]
aliases: [内部函数, imgui_internal]
---

# API 内部函数

- 相关:[[10 API 索引]] · [[25 API 杂项与调试]] · [[21 API Docking]] · [[50 调用链专题]]

> cimgui 除公开 API 外，还把 `imgui_internal.h` 里的 **约 643 个内部函数** 也包装成 `ig*`（本克隆如此）。公开 API 391 个 + 内部 643 个 ≈ 1011 个顶层自由函数基名。内部 API 无稳定性保证，仅供深度集成/调试参考。

## 内部函数家族（首类代表）

| 家族 | 规模 | 示例（准确名） |
|---|---|---|
| Font/Atlas | 72 | `igImFontAtlasAddDrawListSharedData`、`igGetRoundedFontSize`、`igGetDefaultFont` |
| Table | 61 | `igBeginTableEx`、`igGetCurrentTable`、`igTableBeginCell`、`igTableApplyQueuedRequests`、`igTableFixColumnSortDirection` |
| Dock | 43 | `igBeginDocked`、`igBeginDockableDragDropSource/Target`、`igDockBuilderGetNode`、`igDockContextProcessUndockWindow` |
| Debug | 37 | `igDebugBreakClearData`、`igDebugDrawLineExtents`、`igDebugRenderKeyboardPreview` |
| Item | 33 | `igActivateItemByID`、`igCalcItemSize`、`igDebugLocateItem`、`igKeepAliveID` |
| Settings/Ini | 23 | `igAddSettingsHandler`、`igCreateNewWindowSettings`、`igClearWindowSettings`、`igCleanupIniSettings` |
| Nav | 22 | `igSetNavID`、`igNavHighlightActivated`、`igNavInitWindow`、`igGetNavTweakPressedAmount`、`igIsInNavFocusRoute` |
| TabBar | 21 | `igBeginTabBarEx`、`igGetCurrentTabBar`、`igTabBarAddTab`、`igDockNodeBeginAmendTabBar` |
| Window 显示顺序 | 4+ | `igBringWindowToDisplayFront/Back/Behind`、`igBringWindowToFocusFront` |
| 窗口查询/计算 | 5+ | `igFindWindowByID`、`igFindWindowByName`、`igFindWindowDisplayIndex`、`igCalcWindowNextAutoFitSize` |
| 焦点/激活管理 | 7 | `igGetActiveID`、`igSetActiveID`、`igGetHoveredID`、`igSetHoveredID`、`igFocusWindow`、`igClearActiveID`、`igSetActiveIdUsingAllKeyboardKeys` |
| Popup 内部 | 6 | `igClosePopupToLevel`、`igClosePopupsExceptModals`、`igClosePopupsOverWindow`、`igBeginPopupEx`、`igFindBestWindowPosForPopupEx`、`igOpenPopupEx` |
| Columns | 12 | `igBeginColumns`、`igEndColumns`、`igFindOrCreateColumns`、`igPushColumnsBackground` |
| InputText 内部 | 6 | `igInputTextEx`、`igGetInputTextState`、`igTempInputText`、`igIsItemActiveAsInputText` |
| MultiSelect | 6 | `igBeginMultiSelect`/`igEndMultiSelect`、`igGetMultiSelectState`、`igMultiSelectAddSetAll`、`igMultiSelectItemHeader` |
| DataType | 7 | `igDataTypeApplyOp`、`igDataTypeFormatString`、`igDataTypeGetInfo`、`igDataTypeCompare` |
| Button 外壳 | 12 | `igButtonBehavior`、`igButtonEx`、`igArrowButtonEx`、`igCloseButton`、`igCollapseButton` |
| Scroll | 4 | `igScrollToItem`、`igScrollToRect`、`igScrollToRectEx`、`igScrollToBringRectIntoView` |
| ContextHook | 3 | `igAddContextHook`、`igRemoveContextHook`、`igCallContextHooks` |
| 绘制/文本内部 | — | `igTextEx`、`igRenderText`、`igRenderTextClippedEx`、`igCalcClipRectVisibleItemsY`、`igSeparatorTextEx` |

> 完整 476 个"未归类"基名即主要是上述内部函数;精确名单请用 `codegraph explore "igBeginDocked"` 或查 `generator/output/definitions.json`（location 段 = `imgui_internal`）。

## 什么时候会用到内部 API

1. **主机编辑器/引擎集成**:想 hack 焦点系统、窗口排序、上下文钩子（`igAddContextHook`）。
2. **自绘高级 widget**:参考 `igButtonBehavior` + `igButtonEx` 组合。
3. **性能检查**:`igGetCurrentTable`、`igGetInputTextState` 直接读内部状态。
4. **Docking 深度定制**:`igDockBuilderGetNode`、`igDockNodeBeginAmendTabBar`。

> 不写插件库的话多数内部函数用不到;文档仅列出家族首类代表，使用前以 C++ 侧 `imgui_internal.h` 为准（本仓库 `imgui/` 子模块）。

## 相关

- 公开 API 的组织见 [[10 API 索引]];内部几何辅助 `igIm*` 见 [[25 API 杂项与调试]]。
- Docking 内部与 Builder 分工见 [[21 API Docking]]。