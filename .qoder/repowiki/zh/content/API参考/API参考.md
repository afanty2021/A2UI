# API参考

<cite>
**本文档中引用的文件**  
- [core.ts](file://renderers/lit/src/0.8/core.ts)
- [component-registry.ts](file://renderers/lit/src/0.8/ui/component-registry.ts)
- [events.ts](file://renderers/lit/src/0.8/events/events.ts)
- [types.ts](file://renderers/lit/src/0.8/types/types.ts)
- [components.ts](file://renderers/lit/src/0.8/types/components.ts)
- [primitives.ts](file://renderers/lit/src/0.8/types/primitives.ts)
- [model-processor.ts](file://renderers/lit/src/0.8/data/model-processor.ts)
- [signal-model-processor.ts](file://renderers/lit/src/0.8/data/signal-model-processor.ts)
- [renderer.ts](file://renderers/angular/src/lib/rendering/renderer.ts)
- [server_to_client.json](file://specification/0.9/json/server_to_client.json)
- [client_to_server.json](file://specification/0.9/json/client_to_server.json)
</cite>

## 目录
1. [渲染器API](#渲染器api)
2. [消息参考](#消息参考)
3. [组件参考](#组件参考)

## 渲染器API

本节详细说明Lit和Angular渲染器提供的公共接口、类、方法和配置选项。

### Lit渲染器API

Lit渲染器通过`core.ts`文件暴露其主要API，包括数据处理、事件系统和类型定义。

**A2uiMessageProcessor类**
`A2uiMessageProcessor`是Lit渲染器的核心消息处理器，负责处理来自服务器的A2UI消息并维护UI表面状态。

- **构造函数参数**:
  - `opts`: 可选配置对象，允许自定义数据结构的构造函数：
    - `mapCtor`: 用于创建Map实例的构造函数（默认为`Map`）
    - `arrayCtor`: 用于创建数组实例的构造函数（默认为`Array`）
    - `setCtor`: 用于创建Set实例的构造函数（默认为`Set`）
    - `objCtor`: 用于创建对象实例的构造函数（默认为`Object`）

- **公共方法**:
  - `getSurfaces()`: 返回当前所有UI表面的只读映射
  - `clearSurfaces()`: 清除所有UI表面的状态
  - `processMessages(messages)`: 处理一批服务器到客户端的消息
  - `getData(node, relativePath, surfaceId)`: 根据组件节点和相对路径从数据模型中获取数据
  - `setData(node, relativePath, value, surfaceId)`: 在数据模型中设置数据
  - `resolvePath(path, dataContextPath)`: 解析相对于数据上下文路径的路径

**componentRegistry对象**
`componentRegistry`是全局组件注册表，用于注册和获取自定义UI组件。

- **register方法**:
  - `typeName`: 组件类型名称（必须为字母数字）
  - `constructor`: 自定义元素构造函数
  - `tagName`: 可选的自定义标签名称（默认为`a2ui-custom-{typeName.toLowerCase()}`）

- **get方法**:
  - `typeName`: 要检索的组件类型名称
  - 返回指定类型的组件构造函数或`undefined`

**事件系统**
Lit渲染器定义了`StateEvent`类，用于在组件之间传播状态变化。

- `StateEvent`继承自`CustomEvent`，事件类型为`a2uiaction`
- 事件负载包含`A2UIAction`类型的详细信息
- 事件具有冒泡、可取消和跨影子边界传播的特性

**核心导出**
`core.ts`文件通过命名空间导出以下模块：
- `Data`: 包含`A2uiMessageProcessor`、`createSignalA2uiMessageProcessor`和`Guards`
- `Events`: 从`events/events.js`导入的事件相关类型和类
- `Types`: 从`types/types.js`导入的类型定义
- `Primitives`: 从`types/primitives.js`导入的基本类型
- `Styles`: 从`styles/index.js`导入的样式定义
- `Schemas`: 包含客户端事件消息的JSON模式

**Section sources**
- [core.ts](file://renderers/lit/src/0.8/core.ts#L1-L36)
- [component-registry.ts](file://renderers/lit/src/0.8/ui/component-registry.ts#L1-L59)
- [events.ts](file://renderers/lit/src/0.8/events/events.ts#L1-L54)
- [types.ts](file://renderers/lit/src/0.8/types/types.ts#L1-L533)
- [model-processor.ts](file://renderers/lit/src/0.8/data/model-processor.ts#L1-L856)

### Angular渲染器API

Angular渲染器提供了一个指令式API，用于在Angular应用中渲染A2UI组件。

**Renderer指令**
`Renderer`是一个Angular指令，用于动态渲染A2UI组件树。

- **输入属性**:
  - `surfaceId`: 必需，指定要渲染的UI表面ID
  - `component`: 必需，指定要渲染的根组件节点

- **依赖注入**:
  - `ViewContainerRef`: 用于动态创建组件
  - `Catalog`: 组件类型目录，映射组件类型到其Angular组件
  - `DOCUMENT`: 用于注入全局样式
  - `PLATFORM_ID`: 用于平台检测

- **生命周期方法**:
  - `ngOnDestroy`: 清理当前渲染的组件并标记指令为已销毁

- **私有方法**:
  - `render(surfaceId, component)`: 根据组件类型和目录配置渲染组件
  - `clear()`: 销毁当前渲染的组件

**Catalog服务**
`Catalog`服务是一个注入令牌，用于提供组件类型到Angular组件的映射。它是一个对象或函数的映射，其中键是组件类型名称，值是返回组件类型的异步函数或包含类型和绑定配置的对象。

**Section sources**
- [renderer.ts](file://renderers/angular/src/lib/rendering/renderer.ts#L1-L110)

## 消息参考

本节以表格形式列出所有A2UI消息类型，包括消息名、方向、载荷结构和示例。

### 服务器到客户端消息

服务器到客户端的消息用于动态构建和更新用户界面。

| 消息名 | 方向 | 载荷结构 | 示例 |
|--------|------|----------|------|
| createSurface | 服务器 -> 客户端 | `{ createSurface: { surfaceId: string, catalogId: string } }` | `{ "createSurface": { "surfaceId": "chat123", "catalogId": "standard" } }` |
| updateComponents | 服务器 -> 客户端 | `{ updateComponents: { surfaceId: string, components: Array<AnyComponentNode> } }` | `{ "updateComponents": { "surfaceId": "chat123", "components": [ { "id": "root", "type": "Text", "properties": { "text": { "literalString": "Hello" }, "usageHint": "body" } } ] } }` |
| updateDataModel | 服务器 -> 客户端 | `{ updateDataModel: { surfaceId: string, path?: string, op?: "add" \| "replace" \| "remove", value?: DataValue } }` | `{ "updateDataModel": { "surfaceId": "chat123", "path": "/user/name", "op": "replace", "value": "John Doe" } }` |
| deleteSurface | 服务器 -> 客户端 | `{ deleteSurface: { surfaceId: string } }` | `{ "deleteSurface": { "surfaceId": "chat123" } }` |

### 客户端到服务器消息

客户端到服务器的消息用于报告用户操作和客户端错误。

| 消息名 | 方向 | 载荷结构 | 示例 |
|--------|------|----------|------|
| userAction | 客户端 -> 服务器 | `{ userAction: { name: string, surfaceId: string, sourceComponentId: string, timestamp: string, context: object } }` | `{ "userAction": { "name": "submitForm", "surfaceId": "chat123", "sourceComponentId": "btn1", "timestamp": "2025-01-01T12:00:00Z", "context": { "userId": "u123" } } }` |
| error | 客户端 -> 服务器 | `{ error: { code: string, surfaceId: string, message: string, path?: string } }` | `{ "error": { "code": "VALIDATION_FAILED", "surfaceId": "chat123", "path": "/components/0/text", "message": "Text is required" } }` |

**Section sources**
- [server_to_client.json](file://specification/0.9/json/server_to_client.json#L1-L115)
- [client_to_server.json](file://specification/0.9/json/client_to_server.json#L1-L98)

## 组件参考

本节为22个标准组件中的每一个创建条目，详细说明其所有属性（props）、事件（events）和插槽（slots），并提供使用示例。

### 基础组件

#### Text（文本）
- **属性**:
  - `text`: `StringValue` - 文本内容，可以是字面量字符串或数据绑定路径
  - `usageHint`: `"h1" \| "h2" \| "h3" \| "h4" \| "h5" \| "caption" \| "body"` - 用法提示，用于样式化
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "text1",
  "type": "Text",
  "properties": {
    "text": { "literalString": "Hello World" },
    "usageHint": "body"
  }
}
```

#### Image（图像）
- **属性**:
  - `url`: `StringValue` - 图像URL，可以是字面量字符串或数据绑定路径
  - `usageHint`: `"icon" \| "avatar" \| "smallFeature" \| "mediumFeature" \| "largeFeature" \| "header"` - 用法提示
  - `fit`: `"contain" \| "cover" \| "fill" \| "none" \| "scale-down"` - 图像填充方式
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "img1",
  "type": "Image",
  "properties": {
    "url": { "literalString": "https://example.com/image.jpg" },
    "usageHint": "avatar"
  }
}
```

#### Icon（图标）
- **属性**:
  - `name`: `StringValue` - 图标名称，可以是字面量字符串或数据绑定路径
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "icon1",
  "type": "Icon",
  "properties": {
    "name": { "literalString": "home" }
  }
}
```

#### Video（视频）
- **属性**:
  - `url`: `StringValue` - 视频URL，可以是字面量字符串或数据绑定路径
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "video1",
  "type": "Video",
  "properties": {
    "url": { "literalString": "https://example.com/video.mp4" }
  }
}
```

#### AudioPlayer（音频播放器）
- **属性**:
  - `url`: `StringValue` - 音频URL，可以是字面量字符串或数据绑定路径
  - `description`: `StringValue` - 描述、标题或占位文本
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "audio1",
  "type": "AudioPlayer",
  "properties": {
    "url": { "literalString": "https://example.com/audio.mp3" },
    "description": { "literalString": "Background Music" }
  }
}
```

### 布局组件

#### Row（行）
- **属性**:
  - `children`: `Array<AnyComponentNode>` - 子组件数组
  - `distribution`: `"start" \| "center" \| "end" \| "spaceBetween" \| "spaceAround" \| "spaceEvenly"` - 子组件分布方式
  - `alignment`: `"start" \| "center" \| "end" \| "stretch"` - 子组件对齐方式
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "row1",
  "type": "Row",
  "properties": {
    "children": ["text1", "img1"],
    "distribution": "spaceBetween",
    "alignment": "center"
  }
}
```

#### Column（列）
- **属性**:
  - `children`: `Array<AnyComponentNode>` - 子组件数组
  - `distribution`: `"start" \| "center" \| "end" \| "spaceBetween" \| "spaceAround" \| "spaceEvenly"` - 子组件分布方式
  - `alignment`: `"start" \| "center" \| "end" \| "stretch"` - 子组件对齐方式
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "col1",
  "type": "Column",
  "properties": {
    "children": ["text1", "text2"],
    "distribution": "start",
    "alignment": "stretch"
  }
}
```

#### List（列表）
- **属性**:
  - `children`: `Array<AnyComponentNode>` - 子组件数组
  - `direction`: `"vertical" \| "horizontal"` - 列表方向
  - `alignment`: `"start" \| "center" \| "end" \| "stretch"` - 子组件对齐方式
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "list1",
  "type": "List",
  "properties": {
    "children": ["item1", "item2", "item3"],
    "direction": "vertical"
  }
}
```

#### Card（卡片）
- **属性**:
  - `child`: `AnyComponentNode` - 卡片内容组件
  - `children`: `Array<AnyComponentNode>` - 子组件数组（可选）
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "card1",
  "type": "Card",
  "properties": {
    "child": "content1"
  }
}
```

#### Tabs（标签页）
- **属性**:
  - `tabItems`: `Array<{ title: StringValue, child: string }>` - 标签项数组，每个包含标题和子组件ID
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "tabs1",
  "type": "Tabs",
  "properties": {
    "tabItems": [
      { "title": { "literalString": "Tab 1" }, "child": "content1" },
      { "title": { "literalString": "Tab 2" }, "child": "content2" }
    ]
  }
}
```

#### Divider（分隔线）
- **属性**:
  - `axis`: `"horizontal" \| "vertical"` - 分隔线方向
  - `color`: `string` - 分隔线颜色
  - `thickness`: `number` - 分隔线厚度
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "divider1",
  "type": "Divider",
  "properties": {
    "axis": "horizontal",
    "color": "#ccc",
    "thickness": 1
  }
}
```

#### Modal（模态框）
- **属性**:
  - `entryPointChild`: `string` - 触发模态框的组件ID
  - `contentChild`: `string` - 模态框内容组件ID
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "modal1",
  "type": "Modal",
  "properties": {
    "entryPointChild": "btn1",
    "contentChild": "content1"
  }
}
```

### 交互组件

#### Button（按钮）
- **属性**:
  - `child`: `string` - 按钮内容组件ID
  - `action`: `Action` - 用户操作定义
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "btn1",
  "type": "Button",
  "properties": {
    "child": "text1",
    "action": {
      "name": "submitForm",
      "context": [
        { "key": "userId", "value": { "path": "/user/id" } }
      ]
    }
  }
}
```

#### Checkbox（复选框）
- **属性**:
  - `label`: `StringValue` - 标签文本
  - `value`: `{ path?: string, literalBoolean?: boolean }` - 值，可以是数据绑定路径或字面量布尔值
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "checkbox1",
  "type": "CheckBox",
  "properties": {
    "label": { "literalString": "Accept Terms" },
    "value": { "path": "/form/accepted" }
  }
}
```

#### TextField（文本框）
- **属性**:
  - `text`: `StringValue` - 输入文本，可以是字面量字符串或数据绑定路径
  - `label`: `StringValue` - 标签、标题或占位文本
  - `type`: `"shortText" \| "number" \| "date" \| "longText"` - 输入类型
  - `validationRegexp`: `string` - 用于验证输入的正则表达式字符串
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "textfield1",
  "type": "TextField",
  "properties": {
    "label": { "literalString": "Email" },
    "type": "shortText",
    "validationRegexp": "^[^@]+@[^@]+\\.[^@]+$"
  }
}
```

#### DateTimeInput（日期时间输入）
- **属性**:
  - `value`: `StringValue` - 值，可以是字面量字符串或数据绑定路径
  - `enableDate`: `boolean` - 是否启用日期选择
  - `enableTime`: `boolean` - 是否启用时间选择
  - `outputFormat`: `string` - 输出字符串格式（例如'YYYY-MM-DD'）
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "datetime1",
  "type": "DateTimeInput",
  "properties": {
    "value": { "path": "/event/date" },
    "enableDate": true,
    "enableTime": true,
    "outputFormat": "YYYY-MM-DD HH:mm"
  }
}
```

#### MultipleChoice（多选）
- **属性**:
  - `selections`: `{ path?: string, literalArray?: string[] }` - 选择项，可以是数据绑定路径或字面量字符串数组
  - `options`: `Array<{ label: StringValue, value: string }>` - 选项数组
  - `maxAllowedSelections`: `number` - 允许的最大选择数
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "multiple1",
  "type": "MultipleChoice",
  "properties": {
    "selections": { "path": "/form/choices" },
    "options": [
      { "label": { "literalString": "Option 1" }, "value": "opt1" },
      { "label": { "literalString": "Option 2" }, "value": "opt2" }
    ],
    "maxAllowedSelections": 2
  }
}
```

#### Slider（滑块）
- **属性**:
  - `value`: `{ path?: string, literalNumber?: number }` - 值，可以是数据绑定路径或字面量数字
  - `minValue`: `number` - 最小值
  - `maxValue`: `number` - 最大值
- **事件**: 无
- **插槽**: 无
- **使用示例**:
```json
{
  "id": "slider1",
  "type": "Slider",
  "properties": {
    "value": { "path": "/settings/volume" },
    "minValue": 0,
    "maxValue": 100
  }
}
```

**Section sources**
- [types.ts](file://renderers/lit/src/0.8/types/types.ts#L1-L533)
- [components.ts](file://renderers/lit/src/0.8/types/components.ts#L1-L212)
- [primitives.ts](file://renderers/lit/src/0.8/types/primitives.ts#L1-L61)