[根目录](../../../../../CLAUDE.md) > [samples](../../../../../samples/CLAUDE.md) > [client](../../../) > [angular](../) > **gallery**

# A2UI Gallery 展示项目

## 模块职责

Gallery 是 A2UI 组件的交互式展示应用，用于演示 A2UI 协议支持的各种 UI 组件和布局模式。它是一个纯客户端应用，无需服务器即可运行，展示了 A2UI 的强大表现力和灵活性。

## 🎯 项目概述

- **项目类型**: 客户端展示应用
- **框架**: Angular 18+
- **A2UI 版本**: v0.8
- **状态**: 完整功能实现
- **部署**: 独立运行，无需后端

## 🏗️ 技术架构

### 核心依赖
```json
{
  "@a2ui/angular": "^0.8.1",
  "@a2ui/lit": "^0.8.1",
  "@angular/core": "^18.0.0",
  "@angular/common": "^18.0.0"
}
```

### 项目结构
```
gallery/
├── src/
│   ├── app/
│   │   ├── app.config.ts        # Angular 应用配置
│   │   ├── app.ts              # 应用根组件
│   │   ├── app.html            # 主模板
│   │   ├── app.css             # 全局样式
│   │   ├── theme.ts            # A2UI 主题配置
│   │   └── features/
│   │       ├── gallery/        # 画廊展示组件
│   │       │   ├── gallery.component.ts
│   │       │   ├── gallery.html
│   │       │   └── gallery.css
│   │       └── library/        # 组件库展示
│   │           ├── library.component.ts
│   │           ├── library.html
│   │           └── library.css
│   ├── main.ts                 # 应用入口
│   ├── index.html              # HTML 入口
│   └── styles.css              # 全局样式
└── tsconfig.app.json           # TypeScript 配置
```

## 🎨 功能特性

### 1. **组件展示** (gallery/)
交互式展示各种 A2UI 组件的实际效果：
- **Welcome Card**: 欢迎卡片示例
- **Photo List**: 图文列表展示
- **Contact Form**: 表单组件示例
- **动态渲染**: 实时展示组件 JSON 结构

#### 核心实现
```typescript
interface GallerySample {
  id: string;
  title: string;
  description: string;
  surface: v0_8.Types.Surface;
}

@Component({...})
export class GalleryComponent {
  samples: GallerySample[] = [
    {
      id: 'welcome',
      title: 'Welcome Card',
      description: 'A simple welcome card with an image and text.',
      surface: this.createSingleComponentSurface('Card', {
        child: this.createComponent('Column', {
          children: [
            this.createComponent('Image', {
              url: { literalString: 'https://picsum.photos/id/10/600/300' }
            }),
            // ... 更多组件
          ]
        })
      })
    }
  ];
}
```

### 2. **组件库浏览器** (library/)
系统化展示所有 A2UI 标准组件：
- 按类别组织（布局、输入、显示等）
- 每个组件的配置说明
- 实时代码预览
- JSON 结构展示

### 3. **主题系统**
内置多主题支持：
- **Default**: 默认浅色主题
- **Dark**: 深色主题
- **Custom**: 可自定义主题

```typescript
// theme.ts
export const galleryTheme = {
  colors: {
    primary: '#1976d2',
    secondary: '#424242',
    surface: '#ffffff',
    background: '#fafafa'
  },
  typography: {
    fontFamily: 'Roboto, sans-serif',
    fontSize: '14px'
  }
};
```

## 🎮 交互功能

### 1. **实时预览**
- 点击示例卡片查看完整效果
- 模态框展示组件细节
- 滚动导航和进度指示

### 2. **代码查看**
- 显示每个组件的 JSON 结构
- 一键复制代码
- 语法高亮显示

### 3. **响应式设计**
- 移动端适配
- 平板端优化
- 桌面端完整体验

## 🚀 运行和部署

### 本地开发
```bash
# 进入 Angular 项目目录
cd samples/client/angular

# 安装依赖
npm install

# 运行 Gallery 应用
ng serve --project=gallery

# 或使用生产模式
ng build --project=gallery --configuration=production
```

### 构建输出
```bash
# 构建产物目录
dist/gallery/

# 文件大小分析
├── main.js          ~150KB (gzipped)
├── polyfills.js     ~30KB (gzipped)
├── runtime.js       ~5KB (gzipped)
└── styles.css       ~20KB (gzipped)
```

## 🎯 组件示例详解

### 1. **Welcome Card**
展示简单的欢迎界面：
```typescript
{
  type: 'Card',
  properties: {
    child: {
      type: 'Column',
      properties: {
        children: [
          { type: 'Image', properties: { url: '...' } },
          { type: 'Text', properties: { text: '...' } },
          { type: 'Button', properties: { action: { type: 'submit' } } }
        ],
        alignment: 'center'
      }
    }
  }
}
```

### 2. **Photo List**
展示图文混排列表：
```typescript
{
  type: 'Card',
  properties: {
    child: {
      type: 'Column',
      properties: {
        children: [
          // Row 1
          {
            type: 'Row',
            properties: {
              children: [
                { type: 'Image' },
                { type: 'Column', properties: { children: [{ type: 'Text' }] } }
              ]
            }
          },
          // 更多行...
        ]
      }
    }
  }
}
```

### 3. **Contact Form**
展示表单组件的使用：
```typescript
{
  type: 'Card',
  properties: {
    child: {
      type: 'Column',
      properties: {
        children: [
          {
            type: 'Row',
            properties: {
              children: [
                {
                  type: 'TextField',
                  properties: {
                    label: { literalString: 'Name' },
                    type: 'text',
                    text: { literalString: '' }
                  }
                }
              ]
            }
          },
          // 更多表单字段...
          {
            type: 'Button',
            properties: {
              action: { type: 'submit' },
              child: {
                type: 'Text',
                properties: { text: { literalString: 'Send Message' } }
              }
            }
          }
        ]
      }
    }
  }
}
```

## 🎨 样式系统

### CSS 架构
```scss
// gallery.css
.gallery-container {
  // 主容器样式
  display: grid;
  grid-template-columns: 250px 1fr;
  height: 100vh;
}

.sidebar {
  // 侧边栏导航
  background: var(--md-sys-color-surface);
  padding: 1rem;
}

.preview-area {
  // 预览区域
  overflow-y: auto;
  padding: 2rem;
}

.sample-card {
  // 示例卡片
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.2s ease;

  &:hover {
    transform: translateY(-2px);
  }
}
```

### 响应式设计
```scss
@media (max-width: 768px) {
  .gallery-container {
    grid-template-columns: 1fr;
  }

  .sidebar {
    position: fixed;
    transform: translateX(-100%);
    transition: transform 0.3s ease;
  }
}
```

## 📊 性能优化

### 1. **懒加载**
```typescript
// 使用 Angular 的懒加载
const routes: Routes = [
  {
    path: 'gallery',
    loadChildren: () => import('./gallery/gallery.module').then(m => m.GalleryModule)
  }
];
```

### 2. **OnPush 变更检测**
```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class GalleryComponent {
  // 组件实现
}
```

### 3. **虚拟滚动**
对于大量组件列表，实现了虚拟滚动：
```html
<cdk-virtual-scroll-viewport itemSize="300">
  <div *cdkVirtualFor="let sample of samples">
    <gallery-sample [sample]="sample"></gallery-sample>
  </div>
</cdk-virtual-scroll-viewport>
```

## 🔧 扩展和自定义

### 添加新示例
1. 在 `gallery.component.ts` 中添加新的示例：
```typescript
{
  id: 'my-custom-example',
  title: 'My Custom Example',
  description: 'Description of my example',
  surface: this.createMyCustomSurface()
}
```

2. 实现创建方法：
```typescript
private createMyCustomSurface(): v0_8.Types.Surface {
  return this.createSingleComponentSurface('CustomComponent', {
    // 组件属性
  });
}
```

### 自定义主题
在 `theme.ts` 中定义新主题：
```typescript
export const myCustomTheme = {
  colors: {
    primary: '#ff6b6b',
    background: '#f8f9fa'
  },
  components: {
    Button: {
      backgroundColor: 'var(--primary)',
      color: 'white'
    }
  }
};
```

## 🎓 学习价值

Gallery 项目是学习 A2UI 的最佳起点：

### 初学者
- 理解 A2UI 组件的基本概念
- 学习组件的 JSON 结构
- 查看实际运行效果

### 开发者
- 参考最佳实践
- 学习布局技巧
- 获取代码模板

### 设计师
- 了解组件样式定制
- 探索主题系统
- 获取设计灵感

## 🔍 调试和开发

### 开发工具
```typescript
// 启用调试模式
if (environment.production === false) {
  console.log('A2UI Surface:', surface);
  console.log('Component Tree:', componentTree);
}
```

### 常见问题
1. **组件不渲染**: 检查 JSON 结构是否正确
2. **样式问题**: 确认主题配置
3. **性能问题**: 使用 OnPush 策略

## 📈 未来计划

### 功能增强
- [ ] 添加更多组件示例
- [ ] 支持组件配置交互式编辑
- [ ] 集成代码分享功能
- [ ] 添加动画和过渡效果

### 技术改进
- [ ] 迁移到 Angular 19+
- [ ] 使用 Web Workers 处理复杂渲染
- [ ] 实现组件性能分析
- [ ] 添加单元测试

## 🔗 相关资源

- [A2UI 官方文档](https://a2ui.org/)
- [Angular 渲染器文档](../README.md)
- [组件参考文档](../../../../docs/reference/components.md)

## 🔄 变更记录

### 2025-12-18 11:45:00
- 📝 创建 Gallery 项目文档
- 🎨 分析组件展示架构
- 📊 记录技术栈和依赖
- 🚀 整理运行和部署指南
- 🔧 提供扩展和自定义方法

---

*最后更新: 2025-12-18 11:45:00*