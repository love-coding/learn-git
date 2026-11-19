# demo 1：初始化仓库 + 第一次提交

目标：亲手做出 Git 里的第一次"存档"，看到 `git log` 里有记录。

## 步骤（按顺序跑）

```bash
# 1. 进入这个 demo 目录
cd 01-init-first-commit

# 2. 把当前文件夹变成 git 仓库
git init

# 3. 看状态（此时 hello.txt 是"未跟踪"的）
git status

# 4. 把 hello.txt 放进暂存区
git add hello.txt

# 5. 再看状态（hello.txt 变成"已暂存"）
git status

# 6. 正式存档
git commit -m "第一次提交：添加 hello.txt"

# 7. 看存档记录
git log --oneline
```

## 做完应该看到什么

- `git init` 后目录下多出一个 `.git/` 文件夹（Git 的"时光机"就住在这里）。
- `git log --oneline` 打印出一行，带一串字母数字（commit id）和你的说明。
