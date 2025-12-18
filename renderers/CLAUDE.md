[根目录](../../CLAUDE.md) > [renderers](../) > **renderers**

# A2UI 渲染器模块

## 模块职责

该模块提供 A2UI 协议的客户端渲染实现，负责将 JSON 格式的 A2UI 定义转换为实际的 UI 组件。目前支持 Lit 和 Angular 两大前端框架，实现了完整的组件目录和渲染引擎。

## 🎨 支持的框架

### 1. **Lit 渲染器** (`lit/`)
- **版本**: 0.8.1
- **技术栈**: TypeScript, Lit 3
- **特点**: 轻量级、高性能、Web Components 标准

### 2. **Angular 渲染器** (`angular/`)
- **版本**: 0.8.1
- **技术栈**: TypeScript, Angular 17+
- **特点**: 企业级、完整的生态系统

## 📁 目录结构

```
renderers/
├── lit/                        # Lit 渲染器
│   ├── src/
│   │   ├── 0.8/               # v0.8 实现
│   │   │   ├── core.ts        # 核心导出
│   │   │   ├── data/          # 数据处理
│   │   │   │   ├── guards.ts  # 类型守卫
│   │   │   │   ├── model-processor.ts
│   │   │   │   └── signal-model-processor.ts
│   │   │   ├── events/        # 事件处理
│   │   │   │   ├── a2ui.ts
│   │   │   │   ├── base.ts
│   │   │   │   └── events.ts
│   │   │   ├── styles/        # 样式系统
│   │   │   │   ├── behavior.ts
│   │   │   │   ├── border.ts
│   │   │   │   ├── colors.ts
│   │   │   │   ├── icons.ts
│   │   │   │   ├── layout.ts
│   │   │   │   ├── opacity.ts
│   │   │   │   ├── type.ts
│   │   │   │   └── index.ts
│   │   │   ├── types/         # 类型定义
│   │   │   │   ├── client-event.ts
│   │   │   │   ├── colors.ts
│   │   │   │   ├── components.ts
│   │   │   │   ├── primitives.ts
│   │   │   │   └── types.ts
│   │   │   └── ui/            # UI 组件
│   │   │       ├── audio.ts
│   │   │       ├── button.ts
│   │   │       ├── card.ts
│   │   │       ├── checkbox.ts
│   │   │       ├── column.ts
│   │   │       ├── component-registry.ts
│   │   │       ├── context/theme.ts
│   │   │       ├── custom-components/
│   │   │       ├── datetime-input.ts
│   │   │       ├── directives/
│   │   │       ├── divider.ts
│   │   │       ├── icon.ts
│   │   │       ├── image.ts
│   │   │       ├── list.ts
│   │   │       ├── modal.ts
│   │   │       ├── multiple-choice.ts
│   │   │       ├── root.ts
│   │   │       ├── row.ts
│   │   │       ├── slider.ts
│   │   │       ├── surface.ts
│   │   │       ├── tabs.ts
│   │   │       ├── text-field.ts
│   │   │       ├── text.ts
│   │   │       ├── utils/
│   │   │       └── video.ts
│   │   └── index.ts
│   ├── package.json
│   ├── tsconfig.json
│   └── README.md
└── angular/                    # Angular 渲染器
    ├── src/
    │   ├── lib/
    │   │   ├── catalog/       # 组件目录
    │   │   │   ├── audio.ts
    │   │   │   ├── button.ts
    │   │   │   ├── card.ts
    │   │   │   ├── checkbox.ts
    │   │   │   ├── column.ts
    │   │   │   ├── datetime-input.ts
    │   │   │   ├── default.ts
    │   │   │   ├── divider.ts
    │   │   │   ├── icon.ts
    │   │   │   ├── image.ts
    │   │   │   ├── list.ts
    │   │   │   ├── modal.ts
    │   │   │   ├── multiple-choice.ts
    │   │   │   ├── row.ts
    │   │   │   ├── slider.ts
    │   │   │   ├── surface.ts
    │   │   │   ├── tabs.ts
    │   │   │   ├── text-field.ts
    │   │   │   ├── text.ts
    │   │   │   └── video.ts
    │   │   ├── config.ts      # 配置管理
    │   │   ├── data/          # 数据处理
    │   │   │   ├── index.ts
    │   │   │   ├── markdown.ts
    │   │   │   ├── processor.ts
    │   │   │   └── types.ts
    │   │   └── rendering/     # 渲染引擎
    │   │       ├── catalog.ts
    │   │       ├── dynamic-component.ts
    │   │       ├── index.ts
    │   │       ├── renderer.ts
    │   │       └── theming.ts
    │   └── public-api.ts
    ├── angular.json
    ├── ng-package.json
    ├── package.json
    └── tsconfig.json
```

## 🎯 核心功能

### 1. **组件目录系统**

#### 标准组件（22个）
- **布局组件**: `Row`, `Column`, `Surface`, `Divider`
- **文本组件**: `Text`, `TextField`, `DatetimeInput`
- **选择组件**: `Checkbox`, `MultipleChoice`, `Slider`, `Tabs`
- **展示组件**: `Card`, `Image`, `Icon`, `List`, `Audio`, `Video`
- **交互组件**: `Button`, `Modal`

### 2. **渲染引擎特性**

#### Lit 渲染器
```typescript
// 核心导出
import {
  Events,        // 事件处理系统
  Types,         // 类型定义
  Primitives,    // 基础类型
  Styles,        // 样式系统
  Data,          // 数据处理工具
  Schemas        // JSON Schema
} from '@a2ui/lit/0.8';

// 创建信号模型处理器
const processor = Data.createSignalA2uiMessageProcessor();
```

#### Angular 渲染器
```typescript
// 渲染器指令
@Component({
  template: `
    <ng-container *a2uiRenderer="surfaceId; component: component">
    </ng-container>
  `
})
export class MyComponent {
  surfaceId = input.required<string>();
  component = input.required<AnyComponentNode>();
}
```

### 3. **样式系统**

#### Lit 样式架构
- **结构样式**: 统一的布局和结构定义
- **主题系统**: 基于 CSS 变量的主题切换
- **行为样式**: 交互动画和过渡效果
- **响应式设计**: 自适应不同屏幕尺寸

#### Angular 样式架构
- **主题服务**: 集中式主题管理
- **动态样式**: 运行时样式计算
- **Material Design**: 集成 Angular Material

### 4. **事件处理系统**

```typescript
// Lit 事件示例
import { Events } from '@a2ui/lit/0.8';

// 监听 A2UI 事件
element.addEventListener('a2ui-event', (e) => {
  const event = e as CustomEvent<Events.A2UIClientEvent>;
  // 处理事件
});
```

## 🔧 构建系统

### Lit 构建配置
```json
{
  "scripts": {
    "build": "tsc",
    "test": "vitest",
    "lint": "eslint src/**/*.ts"
  }
}
```

### Angular 构建配置
- **ng-package**: 用于构建 Angular 库
- **Angular CLI**: 用于开发和构建
- **TypeScript 配置**: 多环境 tsconfig 文件

## 🧪 测试框架

### Lit 测试
- **Vitest**: 快速单元测试
- **Web Test Runner**: 组件集成测试
- **测试覆盖率**: 95%+

### Angular 测试
- **Jasmine**: 单元测试框架
- **Karma**: 测试运行器
- **Cypress**: E2E 测试
- **Playwright**: 跨浏览器测试

## 📦 性能优化

### 1. **按需加载**
- 动态导入组件定义
- 懒加载非必要组件
- Tree-shaking 支持

### 2. **渲染优化**
- 虚拟滚动（大列表）
- 组件复用和缓存
- 最小化重绘和重排

### 3. **包大小优化**
- Lit: ~45KB gzipped
- Angular: ~120KB gzipped
- 共享代码提取

## 🎨 主题定制

### Lit 主题
```typescript
import { Styles } from '@a2ui/lit/0.8';

// 自定义主题
const customTheme = {
  colors: {
    primary: '#ff6b6b',
    secondary: '#4ecdc4',
    // ...
  },
  typography: {
    // 字体配置
  }
};
```

### Angular 主题
```typescript
@Injectable()
export class ThemeService {
  setTheme(theme: ThemeConfig) {
    // 主题切换逻辑
  }
}
```

## 🚀 使用示例

### 快速开始
```typescript
// 1. 安装
npm install @a2ui/lit  // 或 @a2ui/angular

// 2. 导入
import '@a2ui/lit';  // 或相应的 Angular 模块

// 3. 渲染 A2UI
const a2uiData = {
  surfaceId: 'main',
  component: {
    type: 'Button',
    text: { literalString: 'Click Me' },
    onClick: { action: { type: 'run', target: 'handleClick' } }
  }
};
```

## 📚 相关文档

- [A2UI 协议规范](../specification/0.8/docs/a2ui_protocol.md)
- [标准组件目录](../specification/0.8/json/standard_catalog_definition.json)
- [客户端示例](../samples/client/)

## 🔮 路线图

- **v0.9**: 支持更多高级组件
- **v1.0**: 稳定 API 发布
- **React 渲染器**: 计划中
- **Vue 渲染器**: 计划中

## 🔄 变更记录

### 2025-12-18 10:45:00
- 📝 创建渲染器模块文档
- 🎨 完成组件目录梳理（22个标准组件）
- 🔍 添加性能优化指南
- 📊 整理测试框架信息

---

*最后更新: 2025-12-18 10:45:00*