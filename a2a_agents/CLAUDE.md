[根目录](../../CLAUDE.md) > **a2a_agents**

# A2UI Agent 扩展模块

## 模块职责

该模块提供了 A2UI 协议在 Agent-to-Agent (A2A) 框架中的扩展实现。它使 AI 代理能够通过标准化的 A2A 协议生成和传输用户界面，支持 Python 和 Java 两种实现。

## 🔧 技术栈

- **Python**: 3.10+，基于 a2a-sdk
- **Java**: 基于 io.a2a 框架
- **协议**: A2A Extension for A2UI v0.8

## 📁 目录结构

```
a2a_agents/
├── python/                # Python 实现
│   └── a2ui_extension/
│       ├── src/a2ui/
│       │   ├── __init__.py
│       │   └── a2ui_extension.py  # 核心扩展实现
│       ├── tests/               # 测试文件
│       ├── pyproject.toml       # 项目配置
│       └── README.md           # 详细说明
└── java/                   # Java 实现
    └── src/main/java/org/a2ui/
        └── A2uiExtension.java  # 核心扩展类
    └── src/test/java/          # 测试文件
```

## 🚀 核心功能

### 1. **A2UI 数据处理** (`a2ui_extension.py` / `A2uiExtension.java`)

#### Python API
```python
from a2ui import (
    create_a2ui_part,           # 创建包含 A2UI 数据的 A2A Part
    is_a2ui_part,              # 检查 Part 是否包含 A2UI 数据
    get_a2ui_datapart,         # 提取 A2UI DataPart
    get_a2ui_agent_extension,  # 获取 A2UI 扩展配置
    try_activate_a2ui_extension # 激活 A2UI 扩展
)
```

#### Java API
```java
// 创建 A2UI 数据部分
DataPart part = A2uiExtension.createA2uiDataPart(a2uiData);

// 检查是否为 A2UI 数据
boolean isA2ui = A2uiExtension.isA2uiPart(part);

// 提取 A2UI 数据
Optional<DataPart> dataPart = A2uiExtension.getA2uiDataPart(part);
```

### 2. **MIME 类型处理**
- **A2UI MIME 类型**: `application/json+a2ui`
- **扩展 URI**: `https://a2ui.org/a2a-extension/a2ui/v0.8`
- **标准目录 ID**: `https://raw.githubusercontent.com/google/A2UI/refs/heads/main/specification/0.8/json/standard_catalog_definition.json`

### 3. **安全特性**

该模块强调**安全第一**的设计原则：

- ✅ **输入验证**: 所有来自外部代理的数据都被视为不可信
- ✅ **防止注入攻击**: 防止通过字段值进行提示注入
- ✅ **XSS 防护**: 防止通过属性值注入恶意脚本
- ✅ **DoS 防护**: 防止通过过度复杂的布局降低性能
- ✅ **内容隔离**: 对嵌入内容（iframe/web views）实施严格隔离

## 🔌 集成方式

### Python 集成示例

```python
from a2a.server.agent_execution import RequestContext
from a2ui import try_activate_a2ui_extension, create_a2ui_part

# 1. 激活扩展
if try_activate_a2ui_extension(context):
    # 2. 创建 A2UI 数据
    ui_data = {
        "surfaceId": "main",
        "component": {
            "type": "Text",
            "text": {"literalString": "Hello, A2UI!"}
        }
    }

    # 3. 创建并返回 A2UI 部分
    return create_a2ui_part(ui_data)
```

### Java 集成示例

```java
// 创建 A2UI 数据映射
Map<String, Object> a2uiData = new HashMap<>();
a2uiData.put("surfaceId", "main");
// ... 添加组件定义

// 创建 A2UI 数据部分
DataPart a2uiPart = A2uiExtension.createA2uiDataPart(a2uiData);

// 在响应中使用
Part response = new Part(a2uiPart);
```

## 🧪 测试覆盖

- **Python**: 使用 pytest 测试框架
- **Java**: 使用 JUnit 测试框架
- 测试覆盖所有核心 API 和边界情况

## 📦 依赖管理

### Python 依赖 (`pyproject.toml`)
```toml
[project]
dependencies = ["a2a-sdk>=0.3.0"]
```

### Java 依赖
```xml
<!-- 基于项目的 pom.xml 配置 a2a-sdk 依赖 -->
```

## 🔧 配置选项

### 扩展参数
- `acceptsInlineCustomCatalog`: 是否接受内联自定义目录（默认：false）

### 客户端能力
- `a2uiClientCapabilities`: 客户端支持的 A2UI 能力
- `supportedCatalogIds`: 支持的目录 ID 列表
- `inlineCatalogs`: 内联目录定义

## ⚠️ 安全注意事项

1. **数据验证**: 始终验证来自外部代理的数据
2. **内容安全策略**: 实施 CSP 策略
3. **输入清理**: 对所有用户输入进行清理
4. **凭证管理**: 安全处理 API 密钥和认证信息

## 🚚 部署建议

1. **生产环境**: 确保所有安全措施都已实施
2. **监控**: 监控异常的 UI 定义或数据请求
3. **速率限制**: 对代理请求实施适当的速率限制
4. **日志记录**: 记录所有 A2UI 相关的交互

## 📚 相关文档

- [A2UI 协议规范](../specification/0.8/docs/a2ui_protocol.md)
- [A2UI 扩展规范](../specification/0.8/docs/a2ui_extension_specification.md)
- [示例代码](../samples/agent/)

## 🔄 变更记录

### 2025-12-18 10:30:00
- 📝 创建模块文档
- 📊 完成核心功能分析
- 🔍 添加安全和集成指南

---

*最后更新: 2025-12-18 10:30:00*