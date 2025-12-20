# 视频组件 (Video)

<cite>
**本文档中引用的文件**   
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json)
- [video.ts](file://renderers/lit/src/0.8/ui/video.ts)
- [video.ts](file://renderers/angular/src/lib/catalog/video.ts)
- [common_types.json](file://specification/0.9/json/common_types.json)
</cite>

## 目录
1. [简介](#简介)
2. [核心属性](#核心属性)
3. [事件](#事件)
4. [使用示例](#使用示例)
5. [无障碍访问与响应式行为](#无障碍访问与响应式行为)

## 简介
A2UI视频组件用于在用户界面中嵌入和播放视频内容。该组件基于标准HTML5视频元素构建，支持通过JSON定义的多种配置选项，包括视频源、控制条显示、自动播放等。本组件在Lit和Angular渲染器中均有一致的实现，确保跨框架的兼容性。

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L197-L213)

## 核心属性
视频组件支持以下属性，这些属性定义了视频的外观和行为：

| 属性 | 类型 | 默认值 | 作用 |
|------|------|--------|------|
| `src` | `string` 或 `{ path: string }` | 无 | 指定视频文件的URL地址。可以是直接的字符串值，也可以是数据模型中的路径引用。 |
| `controls` | `boolean` | `true` | 决定是否显示视频播放控制条（包含播放/暂停、音量、进度条等控件）。 |
| `autoplay` | `boolean` | `false` | 指示视频是否在加载后自动开始播放。出于用户体验考虑，此功能可能受浏览器策略限制。 |
| `muted` | `boolean` | `false` | 设置视频是否以静音状态开始播放。常与`autoplay`结合使用以满足浏览器的自动播放策略。 |
| `poster` | `string` 或 `{ path: string }` | 无 | 指定视频开始播放前显示的封面图URL。 |

**Section sources**
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json#L197-L213)
- [common_types.json](file://specification/0.9/json/common_types.json#L7-L19)

## 事件
视频组件通过A2UI事件系统触发以下事件，允许应用对视频播放状态进行响应：

- **`play`**: 当视频开始或恢复播放时触发。
- **`pause`**: 当视频播放被暂停时触发。
- **`ended`**: 当视频播放到达末尾时触发。

这些事件遵循A2UI标准事件模式，可以通过`a2uiaction`自定义事件进行监听和处理。

**Section sources**
- [events.ts](file://renderers/lit/src/0.8/events/events.ts#L35-L37)
- [a2ui.ts](file://renderers/lit/src/0.8/events/a2ui.ts#L23-L28)

## 使用示例
以下是在Lit和Angular框架中通过JSON定义视频组件的示例：

```json
{
  "type": "video",
  "src": "path/to/video.mp4",
  "controls": true,
  "autoplay": false,
  "muted": false,
  "poster": "path/to/poster.jpg"
}
```

在Angular框架中，该组件被实现为`a2ui-video`指令，继承自`DynamicComponent`基类，并通过`input`装饰器接收`url`属性（对应JSON中的`src`）。

在Lit框架中，该组件被实现为名为`a2ui-video`的自定义元素，使用`@property`装饰器定义`url`属性，并在渲染时解析该值以生成`<video>`标签。

**Section sources**
- [video.ts](file://renderers/lit/src/0.8/ui/video.ts#L27-L96)
- [video.ts](file://renderers/angular/src/lib/catalog/video.ts#L21-L50)

## 无障碍访问与响应式行为
### 无障碍访问
视频组件通过内置的播放控制条提供了基本的键盘导航支持。为了提高无障碍性，建议始终提供有意义的`poster`封面图，并确保视频内容有相应的文字描述或字幕。

### 响应式行为
视频组件具有自适应宽高比特性。其CSS样式设置`width: 100%`，确保视频在其容器内完全填充宽度。同时，`box-sizing: border-box`的应用保证了内边距和边框不会影响整体布局。组件的`flex: var(--weight)`属性使其能够根据父容器的布局指令（如Row或Column）进行灵活伸缩。

**Section sources**
- [video.ts](file://renderers/lit/src/0.8/ui/video.ts#L31-L50)
- [video.ts](file://renderers/angular/src/lib/catalog/video.ts#L32-L45)