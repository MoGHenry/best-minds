# GitHub 上传指南

本文件包含将 `best-minds` 项目上传到 GitHub 的完整步骤。

## 前提条件

1. 已安装 Git
2. 已创建 GitHub 账户
3. 已配置 Git 全局用户名和邮箱

## 步骤

### 1. 在 GitHub 创建新仓库

1. 登录 [GitHub](https://github.com)
2. 点击右上角 **+** 按钮，选择 **New repository**
3. 仓库名称填写：`best-minds`
4. 描述：`Best Minds - 模拟器思维：问世界上谁最懂这个，而不是问 AI 怎么看`
5. 设置为 **Public**（公开）
6. **不要** 选择 "Initialize this repository with a README" 等选项（因为我们已有本地文件）
7. 点击 **Create repository**

### 2. 关联远程仓库

复制创建后显示的 HTTPS URL（看起来像 `https://github.com/YOUR_USERNAME/best-minds.git`）

然后在终端运行：

```bash
cd /Users/chengfeng/Desktop/best-minds

# 添加远程仓库
git remote add origin https://github.com/YOUR_USERNAME/best-minds.git

# 重命名默认分支（如果需要）
git branch -M main

# 推送到 GitHub
git push -u origin main
```

**注意:** 将 `YOUR_USERNAME` 替换为你的 GitHub 用户名

### 3. 验证上传成功

访问 `https://github.com/YOUR_USERNAME/best-minds` 应该能看到你的仓库

## 验证清单

上传后检查以下内容：

- ✅ 所有文件都在仓库中显示
- ✅ README.md 在仓库首页显示
- ✅ LICENSE 文件存在
- ✅ SKILL.md 文件存在
- ✅ EXAMPLES.md 文件存在
- ✅ .gitignore 文件存在

## 可选：配置仓库

### 添加 Topics（主题标签）

1. 进入仓库的 **Settings**
2. 向下滚动到 "Topics"
3. 添加以下标签：
   - `claude`
   - `skill`
   - `ai`
   - `prompt-engineering`
   - `best-minds`
   - `expert-simulation`

### 启用 Discussions（讨论）

1. 进入 **Settings**
2. 找到 **Features** 部分
3. 勾选 **Discussions**

这样用户可以提问和讨论使用 Best Minds 的方法。

## 后续更新

如果你在本地做了修改，可以这样推送：

```bash
cd /Users/chengfeng/Desktop/best-minds

# 查看状态
git status

# 添加更改
git add .

# 提交
git commit -m "描述你的更改"

# 推送到 GitHub
git push origin main
```

## 常见问题

### 推送时输入密码

如果要使用 SSH 而不是 HTTPS，可以配置 SSH 密钥。详见 [GitHub SSH 设置](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

### 如果仓库已有提交历史

如果你之前在 GitHub 创建了仓库，可能需要：

```bash
git pull origin main --allow-unrelated-histories
```

### 许可证设置

本项目使用 MIT License，已包含在仓库中。

## 分享你的仓库

上传完成后，你可以分享：
- GitHub 链接：`https://github.com/YOUR_USERNAME/best-minds`
- 在 Claude Code 中安装的命令：
  ```bash
  git clone https://github.com/YOUR_USERNAME/best-minds ~/.claude/skills/best-minds
  ```

## 需要帮助？

- GitHub 官方文档：https://docs.github.com
- Git 学习资源：https://git-scm.com/book/zh/v2
