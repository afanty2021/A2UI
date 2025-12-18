[根目录](../../CLAUDE.md) > [specification](../) > **specification**

# A2UI 协议规范模块

## 模块职责

该模块定义了 A2UI (Agent-to-User Interface) 协议的完整规范，包括协议文档、JSON Schema 定义、标准组件目录和评估工具。它是 A2UI 生态系统的基础，确保了不同实现之间的互操作性。

## 📋 版本管理

### 当前版本
- **v0.8**: 稳定版本，广泛使用
- **v0.9**: 预览版本，新增功能
- **v1.0**: 计划中的稳定发布版

## 📁 目录结构

```
specification/
├── 0.8/                        # v0.8 规范
│   ├── docs/                   # 协议文档
│   │   ├── a2ui_extension_specification.md
│   │   ├── a2ui_protocol.md
│   │   └── custom_catalog_changes.md
│   ├── json/                   # JSON Schema 定义
│   │   ├── a2ui_client_capabilities_schema.json
│   │   ├── catalog_description_schema.json
│   │   ├── client_to_server.json
│   │   ├── server_to_client.json
│   │   ├── server_to_client_with_standard_catalog.json
│   │   └── standard_catalog_definition.json
│   └── eval/                   # 评估工具
│       ├── package.json
│       ├── tsconfig.json
│       ├── pnpm-workspace.yaml
│       ├── README.md
│       └── GEMINI.md
└── 0.9/                        # v0.9 规范（预览）
    ├── docs/
    │   ├── a2ui_protocol.md
    │   └── evolution_guide.md
    ├── json/
    │   ├── client_to_server.json
    │   ├── common_types.json
    │   ├── server_to_client.json
    │   └── standard_catalog_definition.json
    └── eval/
        ├── package.json
        └── README.md
```

## 📜 核心规范文档

### 1. **A2UI 协议规范** (`a2ui_protocol.md`)

定义了 A2UI 的核心协议，包括：
- 消息格式和结构
- 数据模型和类型系统
- 组件渲染机制
- 事件处理模型

#### 关键概念
```json
{
  "surfaceId": "unique_surface_identifier",
  "component": {
    "type": "ComponentType",
    "properties": { /* 组件属性 */ },
    "events": { /* 事件绑定 */ }
  },
  "data": { /* 数据绑定 */ }
}
```

### 2. **A2UI 扩展规范** (`a2ui_extension_specification.md`)

定义了 A2UI 在 A2A (Agent-to-Agent) 框架中的扩展：
- 扩展注册机制
- MIME 类型规范
- 能力协商流程

### 3. **自定义目录变更** (`custom_catalog_changes.md`)

描述了如何创建和使用自定义组件目录：
- 目录定义格式
- 组件扩展机制
- 版本兼容性规则

## 🎯 JSON Schema 定义

### 1. **标准组件目录** (`standard_catalog_definition.json`)

定义了 22 个标准 UI 组件的完整规范：

#### 布局组件
- **Row**: 水平布局容器
- **Column**: 垂直布局容器
- **Surface**: 通用容器组件
- **Divider**: 分割线组件

#### 文本组件
- **Text**: 文本展示组件
- **TextField**: 文本输入组件
- **DatetimeInput**: 日期时间选择器

#### 选择组件
- **Checkbox**: 复选框组件
- **MultipleChoice**: 多选组件
- **Slider**: 滑块组件
- **Tabs**: 标签页组件

#### 展示组件
- **Card**: 卡片容器
- **Image**: 图片展示
- **Icon**: 图标组件
- **List**: 列表组件
- **Audio**: 音频播放器
- **Video**: 视频播放器

#### 交互组件
- **Button**: 按钮组件
- **Modal**: 模态框组件

### 2. **通信协议 Schema**

#### 客户端到服务器 (`client_to_server.json`)
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "A2UI Client to Server Message",
  "type": "object",
  "properties": {
    "type": { "type": "string", "enum": ["a2ui-event"] },
    "surfaceId": { "type": "string" },
    "eventName": { "type": "string" },
    "eventData": { "type": "object" },
    "timestamp": { "type": "string", "format": "date-time" }
  }
}
```

#### 服务器到客户端 (`server_to_client.json`)
```json
{
  "title": "A2UI Server to Client Message",
  "type": "object",
  "properties": {
    "surfaceId": { "type": "string" },
    "component": { "$ref": "#/definitions/Component" },
    "data": { "type": "object" },
    "catalogs": { "type": "array" }
  }
}
```

### 3. **客户端能力 Schema** (`a2ui_client_capabilities_schema.json`)

定义客户端支持的能力：
- 支持的组件类型
- 自定义目录支持
- 事件处理能力
- 主题和样式支持

## 🔧 评估工具

### 目录结构
```
eval/
├── package.json               # 项目配置
├── tsconfig.json             # TypeScript 配置
├── pnpm-workspace.yaml       # PNPM 工作区
├── README.md                 # 工具说明
└── GEMINI.md                 # Gemini 评估指南
```

### 功能特性
- **Schema 验证**: 验证 A2UI 消息格式
- **兼容性测试**: 测试不同版本的兼容性
- **性能评估**: 评估渲染性能
- **AI 评估**: 使用 Gemini 评估 UI 质量

## 📊 版本演进

### v0.8 → v0.9 主要变更

1. **新增通用类型** (`common_types.json`)
   - 统一的类型定义
   - 更好的类型复用

2. **简化的协议**
   - 减少必需字段
   - 更灵活的数据绑定

3. **增强的事件系统**
   - 事件冒泡支持
   - 自定义事件类型

4. **改进的组件目录**
   - 更清晰的组件定义
   - 更好的扩展性

## 🎨 组件开发指南

### 创建自定义组件

1. **定义组件 Schema**
```json
{
  "MyComponent": {
    "type": "object",
    "properties": {
      "title": { "type": "string" },
      "data": { "type": "array" },
      "config": { "type": "object" }
    },
    "required": ["title"]
  }
}
```

2. **实现渲染逻辑**
```typescript
// Lit 实现示例
@customElement('my-component')
export class MyComponent extends LitElement {
  @property() component!: MyComponentNode;

  render() {
    return html`
      <h2>${this.component.title}</h2>
      <!-- 渲染逻辑 -->
    `;
  }
}
```

3. **注册到目录**
```typescript
export const MyCatalog = {
  MyComponent: {
    type: () => import('./my-component'),
    properties: {
      // 属性映射
    }
  }
};
```

## 🧪 测试和验证

### Schema 验证工具
```javascript
import Ajv from 'ajv';

// 加载 Schema
const schema = require('./standard_catalog_definition.json');
const ajv = new Ajv();

// 验证组件
const validate = ajv.compile(schema.components.Text);
const isValid = validate({
  text: { literalString: "Hello World" }
});
```

### 评估工具使用
```bash
# 安装依赖
cd specification/0.8/eval
pnpm install

# 运行验证
pnpm run validate

# 运行性能测试
pnpm run perf

# 运行 AI 评估
pnpm run eval
```

## 📚 实现参考

### 官方实现
- [Lit 渲染器](../renderers/lit/)
- [Angular 渲染器](../renderers/angular/)
- [Python Agent 扩展](../a2a_agents/python/)
- [Java Agent 扩展](../a2a_agents/java/)

### 示例实现
- [联系人应用](../samples/agent/adk/contact_lookup/)
- [数据可视化](../samples/agent/adk/rizzcharts/)

## 🔮 路线图

### v1.0 计划功能
- [ ] 稳定的 API 规范
- [ ] 更多标准组件
- [ ] 高级布局系统
- [ ] 动画和过渡效果
- [ ] 国际化支持

### 长期计划
- [ ] WebAssembly 渲染器
- [ ] 原生移动端支持
- [ ] VR/AR 界面支持
- [ ] 实时协作功能

## 🤝 贡献指南

### 参与规范制定
1. 提交 Issue 讨论新特性
2. 创建 PR 提交规范变更
3. 参与社区讨论

### 扩展组件目录
1. 设计组件 API
2. 编写 Schema 定义
3. 提供参考实现
4. 编写文档和示例

## 📖 参考资料

- [JSON Schema 规范](https://json-schema.org/)
- [A2A 协议文档](https://github.com/google/a2a)
- [Web Components 标准](https://www.webcomponents.org/)

## 🔄 变更记录

### 2025-12-18 11:15:00
- 📝 创建规范模块文档
- 📋 整理所有规范文档结构
- 🔍 分析 v0.8 和 v0.9 差异
- 🧪 添加评估工具说明

### 版本历史
- **v0.8** (2024-12): 初始稳定版本
- **v0.9** (2025-01): 预览版本，新增功能
- **v1.0** (计划中): 稳定发布版本

---

*最后更新: 2025-12-18 11:15:00*