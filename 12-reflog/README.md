# demo 12：git reflog（后悔药 / 保命技能）

目标：即使提交被 `reset --hard` 删掉，也能用 reflog 找回来。

## 故事

`git reset --hard` 把提交删了，`git log` 里也找不到——**天塌了吗？没有。**

Git 在本地悄悄记了一本**"HEAD 移动日志"（reflog）**：
每次 HEAD 移动（commit、reset、merge、checkout……）都会记一笔，**哪怕提交被删，这笔记录还在**。

比喻：reflog = **监控录像**。你以为删了的提交，录像都拍下来了，随时回看、找回。

> 注意：reflog 是**本地**的（不会同步到 GitHub，换电脑就没了）；默认保留约 **90 天**。

---

## 命令

```bash
git reflog              # 看 HEAD 的"监控录像"
git reflog show 分支名   # 看某个分支的录像
git reset --hard HEAD@{1}   # 回到"上一个位置"
git reset --hard <提交id>   # 回到指定提交
```

`HEAD@{1}` = 上一个位置，`HEAD@{2}` = 上上个位置（相对引用，不用记 id）。

---

## 步骤：制造手滑，再救命（自己跑）

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git/12-reflog
git init

# 造两次提交
echo "第一行" > file.txt
git add . && git commit -m "提交1"
echo "第二行" >> file.txt
git add . && git commit -m "提交2"

# 手滑：hard 回退，提交2 消失
git reset --hard HEAD~1
git log --oneline        # 提交2 不见了，天塌了？

# 查"监控录像"
git reflog

# 从录像里找到提交2 的位置（HEAD@{1} 之类），恢复
git reset --hard HEAD@{1}

# 验证
git log --oneline
cat file.txt
```

## reflog 输出长什么样

```
9b5f278 HEAD@{0}: reset: moving to HEAD~1     ← 你刚做的"手滑"
a1b2c3d HEAD@{1}: commit: 提交2               ← 提交2 藏在这里！
9b5f278 HEAD@{2}: commit: 提交1
```

`HEAD@{0}` 是现在，往前数就能找到"丢失"的提交。

---

## reflog 能救的三种典型场景

1. **`reset --hard` 删了提交** → 找到 id 恢复（本 demo）。
2. **误删了分支** → `git branch 分支名 <id>` 用录像里的 id 重建。
3. **误 merge / 误 checkout** → 回到之前的位置。

## 做完应该看到什么

- `git reflog` 里能看到"reset: moving to HEAD~1"和"提交2"两条记录。
- `git reset --hard HEAD@{1}` 后，`git log` 里提交2 回来了，`file.txt` 第2行也回来了。
