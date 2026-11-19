# demo 2：看历史 + 撤销改动

目标：看到文件在"工作区 → 暂存区 → 仓库"三站之间移动；学会改坏了怎么反悔。

## 步骤（按顺序自己跑）

```bash
# 1. 进入 demo2，建仓库，做初始提交
cd 02-history-and-undo
git init
git add hello.txt
git commit -m "初始提交"

# 2. 改文件（在工作区改，还没暂存）——追加一行
echo "第二行：我又来了" >> hello.txt

# 3. 看状态：Git 会告诉你"改了，但还没暂存"
git status

# 4. 看具体改了什么内容（工作区的改动）
git diff

# 5. 撤销工作区的改动，回到最近一次存档的样子
#    注意：git 2.23+ 可用 `git restore hello.txt`，旧版本用下面这条
git checkout -- hello.txt

# 6. 再看状态和 diff，确认已经恢复
git status
git diff

# 7. 进阶：把第 2 步再来一次，然后 add 放进暂存区
echo "第二行：我又来了" >> hello.txt
git add hello.txt

# 8. 看状态：这次是"已暂存，未提交"
git status

# 9. 看暂存区的改动（注意 --staged 参数）
git diff --staged

# 10. 把文件从暂存区拿出来（改动不丢，只是放回工作区）
#     注意：git 2.23+ 可用 `git restore --staged hello.txt`，旧版本用下面这条
git reset HEAD hello.txt
git status
```

## 版本差异说明（重要）

`git restore` / `git switch` 是 git 2.23（2019 年）才引入的新命令。
如果你的 git 比它旧，用老命令，作用完全一样：

| 新命令（≥2.23） | 旧命令（你的 2.22） |
|------|------|
| `git restore 文件` | `git checkout -- 文件` |
| `git restore --staged 文件` | `git reset HEAD 文件` |
| `git switch -c 分支` | `git checkout -b 分支` |

想升级的话：`brew upgrade git`（不急，旧命令完全够用）。

## 做完应该看到什么

- `git status` 会分两段显示：已暂存的改动 / 未暂存的改动。
- `git diff` 显示工作区改动；`git diff --staged` 显示暂存区改动。
- `git restore` 之后文件内容回到初始样子。
- `git restore --staged` 之后，文件改动还在，只是不在暂存区了。
