# 在 VSCode 中使用 SSH 将本地更改推送到 GitHub 已有仓库

本文档整合了从零开始配置 Git/SSH、在 VSCode 中操作并将本地项目更新推送到 GitHub 已有仓库的完整流程。包含 Windows 和 Linux 环境下的示例，并保留了关键截图链接。

# ==------------------------------==

------

# 1. 准备工作：安装 Git 并配置 SSH 密钥

## 1.1 安装 Git

- 从 [git-scm.com](https://git-scm.com/) 下载并安装。
- 安装后在终端（VSCode 终端或系统终端）验证：`git --version`

## 1.2 配置 Git 用户信息（首次使用）

在`vscode`终端上输入:	

```bash
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的邮箱"
#下述为示例
git config --global user.name "yiyinideshang"
git config --global user.email 2779271357@qq.com
```

## 1.3 生成 SSH 密钥对（如果已有可跳过）

在命令行（CMD、PowerShell 或 Linux 终端）执行：

```bash
ssh-keygen -t rsa -b 4096 -C "你的邮箱"
#下述为示例
ssh-keygen -t rsa -b 4096 -C "2779271357@qq.com"
```

- 按提示选择保存路径（默认 `~/.ssh/id_rsa`），可直接回车。
- 设置密码短语（passphrase）可直接回车跳过。

完整示例:

```c
C:\Users\Lenovo>ssh-keygen -t rsa -b 4096 -C "2779271357@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\Lenovo/.ssh/id_rsa): 
#直接按 Enter

Created directory 'C:\Users\Lenovo/.ssh'.  #← 自动创建.ssh目录
Enter passphrase (empty for no passphrase): 
#直接按 Enter（不设置密码）
Enter same passphrase again: 
#直接按 Enter

Your identification has been saved in C:\Users\Lenovo/.ssh/id_rsa
Your public key has been saved in C:\Users\Lenovo/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 2779271357@qq.com
The key's randomart image is:
+---[RSA 4096]----+
|      .....      |
|       ...       |
|      . . .      |
|     . . . .     |
|    . . . . .    |
|   . . . . . .   |
|  . . . . . . .  |
| . . . . . . . . |
|. . . . . . . . .|
+----[SHA256]-----+
```

生成成功后，会有两个文件：：

1. **私钥**：`C:\Users\Lenovo/.ssh/id_rsa` ← **不要分享给任何人**
2. **公钥**：`C:\Users\Lenovo/.ssh/id_rsa.pub` ← **需要添加到 `GitHub`**

> 如果你之前已经生成过 SSH 密钥，会看到：
>
> ```powershell
> C:\Users\Lenovo/.ssh/id_rsa already exists.
> Overwrite (y/n)?
> ```
>
> - 如果选择 `y`：覆盖旧的密钥
> - 如果选择 `n`：保留旧的，需要指定新文件名

## 1.4 将公钥添加到 GitHub

1. 复制公钥内容
   - 在`C:\Users\Lenovo\.ssh`文件夹下找到`id_rsa.pub`,将里面的内容作为**公钥复制到`github`上**
   - （可用命令 `cat ~/.ssh/id_rsa.pub` 查看并复制）。
2. 登录 GitHub → **Settings** → **SSH and GPG keys** → **New SSH key**。
3. Title 随意填写,比如 "My Windows PC"，
4. Key 中粘贴刚刚复制的公钥
5. 点击 **Add SSH key**。

## 1.5 测试 SSH 连接

```bash
ssh -T git@github.com
```

**成功时会显示：**

```powershell
Hi yiyinideshang! You've successfully authenticated, but GitHub does not provide shell access.
```

## 1.6 （可选）修改 SSH 配置文件`config`

如果遇到网络问题，可 在`C:\Users\Lenovo\.ssh`文件夹下找到`config`文件,修改里面的内容,如 用 HTTPS 端口（443）连接 GitHub：

```bash
#使用github的远程ssh
Host github.com
  Hostname ssh.github.com
  Port 443
#使用虚拟机的远程ssh
#Host 192.168.201.128
  #HostName 192.168.201.128
  #User yishang
```

------

# 2. 在 VSCode 中打开或克隆仓库

### 情况 A：本地已有仓库（之前克隆或创建）

- 在 VSCode 中打开该仓库文件夹：**文件 → 打开文件夹**，选择仓库根目录。

### 情况 B：需要从 GitHub 克隆（使用 SSH URL）

1. 在 GitHub 仓库页面，点击绿色 **Code** 按钮，选择 **SSH**，复制类似 `git@github.com:用户名/仓库名.git` 的地址。
2. 在 VSCode 中打开命令面板（`Ctrl+Shift+P`），输入 `Git: Clone`，粘贴 SSH URL，选择本地存放路径，克隆完成后打开仓库。

------

# 3. 将本地项目关联到已有 GitHub 仓库（如果还未关联）

### 3.1 初始化本地仓库（如果项目尚未用 Git 管理）

```bash
# 1. 在 VSCode 中打开项目文件夹
# 2. 初始化本地仓库
git init
# 3. 添加所有文件
git add .

# 4. 提交
git commit -m "初始提交"
```

### 3.2 添加远程仓库地址（SSH 格式）

```bash
git remote add origin git@github.com:用户名/仓库名.git
```

如果已存在远程仓库 `origin`，可先移除再添加：

```bash
git remote remove origin
git remote add origin git@github.com:用户名/仓库名.git
```

验证远程地址：`git remote -v` **查看当前本地仓库所关联的所有远程仓库**

- **`git remote`**：管理远程仓库的命令（列出、添加、删除等）。
- **`-v`**：`--verbose` 的缩写，表示“显示详细信息”。
- 合起来：**列出所有远程仓库的别名和对应的 URL**，并且会标注每个 URL 是用于 **fetch（拉取）** 还是 **push（推送）**。

在你本地的 Git 项目里运行这个命令，可能会看到类似这样的输出：

```bash
origin  https://github.com/你的用户名/项目名.git (fetch)
origin  https://github.com/你的用户名/项目名.git (push)
origin：远程仓库的默认别名（你也可以添加其他名字，如 upstream）。

https://...：远程仓库的实际地址。

(fetch)：从这个地址拉取更新（git pull / git fetch 使用）。

(push)：向这个地址推送更新（git push 使用）。
```



### 3.3 确保分支名匹配（GitHub 默认主分支为 `main`）

```bash
git branch -M main
```

### 3.4 首次推送（如果远程仓库为空）

```bash
git push -u origin main
```

若远程仓库已有内容（如初始化时添加了 README），需先**拉取合并**：

```bash
git pull origin main --allow-unrelated-histories
# 解决冲突后再次推送
git push -u origin main
```

------

# 4. 日常更新：在 VSCode 中修改文件并推送到 GitHub

### 4.1 查看更改

1. 点击左侧活动栏的 **源代码管理** 图标（或 `Ctrl+Shift+G`）。
2. **更改** 列表中会列出所有修改过的文件。

### 4.2 暂存更改

- 暂存所有更改：点击文件列表上方的 **+**。
- 暂存部分文件：点击文件右侧的 **+**。

### 4.3 提交到本地仓库

1. 在顶部的输入框中填写 **提交信息**（==描述本次更改的内容==）。

   ![github1](D:\Typora\typora_work\C++规划\github1.png)

2. 点击 **提交** 按钮（对勾图标）或按 `Ctrl+Enter` 完成本地提交。

   ![github2](D:\Typora\typora_work\C++规划\github2.png)

> 提交信息会永久保存在本地 Git 历史中，可通过 `git log` 或 `git log --oneline` 查看。

### 4.4 推送到 GitHub

- ==在文本框里写下描述==，然后按 `Ctrl+Enter`或者点击“**同步更改**”按钮推送到 GitHub
- 点击源代码管理视图底部的 **同步更改** 按钮（或点击 **...** → **推送**）。
- 或者使用终端命令：`git push`

> 如果远程有更新而本地没有拉取，推送可能会被拒绝。可先点击 **拉取** 或使用 `git pull` 合并后再推送。

------

# 5. 补充：.gitignore 文件（可选）

在项目根目录创建 `.gitignore` 文件，列出不需要版本控制的文件或文件夹（如编译输出、临时文件、敏感信息等）。例如：

```text
__pycache__/
*.pyc
.env
venv/
node_modules/
```

提交并推送 `.gitignore`：

```bash
git add .gitignore
git commit -m "添加 .gitignore"
git push
```

------

# 6.常见错误：

```c
Git: fatal: unable to access 'https://github.com/yiyinideshang/GitHub-User-Guide.git/': SSL certificate OpenSSL verify result: unable to get local issuer certificate (20)
吉特：致命错误：无法访问 'https://github.com/yiyinideshang/GitHub-User-Guide.git/': SSL 证书 OpenSSL 验证结果：无法获取本地颁发机构证书（错误代码 20）
```

### 使用 SSH 协议（推荐）

将远程仓库地址改为 SSH 格式：

```bash
git remote set-url origin git@github.com:yiyinideshang/GitHub-User-Guide.git
```

之后推送、拉取等操作都会通过 SSH，不再受 SSL 证书影响。

# ==------------------------------==

# Linux端的过程截图

## ✅ 第一步：安装并配置 Git（如果还没做）

![1](D:\Typora\typora_work\C++规划\1.png)

## 📁 第二步：在本地代码文件夹中初始化 Git 仓库

![2](D:\Typora\typora_work\C++规划\2.png)

## 📄 第三步：添加文件并提交到本地仓库

![3](D:\Typora\typora_work\C++规划\3.png)

## 第四步：在 GitHub 上创建一个空仓库

1. 登录 GitHub，点击右上角 `+` → **New repository**。
2. 填写仓库名（Repository name），**不要勾选** “Add a README”、“Add .gitignore”、“Choose a license”（避免产生初始提交与本地冲突）。
3. 点击 **Create repository**。

我们将直接使用 **SSH 方式** 进行关联（因为你已经配置好 SSH 密钥）。

## 🔗 第五步：关联远程仓库并推送代码

**1. 添加远程仓库地址（SSH 格式）**

```bash
git remote add origin git@github.com:你的用户名/仓库名.git
```

将 `你的用户名/仓库名` 替换成实际的。

**2. 将本地分支重命名为 `main`（GitHub 默认主分支名）**

```bash
git branch -M main
```

**3. 推送代码到 GitHub**

```bash
git push -u origin main
```

`-u` 表示建立本地分支与远程分支的关联，之后只需 `git push` 即可。

![4](D:\Typora\typora_work\C++规划\4.png)

以后每次修改代码，只需三步即可同步到 GitHub：

```bash
git add .
git commit -m "更新说明"
git push
```

## 📌 补充：常用 .gitignore 文件（可选）

如果项目中包含不需要上传的文件（如编译输出、系统文件、密码配置文件等），可以创建 `.gitignore` 文件并写入忽略规则。

```bash
touch .gitignore
nano .gitignore
```

示例内容（Python 项目）：

```text
__pycache__/
*.pyc
.env
venv/
```

将 `.gitignore` 也提交并推送：

```bash
git add .gitignore
git commit -m "添加忽略文件"
git push
```

在 GitHub 创建仓库时误选了“初始化 README”，解决办法是用 `git pull origin main --allow-unrelated-histories` 合并后再推送

# github分支的使用

## 本地Git切换分支

- 查看当前所在分支

````bash
git branch
````

带 `*` 的是当前分支。

````bash
# 1. 切换到目标分支（比如 dev）
git checkout dev

# 2. 拉取远程最新代码
git fetch origin

# 3. 将主分支（main）合并到当前分支
git merge origin/main

# 4. 推送到远程
git push origin dev
````

> 如果有冲突，需要手动解决冲突后再提交。

## 将主分支的更新同步到另一个分支（合并）

这是最常见的场景：您在主分支上有了新提交，希望另一个分支（如 `dev`）也包含这些更新。

### 方法1：在 GitHub 上创建 Pull Request（推荐，安全）

1. 进入 GitHub 仓库页面。
2. 点击 **Pull requests** 标签 → **New pull request**。
3. **base** 选择目标分支（比如 `dev`），**compare** 选择主分支（比如 `main`）。
4. 点击 **Create pull request**，然后 **Merge pull request**。
5. 合并完成后，`dev` 分支就会包含 `main` 的所有更新。

在 GitHub 的 Pull Request（PR）页面中，**base** 和 **compare** 是两个用来指定合并方向的分支。

- **base**：代表 **目标分支**。也就是你希望把代码**合并进去**的那个分支。通常情况下，如果你的 PR 是要把新功能合并到主分支，那么 `base` 就是 `main`（或 `master`）。
- **compare**：代表 **源分支**。也就是你做了修改、想要**合并出去**的那个分支。通常是你自己开发用的特性分支（如 `feature-xxx`）。

**简单理解**：
PR 的目的是 **把 `compare` 分支里的改动，合并到 `base` 分支里**。

👉 方向：`compare` → `base`

举个例子：

- 你在 `dev` 分支上写了新代码。
- 想把这些代码合并到 `main` 分支。
- 那么在创建 PR 时：**base** 选 `main`，**compare** 选 `dev`。

这个设定决定了 PR 合并后，`main` 分支会获得 `dev` 分支上的所有新增提交。

# 将本地一个文件上传到已有的远程仓库上的一个新建文件夹中

## ✅ 操作思路（三步走）

1. **把远程仓库克隆到本地**（如果还没克隆过）
2. **在克隆下来的仓库中创建一个新文件夹**，并把你本地的 `c++_md` 文件复制进去
3. **通过 VSCode 提交并推送到 GitHub**

这样做既安全又简单，不会影响仓库里已有的其他文件。

------

## 📝 详细步骤

### 第一步：克隆远程仓库到本地

假设你想把仓库放在 `D:\VSCodeProject\C-` 这个位置。

1. 打开 **VSCode**。
2. 按下 `Ctrl + Shift + P` 打开命令面板，输入 `Git: Clone`，然后回车。
3. 粘贴你的 SSH 地址：`git@github.com:yiyinideshang/C-.git`
4. 选择存放的目录，例如 `D:\VSCodeProject\`，VSCode 会自动创建一个名为 `C-` 的文件夹并克隆仓库。
5. 克隆完成后，VSCode 会提示是否打开该仓库，选择 **打开**。

> 如果你之前已经克隆过这个仓库（比如在 `D:\VSCodeProject\C-` 路径下），那么直接打开那个文件夹即可，跳过这一步。

------

### 第二步：在仓库中创建新文件夹并复制文件

1. 在 VSCode 左侧的“资源管理器”中，右键点击空白处，选择 **新建文件夹**，给它起一个名字，比如 `md_files`（你也可以叫 `c++_md` 或任何英文名，**建议不要用中文**）。
2. 打开系统的文件管理器（Windows 资源管理器），进入 `D:\Typora\typora_work\c++_md`，选中里面的**所有文件和子文件夹**（按 `Ctrl + A` 全选）。
3. 复制它们（`Ctrl + C`）。
4. 回到 VSCode 所在的项目目录，你可以右键点击刚创建的 `md_files` 文件夹，选择 **在文件资源管理器中显示**，然后把刚才复制的文件粘贴进去（`Ctrl + V`）。

> 这时候 VSCode 的“源代码管理”面板应该能看到大量新文件显示为 **U**（未跟踪）状态。

------

### 第三步：提交并推送到 GitHub

1. 在 VSCode 左侧点击 **源代码管理** 图标（分支图标）。

2. 在“更改”区域，你会看到 `md_files` 文件夹和里面的文件都列出来了。

3. 鼠标移动到 `更改` 字样旁边，点击出现的 **`+`**（暂存所有更改）。也可以逐个文件点击 `+` 暂存。

4. 在顶部的输入框中写一条提交信息，例如：

   ```tex
   添加 c++_md 笔记文件到 md_files 文件夹
   ```

5. 点击输入框上方的 **提交**（对勾图标）。

6. 提交完成后，点击底部的 **同步更改** 按钮（或者点 `...` 菜单选择 `推送`）。

7. 如果是第一次推送，可能会弹窗让你登录 GitHub 授权，按提示完成即可。

------

## 🎉 完成

刷新你的 GitHub 仓库页面（`https://github.com/yiyinideshang/C-`），你会看到新建的 `md_files` 文件夹，点进去就是你刚刚上传的所有 Markdown 文件了。

------

## 💡 补充说明

- **为什么不直接在 GitHub 网页上创建文件夹？**
  GitHub 网页虽然可以创建空文件夹，但没法批量上传文件。上面的方法是专业且高效的批量上传方式。

- **如果仓库里已有同名文件怎么办？**
  如果 `c++_md` 里的文件和仓库中已有文件重名，Git 会提示冲突。你可以在复制前先备份，或者在提交时手动选择要覆盖的文件。一般 Markdown 笔记冲突概率很小。

- **以后更新文件怎么办？**
  以后你在 `D:\Typora\typora_work\c++_md` 里修改或新增了文件，可以：

  - 复制到 `D:\VSCodeProject\C-\md_files` 覆盖旧文件
  - 在 VSCode 的源代码管理中暂存 → 提交 → 同步

  或者，你也可以直接把 Typora 的工作目录改成 VSCode 仓库里的那个文件夹，这样写完笔记直接就在 Git 管理下了，更方便。

# `git pull`和`git fetch`

## 什么时候用？

当你想让本地仓库同步远程仓库（比如 GitHub 上别人推送了新代码，或者你在另一台电脑上推送了更新），就用 `git pull`。

**`git pull` = 下载远程更新 + 自动合并到本地当前分支**，让你本地的代码和远程保持一致。

| 命令        | 是否下载新提交 | 是否自动合并                 |
| :---------- | :------------- | :--------------------------- |
| `git fetch` | ✅ 是           | ❌ 否（只下载，不改变工作区） |
| `git pull`  | ✅ 是           | ✅ 是（下载+合并）            |

- `git fetch` 更安全：你可以先看看远程更新了什么，再决定是否合并。
- `git pull` 更方便：一步完成同步，适合日常简单场景。

## 可能会遇到的问题

1. **合并冲突**
   如果远程的修改和你本地的未提交修改（或本地的新提交）冲突，Git 会提示冲突，需要你手动解决后再提交。
2. **未完成的合并**（就像你之前遇到的）
   如果上次 `git pull` 或 `git merge` 没有正常完成（比如手动中断、冲突未解决），再次 `git pull` 会报错，需要先处理完上次的合并或执行 `git merge --abort`。

---

# 本地和远程冲突解决

如：我在github上修改了REDME.md文档,但是我在本地直接进行了推送,会导致文件冲突。**所以**我使用了`git pull`**从远程仓库拉取最新内容并合并到本地**,**但是**我没有正常完成(比如手动中断、冲突未解决),再次`git pull`会报错。需要先**处理完上次的合并**或**执行 `git merge --abort`。**

```bash
PS D:\VSCodeProject\c-> git pull 
error: You have not concluded your merge (MERGE_HEAD exists).
hint: Please, commit your changes before merging.

PS：从 D:\VSCodeProject\c 目录执行“git pull”命令时出现错误。
错误信息：您尚未完成合并操作（MERGE_HEAD 存在）。
提示：请在进行合并操作之前先提交您的更改。
```

## 查看冲突文件

```bash
git status
```

会列出哪些文件有冲突（显示为 `both modified`）。

## 处理上次的合并

```bash
PS D:\VSCodeProject\c-> git status
On branch main
Your branch and 'origin/main' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" if you want to integrate the remote branch with yours)

All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
        modified:   README.md
```

这个 `git status` 告诉我们两件事：

1. **合并冲突已解决，但合并还没有最终完成**（需要执行 `git commit`）。
2. **本地分支和远程分支已经分叉**（各有 1 个对方没有的提交）。

- **“All conflicts fixed but you are still merging.”**
  你之前执行 `git pull` 或 `git merge` 时遇到了冲突，但你已经手动解决了所有冲突（`README.md` 已修改并加入暂存区）。不过 Git 还在等待你**提交这次合并结果**，所以状态显示“仍在合并中”。
- **“Your branch and 'origin/main' have diverged, and have 1 and 1 different commits each...”**
  这意味着：
  - 你本地有一个提交，远程有一个提交，这两个提交内容不同，且没有共同历史。
  - 通常是因为你本地做了提交，同时远程也有别人（或你在另一台机器）推送了不同的提交，导致分叉。

接下来应该：

```bash
git commit -m "完成合并" #本地完成合并
git push origin main	#将与远端合并后(本地和远端保持一致),本地新操作的文件上传到远端
```

> 小提示：以后如果遇到合并冲突但不想处理，可以直接用 `git merge --abort` 放弃合并，避免进入这种“未完成合并”的状态。

## 撤销/放弃 合并

```bash
git merge --abort
```

- 撤销正在进行的合并
- 让你的仓库回到执行 `git pull` 之前的状态
- 删除 `MERGE_HEAD` 标记
