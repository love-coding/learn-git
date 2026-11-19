# Git 速查卡（忘了就翻）

> 不用背。忘了就回来查。

## 一、四个核心概念（先懂这个再看命令）

| 概念 | 大白话 | 比喻 |
|------|--------|------|
| 工作区（Working Directory） | 你正在改的文件 | 厨房里正在做的菜 |
| 暂存区（Staging Area） | 挑好"这次要存档哪些改动"的清单 | 上菜前摆盘的托盘 |
| 本地仓库（Local Repository） | 存在你电脑里的所有存档 | 店里的存档柜 |
| 远程仓库（Remote） | 存在网络上的存档（如 GitHub） | 总部的备份柜 |
| 提交（commit） | 一次存档：当前所有文件的快照 | 游戏存档 |
| 分支（branch） | 一条独立的时间线，提交落在你当前所在的分支上 | 平行宇宙 / 房间 |
| HEAD | "你当前站在哪条分支"的标记，跟着你切换 | 手里的激光笔 |

**一次提交的旅程**：工作区改文件 → `git add` 放到暂存区 → `git commit` 存进本地仓库。

## 二、命令速查

### 初始化与提交

```bash
git init                 # 把当前文件夹变成 git 仓库
git add 文件名            # 把某个文件的改动放进暂存区
git add .                # 把所有改动都放进暂存区
git commit -m "说明"      # 正式存档一次
git log --oneline        # 一行一条看存档记录
```

### 看状态、看改动

```bash
git status               # 当前状态：改了什么、哪些已暂存
git diff                 # 看"还没暂存"的具体改动
git diff --staged        # 看"已暂存但还没提交"的具体改动
```

### 撤销

```bash
# 新命令（git 2.23+）
git restore 文件名            # 丢弃工作区的改动（回到最近一次存档）
git restore --staged 文件名   # 把文件从暂存区拿出来（不丢改动）

# 旧命令（git 2.23 之前，如 2.22）——作用完全一样
git checkout -- 文件名        # 丢弃工作区的改动
git reset HEAD 文件名         # 把文件从暂存区拿出来（不丢改动）
```

> 查自己的版本：`git --version`。新命令更语义化，旧命令更通用，写脚本/教程时注意兼容。

### 分支

```bash
git branch               # 看有哪些分支（* 标记当前分支）
git branch 分支名         # 新建分支
git checkout 分支名       # 切换分支（旧版本；2.23+ 用 git switch 分支名）
git checkout -b 分支名    # 新建并切换（旧版本；2.23+ 用 git switch -c 分支名）
git merge 分支名          # 把指定分支合并进当前分支
git branch -d 分支名      # 删除已合并的分支
```

### 合并与冲突

```bash
# 合并：把 feature 合并进当前分支
git merge feature

# 冲突时：
# 1. 打开冲突文件，删掉 <<<<<<<、=======、>>>>>>> 标记，留下最终想要的内容
# 2. 告诉 Git 解决了
git add 冲突文件
git commit -m "解决冲突：保留xxx"
```

> 冲突标记含义：`<<<<<<< HEAD` 到 `=======` 是当前分支的内容；`=======` 到 `>>>>>>> 分支名` 是被合并分支的内容。

### 远程

```bash
# origin 是远程仓库的"别名"（约定俗成，不用改）
git remote add origin 仓库地址   # 关联远程仓库
git push -u origin master        # 第一次推送（-u 记住远程；master 换成你的分支名）
git push                         # 以后推送（-u 已经记住了）
git pull                         # 拉取远程的更新
git clone 仓库地址               # 把远程仓库完整复制到本地
```

> 推送报认证错误时：GitHub 不接受网页密码，用 Token 或 GitHub CLI（`gh auth login`）。

### 踩坑：网络报错

```bash
# 推送/拉取报 `Error in the HTTP2 framing layer`（国内访问 GitHub 常见）
git config --global http.version HTTP/1.1   # 改用 HTTP/1.1 再重试
```

> 这不是操作错误，是网络/协议问题，换 HTTP/1.1 基本能解决。

### .gitignore（黑名单）

```bash
# 语法
secret.txt      # 忽略指定文件
*.log           # 忽略所有 .log 结尾的文件
.venv/          # 忽略整个目录（结尾 / 表示目录）
!keep.log       # 例外：不忽略它
```

```gitignore
# 常用"该忽略"清单（直接抄）
.venv/
venv/
.env
secret*
*.log
__pycache__/
*.pyc
.idea/
.vscode/
.DS_Store
```

## 三、易错点速记（学完后持续补充）

- `git add` 和 `git commit` 是两步：先"放进托盘"，再"存档"。只 add 不 commit，仓库里没有记录。
- `git commit -m "说明"` 的说明要写人话，例如 `git commit -m "添加登录功能"`。
- **撤销两件套（git 2.22 旧命令写法）**：
  - 改了没 add，想彻底扔掉 → `git checkout -- 文件`
  - add 了想反悔、改动还要 → `git reset HEAD 文件`
  - add 了想彻底扔掉 → 先 `git reset HEAD 文件`，再 `git checkout -- 文件`
  - 口诀：**"checkout 丢改动，reset 退暂存"**
- `git status` 空输出 = 工作区干净，没有改动（空就是对的）。
- 提交前先看 `git status`，确认"存了谁"。
- **合并前保持工作区干净**（先提交或丢弃改动），否则 Git 会拦着不让你合并。
- **解决冲突必须删干净标记**（`<<<<<<<`、`=======`、`>>>>>>>`），改完 `git add` 再 `commit`。
- 合并完及时删掉没用的分支：`git branch -d 分支名`。

## 四、排查口诀

- **提交前看 status**：改了什么、暂存了谁，一目了然。
- **提交后看 log**：存档记录都在。
- **改乱了先别慌**：restore 能回到最近存档。
