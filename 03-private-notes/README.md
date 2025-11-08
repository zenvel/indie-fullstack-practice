# Private Notes - 私密笔记应用

> 《独立开发全栈系列》阶段三支线练习项目

## 🎯 学习目标

通过这个项目，你将练习：
- ✅ Supabase 用户认证
- ✅ Row Level Security（RLS）数据隔离
- ✅ 数据库 CRUD 操作
- ✅ 全栈应用开发
- ✅ 数据安全最佳实践

## 💻 功能清单

### 基础功能

- [x] 用户注册/登录
- [x] 创建笔记
- [x] 编辑笔记
- [x] 删除笔记
- [x] 笔记列表
- [x] 数据隔离（用户只能看到自己的笔记）

### 进阶功能（可选）

- [ ] Markdown 实时预览
- [ ] 全文搜索
- [ ] 标签系统
- [ ] 笔记分类
- [ ] 笔记分享功能
- [ ] 导出为 PDF

## 🚀 快速开始

### 前置要求

需要一个 Supabase 账号：
1. 访问 [supabase.com](https://supabase.com)
2. 创建新项目
3. 获取项目 URL 和 API Key

### 配置环境变量

```bash
cd 03-private-notes
cp .env.example .env.local

# 编辑 .env.local，填入你的 Supabase 配置
NEXT_PUBLIC_SUPABASE_URL=your-project-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

### 创建数据库表

在 Supabase Dashboard 执行以下 SQL：

```sql
-- 创建笔记表
CREATE TABLE notes (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id) NOT NULL,
  title TEXT NOT NULL,
  content TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 启用 Row Level Security
ALTER TABLE notes ENABLE ROW LEVEL SECURITY;

-- 创建策略：用户只能管理自己的笔记
CREATE POLICY "Users can manage own notes"
ON notes FOR ALL
USING (auth.uid() = user_id);
```

### 安装依赖

```bash
npm install
```

### 运行项目

```bash
npm run dev
```

访问 [http://localhost:3000](http://localhost:3000)

## 📚 完整教程

详细的手把手教程见：[docs/tutorial.md](docs/tutorial.md)

或阅读系列文章：[3.6 支线挑战 - 私密笔记 App](链接待补充)

## 🔧 技术栈

- Next.js 15.0.3
- React 18.3.1
- Supabase 2.39.0
- Tailwind CSS 3.4.0
- TypeScript 5.3.3

## 📂 项目结构

```
03-private-notes/
├── app/
│   ├── layout.tsx          # 根布局
│   ├── page.tsx            # 登录页
│   ├── register/
│   │   └── page.tsx        # 注册页
│   ├── notes/
│   │   ├── page.tsx        # 笔记列表
│   │   ├── new/
│   │   │   └── page.tsx    # 新建笔记
│   │   └── [id]/
│   │       └── page.tsx    # 编辑笔记
│   └── auth/
│       └── callback/
│           └── route.ts    # Auth 回调
├── components/
│   ├── NoteCard.tsx        # 笔记卡片
│   ├── NoteList.tsx        # 笔记列表
│   ├── NoteEditor.tsx      # 笔记编辑器
│   └── AuthForm.tsx        # 登录/注册表单
└── lib/
    ├── supabase.ts         # Supabase 客户端
    ├── database.ts         # 数据库操作
    └── types.ts            # TypeScript 类型
```

## 💡 实现提示

### 数据模型

```typescript
interface Note {
  id: string;
  user_id: string;
  title: string;
  content: string;
  created_at: string;
  updated_at: string;
}
```

### Supabase 客户端

```typescript
// lib/supabase.ts
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL!;
const supabaseKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!;

export const supabase = createClient(supabaseUrl, supabaseKey);
```

### 用户认证

```typescript
// 注册
const { data, error } = await supabase.auth.signUp({
  email: 'user@example.com',
  password: 'password123',
});

// 登录
const { data, error } = await supabase.auth.signInWithPassword({
  email: 'user@example.com',
  password: 'password123',
});

// 登出
await supabase.auth.signOut();

// 获取当前用户
const { data: { user } } = await supabase.auth.getUser();
```

### CRUD 操作

```typescript
// 创建笔记
const { data, error } = await supabase
  .from('notes')
  .insert({
    title: 'My Note',
    content: 'Note content...',
  })
  .select()
  .single();

// 查询笔记列表
const { data, error } = await supabase
  .from('notes')
  .select('*')
  .order('created_at', { ascending: false });

// 更新笔记
const { data, error } = await supabase
  .from('notes')
  .update({ title: 'New Title', content: 'New content' })
  .eq('id', noteId)
  .select()
  .single();

// 删除笔记
const { error } = await supabase
  .from('notes')
  .delete()
  .eq('id', noteId);
```

## 🔒 安全要点

### Row Level Security

RLS 策略确保：
- ✅ 用户只能看到自己创建的笔记
- ✅ 用户不能查看他人的笔记
- ✅ 用户不能修改他人的笔记
- ✅ 数据库级别的安全保护

### 测试 RLS

创建两个测试账号，验证：
1. 用户 A 创建的笔记
2. 用户 B 看不到用户 A 的笔记
3. 用户 B 不能修改用户 A 的笔记

## 🎨 UI 建议

### 布局设计

- **登录页**：居中的登录表单
- **笔记列表**：侧边栏 + 主内容区
- **笔记编辑**：简洁的编辑器界面

### 组件建议

- `<AuthForm />` - 复用登录/注册表单
- `<NoteCard />` - 笔记卡片（标题、预览、时间）
- `<NoteEditor />` - 文本编辑器或 Markdown 编辑器

## ✅ 完成标准

你的私密笔记应用应该：
- ✅ 用户能注册和登录
- ✅ 能创建、编辑、删除笔记
- ✅ 笔记列表正常显示
- ✅ 数据隔离正确（用户 A 看不到用户 B 的笔记）
- ✅ 登出后无法访问笔记页面
- ✅ 代码结构清晰
- ✅ 错误处理完善

## 🌟 进阶挑战

完成基础版后，可以尝试：

1. **Markdown 支持**
   - 使用 `react-markdown`
   - 实时预览
   - 语法高亮

2. **全文搜索**
   - 搜索标题和内容
   - 高亮搜索结果

3. **标签系统**
   - 为笔记添加标签
   - 按标签筛选

4. **笔记分享**
   - 生成分享链接
   - 设置分享权限
   - 取消分享

## 🔗 相关资源

- [Supabase Auth 文档](https://supabase.com/docs/guides/auth)
- [Supabase Database 文档](https://supabase.com/docs/guides/database)
- [Row Level Security 详解](https://supabase.com/docs/guides/auth/row-level-security)
- [Next.js + Supabase 官方教程](https://supabase.com/docs/guides/getting-started/tutorials/with-nextjs)

---

**难度**: ⭐⭐⭐⭐ (中高级)
**预计时间**: 2-3 天
**状态**: ✅ 可开始
**注意**: 需要 Supabase 账号和项目
