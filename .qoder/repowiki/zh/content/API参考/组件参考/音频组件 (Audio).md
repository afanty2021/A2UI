# 音频组件 (Audio)

<cite>
**本文档中引用的文件**  
- [audio.ts](file://renderers/angular/src/lib/catalog/audio.ts)
- [audio.ts](file://renderers/lit/src/0.8/ui/audio.ts)
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json)
- [common_types.json](file://specification/0.9/json/common_types.json)
</cite>

## 目录
1. [简介](#简介)
2. [核心属性](#核心属性)
3. [事件](#事件)
4. [插槽支持](#插槽支持)
5. [框架使用示例](#框架使用示例)
6. [无障碍访问](#无障碍访问)
7. [响应式设计](#响应式设计)

## 简介
A2UI音频组件（AudioPlayer）是一个用于嵌入和控制音频播放的UI组件。该组件基于HTML5的`<audio>`元素构建，提供基本的音频播放功能，包括播放、暂停和音量控制。组件通过JSON定义，可以在Lit和Angular等不同框架中使用。其设计遵循A2UI标准目录规范，确保跨平台的一致性。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L214-L236)

## 核心属性
音频组件的核心属性定义在标准目录定义中，所有属性都必须符合JSON Schema规范。

| 属性 | 类型 | 是否必需 | 默认值 | 作用 |
|------|------|----------|--------|------|
| `component` | 字符串 | 是 | 无 | 组件类型标识符，必须为`"AudioPlayer"`，用于在渲染器中识别组件类型。 |
| `id` | 字符串 | 是 | 无 | 组件的唯一标识符，继承自`ComponentCommon`。 |
| `weight` | 数字 | 否 | `"initial"` | 组件在布局容器中的相对权重，类似于CSS的`flex-grow`属性。 |
| `url` | 字符串或路径 | 是 | 无 | 音频文件的源URL。可以是直接的字符串值，也可以是数据模型中的路径引用。 |
| `description` | 字符串或路径 | 否 | 无 | 音频内容的描述，如标题或摘要，可用于无障碍访问。 |

**属性类型说明**:
- **字符串或路径 (stringOrPath)**: 该类型允许属性值既可以是直接的字符串字面量，也可以是引用数据模型中某个值的路径对象。例如：`"https://example.com/audio.mp3"` 或 `{ "path": "data.audioUrl" }`。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L214-L236)
- [common_types.json](file://specification/0.9/json/common_types.json#L7-L20)

## 事件
音频组件通过底层的HTML5 `<audio>` 元素触发标准的媒体事件。这些事件可以被上层应用捕获和处理，以实现自定义的交互逻辑。

- **`play`**: 当音频开始播放或从暂停状态恢复时触发。
- **`pause`**: 当音频播放被暂停时触发。
- **`ended`**: 当音频播放自然结束时触发。

这些事件是原生DOM事件，可以通过JavaScript的事件监听器进行捕获。例如，在Lit组件中，可以通过在`render`方法中为`<audio>`元素添加`@play`、`@pause`和`@ended`监听器来处理这些事件。

**Section sources**
- [audio.ts](file://renderers/lit/src/0.8/ui/audio.ts#L52-L84)

## 插槽支持
A2UI音频组件**不支持插槽（slots）**。该组件是一个原子性的媒体播放器，其内部结构是固定的，由一个`<section>`容器包裹着一个`<audio>`元素组成。用户无法向组件内部注入自定义的HTML内容或子组件。所有配置都必须通过组件的属性（如`url`和`description`）来完成。

**Section sources**
- [audio.ts](file://renderers/lit/src/0.8/ui/audio.ts#L86-L94)
- [audio.ts](file://renderers/angular/src/lib/catalog/audio.ts#L26-L29)

## 框架使用示例
以下是在Lit和Angular框架中通过JSON定义音频组件的示例。

### Lit 框架示例
```json
{
  "type": "AudioPlayer",
  "id": "audio1",
  "url": "path/to/audio.mp3",
  "description": "背景音乐"
}
```

### Angular 框架示例
```json
{
  "type": "AudioPlayer",
  "id": "audio1",
  "url": { "literalString": "path/to/audio.mp3" },
  "description": { "literalString": "背景音乐" }
}
```

**说明**:
- 在Lit框架中，`url`可以直接使用字符串字面量。
- 在Angular框架中，由于使用了`Primitives.StringValue`类型，`url`需要包装成一个包含`literalString`或`path`的对象。
- 两个示例都遵循了`standard_catalog_definition.json`中的定义，确保了属性的准确性和一致性。

**Section sources**
- [audio.ts](file://renderers/lit/src/0.8/ui/audio.ts#L28-L29)
- [audio.ts](file://renderers/angular/src/lib/catalog/audio.ts#L48-L49)

## 无障碍访问
音频组件通过`description`属性支持无障碍访问（Accessibility）。该属性可以为音频内容提供文本描述，帮助屏幕阅读器用户理解音频的上下文和内容。例如，`description`可以设置为音频的标题、艺术家或简短摘要。虽然组件本身没有直接实现`aria-label`，但`description`属性在语义上起到了类似的作用，为辅助技术提供了必要的信息。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L227-L230)

## 响应式设计
音频组件具有良好的响应式设计，能够适应不同屏幕尺寸。

- **布局**: 组件的根元素（`:host`）被设置为`display: block`，并使用`flex: var(--weight)`，使其能够根据父容器的布局（如Row或Column）进行伸缩。
- **尺寸**: 内部的`<audio>`元素被设置为`width: 100%`，确保其宽度会随着容器的宽度变化而自动调整，始终填满可用空间。
- **溢出**: 组件设置了`min-height: 0`和`overflow: auto`，这可以防止在某些Flexbox布局中出现的溢出问题。

这些CSS样式确保了音频组件在移动设备和桌面设备上都能正常显示和使用。

```mermaid
flowchart TD
A[音频组件] --> B[根元素 :host]
B --> C[display: block]
B --> D[flex: var(--weight)]
B --> E[min-height: 0]
B --> F[overflow: auto]
A --> G[音频元素 audio]
G --> H[display: block]
G --> I[width: 100%]
G --> J[box-sizing: border-box]
```

**Diagram sources**
- [audio.ts](file://renderers/lit/src/0.8/ui/audio.ts#L38-L48)
- [audio.ts](file://renderers/angular/src/lib/catalog/audio.ts#L33-L44)