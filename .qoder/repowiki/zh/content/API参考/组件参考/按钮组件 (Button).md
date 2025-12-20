# 按钮组件 (Button)

<cite>
**本文档中引用的文件**  
- [button.ts](file://renderers/lit/src/0.8/ui/button.ts)
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json)
- [components.md](file://docs/reference/components.md)
- [components.ts](file://renderers/lit/src/0.8/types/components.ts)
- [events.ts](file://renderers/lit/src/0.8/events/events.ts)
</cite>

## 目录
1. [简介](#简介)
2. [核心属性](#核心属性)
3. [事件](#事件)
4. [内容定义](#内容定义)
5. [框架使用示例](#框架使用示例)
6. [无障碍访问与响应式行为](#无障碍访问与响应式行为)

## 简介
A2UI按钮组件是用户交互的主要入口点，用于触发客户端操作。该组件通过`@property`装饰器在`button.ts`文件中的`A2uiButton`类中定义，并遵循`standard_catalog_definition.json`中的规范。按钮可以包含文本或图标，并支持多种样式变体和交互状态。

**Section sources**
- [button.ts](file://renderers/lit/src/0.8/ui/button.ts#L26-L65)
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L431-L468)

## 核心属性
从`standard_catalog_definition.json`文件中提取的按钮组件完整属性列表如下：

- **`child`**:
  - **类型**: `string`
  - **默认值**: 无
  - **作用**: 指定要在按钮内显示的子组件的ID。通常是一个`Text`组件的ID，用于显示按钮文本。

- **`primary`**:
  - **类型**: `boolean`
  - **默认值**: `false`
  - **作用**: 指示按钮是否应作为主要操作按钮进行样式化。

- **`action`**:
  - **类型**: `object`
  - **默认值**: 无
  - **作用**: 定义在按钮点击时触发的客户端操作，包含操作名称和可选的上下文载荷。

- **`disabled`**:
  - **类型**: `boolean`
  - **默认值**: `false`
  - **作用**: 指示按钮是否被禁用，禁用状态下按钮不可点击且视觉上呈现为灰色。

- **`icon`**:
  - **类型**: `string`
  - **默认值**: 无
  - **作用**: 指定按钮前缀图标的名称，使用Material Icons或其他自定义图标集。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L431-L468)
- [components.ts](file://renderers/lit/src/0.8/types/components.ts#L131-L141)

## 事件
按钮组件触发`click`事件，其载荷结构如下：

- **`eventType`**: `"a2ui.action"`
- **`action`**: 包含操作名称和上下文的`Action`对象
- **`dataContextPath`**: 数据上下文路径
- **`sourceComponentId`**: 触发事件的组件ID
- **`sourceComponent`**: 触发事件的组件实例

该事件通过`StateEvent`类派发，定义在`events.ts`文件中。

**Section sources**
- [events.ts](file://renderers/lit/src/0.8/events/events.ts#L35-L47)
- [button.ts](file://renderers/lit/src/0.8/ui/button.ts#L48-L60)

## 内容定义
按钮文本可以通过`content`属性或子节点定义。在JSON定义中，通常通过引用一个`Text`组件的ID来实现。例如：

```json
{
  "id": "submit-btn-text",
  "component": {
    "Text": {
      "text": { "literalString": "提交" }
    }
  }
}
```

然后在按钮组件中引用该ID：

```json
{
  "id": "submit-btn",
  "component": {
    "Button": {
      "child": "submit-btn-text",
      "primary": true,
      "action": {"name": "submit_form"}
    }
  }
}
```

**Section sources**
- [components.md](file://docs/reference/components.md#L121-L140)
- [button.ts](file://renderers/lit/src/0.8/ui/button.ts#L62)

## 框架使用示例
以下是Lit和Angular框架下通过JSON定义按钮的代码示例：

### Lit框架示例
```json
{ 
  "type": "button", 
  "label": "提交", 
  "variant": "primary" 
}
```

### Angular框架示例
```json
{ 
  "type": "button", 
  "label": "提交", 
  "variant": "primary" 
}
```

这些示例展示了如何在不同框架中使用相同的JSON结构来定义按钮组件。

**Section sources**
- [components.md](file://docs/reference/components.md#L121-L140)
- [gallery.component.ts](file://samples/client/angular/projects/gallery/src/app/features/gallery/gallery.component.ts#L157-L160)

## 无障碍访问与响应式行为
按钮组件支持键盘导航，用户可以通过Tab键聚焦到按钮，并通过Enter或Space键触发点击事件。组件还具有响应式行为，能够根据容器大小自动调整布局和样式，确保在不同设备上都能提供良好的用户体验。

**Section sources**
- [button.ts](file://renderers/lit/src/0.8/ui/button.ts#L48-L60)
- [input-area.html](file://samples/client/angular/projects/a2a-chat-canvas/src/lib/components/chat/input-area/input-area.html#L17-L60)