# 模态框组件 (Modal)

<cite>
**本文档中引用的文件**   
- [modal.ts](file://renderers/angular/src/lib/catalog/modal.ts)
- [modal.ts](file://renderers/lit/src/0.8/ui/modal.ts)
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json)
- [components.ts](file://renderers/lit/src/0.8/types/components.ts)
</cite>

## 目录
1. [简介](#简介)
2. [核心属性](#核心属性)
3. [事件](#事件)
4. [内容嵌套](#内容嵌套)
5. [框架示例](#框架示例)
6. [无障碍访问与响应式行为](#无障碍访问与响应式行为)

## 简介
模态框组件（Modal）是一种用于在当前页面上层显示重要信息或表单的UI元素。它要求用户进行交互后才能关闭，常用于确认对话框、表单输入或关键信息提示。该组件通过遮罩层（backdrop）突出显示其内容，并提供关闭机制。模态框的实现基于HTML5的`<dialog>`元素，确保了良好的浏览器兼容性和原生功能支持。

**Section sources**
- [modal.ts](file://renderers/angular/src/lib/catalog/modal.ts#L1-L114)
- [modal.ts](file://renderers/lit/src/0.8/ui/modal.ts#L1-L132)

## 核心属性
根据`standard_catalog_definition.json`文件中的定义，模态框组件包含以下核心属性：

| 属性名 | 类型 | 默认值 | 作用 |
|-------|------|-------|------|
| `component` | `"Modal"` | 必填 | 组件类型标识符，固定为"Modal" |
| `entryPointChild` | `string` | 必填 | 触发模态框打开的组件ID（例如一个按钮） |
| `contentChild` | `string` | 必填 | 模态框内部显示的内容组件ID |

这些属性在`standard_catalog_definition.json`中被定义为必需属性，确保了模态框的基本功能完整性。`entryPointChild`指向一个可交互的组件（如按钮），当用户点击该组件时，模态框将被打开。`contentChild`则指向一个包含模态框实际内容的组件，可以是文本、表单或其他复杂布局。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L409-L428)
- [components.ts](file://renderers/lit/src/0.8/types/components.ts#L120-L129)

## 事件
模态框组件支持`close`事件，当用户通过点击遮罩层或关闭按钮关闭模态框时触发。该事件的处理逻辑在组件内部实现，通过`handleDialogClick`方法（Angular版本）或内联事件处理器（Lit版本）来检测点击事件的目标元素。只有当点击发生在`<dialog>`元素本身（即遮罩层）时，才会触发关闭操作，从而防止用户意外关闭模态框内容。

**Section sources**
- [modal.ts](file://renderers/angular/src/lib/catalog/modal.ts#L94-L98)
- [modal.ts](file://renderers/lit/src/0.8/ui/modal.ts#L91-L98)

## 内容嵌套
模态框组件支持嵌套丰富的子组件内容。通过`contentChild`属性，可以将任何其他A2UI组件作为模态框的内容。这包括文本（Text）、表单（Form）、按钮（Button）等基本组件，也可以是更复杂的布局组件如行（Row）、列（Column）或标签页（Tabs）。这种设计允许创建高度定制化的模态框，满足各种业务需求。

**Section sources**
- [modal.ts](file://renderers/angular/src/lib/catalog/modal.ts#L35-L39)
- [modal.ts](file://renderers/lit/src/0.8/ui/modal.ts#L127)

## 框架示例
以下是使用Lit和Angular框架通过JSON定义模态框的代码示例，创建一个包含文本、表单和按钮的确认对话框：

```json
{
  "component": "Modal",
  "entryPointChild": "confirmButton",
  "contentChild": "confirmDialogContent"
}
```

其中，`confirmButton`是一个按钮组件，用于触发模态框打开；`confirmDialogContent`是一个包含文本、输入框和确认/取消按钮的复杂布局组件。这种JSON定义方式使得模态框的配置变得简单直观，易于维护和扩展。

**Section sources**
- [modal.ts](file://renderers/angular/src/lib/catalog/modal.ts#L26-L51)
- [modal.ts](file://renderers/lit/src/0.8/ui/modal.ts#L78-L131)

## 无障碍访问与响应式行为
模态框组件实现了焦点陷阱（focus trap）以确保无障碍访问。当模态框打开时，用户的键盘焦点将被限制在模态框内部，防止用户意外操作背景内容。同时，组件支持响应式行为，能够根据屏幕尺寸自动调整布局和样式，确保在不同设备上都能提供良好的用户体验。关闭按钮位于右上角，符合用户习惯，并且可以通过键盘快捷键（如Esc键）快速关闭模态框。

**Section sources**
- [modal.ts](file://renderers/angular/src/lib/catalog/modal.ts#L27-L41)
- [modal.ts](file://renderers/lit/src/0.8/ui/modal.ts#L89-L129)