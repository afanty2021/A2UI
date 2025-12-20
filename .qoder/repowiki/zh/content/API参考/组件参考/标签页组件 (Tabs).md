# 标签页组件 (Tabs)

<cite>
**本文档中引用的文件**  
- [tabs.ts](file://renderers/lit/src/0.8/ui/tabs.ts)
- [tabs.ts](file://renderers/angular/src/lib/catalog/tabs.ts)
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json)
- [components.ts](file://renderers/lit/src/0.8/types/components.ts)
- [root.ts](file://renderers/lit/src/0.8/ui/root.ts)
</cite>

## 目录
1. [简介](#简介)
2. [核心属性](#核心属性)
3. [事件](#事件)
4. [内容结构](#内容结构)
5. [框架实现示例](#框架实现示例)
6. [无障碍访问与响应式行为](#无障碍访问与响应式行为)

## 简介

标签页组件 (Tabs) 是一种用于组织内容的UI控件，它允许用户在多个可切换的面板之间进行导航。每个标签页都有一个标题，用户点击标题即可切换到对应的面板内容。该组件在A2UI框架中被广泛用于信息分组和界面空间优化。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L357-L388)

## 核心属性

根据 `standard_catalog_definition.json` 文件中的定义，标签页组件的核心属性如下：

- **`tabItems`** (数组)
  - **类型**: `Array<{ title: StringValue, child: string }>`
  - **描述**: 定义标签页列表的数组。数组中的每个对象代表一个标签页，包含其标题和关联的内容组件。
  - **作用**: 用于配置标签页的数量、标题文本以及每个标签页所显示的内容。
  - **说明**: `title` 字段支持静态字符串 (`literalString`) 或指向数据模型路径的动态绑定 (`path`)。`child` 字段必须是另一个已定义组件的唯一ID。

在实现层面，组件内部使用以下属性来管理状态：

- **`titles`** (数组)
  - **类型**: `StringValue[] | null`
  - **默认值**: `null`
  - **作用**: 从 `tabItems` 属性中提取出的所有标题的集合，供UI渲染使用。

- **`selected`** (数字)
  - **类型**: `number`
  - **默认值**: `0`
  - **作用**: 表示当前激活的标签页的索引。当用户点击某个标签时，此值会更新，并触发内容区域的重新渲染。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L364-L387)
- [tabs.ts](file://renderers/lit/src/0.8/ui/tabs.ts#L31-L34)
- [components.ts](file://renderers/lit/src/0.8/types/components.ts#L80-L103)

## 事件

标签页组件通过改变其内部状态（`selected` 索引）来响应用户的交互。虽然组件本身没有显式地定义一个名为 `tabChange` 的自定义事件，但其行为逻辑等效于触发了一个 `tabChange` 事件。

- **行为**: 当用户点击一个非活动状态的标签按钮时，组件会将 `selected` 属性更新为该标签的索引。
- **效果**: 这个状态变化会立即导致组件重新渲染，从而显示与新选中标签关联的 `child` 组件内容。
- **监听**: 外部系统或父组件可以通过观察组件的 `selected` 属性变化来实现类似 `tabChange` 事件的监听逻辑。

**Section sources**
- [tabs.ts](file://renderers/lit/src/0.8/ui/tabs.ts#L108-L110)
- [tabs.ts](file://renderers/angular/src/lib/catalog/tabs.ts#L33)

## 内容结构

标签页组件的内容结构设计为高度灵活，支持复杂的组件树。

- **`content` 的本质**: 在A2UI架构中，`content` 并非直接内联在 `tabItems` 中的HTML或文本。相反，`tabItems` 中的 `child` 字段引用了另一个独立的组件ID。
- **组件树**: 被引用的 `child` 组件本身可以是一个简单的文本或图片，也可以是一个复杂的布局组件（如 `Column` 或 `Row`），在其内部再嵌套其他任意数量和类型的组件。
- **实现机制**: 在 `root.ts` 文件中，`Tabs` 组件的渲染逻辑会遍历 `tabItems`，将每个 `title` 和 `child` 分别提取到 `titles` 和 `childComponents` 数组中，并将这些数据传递给 `a2ui-tabs` 自定义元素。这使得标签页的内容可以是任意深度的组件树。

```mermaid
graph TD
A[Tabs 组件] --> B[tabItems]
B --> C[标签1]
B --> D[标签2]
C --> E[标题: "概览"]
C --> F[子组件: card-overview]
D --> G[标题: "详情"]
D --> H[子组件: column-details]
F --> I[Card 组件]
H --> J[Column 组件]
J --> K[Text 组件]
J --> L[Image 组件]
J --> M[Button 组件]
```

**Diagram sources**
- [root.ts](file://renderers/lit/src/0.8/ui/root.ts#L444-L467)
- [tabs.ts](file://renderers/lit/src/0.8/ui/tabs.ts#L122-L131)

## 框架实现示例

以下是如何在Lit和Angular框架中通过JSON定义标签页的示例。

### Lit 框架示例

在Lit框架中，标签页通过JSON消息定义。以下是一个创建包含“概览”和“详情”两个面板的标签页组的示例：

```json
{
  "surfaceUpdate": {
    "surfaceId": "main-surface",
    "components": [
      {
        "id": "main-tabs",
        "component": {
          "Tabs": {
            "tabItems": [
              {
                "title": { "literalString": "概览" },
                "child": "overview-panel"
              },
              {
                "title": { "literalString": "详情" },
                "child": "details-panel"
              }
            ]
          }
        }
      },
      {
        "id": "overview-panel",
        "component": {
          "Text": {
            "text": { "literalString": "这里是概览信息。" }
          }
        }
      },
      {
        "id": "details-panel",
        "component": {
          "Column": {
            "children": {
              "explicitList": ["detail-text", "detail-image"]
            }
          }
        }
      },
      {
        "id": "detail-text",
        "component": {
          "Text": {
            "text": { "literalString": "详细描述..." }
          }
        }
      },
      {
        "id": "detail-image",
        "component": {
          "Image": {
            "url": { "literalString": "https://example.com/detail.jpg" }
          }
        }
      }
    ]
  }
}
```

**Section sources**
- [a2ui_examples.py](file://samples/agent/adk/contact_lookup/a2ui_examples.py)

### Angular 框架示例

在Angular框架中，`Tabs` 组件作为一个Angular组件 (`@Component`) 实现，其模板使用Angular的语法。其核心逻辑与Lit版本一致，接收 `tabs` 输入（对应于 `tabItems`），并使用 `*ngFor` 指令来渲染标签按钮。

```typescript
// Angular 组件定义 (简化)
@Component({
  selector: 'a2ui-tabs',
  template: `
    <section>
      <div>
        @for (tab of tabs(); track tab) {
          <button (click)="selectedIndex.set($index)">
            {{ resolvePrimitive(tab.title) }}
          </button>
        }
      </div>
      <ng-container a2ui-renderer [component]="tabs()[selectedIndex()].child" />
    </section>
  `
})
export class Tabs {
  protected selectedIndex = signal(0);
  readonly tabs = input.required<Types.ResolvedTabItem[]>();
}
```

**Section sources**
- [tabs.ts](file://renderers/angular/src/lib/catalog/tabs.ts)

## 无障碍访问与响应式行为

### 无障碍访问 (Accessibility)

为了确保所有用户都能访问，标签页组件应遵循WAI-ARIA标准。

- **`role="tablist"`**: 应应用于包含所有标签按钮的容器（通常是 `<div>` 元素），以将该组元素标识为一个标签列表。
- **`role="tab"`**: 应应用于每个标签按钮，以标识其为可切换的标签。
- **`role="tabpanel"`**: 应应用于当前显示的内容面板，以标识其为标签页的内容区域。
- **`aria-controls` 和 `aria-labelledby`**: 用于建立标签按钮和其对应内容面板之间的关联，帮助屏幕阅读器用户理解结构。

尽管在提供的代码片段中未直接看到这些属性，但它们是实现无障碍标签页的标准实践，应在最终的DOM结构中体现。

### 响应式行为 (Responsive Behavior)

标签页组件的设计应考虑不同屏幕尺寸下的用户体验。

- **桌面端**: 在大屏幕上，标签通常以水平排列的按钮组形式显示。
- **移动端**: 在小屏幕上，为了节省水平空间并提高触摸操作的便利性，标签页组件可能会自动转换为“手风琴”(Accordion) 效果。这意味着标签标题会垂直堆叠，点击一个标题会展开其对应的内容，同时收起其他已展开的内容。
- **实现**: 这种行为通常通过CSS媒体查询和/或JavaScript逻辑来实现，根据视口宽度动态调整布局和交互模式。

**Section sources**
- [tabs.ts](file://renderers/lit/src/0.8/ui/tabs.ts#L67-L70)
- [tabs.ts](file://renderers/angular/src/lib/catalog/tabs.ts#L31-L39)