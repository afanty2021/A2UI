# 分隔线组件 (Divider)

<cite>
**本文档中引用的文件**  
- [divider.ts](file://renderers/lit/src/0.8/ui/divider.ts)
- [divider.ts](file://renderers/angular/src/lib/catalog/divider.ts)
- [standard_catalog_definition.json](file://specification/0.9/json/standard_catalog_definition.json)
- [components.ts](file://renderers/lit/src/0.8/types/components.ts)
</cite>

## 目录
1. [简介](#简介)
2. [属性说明](#属性说明)
3. [使用示例](#使用示例)
4. [样式与主题](#样式与主题)
5. [响应式设计](#响应式设计)

## 简介
分隔线组件（Divider）是A2UI框架中的一个基础UI元素，用于在内容区域之间创建视觉上的分隔。它不支持子组件或用户交互事件，其主要功能是通过水平或垂直的线条来组织和区分界面中的不同部分。该组件在Lit和Angular框架下均有一致的实现，确保跨平台的一致性。

**分隔线组件的关键特性包括：**
- 仅用于视觉分隔，无交互功能
- 支持水平和垂直两种方向
- 可通过主题系统进行样式定制
- 在不同设备上具有良好的响应式表现

**分隔线组件的实现文件：**
- Lit框架实现：`renderers/lit/src/0.8/ui/divider.ts`
- Angular框架实现：`renderers/angular/src/lib/catalog/divider.ts`

**分隔线组件的JSON定义文件：**
- `specification/0.9/json/standard_catalog_definition.json`

**分隔线组件的类型定义文件：**
- `renderers/lit/src/0.8/types/components.ts`

## 属性说明
根据`standard_catalog_definition.json`文件中的定义，分隔线组件具有以下属性：

### axis (方向)
- **类型**: string
- **默认值**: "horizontal"
- **作用**: 定义分隔线的方向。可选值为"horizontal"（水平）和"vertical"（垂直）。水平分隔线常用于段落之间，垂直分隔线常用于侧边栏与主内容区之间。

### color (颜色)
- **类型**: string
- **默认值**: "#ccc" (浅灰色)
- **作用**: 定义分隔线的颜色。可以是十六进制颜色代码（如"#FF0000"表示红色）或语义化颜色名称。该属性允许开发者根据主题需求自定义分隔线颜色。

### thickness (厚度)
- **类型**: number
- **默认值**: 1
- **作用**: 定义分隔线的像素厚度。数值越大，分隔线越粗。该属性提供了对分隔线视觉权重的精细控制。

**分隔线组件的属性定义文件：**
- `specification/0.9/json/standard_catalog_definition.json`
- `renderers/lit/src/0.8/types/components.ts`

## 使用示例
以下是分隔线组件在Lit和Angular框架下通过JSON定义的使用示例。

### Lit框架示例
```json
{
  "id": "content-divider",
  "component": {
    "Divider": {
      "axis": "horizontal"
    }
  }
}
```

### Angular框架示例
```json
{
  "id": "sidebar-divider",
  "component": {
    "Divider": {
      "axis": "vertical"
    }
  }
}
```

### 在卡片组件之间插入分隔线
以下示例展示了如何在两个卡片组件之间插入一条水平分隔线：

```json
{
  "id": "main-column",
  "component": {
    "Column": {
      "children": {
        "explicitList": [
          "card-1",
          "divider-1",
          "card-2"
        ]
      }
    }
  }
},
{
  "id": "card-1",
  "component": {
    "Card": {
      "child": "card-content-1"
    }
  }
},
{
  "id": "divider-1",
  "component": {
    "Divider": {
      "axis": "horizontal"
    }
  }
},
{
  "id": "card-2",
  "component": {
    "Card": {
      "child": "card-content-2"
    }
  }
}
```

**分隔线组件的使用示例文件：**
- `samples/agent/adk/rizzcharts/examples/standard_catalog/chart.json`

## 样式与主题
分隔线组件的样式可以通过主题系统进行定制。在Lit和Angular框架中，分隔线都使用`<hr>`元素实现，并通过CSS类和内联样式进行控制。

### 默认样式
分隔线的默认样式如下：
- 高度：1px
- 背景颜色：#ccc（浅灰色）
- 无边框

### 主题定制
开发者可以通过主题配置文件覆盖分隔线的默认样式。在主题对象中，可以通过`components.Divider`属性来定义分隔线的CSS类，或通过`additionalStyles?.Divider`属性来定义内联样式。

### 主题文件结构
```json
{
  "components": {
    "Divider": "custom-divider-class"
  },
  "additionalStyles": {
    "Divider": {
      "background": "#000000",
      "height": "2px"
    }
  }
}
```

**分隔线组件的样式文件：**
- `renderers/lit/src/0.8/ui/divider.ts`
- `renderers/angular/src/lib/catalog/divider.ts`

## 响应式设计
分隔线组件在不同屏幕尺寸下具有良好的响应式表现。

### 水平分隔线
- 在桌面端：通常作为段落或卡片之间的分隔
- 在移动端：自动适应屏幕宽度，保持视觉分隔功能

### 垂直分隔线
- 在桌面端：常用于侧边栏与主内容区之间
- 在移动端：通常隐藏或转换为水平分隔线，以适应窄屏幕

### 响应式策略
分隔线组件的响应式设计遵循以下原则：
1. **自适应宽度**：水平分隔线自动填充其容器的宽度
2. **自适应高度**：垂直分隔线自动填充其容器的高度
3. **主题一致性**：在不同设备上保持一致的视觉风格
4. **性能优化**：使用轻量级的`<hr>`元素，确保渲染性能

**分隔线组件的响应式设计实现文件：**
- `renderers/lit/src/0.8/ui/divider.ts`
- `renderers/angular/src/lib/catalog/divider.ts`