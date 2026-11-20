# Learn_git —— 边做边学 Git

这个项目是 Git 的"实战教材"。每个目录是一个**自包含的小 demo**，从最小能跑，到真实项目。

## 这个目录在学什么

| 目录 | 知识点 | 状态 |
|------|--------|------|
| `01-init-first-commit/` | 初始化仓库、第一次提交（add / commit / log） | ✅ 已完成 |
| `02-history-and-undo/` | 看历史、撤销改动（status / diff / restore） | ✅ 已完成 |
| `03-branches/` | 分支：平行宇宙（branch / switch / checkout） | ✅ 已完成 |
| `04-merge-and-conflict/` | 合并与冲突处理 | ✅ 已完成 |
| `05-remote-github/` | 远程仓库 + GitHub 推送 | ✅ 已完成 |
| `06-gitignore-and-habits/` | .gitignore + 日常好习惯 | ✅ 已完成 |
| `07-real-project/` | 综合实战：把整个项目管起来并推送 | ✅ 已完成 |
| `08-pull-request/` | Pull Request 工作流（团队协作核心） | ✅ 已完成 |
| `09-reset-and-revert/` | reset 与 revert（提交之后想反悔） | ✅ 已完成 |
| `10-stash/` | stash 临时收纳改动 | ✅ 已完成 |
| `11-pr-conflicts/` | PR 冲突处理（进阶） | ✅ 进行中 |

## 进度打卡

- [x] demo 1：能独立做出第一次提交
- [x] demo 2：能看懂 status / diff，会撤销
- [x] demo 3：会建分支、切分支
- [x] demo 4：会合并，能处理简单冲突
- [x] demo 5：能把本地项目推到 GitHub
- [x] demo 6：会写 .gitignore，养成好习惯
- [x] demo 7：独立完成一次"建仓 → 开发 → 提交 → 推送"全流程
- [x] demo 8：走通"建分支 → 推分支 → 开 PR → 审查 → 合并 → 清理"全流程
- [x] demo 9：会 reset（三种模式）与 revert，知道该用哪个
- [x] demo 10：会用 stash 临时收纳改动（含 -u）

## 常用操作速查（忘了就翻 cheatsheet.md）

```bash
git init            # 把当前文件夹变成 git 仓库
git status          # 看当前状态（改了什么、存了没）
git add 文件名       # 把改动放进"暂存区"（准备存档）
git commit -m "说明" # 正式存档一次
git log --oneline   # 看所有存档记录
```

更全的命令、概念、踩坑都在 [`cheatsheet.md`](cheatsheet.md)。
