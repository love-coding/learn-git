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

### 回退与反悔

```bash
# reset：倒带（改写历史，后面的提交消失）
git reset --soft HEAD~1     # 退存档，改动留暂存区（绿）
git reset HEAD~1            # 默认 mixed：改动退到工作区（红）
git reset --hard HEAD~1     # 全丢（危险！）

# revert：新增一条"反向提交"抵消（保留历史）
git revert --no-edit HEAD   # 抵消最近一次提交（--no-edit 跳过编辑器）
```

> **没推送 → reset 随便用；已推送 → 用 revert。**
> `HEAD~1` = 往前数 1 个提交，`HEAD~2` = 往前 2 个（不用记 id）。

### stash（临时收纳改动）

```bash
git stash          # 收起改动（工作区立刻变干净）
git stash -u       # 连"未跟踪的新文件"一起收（默认不收！）
git stash list     # 看口袋列表（stash@{0} 是最近一次的）
git stash pop      # 掏出最近一次，并删掉该口袋
git stash apply    # 掏出改动但保留口袋
git stash drop     # 手动扔掉一个口袋
```

> 后进先出；`pop` 会删口袋，`apply` 不删。

### PR 冲突处理（本地解决）

```bash
# 1. 切回 master 拉最新
git checkout master && git pull
# 2. 切回 feature，把最新 master 合并进来
git checkout feature/xxx
git merge master              # → 出现 CONFLICT
# 3. 编辑冲突文件：删掉 <<<<<<< ======= >>>>>>> 标记，留下最终内容
# 4. 提交并推送
git add 冲突文件
git commit -m "合并 master，解决冲突"
git push
# 5. 回 GitHub：冲突提示消失 → Merge pull request → 清理分支
```

### reflog（后悔药）

```bash
git reflog                  # 看 HEAD 的移动记录（本地，约保留 90 天）
git reset --hard HEAD@{1}   # 回到"上一个位置"
git reset --hard <提交id>   # 回到指定提交（从 reflog 里找 id）
```

> 手滑 `reset --hard` 删了提交？reflog 里翻出来复活。
> 但 **reflog 救不了"从未 commit 过的改动"**（没进过 Git 的东西它管不了），也换不了电脑。

### tag（版本标签）

```bash
git tag                      # 列出所有标签
git tag -a v1.0.0 -m "说明"   # 创建附注标签（推荐）
git tag -a v1.0.0 -m "说明" <提交id>   # 打在历史提交上
git push origin v1.0.0       # 推单个标签
git push origin --tags       # 推所有标签
git show v1.0.0              # 看标签详情
git tag -d v1.0.0            # 删本地标签
```

> **坑：`git push` 默认不推 tag！** 标签要单独推，否则 GitHub 上看不到。

### 团队协作（PR 工作流）

```bash
git pull                                  # 先同步最新代码
git checkout -b feature/xxx               # 建功能分支
# 开发 → git add → git commit
git push -u origin feature/xxx            # 推分支到 GitHub
# 网页：Compare & pull request → Create pull request
# 网页：Files changed 审查 → Merge pull request → Delete branch
git checkout master && git pull           # 回主线同步
git branch -d feature/xxx                 # 删本地分支
git push origin --delete feature/xxx      # 删远程分支
```

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
- **`git reset --hard` 会真丢改动**，手滑前先看 `git status`；真丢了用 `git reflog` 救命（进阶）。
- **stash 默认不收新文件**（未跟踪），想收干净用 `git stash -u`。

## 四、排查口诀

- **提交前看 status**：改了什么、暂存了谁，一目了然。
- **提交后看 log**：存档记录都在。
- **改乱了先别慌**：restore 能回到最近存档。
