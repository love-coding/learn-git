# demo 3：分支——平行宇宙

目标：新建分支、切换分支，亲眼看到"同一个文件在不同分支下内容不同"。

## 故事

你在 master（主线）上开发到一半，想试个大胆的想法，又怕把主线搞坏。
**开一个分支 = 开一个平行宇宙**：在里面随便折腾，主线纹丝不动；
玩好了再合并回来（demo4 讲合并），玩坏了直接扔掉。

## 步骤（按顺序自己跑）

```bash
# 1. 建仓库 + 初始提交
cd /Users/lipengcheng/LLM_Learn/Learn_git/03-branches
git init
git add hello.txt
git commit -m "初始提交"

# 2. 看当前有哪些分支（只有 master，前面带 * 的是你所在的分支）
git branch

# 3. 新建一个分支 feature
git branch feature
git branch          # 再看：多了一个 feature，但 * 还在 master

# 4. 切换到 feature 分支（旧版本命令；2.23+ 可用 git switch feature）
git checkout feature
git branch          # 看 * 是不是移到 feature 了

# 5. 在 feature 里改文件、提交
echo "这是 feature 分支的改动" >> hello.txt
git add hello.txt
git commit -m "feature 分支的提交"

# 6. 切回 master，看文件内容（cat 是查看文件内容的系统命令）
git checkout master
cat hello.txt

# 7. 再切回 feature，看文件内容
git checkout feature
cat hello.txt

# 8. 看两个分支各自的提交记录
git log --oneline
git checkout master
git log --oneline
```

## 做完应该看到什么

- `git branch` 里 `*` 标记你**当前站在哪条时间线**。
- 切回 master 后，`cat hello.txt` **看不到** feature 的那行改动（它存在 feature 上）。
- 切回 feature，改动**又回来了**。
- master 和 feature 的 `git log` 记录不同：feature 多一次提交，master 没有。
