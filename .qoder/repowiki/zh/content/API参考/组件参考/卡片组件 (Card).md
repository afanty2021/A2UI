# 卡片组件 (Card)

<cite>
**本文档中引用的文件**  
- [card.ts](file://renderers/angular/src/lib/catalog/card.ts)
- [card.ts](file://renderers/lit/src/0.8/ui/card.ts)
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json)
- [contacts.ts](file://samples/client/lit/shell/configs/contacts.ts)
- [chart.json](file://samples/agent/adk/rizzcharts/examples/standard_catalog/chart.json)
- [map.json](file://samples/agent/adk/rizzcharts/examples/standard_catalog/map.json)
- [gallery.component.ts](file://samples/client/angular/projects/gallery/src/app/features/gallery/gallery.component.ts)
- [components.md](file://docs/reference/components.md)
</cite>

## 目录
1. [简介](#简介)
2. [属性列表](#属性列表)
3. [事件](#事件)
4. [嵌套子组件](#嵌套子组件)
5. [布局示例](#布局示例)
6. [框架代码示例](#框架代码示例)
7. [无障碍访问与响应式设计](#无障碍访问与响应式设计)
8. [结论](#结论)

## 简介

卡片组件（Card）是一种用于将相关内容组织在视觉容器中的UI元素。它通过提供一个带有内边距、边框或阴影的独立区域，帮助用户区分不同内容块，提升界面的可读性和美观性。卡片组件在A2UI系统中被广泛用于展示信息摘要、表单、图片集等内容。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L339-L355)

## 属性列表

根据 `standard_catalog_definition.json` 文件中的定义，卡片组件的完整属性如下：

- **component**  
  - 类型: `"Card"` (常量)  
  - 默认值: 无  
  - 作用: 标识该组件为卡片类型。

- **child**  
  - 类型: `string`  
  - 默认值: 无  
  - 作用: 指定要渲染在卡片内部的单个子组件的ID。若需显示多个元素，必须先将它们包装在一个布局组件（如 `Column` 或 `Row`）中，然后将该容器的ID传入此属性。不允许直接内联定义子组件或传入多个ID。

- **id**  
  - 类型: `string`  
  - 默认值: 无  
  - 作用: 组件实例的唯一标识符，继承自 `ComponentCommon`。

- **weight**  
  - 类型: `number`  
  - 默认值: 无  
  - 作用: 当卡片作为 `Row` 或 `Column` 的直接子元素时，其相对权重（类似于CSS的 `flex-grow` 属性），继承自 `ComponentCommon`。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L339-L355)
- [common_types.json](file://specification/0.9/json/common_types.json#L70-L80)

## 事件

目前的分析显示，卡片组件本身不触发特定的用户交互事件（如点击、悬停等）。它的主要功能是作为内容的容器。任何交互行为通常由其内部的子组件（如按钮、复选框等）处理。

**Section sources**
- [card.ts](file://renderers/lit/src/0.8/ui/card.ts)
- [card.ts](file://renderers/angular/src/lib/catalog/card.ts)

## 嵌套子组件

卡片组件支持嵌套各种子组件，以构建丰富的内容。常见的可嵌套组件包括：
- `Text`：用于显示标题、描述等文本内容。
- `Image`：用于展示图片。
- `Button`：用于添加操作按钮。
- `Row` 和 `Column`：用于创建复杂的内部布局。

要在一个卡片中包含多个子组件，必须使用 `Row` 或 `Column` 作为中间容器。例如，可以创建一个 `Column`，在其 `children` 属性中添加 `Text`、`Image` 和 `Button`，然后将这个 `Column` 的ID赋值给卡片的 `child` 属性。

**Section sources**
- [chart.json](file://samples/agent/adk/rizzcharts/examples/standard_catalog/chart.json#L54-L75)
- [map.json](file://samples/agent/adk/rizzcharts/examples/standard_catalog/map.json#L56-L73)
- [gallery.component.ts](file://samples/client/angular/projects/gallery/src/app/features/gallery/gallery.component.ts#L46-L97)

## 布局示例

一个典型的卡片布局可能包含一个标题、一张图片和一个操作按钮，它们垂直排列。这可以通过以下结构实现：
1. 创建一个 `Column` 组件作为卡片的直接子元素。
2. 在 `Column` 的 `children` 数组中，依次添加：
   - 一个 `Text` 组件用于标题。
   - 一个 `Image` 组件用于图片。
   - 一个 `Button` 组件用于操作。
3. 将 `Column` 的ID赋值给 `Card` 的 `child` 属性。

这种结构确保了内容在卡片内的有序和美观排列。

**Section sources**
- [gallery.component.ts](file://samples/client/angular/projects/gallery/src/app/features/gallery/gallery.component.ts#L103-L122)

## 框架代码示例

### Lit 框架下的JSON定义

```json
{
  "id": "welcome-card",
  "component": {
    "Card": {
      "child": "card-content"
    }
  }
}
```

其中，`card-content` 是一个 `Column` 组件的ID，其定义如下：
```json
{
  "id": "card-content",
  "component": {
    "Column": {
      "children": [
        "card-title",
        "card-image",
        "card-button"
      ],
      "alignment": "center"
    }
  }
}
```

### Angular 框架下的JSON定义

在Angular中，通过TypeScript代码创建卡片的示例如下（来自 `gallery.component.ts`）：

```typescript
this.createSingleComponentSurface('Card', {
  child: this.createComponent('Column', {
    children: [
      this.createComponent('Text', { text: { literalString: '欢迎' } }),
      this.createComponent('Image', { url: { literalString: 'https://example.com/image.jpg' } }),
      this.createComponent('Button', { 
        action: { type: 'submit' }, 
        child: this.createComponent('Text', { text: { literalString: '开始' } })
      })
    ],
    alignment: 'center'
  })
})
```

此代码创建了一个包含标题、图片和按钮的卡片。

**Section sources**
- [gallery.component.ts](file://samples/client/angular/projects/gallery/src/app/features/gallery/gallery.component.ts#L103-L122)
- [chart.json](file://samples/agent/adk/rizzcharts/examples/standard_catalog/chart.json#L54-L60)

## 无障碍访问与响应式设计

### 无障碍访问 (Accessibility)

卡片组件通过其语义化的容器角色，有助于屏幕阅读器用户理解内容的分组。为了确保最佳的无障碍体验，应确保其内部的文本内容（如 `Text` 组件）具有适当的语义标签（通过 `usageHint` 属性，如 `h1`, `h2` 等），并且所有交互元素（如按钮）都具备清晰的标签。

### 响应式设计 (Responsive Design)

卡片组件在移动端会自动堆叠其内容。由于其内部通常使用 `Column` 布局，子元素会自然地垂直排列，非常适合移动设备的小屏幕。此外，卡片的CSS样式（如 `min-width` 和 `max-width`）可以在主题中进行配置，以适应不同屏幕尺寸。例如，在 `contacts.ts` 配置文件中，卡片的宽度被限制在320px到400px之间，这确保了它在移动设备上不会过宽。

```css
Card: {
  "min-width": "320px",
  "max-width": "400px",
  margin: "0 auto"
}
```

**Section sources**
- [contacts.ts](file://samples/client/lit/shell/configs/contacts.ts#L71-L80)
- [card.ts](file://renderers/lit/src/0.8/ui/card.ts#L33-L38)
- [card.ts](file://renderers/angular/src/lib/catalog/card.ts#L27-L32)

## 结论

A2UI的卡片组件是一个功能强大且灵活的容器，用于组织和展示相关内容。它通过 `child` 属性接收一个子组件ID，支持嵌套 `Text`、`Image`、`Button` 等多种子组件，并通过 `Row` 和 `Column` 实现复杂的内部布局。该组件在Lit和Angular框架下均可通过JSON或代码定义，且具备良好的无障碍访问和响应式设计特性，是构建现代用户界面的核心组件之一。