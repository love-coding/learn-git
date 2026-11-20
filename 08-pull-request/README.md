# demo 8：Pull Request 工作流（团队协作核心）

目标：走一遍完整的"建分支 → 开发 → 推分支 → 开 PR → 审查 → 合并"流程。

## 故事

直接往 master 上改，风险大、也没人帮你把关。团队的做法是：

**Pull Request（拉取请求）= 给主线"提交合并申请"。**

你在自己的分支上改好 → 提 PR → 同事（或你）**审查（review）**代码 → 通过了才合并进 master。

比喻：master 是**正式出版物**，你的分支是**草稿**。PR 就是"投稿"，review 是"编辑部审稿"，merge 是"过审刊发"。

> 本 demo 直接在 `learn-git` 主仓库上操作（不新建仓库）——PR 就是给已有仓库提合并申请。

---

## 第一步：本地准备（自己跑）

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git

# 先同步远程最新状态（真实工作中第一步永远是 pull）
git pull

# 建一个功能分支（feature/ 是团队常用的命名前缀）
git checkout -b feature/add-notes

# 在分支上开发：加一个文件
echo "PR 练习：这是我加的一行笔记" > notes.txt

# 提交
git add notes.txt
git commit -m "添加学习笔记"

# 推送分支到 GitHub（推的是 feature 分支，不是 master）
git push -u origin feature/add-notes
```

---

## 第二步：在 GitHub 上开 PR（网页操作）

1. 打开 https://github.com/love-coding/learn-git
2. 仓库首页通常会出现黄色提示条：
   **"feature/add-notes had recent pushes"** → 点 **Compare & pull request**
   （没有提示条的话：点 **Pull requests** 标签 → **New pull request** → 选 `feature/add-notes` → **Create pull request**）
3. 填写 PR 标题和说明（写清楚"这次改了什么、为什么"）→ **Create pull request**

---

## 第三步：Review + 合并（网页操作）

1. 进入 PR 页面，点 **Files changed** —— 看这次分支改了哪些代码（这就是"审查"）
2. 确认没问题 → 点绿色按钮 **Merge pull request**
3. 点 **Confirm merge** —— 合并完成，GitHub 上的 master 已更新
4. （可选）点 **Delete branch** —— 顺手删掉 GitHub 上已完成使命的功能分支

---

## 第四步：本地收尾（自己跑）

```bash
# 切回 master，拉取合并后的最新代码
git checkout master
git pull

# 确认合并记录
git log --oneline

# 删掉已完成使命的功能分支（本地 + 远程）
git branch -d feature/add-notes
git push origin --delete feature/add-notes
```

## 做完应该看到什么

- GitHub 上出现一次合并记录，master 包含 notes.txt。
- 本地 `git pull` 后 `git log` 能看到合并历史。
- `git branch -a` 里功能分支已被清理。

## 进阶预告

如果提 PR 时 GitHub 显示 **"This branch has conflicts"**（master 被别人改过，和你撞车了），需要在本地先同步 master 再解决冲突。下次我们专门练这个。
