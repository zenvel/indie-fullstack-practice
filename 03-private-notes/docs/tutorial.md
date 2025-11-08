# 私密笔记 App 完整教程

> 本教程将带你构建一个全栈私密笔记应用，深入学习 Supabase 认证、数据库和行级安全策略（RLS）。

## 📚 学习目标

完成本教程后，你将掌握：
- Supabase 项目创建和配置
- 邮箱/密码认证实现
- PostgreSQL 数据库设计
- 行级安全策略（RLS）配置
- 实时数据订阅
- CRUD 操作完整实现
- 用户会话管理

---

## 🚀 快速开始

### 前置要求

- 完成前两个侧边项目（推荐）
- Node.js 18+ 已安装
- Supabase 账号（免费注册：https://supabase.com）
- 基础的 SQL 知识（可选）

### 环境准备

```bash
# 进入项目目录
cd indie-fullstack-practice/03-private-notes

# 安装依赖
npm install

# 配置环境变量（见步骤一）
cp .env.example .env.local

# 启动开发服务器
npm run dev
```

---

## 第一步：创建 Supabase 项目

### 1.1 注册并创建项目

1. 访问 https://supabase.com 并登录
2. 点击 "New Project"
3. 填写项目信息：
   - Name: `private-notes-app`
   - Database Password: 生成强密码并保存
   - Region: 选择离你最近的区域
4. 等待项目创建（约 2 分钟）

### 1.2 获取 API 密钥

**在项目仪表板中：**
1. 进入 Settings → API
2. 复制以下信息：
   - Project URL
   - `anon` public key

### 1.3 配置环境变量

```bash
# .env.local
NEXT_PUBLIC_SUPABASE_URL=your-project-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

⚠️ **安全提示：** 永远不要将 `.env.local` 提交到 Git！

---

## 第二步：设计数据库架构

### 2.1 数据表设计

**notes 表结构：**

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | uuid | PRIMARY KEY, DEFAULT uuid_generate_v4() | 笔记ID |
| user_id | uuid | NOT NULL, REFERENCES auth.users(id) | 用户ID（外键） |
| title | text | NOT NULL | 笔记标题 |
| content | text | - | 笔记内容 |
| created_at | timestamptz | DEFAULT now() | 创建时间 |
| updated_at | timestamptz | DEFAULT now() | 更新时间 |

### 2.2 在 Supabase 中创建表

**方式一：使用 SQL 编辑器**

进入 Supabase Dashboard → SQL Editor，执行：

```sql
-- 创建 notes 表
CREATE TABLE notes (
  id uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id uuid NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  title text NOT NULL,
  content text,
  created_at timestamptz DEFAULT now(),
  updated_at timestamptz DEFAULT now()
);

-- 创建索引以提高查询性能
CREATE INDEX notes_user_id_idx ON notes(user_id);
CREATE INDEX notes_created_at_idx ON notes(created_at DESC);

-- 创建自动更新 updated_at 的触发器
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = now();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_notes_updated_at
  BEFORE UPDATE ON notes
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();
```

**方式二：使用表编辑器**

<!-- 详细步骤将在撰写系列文章时填充 -->

### 2.3 配置行级安全策略（RLS）

**启用 RLS：**

```sql
-- 启用行级安全
ALTER TABLE notes ENABLE ROW LEVEL SECURITY;

-- 策略 1：用户只能查看自己的笔记
CREATE POLICY "Users can view own notes"
  ON notes
  FOR SELECT
  USING (auth.uid() = user_id);

-- 策略 2：用户只能插入自己的笔记
CREATE POLICY "Users can insert own notes"
  ON notes
  FOR INSERT
  WITH CHECK (auth.uid() = user_id);

-- 策略 3：用户只能更新自己的笔记
CREATE POLICY "Users can update own notes"
  ON notes
  FOR UPDATE
  USING (auth.uid() = user_id)
  WITH CHECK (auth.uid() = user_id);

-- 策略 4：用户只能删除自己的笔记
CREATE POLICY "Users can delete own notes"
  ON notes
  FOR DELETE
  USING (auth.uid() = user_id);
```

**RLS 策略解释：**
- `auth.uid()` 返回当前登录用户的 ID
- `USING` 子句定义哪些行可以被操作
- `WITH CHECK` 子句定义插入/更新时的约束

---

## 第三步：配置 Supabase 客户端

### 3.1 创建客户端工具

```typescript
// lib/supabase.ts
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL!
const supabaseAnonKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!

export const supabase = createClient(supabaseUrl, supabaseAnonKey)

// 类型定义
export type Note = {
  id: string
  user_id: string
  title: string
  content: string | null
  created_at: string
  updated_at: string
}
```

### 3.2 创建认证工具函数

```typescript
// lib/auth.ts
import { supabase } from './supabase'

// 注册
export async function signUp(email: string, password: string) {
  const { data, error } = await supabase.auth.signUp({
    email,
    password,
  })

  if (error) throw error
  return data
}

// 登录
export async function signIn(email: string, password: string) {
  const { data, error } = await supabase.auth.signInWithPassword({
    email,
    password,
  })

  if (error) throw error
  return data
}

// 登出
export async function signOut() {
  const { error } = await supabase.auth.signOut()
  if (error) throw error
}

// 获取当前用户
export async function getCurrentUser() {
  const { data: { user } } = await supabase.auth.getUser()
  return user
}
```

### 3.3 设置认证监听器

```typescript
// app/providers.tsx
'use client'

import { createContext, useContext, useEffect, useState } from 'react'
import { User } from '@supabase/supabase-js'
import { supabase } from '@/lib/supabase'

interface AuthContextType {
  user: User | null
  loading: boolean
}

const AuthContext = createContext<AuthContextType>({
  user: null,
  loading: true,
})

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState<User | null>(null)
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    // 获取初始会话
    supabase.auth.getSession().then(({ data: { session } }) => {
      setUser(session?.user ?? null)
      setLoading(false)
    })

    // 监听认证状态变化
    const { data: { subscription } } = supabase.auth.onAuthStateChange(
      (_event, session) => {
        setUser(session?.user ?? null)
      }
    )

    return () => subscription.unsubscribe()
  }, [])

  return (
    <AuthContext.Provider value={{ user, loading }}>
      {children}
    </AuthContext.Provider>
  )
}

export const useAuth = () => useContext(AuthContext)
```

---

## 第四步：实现认证界面

### 4.1 创建登录/注册表单

```tsx
// components/AuthForm.tsx
'use client'

import { useState } from 'react'
import { signIn, signUp } from '@/lib/auth'
import { useRouter } from 'next/navigation'

export default function AuthForm() {
  const [isLogin, setIsLogin] = useState(true)
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState<string | null>(null)
  const router = useRouter()

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault()
    setLoading(true)
    setError(null)

    try {
      if (isLogin) {
        await signIn(email, password)
        router.push('/notes')
      } else {
        await signUp(email, password)
        setError('请检查邮箱完成注册验证')
      }
    } catch (err: any) {
      setError(err.message)
    } finally {
      setLoading(false)
    }
  }

  // 表单 UI 实现
  // 详细内容将在撰写系列文章时填充
}
```

### 4.2 创建登录页面

```tsx
// app/login/page.tsx
import AuthForm from '@/components/AuthForm'

export default function LoginPage() {
  return (
    <div className="min-h-screen flex items-center justify-center">
      <div className="w-full max-w-md">
        <h1 className="text-3xl font-bold text-center mb-8">
          私密笔记
        </h1>
        <AuthForm />
      </div>
    </div>
  )
}
```

### 4.3 实现路由保护

```tsx
// middleware.ts
import { createMiddlewareClient } from '@supabase/auth-helpers-nextjs'
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export async function middleware(req: NextRequest) {
  const res = NextResponse.next()
  const supabase = createMiddlewareClient({ req, res })

  const {
    data: { session },
  } = await supabase.auth.getSession()

  // 如果访问 /notes 但未登录，重定向到登录页
  if (!session && req.nextUrl.pathname.startsWith('/notes')) {
    return NextResponse.redirect(new URL('/login', req.url))
  }

  // 如果已登录但访问登录页，重定向到笔记页
  if (session && req.nextUrl.pathname === '/login') {
    return NextResponse.redirect(new URL('/notes', req.url))
  }

  return res
}

export const config = {
  matcher: ['/notes/:path*', '/login'],
}
```

---

## 第五步：实现笔记 CRUD 功能

### 5.1 创建笔记服务

```typescript
// lib/notes.ts
import { supabase, Note } from './supabase'

// 获取所有笔记
export async function getNotes(): Promise<Note[]> {
  const { data, error } = await supabase
    .from('notes')
    .select('*')
    .order('created_at', { ascending: false })

  if (error) throw error
  return data || []
}

// 创建笔记
export async function createNote(
  title: string,
  content: string
): Promise<Note> {
  const { data: { user } } = await supabase.auth.getUser()
  if (!user) throw new Error('未登录')

  const { data, error } = await supabase
    .from('notes')
    .insert([{ title, content, user_id: user.id }])
    .select()
    .single()

  if (error) throw error
  return data
}

// 更新笔记
export async function updateNote(
  id: string,
  title: string,
  content: string
): Promise<Note> {
  const { data, error } = await supabase
    .from('notes')
    .update({ title, content })
    .eq('id', id)
    .select()
    .single()

  if (error) throw error
  return data
}

// 删除笔记
export async function deleteNote(id: string): Promise<void> {
  const { error } = await supabase
    .from('notes')
    .delete()
    .eq('id', id)

  if (error) throw error
}
```

### 5.2 创建笔记列表组件

```tsx
// components/NotesList.tsx
'use client'

import { useEffect, useState } from 'react'
import { getNotes, deleteNote } from '@/lib/notes'
import type { Note } from '@/lib/supabase'

export default function NotesList() {
  const [notes, setNotes] = useState<Note[]>([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    loadNotes()
  }, [])

  async function loadNotes() {
    try {
      const data = await getNotes()
      setNotes(data)
    } catch (error) {
      console.error('加载笔记失败:', error)
    } finally {
      setLoading(false)
    }
  }

  async function handleDelete(id: string) {
    if (!confirm('确定要删除这条笔记吗？')) return

    try {
      await deleteNote(id)
      setNotes(notes.filter(note => note.id !== id))
    } catch (error) {
      console.error('删除失败:', error)
    }
  }

  // 组件 UI 实现
  // 详细内容将在撰写系列文章时填充
}
```

### 5.3 创建笔记编辑器组件

```tsx
// components/NoteEditor.tsx
'use client'

import { useState } from 'react'
import { createNote, updateNote } from '@/lib/notes'
import type { Note } from '@/lib/supabase'

interface NoteEditorProps {
  note?: Note
  onSave: (note: Note) => void
  onCancel: () => void
}

export default function NoteEditor({ note, onSave, onCancel }: NoteEditorProps) {
  const [title, setTitle] = useState(note?.title || '')
  const [content, setContent] = useState(note?.content || '')
  const [saving, setSaving] = useState(false)

  async function handleSave() {
    setSaving(true)
    try {
      const savedNote = note
        ? await updateNote(note.id, title, content)
        : await createNote(title, content)

      onSave(savedNote)
    } catch (error) {
      console.error('保存失败:', error)
    } finally {
      setSaving(false)
    }
  }

  // 组件 UI 实现
  // 详细内容将在撰写系列文章时填充
}
```

### 5.4 创建笔记主页面

```tsx
// app/notes/page.tsx
'use client'

import { useState } from 'react'
import NotesList from '@/components/NotesList'
import NoteEditor from '@/components/NoteEditor'
import { signOut } from '@/lib/auth'
import { useRouter } from 'next/navigation'

export default function NotesPage() {
  const [showEditor, setShowEditor] = useState(false)
  const router = useRouter()

  async function handleSignOut() {
    await signOut()
    router.push('/login')
  }

  return (
    <div className="min-h-screen bg-gray-50">
      <header className="bg-white border-b">
        <div className="container mx-auto px-4 py-4 flex justify-between items-center">
          <h1 className="text-2xl font-bold">我的笔记</h1>
          <div className="flex gap-4">
            <button
              onClick={() => setShowEditor(true)}
              className="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700"
            >
              新建笔记
            </button>
            <button
              onClick={handleSignOut}
              className="px-4 py-2 border rounded hover:bg-gray-50"
            >
              退出登录
            </button>
          </div>
        </div>
      </header>

      <main className="container mx-auto px-4 py-8">
        {showEditor ? (
          <NoteEditor
            onSave={(note) => {
              setShowEditor(false)
              // 刷新列表
            }}
            onCancel={() => setShowEditor(false)}
          />
        ) : (
          <NotesList />
        )}
      </main>
    </div>
  )
}
```

---

## 第六步：实现实时功能

### 6.1 订阅数据变化

```tsx
// components/NotesList.tsx (添加实时订阅)
useEffect(() => {
  loadNotes()

  // 订阅笔记表的变化
  const channel = supabase
    .channel('notes-changes')
    .on(
      'postgres_changes',
      {
        event: '*', // 监听所有事件：INSERT, UPDATE, DELETE
        schema: 'public',
        table: 'notes',
      },
      (payload) => {
        console.log('数据变化:', payload)

        if (payload.eventType === 'INSERT') {
          setNotes(prev => [payload.new as Note, ...prev])
        } else if (payload.eventType === 'UPDATE') {
          setNotes(prev =>
            prev.map(note =>
              note.id === payload.new.id ? (payload.new as Note) : note
            )
          )
        } else if (payload.eventType === 'DELETE') {
          setNotes(prev => prev.filter(note => note.id !== payload.old.id))
        }
      }
    )
    .subscribe()

  return () => {
    supabase.removeChannel(channel)
  }
}, [])
```

### 6.2 优化实时体验

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 第七步：进阶功能

### 7.1 富文本编辑器

**集成 Tiptap 或 Quill：**

```bash
npm install @tiptap/react @tiptap/starter-kit
```

<!-- 详细实现将在撰写系列文章时填充 -->

### 7.2 笔记分类和标签

**数据库扩展：**

```sql
-- 创建标签表
CREATE TABLE tags (
  id uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  name text UNIQUE NOT NULL,
  color text
);

-- 创建笔记-标签关联表
CREATE TABLE note_tags (
  note_id uuid REFERENCES notes(id) ON DELETE CASCADE,
  tag_id uuid REFERENCES tags(id) ON DELETE CASCADE,
  PRIMARY KEY (note_id, tag_id)
);
```

### 7.3 搜索功能

```typescript
// 全文搜索
export async function searchNotes(query: string): Promise<Note[]> {
  const { data, error } = await supabase
    .from('notes')
    .select('*')
    .or(`title.ilike.%${query}%,content.ilike.%${query}%`)
    .order('created_at', { ascending: false })

  if (error) throw error
  return data || []
}
```

### 7.4 笔记分享

**实现公开链接分享：**

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 🧪 测试和调试

### 功能测试清单

**认证功能：**
- [ ] 用户注册
- [ ] 邮箱验证
- [ ] 用户登录
- [ ] 用户登出
- [ ] 未登录时重定向

**笔记功能：**
- [ ] 创建笔记
- [ ] 编辑笔记
- [ ] 删除笔记
- [ ] 查看笔记列表
- [ ] 实时同步

**安全测试：**
- [ ] RLS 策略正常工作（用户 A 看不到用户 B 的笔记）
- [ ] API 密钥正确配置
- [ ] 环境变量不泄露

### 使用 Supabase 日志调试

1. 进入 Dashboard → Logs
2. 查看 API 请求日志
3. 检查 PostgreSQL 日志
4. 查看认证事件

---

## 📖 相关资源

### 系列文章
- **3.6 支线挑战 - 私密笔记 App**（待发布）

### 官方文档
- [Supabase 文档](https://supabase.com/docs)
- [Supabase Auth](https://supabase.com/docs/guides/auth)
- [Row Level Security](https://supabase.com/docs/guides/auth/row-level-security)
- [Realtime](https://supabase.com/docs/guides/realtime)

### 视频教程
- [Supabase 完整教程 - Fireship](https://www.youtube.com/watch?v=zBZgdTb-dns)
- [Next.js + Supabase - Full Course](https://www.youtube.com/results?search_query=nextjs+supabase+tutorial)

---

## 🎯 下一步

完成私密笔记 App 后，建议：

1. **功能扩展**
   - 添加 Markdown 支持
   - 实现笔记版本历史
   - 添加笔记导出功能（PDF、Markdown）
   - 实现多设备同步

2. **性能优化**
   - 实现虚拟滚动（处理大量笔记）
   - 添加离线支持（PWA）
   - 优化数据库查询

3. **回到主项目**
   - 将 Supabase 认证和数据库知识应用到 [Indie Resource Hub](../../indie-resource-hub/README.md)
   - 实现用户收藏、评论等功能

---

## 🔒 安全最佳实践

1. **永远不要在客户端暴露 service_role 密钥**
2. **使用 RLS 策略保护所有表**
3. **验证用户输入防止 SQL 注入**
4. **使用 HTTPS（生产环境）**
5. **定期审计 RLS 策略**
6. **启用邮箱验证**
7. **实现速率限制（防止滥用）**

---

## 💬 反馈和支持

如有问题或建议，欢迎：
- 提交 Issue
- 加入开发者社区讨论
- 查看系列文章评论区

**祝学习愉快！🚀**
