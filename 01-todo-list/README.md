# Todo List - 待办事项应用

> 《独立开发全栈系列》阶段一支线练习项目

## 🎯 学习目标

通过这个项目，你将练习：
- ✅ React 状态管理（`useState`）
- ✅ 组件化开发
- ✅ 事件处理（`onClick`, `onChange`）
- ✅ 列表渲染（`map`）
- ✅ 条件渲染
- ✅ 本地存储（`localStorage`）

## 💻 功能清单

### 基础功能

- [x] 添加待办事项
- [x] 标记完成/未完成
- [x] 删除待办事项
- [x] 筛选（全部/已完成/未完成）
- [x] 数据持久化（localStorage）

### 进阶功能（可选）

- [ ] 编辑待办事项
- [ ] 拖拽排序
- [ ] 分类管理
- [ ] 截止日期
- [ ] 优先级设置

## 🚀 快速开始

### 安装依赖

```bash
cd 01-todo-list
npm install
```

### 运行项目

```bash
npm run dev
```

访问 [http://localhost:3000](http://localhost:3000)

## 📚 完整教程

详细的手把手教程见：[docs/tutorial.md](docs/tutorial.md)

或阅读系列文章：[1.5 支线挑战 - Todo List](链接待补充)

## 🔧 技术栈

- Next.js 15.0.3
- React 18.3.1
- Tailwind CSS 3.4.0
- TypeScript 5.3.3

## 📝 学习建议

1. **先自己尝试**
   - 不看答案，根据功能清单自己实现
   - 遇到问题先思考，再查文档

2. **对比答案代码**
   - 完成后对比你的实现和答案代码
   - 学习更好的实现方式

3. **尝试进阶功能**
   - 完成基础功能后，挑战进阶功能
   - 自由发挥，添加你想要的功能

## 💡 实现提示

### 数据结构

```typescript
interface Todo {
  id: string;
  text: string;
  completed: boolean;
  createdAt: string;
}
```

### 核心逻辑

```typescript
// 添加 Todo
const addTodo = (text: string) => {
  const newTodo = {
    id: Date.now().toString(),
    text,
    completed: false,
    createdAt: new Date().toISOString(),
  };
  setTodos([...todos, newTodo]);
};

// 切换完成状态
const toggleTodo = (id: string) => {
  setTodos(todos.map(todo =>
    todo.id === id ? { ...todo, completed: !todo.completed } : todo
  ));
};

// 删除 Todo
const deleteTodo = (id: string) => {
  setTodos(todos.filter(todo => todo.id !== id));
};
```

### localStorage 持久化

```typescript
// 保存到 localStorage
useEffect(() => {
  localStorage.setItem('todos', JSON.stringify(todos));
}, [todos]);

// 从 localStorage 读取
useEffect(() => {
  const saved = localStorage.getItem('todos');
  if (saved) {
    setTodos(JSON.parse(saved));
  }
}, []);
```

## 🎨 UI 参考

可以参考以下设计：
- 简洁的输入框
- 清晰的待办列表
- 明显的完成标记
- 筛选按钮组

## ✅ 完成标准

你的 Todo List 应该能够：
- ✅ 正常添加、删除、切换状态
- ✅ 筛选功能正常工作
- ✅ 刷新页面后数据不丢失
- ✅ 代码组织合理，易于理解
- ✅ UI 清晰美观（使用 Tailwind CSS）

## 🔗 相关资源

- [React 官方文档 - State](https://react.dev/learn/state-a-components-memory)
- [React 官方文档 - 列表渲染](https://react.dev/learn/rendering-lists)
- [MDN - localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

---

**难度**: ⭐⭐ (初级)
**预计时间**: 0.5-2 小时
**状态**: ✅ 可开始
