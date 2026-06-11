# Tech Stack

## 项目类型与入口

- **类型**：CLI 工具
- **运行方式**：`uv run python todo.py <subcommand> [args]`
- **交付形式**：单个 Python 脚本 `todo.py`，放到任意目录即可用

## 语言与运行时

- **语言**：Python 3
- **版本下限**：Python 3.10（macOS 自带或 `uv` 管理）
- **运行工具**：`uv` — 负责 Python 版本管理和脚本执行，不在项目内引入依赖声明

## 核心选择

| 选择 | 选了什么 | 为什么 |
|---|---|---|
| 语言 | Python | 用户熟悉，标准库够用，`uv` 零配置跑脚本 |
| 数据存储 | JSON 文件（`tasks.json`） | 人可读、标准库 `json` 直接读写、不需要解析器 |
| 存储位置 | 用户家目录 `~/.todo_cli/tasks.json` | 全局唯一，任意目录都能访问同一份 TODO 列表 |
| CLI 解析 | `argparse`（标准库） | 子命令模式天然支持，help 自动生成 |
| 数据模型 | 列表 of dict，字段：`id`, `title`, `done`, `created_at` | 够用，不引入 dataclass 以外的抽象 |

## 数据结构

```json
[
  {
    "id": 1,
    "title": "修登录页的样式 bug",
    "done": false,
    "created_at": "2026-06-11T14:30:00"
  }
]
```

- `id`：自增整数，删除后不复用
- `title`：任务内容，必填
- `done`：布尔，默认 `false`
- `created_at`：ISO 8601 字符串，创建时自动生成

## 命令列表

| 命令 | 参数 | 说明 |
|---|---|---|
| `add` | `<title>` | 新增一条 TODO |
| `list` | `--all` / `--done` / `--todo` | 列出任务（默认只显示未完成） |
| `done` | `<id>` | 标记任务为已完成 |
| `delete` | `<id>` | 删除一条任务 |

## 测试与验证

- **测试方式**：手动功能测试，暂不引入测试框架
- **验证标准**：在终端执行上述 5 个命令，`tasks.json` 内容符合预期
- **后续可加**：`pytest` + `unittest.mock` 做单元测试（不在此阶段范围）

## 约束

- **零第三方依赖**：`import` 只允许标准库模块
- **不需要 `pip install`**：不写 `setup.py` / `pyproject.toml` 的依赖段
- **数据安全**：不联网、不上传、不读 `tasks.json` 以外任何文件
- **不处理并发**：单用户单终端，不引入文件锁
- **macOS only**：路径、编码假设 macOS 环境，不测 Windows/Linux

## 已知空白

- 不做模糊搜索 / 关键词过滤
- 不做优先级、标签、分类
- 不做截止日期、提醒
- 不做配置文件 / 自定义存储路径
- 不做 shell 补全
- 不做 `uv` 以外的运行方式（不保证 `python todo.py` 行为）
