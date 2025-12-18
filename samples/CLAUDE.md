[根目录](../../CLAUDE.md) > [samples](../) > **samples**

# A2UI 示例代码模块

## 模块职责

该模块包含 A2UI 协议的实际应用示例，展示了如何在不同场景下集成和使用 A2UI。示例涵盖了从简单的 Agent 实现到完整的客户端应用，是学习和参考 A2UI 的最佳实践集合。

## 🎯 示例分类

### 1. **Agent 示例** (`agent/`)
基于 Agent Development Kit (ADK) 的服务端实现示例

### 2. **客户端示例** (`client/`)
使用不同前端框架实现的客户端应用示例

## 📁 目录结构

```
samples/
├── agent/                      # Agent 实现示例
│   └── adk/                   # 基于 ADK 框架
│       ├── contact_lookup/    # 联系人查询示例
│       │   ├── agent.py       # Agent 主逻辑
│       │   ├── tools.py       # 工具函数
│       │   ├── a2ui_examples.py
│       │   ├── contact_data.json
│       │   └── README.md
│       ├── orchestrator/      # 多 Agent 编排示例
│       │   ├── agent.py
│       │   ├── subagent_route_manager.py
│       │   ├── part_converters.py
│       │   └── README.md
│       ├── restaurant_finder/ # 餐厅查找示例
│       │   ├── agent.py
│       │   ├── tools.py
│       │   ├── restaurant_data.json
│       │   └── README.md
│       └── rizzcharts/        # 数据可视化示例
│           ├── agent.py
│           ├── tools.py
│           ├── component_catalog_builder.py
│           ├── examples/
│           │   ├── rizzcharts_catalog/
│           │   └── standard_catalog/
│           └── README.md
└── client/                     # 客户端实现示例
    ├── angular/                # Angular 客户端
    │   ├── projects/
    │   │   ├── contact/       # 联系人应用
    │   │   ├── orchestrator/  # 编排器界面
    │   │   ├── restaurant/    # 餐厅查找应用
    │   │   └── a2a-chat-canvas/ # 聊天画布组件
    │   ├── package.json
    │   └── angular.json
    ├── lit/                    # Lit 客户端
    │   └── web/               # Web Components 示例
    └── web/                    # 原生 Web 示例
        └── minimal/           # 最小化实现
```

## 🚀 Agent 示例详解

### 1. **联系人查询** (`contact_lookup`)

#### 功能特性
- 查找和管理联系人信息
- 支持搜索、添加、更新操作
- 展示表格和表单 UI 组件的使用

#### 技术栈
```python
# 依赖
a2a-sdk>=0.3.0
a2ui>=0.1.0
uv  # 包管理器
```

#### 核心代码结构
```python
# agent.py - 主 Agent 实现
class ContactLookupAgent:
    def __init__(self):
        self.tools = ContactTools()
        self.a2ui_handler = A2UIHandler()

    async def handle_message(self, message):
        # 处理消息并生成 A2UI 响应
        pass

# tools.py - 工具函数
class ContactTools:
    def search_contacts(self, query):
        # 搜索联系人
        pass

    def add_contact(self, contact_data):
        # 添加联系人
        pass
```

#### 运行方式
```bash
cd samples/agent/adk/contact_lookup
echo "GEMINI_API_KEY=your_api_key" > .env
uv run .
```

### 2. **多 Agent 编排** (`orchestrator`)

#### 功能特性
- 管理多个子 Agent 的协作
- 动态路由任务到合适的 Agent
- 展示复杂的工作流编排

#### 核心组件
- `subagent_route_manager.py`: 路由管理器
- `part_converters.py`: A2A Part 转换器
- `agent.py`: 主编排器 Agent

### 3. **餐厅查找** (`restaurant_finder`)

#### 功能特性
- 基于位置和偏好查找餐厅
- 展示地图和列表视图
- 支持筛选和排序功能

### 4. **数据可视化** (`rizzcharts`)

#### 功能特性
- **自定义组件目录**: 扩展标准 A2UI 组件
- **图表组件**: 集成图表库（Chart.js, D3.js）
- **地图组件**: 支持交互式地图
- **动态主题**: 可切换的数据可视化主题

#### 自定义组件示例
```json
// rizzcharts_catalog_definition.json
{
  "components": {
    "Chart": {
      "type": "object",
      "properties": {
        "chartType": { "type": "string" },
        "data": { "type": "object" },
        "options": { "type": "object" }
      }
    },
    "Map": {
      "type": "object",
      "properties": {
        "markers": { "type": "array" },
        "center": { "type": "object" },
        "zoom": { "type": "number" }
      }
    }
  }
}
```

## 🎨 客户端示例详解

### 1. **Angular 聊天画布** (`a2a-chat-canvas`)

这是一个完整的 Angular 组件库，实现了与 A2A Agent 的交互界面。

#### 核心组件
```typescript
// chat-canvas.ts - 主聊天界面
@Component({
  selector: 'a2a-chat-canvas',
  template: `
    <chat-history [messages]="messages"></chat-history>
    <input-area (sendMessage)="onSendMessage($event)"></input-area>
    <a2a-renderer
      [a2uiData]="currentA2UIData"
      (event)="onA2UIEvent($event)">
    </a2a-renderer>
  `
})
```

#### 功能特性
- **消息历史**: 展示对话历史
- **实时渲染**: 动态渲染 A2UI 组件
- **事件处理**: 处理用户交互事件
- **Markdown 支持**: 支持富文本消息
- **主题系统**: 支持明暗主题切换

#### 子组件架构
- `chat/`: 聊天相关组件
  - `chat-history/`: 消息历史
  - `message/`: 单条消息
  - `input-area/`: 输入区域
  - `avatar/`: 用户头像
- `canvas/`: 画布和渲染组件

### 2. **联系人应用** (`contact`)

展示如何构建一个完整的 CRUD 应用
- 联系人列表视图
- 添加/编辑表单
- 搜索和筛选功能

### 3. **餐厅应用** (`restaurant`)

展示复杂的数据展示和交互
- 餐厅卡片视图
- 地图集成
- 评分和评论系统

## 🛠️ 开发指南

### 环境准备

1. **Python 环境** (Agent 示例)
   ```bash
   # 安装 uv
   pip install uv

   # 创建虚拟环境
   uv venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   ```

2. **Node.js 环境** (客户端示例)
   ```bash
   # 安装依赖
   npm install

   # 或使用 yarn
   yarn install
   ```

### 运行 Agent 示例

1. 设置 API 密钥
   ```bash
   # Gemini API
   echo "GEMINI_API_KEY=your_key" > .env

   # OpenAI API
   echo "OPENAI_API_KEY=your_key" >> .env
   ```

2. 启动 Agent
   ```bash
   cd samples/agent/adk/contact_lookup
   uv run .
   ```

### 运行客户端示例

1. Angular 应用
   ```bash
   cd samples/client/angular
   npm install
   ng serve
   ```

2. Lit 应用
   ```bash
   cd samples/client/lit
   npm install
   npm run dev
   ```

## 📚 学习路径

### 初学者
1. 先运行 `contact_lookup` 示例
2. 了解 Agent 基本结构
3. 查看生成的 A2UI JSON

### 进阶开发者
1. 研究 `orchestrator` 示例
2. 理解多 Agent 协作模式
3. 学习自定义组件开发

### 高级开发者
1. 深入 `rizzcharts` 示例
2. 创建自己的组件目录
3. 集成第三方库和组件

## 🔧 自定义和扩展

### 添加新组件
```typescript
// 在 Angular 中添加自定义组件
export class MyCustomComponent {
  @Input() component!: MyCustomComponentNode;
  @Input() surfaceId!: string;

  // 组件逻辑
}

// 注册到目录
export const CustomCatalog = {
  MyCustom: {
    type: () => import('./my-custom.component').then(m => m.MyCustomComponent),
    bindings: (component) => [
      inputBinding('data', () => component.data)
    ]
  }
};
```

### 自定义样式
```scss
// 覆盖默认主题
:host {
  --a2ui-primary-color: #ff6b6b;
  --a2ui-border-radius: 8px;
  --a2ui-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
```

## 🎯 最佳实践

### 1. **安全性**
- 始终验证来自 Agent 的数据
- 实施内容安全策略（CSP）
- 对用户输入进行清理

### 2. **性能**
- 使用虚拟滚动处理大列表
- 实施懒加载策略
- 优化包大小

### 3. **可访问性**
- 提供键盘导航支持
- 添加 ARIA 标签
- 确保颜色对比度

### 4. **错误处理**
- 优雅处理网络错误
- 提供回退 UI
- 记录错误日志

## 🔗 相关资源

- [A2UI 官方文档](https://a2ui.org/)
- [Agent Development Kit](https://github.com/google/a2a)
- [Angular 渲染器文档](../renderers/angular/README.md)
- [Lit 渲染器文档](../renderers/lit/README.md)

## 🔄 变更记录

### 2025-12-18 11:00:00
- 📝 创建示例模块文档
- 🎯 分析所有示例项目结构和功能
- 📚 整理学习路径和最佳实践
- 🔧 添加自定义开发指南

---

*最后更新: 2025-12-18 11:00:00*