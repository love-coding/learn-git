# demo 4：合并 + 冲突

目标：把分支的成果合并回主线；遇到"冲突"不慌，亲手解决它。

## 故事

feature 分支里改好了东西，现在要**合并（merge）**回 master。
大部分时候 Git 自动合并，你啥都不用管。
但如果**两个分支改了同一个地方**，Git 不敢替你拿主意，就会报"冲突"，让你手动选。
这一集就学这个——冲突不可怕，它是 Git 在问你："这里听谁的？"

---

## 第一关：顺利的合并（无冲突）

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git/04-merge-and-conflict
git init
git add hello.txt
git commit -m "初始提交"

# 新建并切到 feature 分支（checkout -b = 新建+切换）
git checkout -b feature
echo "feature 加的一行" >> hello.txt
git add hello.txt
git commit -m "feature 的提交"

# 切回 master，把 feature 合并进来
git checkout master
git merge feature

# 看结果
cat hello.txt          # feature 加的那行出现在 master 上了
git log --oneline      # feature 的提交也进入 master 的历史了
```

---

## 第二关：制造冲突，然后解决它

```bash
# 1. 在 master 上改第一行，提交
echo "master 版本第一行" > hello.txt
echo "第二行" >> hello.txt
git add hello.txt
git commit -m "master 改了第一行"

# 2. 切到 feature，改同一行，提交
git checkout feature
echo "feature 版本第一行" > hello.txt
echo "第二行" >> hello.txt
git add hello.txt
git commit -m "feature 改了第一行"

# 3. 回 master 合并 —— 这次会冲突
git checkout master
git merge feature

# 4. 看冲突长什么样
cat hello.txt
```

此时文件里会出现这样的标记（冲突块）：

```
<<<<<<< HEAD
master 版本第一行
=======
feature 版本第一行
>>>>>>> feature
```

含义：

- `<<<<<<< HEAD` 到 `=======`：**你当前分支（master）**的内容
- `=======` 到 `>>>>>>> feature`：**被合并分支（feature）**的内容

**解决步骤**：

1. 用编辑器打开 `hello.txt`（用 PyCharm / VS Code / 或终端 `nano hello.txt`）。
2. 把文件改成你**最终想要的样子**，删掉所有 `<<<<<<<`、`=======`、`>>>>>>>` 标记。
3. 保存。
4. 告诉 Git"冲突解决了"，然后提交：

```bash
git add hello.txt
git commit -m "解决冲突：第一行保留 master 版本"
git log --oneline
```

## 做完应该看到什么

- 第一关：`git merge feature` 后 master 自动包含 feature 的改动，全程无冲突。
- 第二关：`git merge` 输出冲突提示；`cat hello.txt` 看到冲突标记；解决并提交后，`git log` 里能看到合并后的完整历史。
