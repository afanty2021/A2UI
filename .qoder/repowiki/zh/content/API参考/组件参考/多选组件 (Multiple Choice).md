# 多选组件 (Multiple Choice)

<cite>
**本文档中引用的文件**  
- [multiple-choice.ts](file://renderers/angular/src/lib/catalog/multiple-choice.ts)
- [multiple-choice.ts](file://renderers/lit/src/0.8/ui/multiple-choice.ts)
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json)
- [common_types.json](file://specification/0.9/json/common_types.json)
</cite>

## 目录
1. [简介](#简介)
2. [核心属性](#核心属性)
3. [事件](#事件)
4. [框架实现示例](#框架实现示例)
5. [无障碍访问与响应式行为](#无障碍访问与响应式行为)
6. [结论](#结论)

## 简介
多选组件（Multiple Choice）是A2UI框架中的一个核心表单元素，用于从一组预定义的选项中选择一个或多个值。该组件在用户界面中广泛应用于问卷调查、设置配置、筛选条件等场景。根据`standard_catalog_definition.json`中的定义，该组件通过JSON结构进行配置，支持数据绑定和动态更新。本文档将详细说明该组件的属性、事件、使用示例以及其实现在Angular和Lit框架中的差异。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L556-L604)

## 核心属性
多选组件的核心属性定义了其外观、行为和数据绑定方式。这些属性在`standard_catalog_definition.json`文件中通过JSON Schema进行定义，并在Angular和Lit的实现中被具体化。

| 属性 | 类型 | 默认值 | 作用 |
| :--- | :--- | :--- | :--- |
| `options` | 数组<{label: stringOrPath, value: string}> | 无 | 定义可选择的选项列表。每个选项包含一个`label`（显示文本）和一个`value`（唯一标识符）。`label`可以是静态字符串或指向数据模型的路径。 |
| `value` | stringOrPath | 无 | 表示当前选中的值或值数组。此属性支持数据绑定，可以是一个字面量字符串或一个指向数据模型中特定路径的对象。 |
| `type` | 枚举(radio, checkbox) | 无 | 指定选择类型。`radio`表示单选按钮，用户只能选择一个选项；`checkbox`表示复选框，用户可以选择多个选项。 |
| `label` | stringOrPath | 无 | 组件的标签文本，用于描述选择题目的内容。 |

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L576-L598)
- [common_types.json](file://specification/0.9/json/common_types.json#L7-L20)

## 事件
多选组件会触发`change`事件，当用户的选中状态发生改变时，该事件会被派发。事件的载荷（payload）包含了更新后的选中值。

- **事件名称**: `change`
- **载荷结构**: 
  ```json
  {
    "value": "selected_value"
  }
  ```
  其中`value`字段是选中项的`value`属性值。在多选模式下，`value`将是一个字符串数组。

组件通过内部的`handleChange`方法处理DOM的`change`事件，并调用`processor.setData`方法来更新数据模型，确保UI状态与数据模型保持同步。

**Section sources**
- [multiple-choice.ts](file://renderers/angular/src/lib/catalog/multiple-choice.ts#L64-L76)
- [multiple-choice.ts](file://renderers/lit/src/0.8/ui/multiple-choice.ts#L122-L128)

## 框架实现示例
以下是在Lit和Angular框架下通过JSON定义多选组件的代码示例。

### Lit框架示例
```json
{
  "id": "color_picker",
  "component": "ChoicePicker",
  "usageHint": "mutuallyExclusive",
  "label": "选择颜色",
  "options": [
    { "label": "红色", "value": "red" },
    { "label": "绿色", "value": "green" },
    { "label": "蓝色", "value": "blue" }
  ],
  "value": { "path": "/user/preferences/color" }
}
```

### Angular框架示例
```json
{
  "id": "hobby_picker",
  "component": "ChoicePicker",
  "usageHint": "multipleSelection",
  "label": "选择爱好",
  "options": [
    { "label": "阅读", "value": "reading" },
    { "label": "运动", "value": "sports" },
    { "label": "音乐", "value": "music" }
  ],
  "value": { "path": "/user/profile/hobbies" }
}
```

**Section sources**
- [multiple-choice.ts](file://renderers/angular/src/lib/catalog/multiple-choice.ts#L57-L59)
- [multiple-choice.ts](file://renderers/lit/src/0.8/ui/multiple-choice.ts#L32-L36)

## 无障碍访问与响应式行为
多选组件的设计遵循无障碍访问（Accessibility）最佳实践。组件使用标准的`<label>`标签与`<select>`元素关联，确保屏幕阅读器能够正确识别和朗读组件的标签。`for`属性与`id`属性的绑定保证了点击标签时能聚焦到相应的选择控件。

在响应式行为方面，组件的CSS样式使用了`flex: var(--weight)`和`width: 100%`，使其能够根据父容器的尺寸自动调整大小。`min-height: 0`和`overflow: auto`的设置确保了在布局中不会出现意外的溢出问题。这些样式在Angular和Lit的实现中保持一致，保证了跨框架的一致性体验。

**Section sources**
- [multiple-choice.ts](file://renderers/angular/src/lib/catalog/multiple-choice.ts#L43-L54)
- [multiple-choice.ts](file://renderers/lit/src/0.8/ui/multiple-choice.ts#L38-L58)

## 结论
A2UI的多选组件提供了一个强大且灵活的界面元素，用于处理单选和多选场景。通过`standard_catalog_definition.json`中的`ChoicePicker`定义，结合`usageHint`属性，可以轻松实现单选题和复选题。其属性设计支持数据绑定，事件机制确保了状态的同步，而无障碍和响应式设计则保证了良好的用户体验。开发者可以根据具体需求，在Angular或Lit框架中使用JSON配置来快速集成该组件。