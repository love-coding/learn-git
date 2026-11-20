# demo 11：PR 冲突处理（进阶）

目标：走通"提 PR 时提示 has conflicts → 本地同步 master → 解决冲突 → 推上去 → 合并"的完整流程。

## 故事

你提 PR 的时候，GitHub 有时会在 PR 页面红字提示：

> **This branch has conflicts with the base branch**（此分支与基准分支冲突）

意思是：**master 在你开发期间被别人更新了，而且和你改了同一个地方**，GitHub 不敢自动合并，让你先解决。

这不是什么大事，团队里天天发生。标准解法就一句话：

> **把最新的 master 合并进你的分支，在本地解决冲突，再推上去。**

## 流程全景

```
1. feature 分支改文件、push
2. master 被"别人"改了同一处（本 demo 用本地改+push 模拟）、push
3. GitHub 提 PR → 显示 has conflicts
4. 本地：切回 master → pull → 切回 feature → merge master → 冲突！
5. 解决冲突（删标记，留下最终内容）→ add → commit → push
6. 回 GitHub：冲突提示消失 → Merge → 清理分支
```

> 本 demo 在 `learn-git` 主仓库上做（PR 必须基于真实远程仓库）。

---

## 第一步：制造分叉（自己跑）

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git
git checkout master
git pull                                   # 先同步，保持干净起点

# 在 master 上放一个目标文件
echo "第一行（master 原版）" > demo.txt
git add . && git commit -m "添加 demo.txt"
git push

# 建 feature 分支，改这个文件
git checkout -b feature/conflict-demo
echo "第一行（feature 的修改）" > demo.txt
git add . && git commit -m "feature 修改第一行"
git push -u origin feature/conflict-demo
```

## 第二步：模拟"同事改了 master"（自己跑）

```bash
# 切回 master，改同一个文件的同一行（模拟别人在远程改）
git checkout master
echo "第一行（master 的修改）" > demo.txt
git add . && git commit -m "master 也改第一行"
git push
```

现在 feature 和 master 都改了 `demo.txt` 第一行 → **分叉完成**。

## 第三步：开 PR，看冲突提示（网页操作）

1. 打开 https://github.com/love-coding/learn-git
2. 出现黄色提示条 → **Compare & pull request**（或 Pull requests → New pull request → 选 `feature/conflict-demo`）
3. 填写后点 **Create pull request**
4. 观察 PR 页面：应该出现 **"This branch has conflicts with the base branch"** 红字提示

## 第四步：本地解决冲突（自己跑）

```bash
# 1. 切回 master 拉到最新
git checkout master
git pull

# 2. 切回 feature，把最新 master 合并进来
git checkout feature/conflict-demo
git merge master
# → 输出 CONFLICT，冲突出现

# 3. 看冲突内容
cat demo.txt
```

此时 `demo.txt` 长这样：

```
<<<<<<< HEAD
第一行（feature 的修改）
=======
第一行（master 的修改）
>>>>>>> master
```

**解决**：用编辑器打开 `demo.txt`，改成你最终想要的样子，删掉所有 `<<<<<<<`、`=======`、`>>>>>>>` 标记（例如保留 master 的版本）。

```bash
# 4. 告诉 Git 解决了
git add demo.txt
git commit -m "合并 master，解决冲突"

# 5. 推上去
git push
```

## 第五步：回 GitHub 合并 + 收尾（网页 + 本地）

1. 回到 PR 页面 → 冲突提示已消失 → **Merge pull request** → **Confirm merge**
2. 本地收尾：

```bash
git checkout master
git pull
git branch -d feature/conflict-demo
git push origin --delete feature/conflict-demo
```

## 做完应该看到什么

- PR 页面一度出现 "has conflicts"，解决并 push 后提示消失、可以 Merge。
- 本地 `git merge master` 时看到 CONFLICT 和冲突标记。
- 最终 master 的 `demo.txt` 是你解决后的内容，历史完整。

## 小知识

- 也可以不按本地流程，直接用 GitHub 网页的 **"Resolve conflicts"** 按钮处理简单冲突；但行业标准是**本地解决**（能运行测试、更可控）。
- 想提前避免冲突：开发期间经常 `git pull`（或把 master 合进自己分支），保持分支"新鲜"。
