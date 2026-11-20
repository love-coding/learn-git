# demo 10：git stash（临时收纳改动）

目标：改到一半要切分支时，把改动临时收起来，回头再掏出来。

## 故事

你正在 feature 分支改代码，改到一半（没法提交），突然要切到别的分支处理紧急事情。
但工作区有没提交的改动，Git 会**拦着不让你切**（怕你丢改动）。

**stash = 口袋**：把改到一半的改动**临时塞进口袋** → 工作区立刻干净 → 随便切分支 →
回来再**掏出来**接着改。

## 命令速览

```bash
git stash          # 收起改动（工作区变干净）
git stash list     # 看口袋里有哪些（stash@{0}、stash@{1}...）
git stash pop      # 掏出最近一次，并从口袋删掉
git stash apply    # 掏出改动，但保留在口袋里
git stash drop     # 手动扔掉一个口袋
git stash show     # 看某个口袋里改了什么
git stash -u       # 连"新建的未跟踪文件"一起收（默认不收）
```

---

## 第一部分：基本用法（自己跑）

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git/10-stash
git init

echo "第一行" > file.txt
git add . && git commit -m "初始提交"

# 改到一半
echo "改到一半的内容" >> file.txt
git status          # 有未提交改动

# 塞进口袋
git stash
git status          # 干净了！

# 掏出来继续
git stash pop
git status          # 改动回来了
cat file.txt
```

---

## 第二部分：多个口袋 + 未跟踪文件（自己跑）

```bash
# 先制造两个口袋
echo "改动A" >> file.txt
git stash

echo "改动B" > new.txt    # 新文件（未跟踪）
git stash -u              # 注意：不加 -u 收不了新文件

# 看口袋列表
git stash list

# 掏出最近一次
git stash pop
git status

# 看还剩什么
git stash list
git stash pop
```

## 常见坑

- **默认只收"已跟踪文件"的改动**，新建的文件（未跟踪）不收 → 用 `git stash -u` 才收得干净。
- `pop` 会删掉口袋，`apply` 只掏东西不删。怕丢就用 `apply`。
- 口袋是**本地的**，不会推到 GitHub，换电脑就没了。

## 做完应该看到什么

- `git stash` 后 `git status` 干净。
- `git stash pop` 后改动恢复，`cat file.txt` 内容回来了。
- `git stash list` 能看到 `stash@{0}` 这样的条目。
