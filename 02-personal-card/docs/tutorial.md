# 个人名片页完整教程

> 本教程将带你构建一个专业的个人名片网站，掌握 Next.js 的核心特性和 SEO 优化技巧。

## 📚 学习目标

完成本教程后，你将掌握：
- Next.js App Router 的使用
- 文件系统路由和动态路由
- SEO 优化和 Metadata API
- 响应式设计和移动端适配
- 静态资源管理
- 部署到 Vercel

---

## 🚀 快速开始

### 前置要求

- 完成 [01-todo-list](../../01-todo-list/README.md) 项目（推荐）
- Node.js 18+ 已安装
- 熟悉 React 基础概念

### 环境准备

```bash
# 进入项目目录
cd indie-fullstack-practice/02-personal-card

# 安装依赖
npm install

# 启动开发服务器
npm run dev
```

访问 http://localhost:3000 查看效果。

---

## 第一步：理解 Next.js App Router

### 1.1 项目结构解析

```
02-personal-card/
├── app/
│   ├── layout.tsx          # 根布局
│   ├── page.tsx            # 首页（/）
│   ├── about/
│   │   └── page.tsx        # 关于页（/about）
│   ├── projects/
│   │   └── page.tsx        # 项目页（/projects）
│   └── contact/
│       └── page.tsx        # 联系页（/contact）
├── components/
│   ├── Header.tsx          # 待创建
│   ├── Footer.tsx          # 待创建
│   ├── Hero.tsx            # 待创建
│   └── ProjectCard.tsx     # 待创建
├── public/
│   ├── avatar.jpg          # 待添加
│   └── projects/           # 待添加
└── lib/
    └── data.ts             # 数据配置
```

### 1.2 App Router vs Pages Router

<!-- 详细内容将在撰写系列文章时填充 -->

### 1.3 文件约定

- `page.tsx` - 页面组件
- `layout.tsx` - 布局组件
- `loading.tsx` - 加载状态
- `error.tsx` - 错误处理
- `not-found.tsx` - 404 页面

---

## 第二步：创建根布局

### 2.1 设计全局布局

**布局结构：**
```
┌─────────────────────────────┐
│         Header              │
├─────────────────────────────┤
│                             │
│         Page Content        │
│                             │
├─────────────────────────────┤
│         Footer              │
└─────────────────────────────┘
```

### 2.2 实现 Layout 组件

```tsx
// app/layout.tsx
import type { Metadata } from 'next'
import { Inter } from 'next/font/google'
import './globals.css'
import Header from '@/components/Header'
import Footer from '@/components/Footer'

const inter = Inter({ subsets: ['latin'] })

export const metadata: Metadata = {
  // Metadata 配置
  // 详细内容将在撰写系列文章时填充
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="zh-CN">
      <body className={inter.className}>
        <Header />
        <main className="min-h-screen">
          {children}
        </main>
        <Footer />
      </body>
    </html>
  )
}
```

### 2.3 配置字体优化

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 第三步：创建导航组件

### 3.1 Header 组件设计

**功能需求：**
- [ ] Logo/名字显示
- [ ] 导航链接（首页、关于、项目、联系）
- [ ] 移动端响应式菜单
- [ ] 活动状态高亮
- [ ] 暗色模式切换（可选）

### 3.2 实现 Header 组件

```tsx
// components/Header.tsx
'use client'

import Link from 'next/link'
import { usePathname } from 'next/navigation'

const navItems = [
  { name: '首页', path: '/' },
  { name: '关于', path: '/about' },
  { name: '项目', path: '/projects' },
  { name: '联系', path: '/contact' },
]

export default function Header() {
  const pathname = usePathname()

  // 组件实现代码
  // 详细内容将在撰写系列文章时填充
}
```

### 3.3 使用 `usePathname` Hook

<!-- 详细内容将在撰写系列文章时填充 -->

### 3.4 移动端菜单实现

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 第四步：创建 Hero 区域

### 4.1 Hero 区域设计

**内容元素：**
- 头像照片
- 姓名和职位
- 个人简介
- 社交链接（GitHub、Twitter、Email）
- CTA 按钮（联系我、查看项目）

### 4.2 实现 Hero 组件

```tsx
// components/Hero.tsx
import Image from 'next/image'
import Link from 'next/link'

export default function Hero() {
  return (
    <section className="py-20 px-4">
      <div className="container mx-auto max-w-4xl">
        {/* 组件内容 */}
        {/* 详细内容将在撰写系列文章时填充 */}
      </div>
    </section>
  )
}
```

### 4.3 使用 Next.js Image 组件

**优化特性：**
- 自动图片优化
- 懒加载
- 响应式图片
- 防止布局偏移（CLS）

```tsx
<Image
  src="/avatar.jpg"
  alt="Your Name"
  width={200}
  height={200}
  priority
  className="rounded-full"
/>
```

### 4.4 响应式设计

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 第五步：配置 SEO Metadata

### 5.1 理解 Metadata API

<!-- 详细内容将在撰写系列文章时填充 -->

### 5.2 根布局 Metadata

```tsx
// app/layout.tsx
export const metadata: Metadata = {
  title: {
    default: 'Your Name - Full Stack Developer',
    template: '%s | Your Name'
  },
  description: 'Personal website of Your Name, a passionate full-stack developer',
  keywords: ['Next.js', 'React', 'Full Stack', 'Web Development'],
  authors: [{ name: 'Your Name' }],
  creator: 'Your Name',
  openGraph: {
    type: 'website',
    locale: 'zh_CN',
    url: 'https://yourname.com',
    siteName: 'Your Name',
    images: [
      {
        url: '/og-image.jpg',
        width: 1200,
        height: 630,
        alt: 'Your Name - Full Stack Developer',
      },
    ],
  },
  twitter: {
    card: 'summary_large_image',
    title: 'Your Name - Full Stack Developer',
    description: 'Personal website of Your Name',
    images: ['/og-image.jpg'],
  },
}
```

### 5.3 页面级 Metadata

```tsx
// app/about/page.tsx
export const metadata: Metadata = {
  title: '关于我',
  description: '了解更多关于我的背景、技能和经验',
}
```

### 5.4 动态 Metadata

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 第六步：实现关于页面

### 6.1 页面结构设计

**内容板块：**
1. 个人介绍
2. 技能列表
3. 工作经历
4. 教育背景

### 6.2 创建数据配置

```typescript
// lib/data.ts
export const skills = [
  {
    category: '前端',
    items: ['React', 'Next.js', 'TypeScript', 'Tailwind CSS']
  },
  {
    category: '后端',
    items: ['Node.js', 'Supabase', 'PostgreSQL']
  },
  // 更多技能分类
]

export const experiences = [
  {
    company: '公司名称',
    position: '职位',
    period: '2023.01 - 至今',
    description: '工作描述...',
  },
  // 更多经历
]
```

### 6.3 实现关于页面

```tsx
// app/about/page.tsx
import { skills, experiences } from '@/lib/data'

export default function AboutPage() {
  // 页面实现代码
  // 详细内容将在撰写系列文章时填充
}
```

---

## 第七步：创建项目展示页

### 7.1 ProjectCard 组件

```tsx
// components/ProjectCard.tsx
interface Project {
  id: string;
  title: string;
  description: string;
  image: string;
  tags: string[];
  liveUrl?: string;
  githubUrl?: string;
}

interface ProjectCardProps {
  project: Project;
}

export default function ProjectCard({ project }: ProjectCardProps) {
  // 组件实现代码
  // 详细内容将在撰写系列文章时填充
}
```

### 7.2 项目数据配置

```typescript
// lib/data.ts
export const projects: Project[] = [
  {
    id: '1',
    title: 'Indie Resource Hub',
    description: '独立开发者资源导航平台',
    image: '/projects/resource-hub.jpg',
    tags: ['Next.js', 'Supabase', 'TypeScript'],
    liveUrl: 'https://example.com',
    githubUrl: 'https://github.com/yourusername/project',
  },
  // 更多项目
]
```

### 7.3 实现项目页面

```tsx
// app/projects/page.tsx
import ProjectCard from '@/components/ProjectCard'
import { projects } from '@/lib/data'

export default function ProjectsPage() {
  return (
    <div className="container mx-auto px-4 py-12">
      <h1 className="text-4xl font-bold mb-8">我的项目</h1>
      <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
        {projects.map(project => (
          <ProjectCard key={project.id} project={project} />
        ))}
      </div>
    </div>
  )
}
```

---

## 第八步：创建联系页面

### 8.1 联系表单设计

**表单字段：**
- 姓名
- 邮箱
- 主题
- 消息内容

### 8.2 客户端表单实现

```tsx
// app/contact/page.tsx
'use client'

import { useState } from 'react'

export default function ContactPage() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    subject: '',
    message: '',
  })

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault()
    // 表单提交逻辑
    // 详细内容将在撰写系列文章时填充
  }

  // 组件实现代码
}
```

### 8.3 表单验证

<!-- 详细内容将在撰写系列文章时填充 -->

### 8.4 集成邮件服务（可选）

- 使用 Resend / SendGrid
- 创建 API Route
- 处理表单提交

---

## 第九步：优化和部署

### 9.1 性能优化清单

- [ ] 图片优化（使用 WebP 格式）
- [ ] 字体优化（使用 next/font）
- [ ] 代码分割（自动处理）
- [ ] 压缩静态资源
- [ ] 启用 Gzip/Brotli 压缩

### 9.2 SEO 优化清单

- [ ] 添加 robots.txt
- [ ] 生成 sitemap.xml
- [ ] 配置 Open Graph 图片
- [ ] 添加结构化数据（JSON-LD）
- [ ] 优化页面标题和描述

### 9.3 创建 sitemap

```typescript
// app/sitemap.ts
import { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  return [
    {
      url: 'https://yourname.com',
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 1,
    },
    {
      url: 'https://yourname.com/about',
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 0.8,
    },
    // 更多页面
  ]
}
```

### 9.4 部署到 Vercel

```bash
# 安装 Vercel CLI
npm i -g vercel

# 部署
vercel

# 生产环境部署
vercel --prod
```

<!-- 详细部署步骤将在撰写系列文章时填充 -->

---

## 🧪 测试和调试

### 功能测试清单

- [ ] 所有页面正常访问
- [ ] 导航链接正确跳转
- [ ] 图片正常加载
- [ ] 联系表单提交成功
- [ ] 移动端响应式正常
- [ ] SEO meta 标签正确

### 性能测试

使用 Lighthouse 检查：
- Performance: > 90
- Accessibility: > 90
- Best Practices: > 90
- SEO: > 90

### 浏览器兼容性

- Chrome/Edge (最新版本)
- Firefox (最新版本)
- Safari (最新版本)
- 移动端浏览器

---

## 📖 相关资源

### 系列文章
- **2.6 支线挑战 - 个人名片页**（待发布）

### 官方文档
- [Next.js App Router](https://nextjs.org/docs/app)
- [Next.js Metadata API](https://nextjs.org/docs/app/api-reference/functions/generate-metadata)
- [Vercel 部署文档](https://vercel.com/docs)

### 设计灵感
- [Dribbble - Personal Website](https://dribbble.com/tags/personal-website)
- [Awwwards - Portfolio](https://www.awwwards.com/websites/portfolio/)

---

## 🎯 下一步

完成个人名片页后，建议：

1. **持续优化**
   - 添加博客功能
   - 集成 Google Analytics
   - 添加暗色模式
   - 实现多语言支持

2. **学习下一个项目**
   - [03-private-notes](../03-private-notes/README.md) - 学习 Supabase 认证和数据库

3. **回到主项目**
   - 将 SEO 优化技巧应用到 [Indie Resource Hub](../../indie-resource-hub/README.md)

---

## 💬 反馈和支持

如有问题或建议，欢迎：
- 提交 Issue
- 加入开发者社区讨论
- 查看系列文章评论区

**祝学习愉快！🚀**
