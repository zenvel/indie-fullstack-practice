# Todo List 完整教程

> 本教程将带你从零开始构建一个功能完整的 Todo List 应用，掌握 React 组件化开发的核心概念。

## 📚 学习目标

完成本教程后，你将掌握：
- React 组件的创建和组合
- useState Hook 进行状态管理
- 事件处理和表单控制
- 列表渲染和条件渲染
- 组件间数据传递（Props）
- LocalStorage 数据持久化

---

## 🚀 快速开始

### 前置要求

- Node.js 18+ 已安装
- 基础的 HTML/CSS/JavaScript 知识
- 了解 ES6+ 语法（箭头函数、解构等）

### 环境准备

```bash
# 克隆项目
cd indie-fullstack-practice/01-todo-list

# 安装依赖
npm install

# 启动开发服务器
npm run dev
```

---

## 第一步：项目初始化

### 1.1 理解项目结构

```
01-todo-list/
├── app/
│   ├── layout.tsx          # 根布局
│   ├── page.tsx            # 主页面
│   └── globals.css         # 全局样式
├── components/
│   ├── TodoList.tsx        # 待创建
│   ├── TodoItem.tsx        # 待创建
│   └── AddTodoForm.tsx     # 待创建
└── package.json
```

### 1.2 安装依赖

<!-- 详细内容将在撰写系列文章时填充 -->

### 1.3 配置 Tailwind CSS

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 第二步：创建 TodoItem 组件

### 2.1 组件设计

**功能需求：**
- [ ] 显示任务文本
- [ ] 显示完成状态（checkbox）
- [ ] 删除按钮
- [ ] 完成状态的视觉反馈（删除线）

### 2.2 定义 TypeScript 类型

```typescript
// types/todo.ts
interface Todo {
  id: string;
  text: string;
  completed: boolean;
  createdAt: Date;
}
```

### 2.3 实现 TodoItem 组件

```tsx
// components/TodoItem.tsx
interface TodoItemProps {
  todo: Todo;
  onToggle: (id: string) => void;
  onDelete: (id: string) => void;
}

export default function TodoItem({ todo, onToggle, onDelete }: TodoItemProps) {
  // 组件实现代码
  // 详细内容将在撰写系列文章时填充
}
```

### 2.4 样式设计

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 第三步：创建 AddTodoForm 组件

### 3.1 受控组件概念

<!-- 详细内容将在撰写系列文章时填充 -->

### 3.2 实现表单组件

```tsx
// components/AddTodoForm.tsx
interface AddTodoFormProps {
  onAdd: (text: string) => void;
}

export default function AddTodoForm({ onAdd }: AddTodoFormProps) {
  // 组件实现代码
  // 详细内容将在撰写系列文章时填充
}
```

### 3.3 表单验证

- 空值检查
- 去除首尾空格
- 字符长度限制

---

## 第四步：实现 TodoList 组件

### 4.1 状态管理设计

**需要管理的状态：**
- `todos: Todo[]` - 任务列表
- `filter: 'all' | 'active' | 'completed'` - 筛选状态

### 4.2 核心功能实现

```tsx
// components/TodoList.tsx
export default function TodoList() {
  const [todos, setTodos] = useState<Todo[]>([]);

  // 添加任务
  const handleAddTodo = (text: string) => {
    // 实现代码
  };

  // 切换完成状态
  const handleToggleTodo = (id: string) => {
    // 实现代码
  };

  // 删除任务
  const handleDeleteTodo = (id: string) => {
    // 实现代码
  };

  // 详细内容将在撰写系列文章时填充
}
```

### 4.3 列表渲染

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 第五步：添加筛选功能

### 5.1 筛选按钮设计

<!-- 详细内容将在撰写系列文章时填充 -->

### 5.2 实现筛选逻辑

```tsx
const filteredTodos = useMemo(() => {
  switch (filter) {
    case 'active':
      return todos.filter(todo => !todo.completed);
    case 'completed':
      return todos.filter(todo => todo.completed);
    default:
      return todos;
  }
}, [todos, filter]);
```

### 5.3 统计信息显示

- 总任务数
- 已完成数
- 未完成数

---

## 第六步：数据持久化

### 6.1 LocalStorage 基础

<!-- 详细内容将在撰写系列文章时填充 -->

### 6.2 实现保存和加载

```tsx
// 保存到 LocalStorage
useEffect(() => {
  localStorage.setItem('todos', JSON.stringify(todos));
}, [todos]);

// 从 LocalStorage 加载
useEffect(() => {
  const saved = localStorage.getItem('todos');
  if (saved) {
    setTodos(JSON.parse(saved));
  }
}, []);
```

### 6.3 处理数据序列化

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 第七步：进阶功能（可选）

### 7.1 编辑任务

- 双击进入编辑模式
- 保存和取消编辑

### 7.2 批量操作

- 全选/取消全选
- 清除已完成任务

### 7.3 拖拽排序

```tsx
// 使用 react-beautiful-dnd 或其他拖拽库
// 详细内容将在撰写系列文章时填充
```

### 7.4 优先级标记

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 🧪 测试和调试

### 功能测试清单

- [ ] 添加任务
- [ ] 标记完成/取消完成
- [ ] 删除任务
- [ ] 筛选功能（全部/进行中/已完成）
- [ ] 数据持久化（刷新页面后数据保留）
- [ ] 边界情况（空输入、特殊字符等）

### 常见问题排查

<!-- 详细内容将在撰写系列文章时填充 -->

---

## 📖 相关资源

### 系列文章
- **1.5 支线挑战 - Todo List**（待发布）

### 官方文档
- [React Hooks 文档](https://react.dev/reference/react)
- [TypeScript 手册](https://www.typescriptlang.org/docs/)

### 参考实现
- 本教程完整代码：查看仓库分支 `todo-list-complete`

---

## 🎯 下一步

完成 Todo List 后，建议：

1. **优化用户体验**
   - 添加动画效果
   - 改进移动端适配
   - 添加键盘快捷键

2. **学习下一个项目**
   - [02-personal-card](../02-personal-card/README.md) - 学习 Next.js 路由和 SEO
   - [03-private-notes](../03-private-notes/README.md) - 学习 Supabase 认证和数据库

3. **回到主项目**
   - 将 Todo List 的概念应用到 [Indie Resource Hub](../../indie-resource-hub/README.md) 中

---

## 💬 反馈和支持

如有问题或建议，欢迎：
- 提交 Issue
- 加入开发者社区讨论
- 查看系列文章评论区

**祝学习愉快！🚀**
