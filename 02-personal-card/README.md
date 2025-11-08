# Personal Card - 个人名片页

> 《独立开发全栈系列》阶段二支线练习项目

## 🎯 学习目标

通过这个项目，你将练习：
- ✅ Next.js App Router 文件路由
- ✅ 布局系统（Layout）
- ✅ SEO 配置（Metadata API）
- ✅ 多页面应用开发
- ✅ 部署到 Vercel

## 💻 功能清单

### 基础功能

- [x] 首页（Hero Section + 自我介绍）
- [x] 项目展示页
- [x] 联系页面
- [x] 统一导航栏
- [x] 响应式设计
- [x] SEO 优化

### 进阶功能（可选）

- [ ] 博客列表页（MDX）
- [ ] 中英文切换
- [ ] Dark Mode
- [ ] 动画效果（Framer Motion）
- [ ] RSS 订阅

## 🚀 快速开始

### 安装依赖

```bash
cd 02-personal-card
npm install
```

### 运行项目

```bash
npm run dev
```

访问 [http://localhost:3000](http://localhost:3000)

## 📚 完整教程

详细的手把手教程见：[docs/tutorial.md](docs/tutorial.md)

或阅读系列文章：[2.6 支线挑战 - 个人名片页](链接待补充)

## 🔧 技术栈

- Next.js 15.0.3
- React 18.3.1
- Tailwind CSS 3.4.0
- Shadcn/ui（可选）
- TypeScript 5.3.3

## 📝 页面结构

```
app/
├── layout.tsx          # 根布局（导航 + Footer）
├── page.tsx            # 首页
├── projects/
│   └── page.tsx        # 项目展示
├── blog/
│   └── page.tsx        # 博客列表（可选）
└── contact/
    └── page.tsx        # 联系方式
```

## 💡 实现提示

### 首页 Hero Section

```tsx
export default function Home() {
  return (
    <main className="min-h-screen flex items-center justify-center">
      <div className="text-center">
        <h1 className="text-5xl font-bold mb-4">你的名字</h1>
        <p className="text-xl text-gray-600">全栈开发者 / 独立开发者</p>
        <div className="mt-8 flex gap-4 justify-center">
          <a href="/projects" className="btn-primary">查看项目</a>
          <a href="/contact" className="btn-secondary">联系我</a>
        </div>
      </div>
    </main>
  );
}
```

### SEO Metadata

```tsx
// app/layout.tsx
import type { Metadata } from 'next';

export const metadata: Metadata = {
  title: {
    default: '你的名字 - 全栈开发者',
    template: '%s | 你的名字',
  },
  description: '我是一名全栈开发者，专注于...',
  openGraph: {
    type: 'website',
    locale: 'zh_CN',
    url: 'https://yourname.com',
    siteName: '你的名字',
  },
};
```

### 导航组件

```tsx
// components/Header.tsx
import Link from 'next/link';

export function Header() {
  return (
    <header className="border-b">
      <nav className="container mx-auto px-4 py-4">
        <div className="flex justify-between items-center">
          <Link href="/" className="text-xl font-bold">你的名字</Link>
          <div className="flex gap-6">
            <Link href="/projects">项目</Link>
            <Link href="/blog">博客</Link>
            <Link href="/contact">联系</Link>
          </div>
        </div>
      </nav>
    </header>
  );
}
```

## 🎨 设计建议

### 配色方案

使用 Tailwind CSS 的默认色板，或自定义：
- 主色：`blue-600`
- 辅色：`gray-600`
- 背景：`white` / `gray-50`

### 布局建议

- **首页**：全屏居中的 Hero Section
- **项目页**：Grid 布局展示项目卡片
- **联系页**：简洁的联系方式列表
- **导航**：固定在顶部或简洁的水平导航

### 响应式

```css
/* Mobile First */
.grid {
  @apply grid-cols-1;      /* 手机：1列 */
  @apply md:grid-cols-2;   /* 平板：2列 */
  @apply lg:grid-cols-3;   /* 桌面：3列 */
}
```

## 🚀 部署到 Vercel

1. 推送代码到 GitHub
2. 访问 [vercel.com](https://vercel.com)
3. 导入项目
4. 一键部署

详细步骤见教程。

## ✅ 完成标准

你的个人名片页应该：
- ✅ 至少有 3 个页面（首页/项目/联系）
- ✅ 导航和布局正常工作
- ✅ 每个页面都配置了正确的 SEO Metadata
- ✅ 响应式设计在各种屏幕尺寸下都正常
- ✅ 部署到 Vercel 并能访问
- ✅ 代码结构清晰

## 🌟 进阶挑战

完成基础版后，可以尝试：

1. **添加博客系统**
   - 使用 MDX 编写文章
   - 文章列表和详情页
   - 分类和标签

2. **国际化**
   - 中英文切换
   - 使用 next-intl

3. **动画效果**
   - 使用 Framer Motion
   - 页面切换动画
   - 元素进入动画

4. **Dark Mode**
   - 使用 next-themes
   - 系统主题检测
   - 主题切换按钮

## 🔗 相关资源

- [Next.js App Router 文档](https://nextjs.org/docs/app)
- [Next.js Metadata API](https://nextjs.org/docs/app/building-your-application/optimizing/metadata)
- [Tailwind CSS 文档](https://tailwindcss.com/docs)
- [Vercel 部署文档](https://vercel.com/docs)

---

**难度**: ⭐⭐⭐ (中级)
**预计时间**: 1-2 天
**状态**: ✅ 可开始
