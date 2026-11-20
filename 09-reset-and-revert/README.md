# demo 9：reset 与 revert（提交之后想反悔）

目标：学会两种"反悔"——回退历史（reset）和安全抵消（revert），并知道什么场合用哪个。

## 两个概念（先懂再看命令）

**reset = 倒带**：把分支往回拨到某次提交，**后面的提交直接消失**（历史被改写）。
三种力度：

| 参数 | 效果 | 改动去哪了 |
|------|------|-----------|
| `--soft` | 只移"存档指针" | 改动保留在**暂存区**（绿色） |
| `--mixed`（默认） | 移指针 + 取消暂存 | 改动退回**工作区**（红色） |
| `--hard` | 移指针 + 清空改动 | **全部丢弃**（危险！） |

**revert = 拍一集"纠错特别篇"**：历史不动，**新建一个反向提交**把目标提交的效果抵消掉。

**用哪个？一句话：**

> 提交**还没推送** → `reset` 随便用（倒带）。
> 提交**已推送**（别人可能已拉走）→ 用 `revert`，千万别 reset（会改写公共历史，同事崩溃）。

---

## 第一部分：reset

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git/09-reset-and-revert
git init

# 造 3 次提交
echo "第1行" > file.txt
git add . && git commit -m "提交1"
echo "第2行" >> file.txt
git add . && git commit -m "提交2"
echo "第3行" >> file.txt
git add . && git commit -m "提交3"
git log --oneline
```

**小知识**：`HEAD~1` = "当前提交往前数 1 个"，不用记 id 也能回退。

### 演示 1：--hard（彻底回退，改动全丢）

```bash
git reset --hard HEAD~1
git log --oneline     # 提交3 消失
cat file.txt          # 第3行也消失了
```

### 演示 2：--soft（回退存档，改动留暂存区）

```bash
# 先恢复原状：把提交3 找回来（用 reflog 或直接重造）
# 简单做法：重新提交一次第3行
echo "第3行" >> file.txt
git add . && git commit -m "提交3"
git log --oneline     # 3 条回来了

git reset --soft HEAD~1
git status            # 改动在暂存区（绿）——存档退了，但改动还在托盘上
git log --oneline     # 提交3 消失
```

### 演示 3：--mixed（默认，改动退到工作区）

```bash
# 先恢复：把刚才的改动提交回去
git add . && git commit -m "提交3"

git reset HEAD~1      # 不加参数就是 mixed
git status            # 改动在工作区（红）——存档退了，改动还在，只是没暂存
git log --oneline     # 提交3 消失
```

---

## 第二部分：revert

```bash
# 造一个"错误提交"
echo "错误内容" > bad.txt
git add . && git commit -m "引入错误"

# 用一次"反向提交"抵消它（--no-edit 跳过编辑器）
git revert --no-edit HEAD

git log --oneline     # 多了一条 "Revert 引入错误"，历史完整
cat bad.txt           # 错误内容被撤销了
```

---

## 对比总结

| | git reset | git revert |
|---|---|---|
| 历史 | 后面的提交**消失**（改写历史） | 历史完整，多一条 revert |
| 已推送的提交 | ❌ 别用 | ✅ 用它 |
| 危险度 | `--hard` 会丢改动 | 安全 |
| 比喻 | 倒带 | 拍纠错特别篇 |

## 做完应该看到什么

- `--hard` 后 log 少一条、文件内容也回退。
- `--soft` 后 log 少一条，但 `git status` 还有绿色改动。
- `--mixed` 后 log 少一条，改动变红色。
- `revert` 后 log 多一条 revert，文件内容被撤销，但历史一条不少。
