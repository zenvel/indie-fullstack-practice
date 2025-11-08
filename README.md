# Indie Fullstack Practice - 独立开发全栈练习项目

> 本仓库包含《独立开发全栈系列》的支线练习项目，用于巩固每个阶段的核心知识。

## 🎯 项目简介

本仓库包含 3 个独立的练习项目，每个项目对应系列的一个阶段：

| 项目 | 对应阶段 | 核心技能 | 完成时间 |
|------|----------|----------|----------|
| **01-todo-list** | 阶段一 | React 基础、状态管理 | 0.5-2 小时 |
| **02-personal-card** | 阶段二 | Next.js 路由、SEO | 1-2 天 |
| **03-private-notes** | 阶段三 | Supabase 认证、RLS | 1-2 天 |

## 📂 项目结构

```
indie-fullstack-practice/
├── 01-todo-list/              # 阶段一练习
│   ├── README.md             # 项目说明
│   ├── package.json
│   ├── app/                  # Next.js App
│   ├── components/           # React 组件
│   └── docs/
│       └── tutorial.md       # 手把手教程
├── 02-personal-card/          # 阶段二练习
│   ├── README.md
│   ├── package.json
│   ├── app/
│   ├── components/
│   └── docs/
│       └── tutorial.md
├── 03-private-notes/          # 阶段三练习
│   ├── README.md
│   ├── package.json
│   ├── app/
│   ├── components/
│   ├── lib/
│   └── docs/
│       └── tutorial.md
└── README.md                  # 本文件
```

## 🚀 快速开始

### 前置要求

- Node.js 18.17 或更高版本
- npm 或 pnpm 或 yarn

### 克隆仓库

```bash
git clone https://github.com/yourusername/indie-fullstack-practice.git
cd indie-fullstack-practice
```

### 运行项目

每个项目都是独立的，进入对应目录即可运行：

```bash
# 运行 Todo List
cd 01-todo-list
npm install
npm run dev

# 运行个人名片页
cd 02-personal-card
npm install
npm run dev

# 运行私密笔记
cd 03-private-notes
npm install
npm run dev
```

## 📚 项目详情

---

## 01 - Todo List（阶段一练习）

### 🎯 学习目标

- 掌握 React 状态管理（`useState`）
- 理解组件化开发
- 练习增删改查操作
- 学会本地存储（`localStorage`）

### 💻 功能清单

- [x] 添加 Todo
- [x] 标记完成/未完成
- [x] 删除 Todo
- [x] 筛选（全部/已完成/未完成）
- [x] 数据持久化（localStorage）

### 📖 教程

详细教程见：[01-todo-list/docs/tutorial.md](01-todo-list/docs/tutorial.md)

或阅读系列文章：[1.5 支线挑战 - Todo List](链接待补充)

### 🌟 进阶挑战

完成基础版后，可以尝试：
- [ ] 编辑 Todo
- [ ] 拖拽排序
- [ ] 多分类
- [ ] 截止日期

---

## 02 - 个人名片页（阶段二练习）

### 🎯 学习目标

- 掌握 Next.js App Router
- 实践布局系统
- 配置 SEO Metadata
- 部署到 Vercel

### 💻 功能清单

- [x] 首页（自我介绍）
- [x] 项目展示页
- [x] 联系页面
- [x] 统一导航和布局
- [x] SEO 优化
- [x] 响应式设计

### 📖 教程

详细教程见：[02-personal-card/docs/tutorial.md](02-personal-card/docs/tutorial.md)

或阅读系列文章：[2.6 支线挑战 - 个人名片页](链接待补充)

### 🌟 进阶挑战

完成基础版后，可以尝试：
- [ ] 支持中英文切换
- [ ] Dark mode
- [ ] 动画效果（Framer Motion）
- [ ] MDX 博客系统

---

## 03 - 私密笔记 App（阶段三练习）

### 🎯 学习目标

- 掌握 Supabase 认证
- 理解 Row Level Security
- 实践数据 CRUD
- 学会数据隔离

### 💻 功能清单

- [x] 用户注册/登录
- [x] 创建、编辑、删除笔记
- [x] 笔记列表
- [x] 数据隔离（用户只能看到自己的笔记）
- [x] Markdown 支持（可选）

### 📖 教程

详细教程见：[03-private-notes/docs/tutorial.md](03-private-notes/docs/tutorial.md)

或阅读系列文章：[3.6 支线挑战 - 私密笔记 App](链接待补充)

### 🌟 进阶挑战

完成基础版后，可以尝试：
- [ ] Markdown 实时预览
- [ ] 全文搜索
- [ ] 标签系统
- [ ] 笔记分类
- [ ] 笔记分享功能

---

## 📚 学习路径建议

### 初学者路径

1. **先跟着主线项目学习**
   - 完成[独立开发者资源库](https://github.com/yourusername/indie-resource-hub)的对应阶段
   - 理解核心概念和技术

2. **再做支线练习巩固**
   - 独立完成练习项目
   - 不看答案先尝试
   - 遇到问题再查看教程

3. **最后做自己的项目**
   - 把学到的技能应用到自己的想法上
   - 参考主线和支线项目的代码结构

### 时间规划建议

- **Todo List**: 半天到1天（重点是 React 状态管理）
- **个人名片页**: 1-2天（重点是 Next.js 路由和 SEO）
- **私密笔记**: 2-3天（重点是 Supabase 认证和 RLS）

## 🛠️ 技术栈

所有项目使用统一的技术栈（锁定 2025-11）：

| 技术栈 | 版本 | 说明 |
|--------|------|------|
| Next.js | 15.0.3 | App Router |
| React | 18.3.1 | Server Components |
| Supabase | 2.39.0 | 数据库 + 认证（仅 03 项目使用） |
| Tailwind CSS | 3.4.0 | 样式框架 |
| TypeScript | 5.3.3 | 类型系统 |

## 💡 学习建议

### 如何使用这些练习项目？

**初学者（推荐）**：
1. 先阅读对应阶段的系列文章
2. 跟着主线项目学习核心概念
3. 独立完成支线练习（不看答案）
4. 遇到问题时查看教程或答案代码
5. 对比自己的实现和答案的差异

**有经验的开发者**：
1. 直接阅读需求和功能清单
2. 独立完成项目
3. 查看答案代码，学习最佳实践
4. 尝试进阶挑战

### 常见问题

**Q: 必须做支线项目吗？**
A: 不是必须的，但强烈推荐。支线项目可以帮你巩固知识，加深理解。

**Q: 可以跳过某个项目吗？**
A: 可以，但建议按顺序完成，因为后面的项目会用到前面学到的知识。

**Q: 我的实现和答案不一样，是错的吗？**
A: 不一定！只要功能正常，代码清晰，就是好的实现。答案只是参考。

**Q: 进阶挑战必须做吗？**
A: 不必须，但可以作为额外练习。如果有时间，强烈建议尝试。

## 🤝 贡献

欢迎：
- 提交更好的实现方案
- 补充更多练习挑战
- 改进教程文档
- 报告 Bug

提交 Pull Request 前，请确保代码能正常运行。

## 📄 许可证

MIT License

## 🔗 相关链接

- [主线项目](https://github.com/yourusername/indie-resource-hub) - 独立开发者资源库
- [系列导读](链接待补充) - 了解系列定位和学习路径
- [学习资源导航](链接待补充) - 100+ 优质学习资源

---

**系列**: 独立开发全栈速成
**作者**: Barry
**创建日期**: 2025-11-07

---

## 🎉 开始学习

选择一个项目开始吧！

```bash
# Todo List（最简单，推荐先做）
cd 01-todo-list

# 个人名片页
cd 02-personal-card

# 私密笔记（最复杂，需要 Supabase 配置）
cd 03-private-notes
```

祝学习顺利！有问题欢迎提 Issue 交流。
