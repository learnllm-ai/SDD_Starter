# Roadmap

---

## Phase 1: 骨架 — 能跑起来的空壳 ✅

**目标：** `todo.py` 脚本存在，能解析子命令，`tasks.json` 不存在时不报错。

`add` 命令能把一条任务写进 JSON 文件，`list` 能读出来打印到终端。

---

## Phase 2: 增删查改闭环

**目标：** 四个核心操作全部可用——添加、列出、标记完成、删除。

- `add "内容"` → 追加任务
- `list` → 显示所有未完成任务（带 id）
- `done <id>` → 标记完成
- `delete <id>` → 删除任务
- `list --all` → 看全部，含已完成

到这个阶段，工具已经能满足"记一条 → 查一下 → 划掉 → 删掉"的日常使用。

---

## Phase 3: 打磨

**目标：** 输出更友好，边界处理到位。

- `list` 输出格式化：对齐、emoji 标记（✅ / ⬜）、时间戳展示
- 边界处理：操作不存在的 id 时给出明确提示，而非 traceback
- 帮助信息：`--help` 输出清晰可用

---

## Later / Backlog

- 多列表支持（`--file` 指定 JSON 路径）
- 编辑任务标题（`edit <id> "新内容"`）
- 归档：把已完成任务移到 `tasks.archive.json`
- shell 自动补全脚本
- 安装到 PATH 的 alias / wrapper
