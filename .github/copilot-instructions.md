# SwaggerSloop - Copilot 开发指南

## 📌 项目概述

SwaggerSloop 是一个美观的 Swagger UI 皮肤，支持 **.NET** 和 **Java (Spring Boot)**。项目灵感来自 Art-Design-Pro 风格，具有现代化的 UI 设计。

## 🏗️ 项目结构

```
SwaggerSloop/
├── shared/resources/          # 🎯 核心前端资源 (唯一源头!)
│   ├── index.html             # 主 HTML 模板
│   ├── swagger-sloop.css      # 样式文件
│   ├── swagger-sloop.js       # JavaScript 逻辑
│   └── favicon.svg            # 网站图标
├── dotnet/                    # .NET 实现
│   ├── src/SwaggerSloop/      # NuGet 包源码
│   │   ├── SwaggerSloopExtensions.cs  # 中间件扩展
│   │   ├── SwaggerSloopOptions.cs     # 配置选项
│   │   └── wwwroot/           # 前端资源 (从 shared 同步)
│   └── samples/               # 示例项目
│       └── SwaggerSloop.Sample/   # ⭐ 用于测试的 Demo
├── java/                      # Java 实现
│   ├── swagger-sloop-spring-boot-starter/  # Spring Boot Starter
│   │   └── src/main/resources/static/swagger-sloop/  # 前端资源 (从 shared 同步)
│   └── samples/               # 示例项目
│       ├── swagger-sloop-sample/      # 基础示例
│       └── swagger-sloop-auth-demo/   # 认证示例
├── sync-resources.sh          # 🔄 资源同步脚本
└── docs/                      # 文档和图片
```

## ⚠️ 重要开发规则

### 1. 前端资源修改规则

**永远只修改 `shared/resources/` 目录下的文件！**

- `shared/resources/index.html` - HTML 结构
- `shared/resources/swagger-sloop.css` - 样式
- `shared/resources/swagger-sloop.js` - JavaScript 逻辑

修改后必须执行同步脚本:

```bash
./sync-resources.sh
```

这会将资源同步到:

- `dotnet/src/SwaggerSloop/wwwroot/`
- `java/swagger-sloop-spring-boot-starter/src/main/resources/static/swagger-sloop/`

### 2. 模板变量

HTML 中使用以下占位符，会在运行时被替换:

| 占位符             | 说明                  |
| ------------------ | --------------------- |
| `%(DocumentTitle)` | 文档标题              |
| `%(ConfigJson)`    | JSON 配置对象         |
| `%(Version)`       | 版本号 (用于缓存破坏) |

### 3. CSS 设计系统

项目使用 OKLCH 颜色系统和 CSS 变量:

```css
/* 主要颜色变量 */
--art-primary: oklch(65% 0.2 255); /* 主色 */
--art-bg: oklch(98% 0 0); /* 背景色 */
--art-text: oklch(20% 0 0); /* 文字颜色 */
```

支持亮色/暗色主题，通过 `[data-theme="dark"]` 切换。

### 4. HTTP 方法颜色

```css
.art-method-get {
  --method-color: #61affe;
} /* 蓝色 */
.art-method-post {
  --method-color: #49cc90;
} /* 绿色 */
.art-method-put {
  --method-color: #fca130;
} /* 橙色 */
.art-method-delete {
  --method-color: #f93e3e;
} /* 红色 */
.art-method-patch {
  --method-color: #50e3c2;
} /* 青色 */
```

## 🧪 测试方法

### 推荐: 使用 .NET Demo 测试

```bash
# 1. 进入 .NET 示例目录
cd dotnet/samples/SwaggerSloop.Sample

# 2. 运行项目
dotnet run

# 3. 访问测试
open http://localhost:5000/swagger/
```

### 完整测试流程

```bash
# 1. 修改 shared/resources/ 下的文件

# 2. 同步资源
./sync-resources.sh

# 3. 重新构建 .NET 项目
cd dotnet && dotnet build

# 4. 运行测试
cd samples/SwaggerSloop.Sample && dotnet run
```

### Java 测试 (如需要)

```bash
# 1. 重新构建 Starter
cd java/swagger-sloop-spring-boot-starter
mvn clean install -DskipTests

# 2. 运行示例
cd ../samples/swagger-sloop-sample
mvn spring-boot:run
```

## 🔧 开发任务类型

### 修改 UI 样式

1. 编辑 `shared/resources/swagger-sloop.css`
2. 运行 `./sync-resources.sh`
3. 用 .NET Demo 测试

### 修改功能逻辑

1. 编辑 `shared/resources/swagger-sloop.js`
2. 运行 `./sync-resources.sh`
3. 用 .NET Demo 测试

### 修改 HTML 结构

1. 编辑 `shared/resources/index.html`
2. 运行 `./sync-resources.sh`
3. 用 .NET Demo 测试

### 修改 .NET 配置选项

- 编辑 `dotnet/src/SwaggerSloop/SwaggerSloopOptions.cs`
- 编辑 `dotnet/src/SwaggerSloop/SwaggerSloopExtensions.cs`

### 修改 Java 配置选项

- 编辑 `java/swagger-sloop-spring-boot-starter/src/main/java/io/github/rain7788/swaggersloop/`

## 📋 代码规范

### CSS

- 使用 `.art-` 前缀命名
- 使用 CSS 变量保持主题一致性
- 支持响应式设计

### JavaScript

- 使用原生 JS，不依赖框架
- 事件监听使用事件委托
- 保持代码模块化

### HTML

- 语义化标签
- 支持可访问性 (a11y)
- 使用模板变量而非硬编码

## 🚫 避免的操作

1. **不要直接修改** `dotnet/src/SwaggerSloop/wwwroot/` 或 `java/.../static/swagger-sloop/`
2. **不要忘记** 运行 `sync-resources.sh`
3. **不要** 在前端代码中使用第三方库 (保持零依赖)
4. **不要** 硬编码配置值，使用模板变量

## 🔗 关键文件速查

| 用途           | 文件路径                                                   |
| -------------- | ---------------------------------------------------------- |
| 前端 HTML      | `shared/resources/index.html`                              |
| 前端样式       | `shared/resources/swagger-sloop.css`                       |
| 前端逻辑       | `shared/resources/swagger-sloop.js`                        |
| .NET 配置      | `dotnet/src/SwaggerSloop/SwaggerSloopOptions.cs`           |
| .NET 中间件    | `dotnet/src/SwaggerSloop/SwaggerSloopExtensions.cs`        |
| .NET 测试 Demo | `dotnet/samples/SwaggerSloop.Sample/Program.cs`            |
| Java 自动配置  | `java/swagger-sloop-spring-boot-starter/src/main/java/...` |
| 资源同步       | `sync-resources.sh`                                        |
