# Skills 技能库使用指南

本指南详细介绍如何高效使用技能库中的各项技能，涵盖调用机制、使用场景、配置方法和最佳实践。

---

## 目录

1. [技能调用机制](#1-技能调用机制)
2. [技能分类详解](#2-技能分类详解)
3. [核心工作流程](#3-核心工作流程)
4. [技能配置](#4-技能配置)
5. [使用示例](#5-使用示例)
6. [最佳实践](#6-最佳实践)
7. [故障排除](#7-故障排除)

---

## 1. 技能调用机制

### 1.1 自动触发

AI 根据任务上下文自动判断并调用相关技能：

- 当检测到 React/Next.js 代码时，自动应用 `react-best-practices`
- 当涉及 PR 相关操作时，自动调用 `pr-creator`
- 当进行代码审查时，自动使用 `code-reviewer`

### 1.2 手动调用

在对话中显式指定技能名称：

```
@skill-name
```

例如：
```
@frontend-design
帮我设计一个电商产品列表页面
```

### 1.3 技能组合

可以同时调用多个技能协同工作：

```
@brainstorming + @frontend-design
帮我头脑风暴一个社交App的UI设计方向
```

---

## 2. 技能分类详解

### 2.1 Deep Agents 智能体框架（2个）

#### deepagents-setup-configuration

**用途**：初始化 Deep Agents 项目

**前置条件**：
- Python 3.10+ 或 Node.js 18+
- Git 已安装

**使用方式**：
```bash
# 在项目目录执行
npx deepagents init
# 或
python -m deepagents init
```

**功能**：
- 自动创建项目结构
- 配置中间件（规划、文件系统、子智能体）
- 生成示例代码

#### deepagents-planning-todos

**用途**：任务规划和分解

**使用方式**：
```
@deepagents-planning-todos
将这个功能需求分解成可执行的任务列表
```

**特点**：
- 自动识别任务依赖关系
- 生成优先级排序
- 支持里程碑设置

---

### 2.2 规划与设计（2个）

#### brainstorming

**用途**：创意头脑风暴

**工作流程**：
1. 初始讨论 → 2. 方案探索 → 3. 设计定稿 → 4. 评审批准

**使用方式**：
```
@brainstorming
我想做一个AI驱动的日程管理App，需要与日历同步
```

**输出**：
- 完整设计方案
- 技术规格文档
- 视觉参考

#### write-a-prd

**用途**：创建产品需求文档

**工作流程**：
1. 用户访谈 → 2. 代码库探索 → 3. 模块设计 → 4. 生成 PRD → 5. 提交 GitHub Issue

**前置条件**：
- GitHub CLI 已登录 (`gh auth login`)
- 仓库已配置

**使用方式**：
```
@write-a-prd
为这个社交App的消息系统创建PRD
```

---

### 2.3 前端开发（10个）

#### frontend-design

**用途**：创建高质量前端界面

**特点**：
- 避免通用 AI 美学
- 注重创意和设计细节
- 支持多框架输出

**使用方式**：
```
@frontend-design
设计一个会员Dashboard页面，包含统计卡片、图表、近期活动列表
```

#### react-best-practices

**用途**：React/Next.js 性能优化

**核心规则（65条，8大类）**：

| 优先级 | 类别 | 示例规则 |
|-------|------|---------|
| CRITICAL | 瀑布消除 | async-parallel、async-defer-await |
| CRITICAL | Bundle 优化 | bundle-dynamic-imports、bundle-barrel-imports |
| HIGH | 服务端性能 | server-cache-react、server-parallel-fetching |
| MEDIUM-HIGH | 客户端数据获取 | client-swr-dedup、client-event-listeners |
| MEDIUM | 重渲染优化 | rerender-memo、rerender-transitions |
| MEDIUM | 渲染性能 | rendering-content-visibility、rendering-hoist-jsx |
| LOW-MEDIUM | JavaScript 性能 | js-index-maps、js-set-map-lookups |
| LOW | 高级模式 | advanced-event-handler-refs、advanced-use-latest |

**使用方式**：
```
@react-best-practices
帮我审查这个数据获取组件的性能问题
```

#### ui-ux-pro-max

**用途**：UI/UX 设计参考

**资源库**：
- 50 种样式变体
- 21 种调色板
- 字体配对方案
- 图表类型选择
- 多框架支持（React、Vue、Svelte、Flutter、SwiftUI）

**使用方式**：
```
@ui-ux-pro-max
提供一个B2B SaaS仪表板的配色方案和样式参考
```

#### cache-components

**用途**：Next.js 缓存深度理解

**核心主题**：
- Cache Components 机制
- Partial Prerendering (PPR)
- 缓存失效策略

**使用方式**：
```
@cache-components
解释一下这个页面的缓存行为和PPR实现
```

#### web-design-guidelines

**用途**：Web 界面合规审查

**检查项**：
- 可访问性（a11y）
- 最佳实践
- UX 设计规范

**使用方式**：
```
@web-design-guidelines
审查这个登录表单的UI代码
```

#### react-native-skills

**用途**：React Native/Expo 性能优化

**核心规则（35+条）**：
- 列表性能优化
- 动画性能
- 导航优化
- 原生模块使用

**使用方式**：
```
@react-native-skills
优化这个长列表的滚动性能
```

#### composition-patterns

**用途**：React 组合模式最佳实践

**核心原则**：
- 避免布尔属性泛滥
- 使用复合组件
- 状态提升
- 依赖注入

**使用方式**：
```
@composition-patterns
设计一个可复用的表单组件架构
```

#### vtable-tanstack-guardrails

**用途**：大型表格/数据集处理

**技术栈**：
- @tanstack/table-core - 表格语义
- @tanstack/virtual-core - 虚拟化

**适用场景**：大型 ERP 数据集、实时数据表格

**使用方式**：
```
@vtable-tanstack-guardrails
创建一个10万行数据的虚拟滚动表格
```

#### shadcn

**用途**：shadcn/ui 组件管理

**功能**：
- 组件搜索：`npx shadcn@latest add button`
- 组件调试
- 样式化定制
- 组件组合

**使用方式**：
```
@shadcn
添加一个带动画的数据展示卡片组件
```

#### antd

**用途**：Ant Design 组件开发

**工具**：@ant-design/cli

**功能**：
- API 查询：`antd info Button --format json`
- Demo 获取：`antd demo Table basic --format json`
- 版本迁移：`antd migrate 4 5 --format json`
- 项目分析：`antd usage ./src --format json`

**前置条件**：
```bash
npm install -g @ant-design/cli
```

**使用方式**：
```
@antd
查询Table组件的所有可用props和示例
```

---

### 2.4 AI 内容生成（2个）

#### seedance

**用途**：字节跳动即梦平台视频生成

**支持功能**：
- 15秒视频生成
- 视频延长
- 一镜到底

**使用方式**：
```
@seedance
生成一个科技产品展示视频的提示词
```

#### nano-banana-pro

**用途**：Google Gemini 图像生成/编辑

**分辨率**：1K / 2K / 4K

**工作流**：Draft → Iterate → Final

**使用方式**：
```
@nano-banana-pro
为这个App首页生成一张Hero图片
```

---

### 2.5 元技能（2个）

#### find-skills

**用途**：发现和安装新技能

**命令**：
```bash
npx skills find <keyword>
npx skills add <skill-name>
```

**使用方式**：
```
@find-skills
搜索与数据库相关的技能
```

#### skill-creator

**用途**：创建自定义技能

**创建流程**：
1. 初始化：`npx skills create my-skill`
2. 定义技能结构
3. 编写 SKILL.md
4. 打包发布

**使用方式**：
```
@skill-creator
帮我创建一个处理图片上传的技能
```

---

### 2.6 开发工具（6个）

#### code-reviewer

**用途**：代码审查

**支持范围**：
- 本地更改（暂存/工作区）
- 远程 Pull Requests

**审查维度**：
- 正确性
- 可维护性
- 安全性
- 性能

**使用方式**：
```
@code-reviewer
审查这个分支的代码改动
```

#### frontend-code-review

**用途**：前端专项审查

**支持文件**：.tsx / .ts / .js

**检查清单**：
- 代码质量
- 性能问题
- 业务逻辑

**使用方式**：
```
@frontend-code-review
审查这个Button组件的代码
```

#### pr-creator

**用途**：Pull Request 管理

**功能**：
- 分支管理
- 模板定位
- 描述起草
- 预检运行

**前置条件**：
```bash
gh auth login
```

**使用方式**：
```
@pr-creator
为这个功能创建一个PR
```

#### update-docs

**用途**：Next.js 文档更新

**功能**：
- API 参考更新
- 新功能文档脚手架

**使用方式**：
```
@update-docs
根据代码改动更新API文档
```

#### webapp-testing

**用途**：Web 应用测试

**工具**：Playwright

**功能**：
- 前端功能验证
- UI 行为调试
- 浏览器截图
- 日志捕获

**使用方式**：
```
@webapp-testing
测试用户登录流程
```

#### agent-browser

**用途**：浏览器自动化

**功能**：
- 网页浏览
- 数据抓取
- 表单提交
- 页面交互

**使用方式**：
```
@agent-browser
自动填写并提交这个调查问卷
```

---

### 2.7 全栈开发（1个）

#### fullstack-developer

**用途**：全栈开发指导

**覆盖领域**：
- React / Node.js
- 数据库设计
- API 开发
- 部署上线

**使用方式**：
```
@fullstack-developer
帮我设计一个电商后端API架构
```

---

## 3. 核心工作流程

### 3.1 新项目启动

```
@deepagents-setup-configuration    # 1. 初始化项目
    ↓
@brainstorming                       # 2. 头脑风暴设计
    ↓
@write-a-prd                         # 3. 创建PRD
    ↓
@deepagents-planning-todos          # 4. 任务分解
```

### 3.2 前端开发

```
@frontend-design                    # 1. UI设计
    ↓
@react-best-practices               # 2. 性能优化
    ↓
@frontend-code-review               # 3. 代码审查
    ↓
@shadcn 或 @antd                    # 4. 组件实现
```

### 3.3 代码发布

```
@code-reviewer                       # 1. 完整审查
    ↓
@update-docs                        # 2. 更新文档
    ↓
@webapp-testing                     # 3. 功能测试
    ↓
@pr-creator                         # 4. 创建PR
```

### 3.4 内容创作

```
@nano-banana-pro                    # 1. 生成图片
    ↓
@seedance                          # 2. 生成视频
```

---

## 4. 技能配置

### 4.1 环境要求

| 技能 | 必需工具 |
|-----|---------|
| deepagents-* | Python 3.10+ 或 Node.js 18+ |
| pr-creator | GitHub CLI (`gh`) |
| antd | @ant-design/cli |
| webapp-testing | Playwright |
| find-skills | npx |

### 4.2 安装命令

```bash
# GitHub CLI
gh auth login

# Ant Design CLI
npm install -g @ant-design/cli

# Playwright
npx playwright install
```

---

## 5. 使用示例

### 示例 1：构建 React/Next.js 电商产品页

```
@deepagents-setup-configuration
初始化一个Next.js电商项目

@frontend-design
设计产品列表页面，包含筛选、排序、分页

@react-best-practices
优化数据获取，避免瀑布流

@vtable-tanstack-guardrails
实现千级SKU数据的高性能虚拟列表

@web-design-guidelines
确保页面符合可访问性标准

@frontend-code-review
审查整个页面的代码实现
```

### 示例 2：创建 AI 生成工具

```
@brainstorming
头脑风暴一个AI驱动的图标生成App

@write-a-prd
创建完整的产品需求文档

@fullstack-developer
设计前后端架构

@nano-banana-pro
生成App的UI素材

@pr-creator
创建PR并提交审核
```

---

## 6. 最佳实践

### 6.1 技能调用顺序

1. **规划先行**：复杂项目先使用 brainstorming 和 write-a-prd
2. **设计确认**：UI 设计获得批准后再实现
3. **性能同步**：开发过程中应用 react-best-practices
4. **审查前置**：代码审查应在提交前完成
5. **测试验证**：使用 webapp-testing 确保功能正确

### 6.2 技能组合策略

| 场景 | 推荐组合 |
|-----|---------|
| 新项目 | deepagents-setup + brainstorming + write-a-prd |
| UI 开发 | frontend-design + shadcn/antd + react-best-practices |
| 性能优化 | react-best-practices + cache-components |
| 代码质量 | code-reviewer + frontend-code-review |
| 发布流程 | pr-creator + update-docs + webapp-testing |

### 6.3 避免常见错误

- ❌ 不先查询组件 API 就直接编写代码
- ❌ 跳过 brainstorming 直接开始实现
- ❌ 忽略性能优化规则导致页面卡顿
- ❌ 提交代码前不进行审查
- ❌ 缺少测试覆盖就创建 PR

---

## 7. 故障排除

### 7.1 技能不响应

**检查项**：
1. 技能名称拼写是否正确
2. 是否在正确的对话上下文中
3. 技能目录是否存在

**解决方案**：
```bash
# 重新安装技能
npx skills add <skill-name>

# 查看可用技能
npx skills list
```

### 7.2 技能输出不符合预期

**检查项**：
1. 是否提供了足够的上下文
2. 是否有特殊约束未说明

**解决方案**：
- 在提示中明确说明：
  - 技术栈版本
  - 设计偏好
  - 性能要求
  - 约束条件

### 7.3 技能冲突

**情况**：多个技能可能对同一问题给出不同建议

**解决方式**：
1. 优先信任专业技能（如 react-best-practices 对于性能问题）
2. 使用 brainstorming 讨论确定最终方案

---

## 更新日志

| 日期 | 版本 | 更新内容 |
|-----|------|---------|
| 2026-03-28 | 1.0 | 初始版本，包含24个技能详细指南 |

---

*最后更新：2026-03-28*