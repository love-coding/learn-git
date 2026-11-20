# demo 13：git tag（版本标签）

目标：给提交打版本标签（如 v1.0.0），并推送到 GitHub。

## 故事

分支会"移动"（新提交不断追加）；**tag 是钉死的**——永远指着一个特定提交，纪念"这就是 1.0 版"。

常见用途：发版时打 `v1.0.0`、`v2.1.3`，以后能精确找到、下载那个版本。

比喻：分支像"**进行中的工作线**"，tag 像"**里程碑石碑**"——立在某个提交前，不动。

> 版本号惯例（语义化版本）：`v主版本.次版本.修订号`，如 `v1.0.0`、`v1.2.0`。

---

## 两种 tag

| 类型 | 命令 | 特点 |
|------|------|------|
| 轻量标签 | `git tag v1.0.0` | 只是个引用，没说明 |
| 附注标签（推荐） | `git tag -a v1.0.0 -m "首个正式版"` | 带作者、日期、说明，可签名 |

---

## 步骤一：本地打标签（自己跑）

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git/13-tag
git init

# 造两次提交
echo "第一行" > file.txt
git add . && git commit -m "提交1"
echo "第二行" >> file.txt
git add . && git commit -m "提交2"

# 打轻量标签（打在当前位置）
git tag v0.1.0

# 打附注标签（推荐）
git tag -a v0.2.0 -m "第二个版本"

# 看标签列表
git tag

# 看标签详情
git show v0.2.0

# 给"历史提交"打标签：先找到提交1 的 id，再打
git log --oneline
git tag -a v0.1.0 -m "首个版本" <提交1的id>
```

## 步骤二：把 tag 推送到 GitHub（重要坑）

> **坑：`git push` 默认不推 tag！** tag 要单独推。

在 `learn-git` 主仓库里练（tag 不推远程，别人看不到）：

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git
git checkout master

# 给当前最新提交打个正式版标签
git tag -a v1.0.0 -m "Learn_git 学习完成纪念版"

# 推送单个标签
git push origin v1.0.0
# 或者推送全部标签：git push origin --tags
```

推送后打开 GitHub 仓库 → **Releases / Tags** 页面，能看到 `v1.0.0`。

## 常用命令速查

```bash
git tag                      # 列出所有标签
git tag -a v1.0.0 -m "说明"   # 创建附注标签
git show v1.0.0              # 看标签详情
git push origin v1.0.0       # 推单个标签
git push origin --tags       # 推所有标签
git tag -d v1.0.0            # 删本地标签
git checkout v1.0.0          # 切到标签（看历史版本，只读）
```

## 做完应该看到什么

- `git tag` 列出 v0.1.0、v0.2.0。
- `git show v0.2.0` 显示标签说明和对应提交。
- GitHub 仓库的 Tags 页面能看到 v1.0.0。
