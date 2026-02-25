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

验证远程地址：`git remote -v`

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

