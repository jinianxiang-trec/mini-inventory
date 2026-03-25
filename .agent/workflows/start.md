---
description: 启动 mini-inventory 项目开发服务器
---

// turbo-all

1. 直接启动 Django 开发服务器（环境已配置完毕，无需检查依赖或 migrate）：

```
.\venv\Scripts\python manage.py runserver 0.0.0.0:8000
```

2. 等待几秒后检查服务器是否正常启动，确认输出中包含 "Starting development server"。

注意：
- venv 路径：`d:\project\mini-inventory\venv`
- 数据库：SQLite（`db/db.sqlite3`）
- 访问地址：http://localhost:8000
