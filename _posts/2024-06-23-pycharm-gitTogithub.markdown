---
layout: post
title:  "如何用 PyCharm 通过 Git 推送文件到 GitHub"
categories: Github
tags:  Github
author: FTX
---

* content
{:toc}


如何用 PyCharm 通过 Git 推送文件到 GitHub
#### 步骤一：配置 GitHub 账号

1. **登录 GitHub**：确保你已经在 GitHub 上创建了账号，并且了解你的用户名和密码。

2. **配置 PyCharm 中的 GitHub**：
   - 打开 PyCharm，进入 `File -> Settings`。
   - 在设置窗口中，展开 `Version Control`，然后选择 `GitHub`。
   - 点击 `+` 号添加你的 GitHub 账号，输入你的用户名和密码。





#### 步骤二：在 PyCharm 中设置 Git

1. **打开项目**：在 PyCharm 中打开你的项目或者创建一个新项目。

2. **初始化 Git**：如果你的项目还没有使用 Git 进行版本控制，需要初始化 Git：
   - 在 PyCharm 中，右键点击项目根目录，选择 `Git -> Initialize Repository`。
   - 确认初始化后，PyCharm 会自动为你的项目创建一个本地的 Git 仓库。

#### 步骤三：提交和推送更改

1. **添加远程仓库**：
   - 在 PyCharm 中，打开 `VCS` 菜单，选择 `Git -> Remotes...`。
   - 点击 `+` 号添加远程仓库，填入 GitHub 仓库的 URL。通常是 `https://github.com/你的用户名/你的仓库名.git`。

2. **进行修改并提交**：
   - 在 PyCharm 中进行你的代码修改。
   - 每次修改后，可以在 `VCS` 菜单中选择 `Commit` 或者使用快捷键 `Ctrl + K` 提交更改。填写提交信息并点击 `Commit` 按钮。

3. **推送到 GitHub**：
   - 提交后，选择 `VCS -> Git -> Push...`。
   - 确认推送的分支和远程仓库，然后点击 `Push` 按钮。

#### 步骤四：验证推送

1. **检查 GitHub 上的更新**：
   - 打开你的 GitHub 仓库页面，确认你的更改已经成功推送到远程仓库。

通过以上步骤，你就可以使用 PyCharm 将文件推送到 GitHub 仓库了。
