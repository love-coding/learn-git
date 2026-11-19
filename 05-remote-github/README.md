# demo 5：远程仓库 + GitHub

目标：把本地仓库推送到 GitHub，并学会"改 → 提交 → 推送"的日常循环。

## 故事

本地仓库 = 你电脑里的存档柜；GitHub = 总部的备份柜。
推送（push）= 把本地的存档备份到总部；拉取（pull）= 把总部的更新拿回本地。
好处：电脑坏了不丢代码、多人协作、作品能展示给别人看。

---

## 第一部分：在 GitHub 建一个空仓库（网页操作）

1. 打开 https://github.com 并登录
2. 点右上角 **"+" → New repository**（或直接访问 https://github.com/new ）
3. **Repository name** 填 `learn-git-demo5`
4. 选 **Public**
5. **不要勾选** "Add a README file"（保持空仓库，避免和本地冲突）
6. 点 **Create repository**
7. 进入仓库页后，点绿色 **Code** 按钮，复制 **HTTPS** 地址
   （形如 `https://github.com/你的用户名/learn-git-demo5.git`）

---

## 第二部分：本地关联 + 推送

```bash
cd /Users/lipengcheng/LLM_Learn/Learn_git/05-remote-github
git init
git add hello.txt
git commit -m "初始提交"

# 关联远程（把地址换成你刚才复制的）
git remote add origin https://github.com/你的用户名/learn-git-demo5.git

# 推送（-u 会记住远程，以后直接 git push 就行）
git push -u origin master
```

---

## 第三部分：验证 + 日常循环

1. 刷新 GitHub 仓库页面 → 能看到 `hello.txt` 和提交说明。
2. 本地改文件 → 提交 → 推送：

```bash
echo "又改了一行" >> hello.txt
git add hello.txt
git commit -m "更新 hello.txt"
git push
```

3. 刷新 GitHub → 看到新内容。

---

## 常见坑：推送要认证

GitHub 不允许用网页账号密码推送代码。如果 `git push` 报认证错误，两种解法：

- **用 Token**：GitHub → 头像 → Settings → Developer settings → Personal access tokens → 生成一个（勾选 repo 权限），推送时密码栏粘贴 token。
- **用 GitHub CLI**：`brew install gh`，然后 `gh auth login` 一键搞定。

先跑，报错了我们再处理。
