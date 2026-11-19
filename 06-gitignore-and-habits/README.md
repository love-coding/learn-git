# demo 6：.gitignore + 日常好习惯

目标：学会把"不该进仓库的文件"拉黑，让提交保持干净。

## 故事

真实项目里有一堆**不该提交**的文件：

- 密码、密钥（泄露了就是事故）
- 日志、缓存（每时每刻在变，没意义）
- 虚拟环境（.venv，几个 G，别人不需要）
- IDE 配置（.idea/，每个人的设置不一样）
- 系统垃圾文件（mac 的 .DS_Store）

`.gitignore` 就是一张**黑名单**：写进去的文件/目录，Git 直接当看不见。

> 顺带说：你这个 Learn_git 项目里就有 `.venv/` 和 `.idea/`，将来建仓库时正好用得上。

---

## 步骤（按顺序自己跑）

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git/06-gitignore-and-habits
git init

# 1. 造几个"不该提交"的文件
echo "password=123456" > secret.txt
echo "some log" > app.log
mkdir -p .venv
echo "binary" > .venv/python

# 2. 写 .gitignore（第一次用 > 创建，之后用 >> 追加）
echo "secret.txt" > .gitignore
echo "*.log" >> .gitignore
echo ".venv/" >> .gitignore

# 3. 看黑名单生效了没
git status
# 应该只能看到 hello.txt 和 .gitignore，
# secret.txt / app.log / .venv 都"消失"了

# 4. 提交
git add .
git commit -m "添加 .gitignore"
git status
```

## 语法小抄

| 写法 | 含义 |
|------|------|
| `secret.txt` | 忽略这个文件 |
| `*.log` | 忽略所有 .log 结尾的文件 |
| `.venv/` | 忽略整个目录（结尾 `/` 表示目录） |
| `!keep.log` | 例外：不忽略它（前面加 `!`） |
| `/build` | 只忽略根目录下的 build（前面加 `/`） |

## 常见"该忽略"清单（以后直接用）

```gitignore
# 密钥/环境变量
.env
secret*

# 虚拟环境
.venv/
venv/

# IDE
.idea/
.vscode/

# 日志/缓存
*.log
__pycache__/
*.pyc

# macOS 系统文件
.DS_Store
```

## 日常好习惯（背下来，受益三年）

1. **提交前**先看 `git status`，确认"这次存了谁"。
2. **commit 说明写人话**：`"修复登录后闪退"` 而不是 `"修改"`。
3. **拉取前先提交**：`git pull` 前保持工作区干净，避免合并闹心事。
4. **推送前先看状态**：`git status` → `git log --oneline`，确认没有多余文件混进去。
5. **敏感文件绝不提交**：密码、密钥、公司内部数据。

## 做完应该看到什么

- `git status` 里看不到 secret.txt、app.log、.venv（被黑名单过滤了）。
- 提交后 `git log --oneline` 只有 .gitignore 和 hello.txt 进入历史。
