# OpenCode

菜鸟教程：

- https://www.runoob.com/ai-agent/opencode-coding-agent.html

- https://www.runoob.com/opencode/opencode-install.html

# 下载

在WSL中输入：

```bash
curl -fsSL https://opencode.ai/install | bash
```

说明：

- 自动下载最新版本
- 自动配置环境
- 适用于 macOS / Linux / WSL

安装完成后，还会提示你怎么用，OpenCode 包含免费模式，使用方式：

```bash
cd <project>  # 进入项目目录
opencode      # 启动 OpenCode
```

**重新加载 Bash 配置文件，并查看版本信息**

```bash
source ~/.bashrc
opencode --version

yishang@LAPTOP-LA01A7QT:/mnt/d/VSCodeProject/opencode_project$ source ~/.bashrc
yishang@LAPTOP-LA01A7QT:/mnt/d/VSCodeProject/opencode_project$ opencode --version
1.15.5
```

**其他安装**：

- **Ripgrep (rg)**：用于高速代码搜索。
  安装：`sudo apt update && sudo apt install ripgrep`
- **FZF**：提供模糊搜索/交互式过滤，用于在终端中快速选择文件等。
  安装：`sudo apt install fzf`

安装后可重新打开终端（或运行 `source ~/.bashrc`）让命令生效。此后 OpenCode 的所有功能都能完整发挥。

# 启动opencode

```bash
opencode

yishang@LAPTOP-LA01A7QT:/mnt/d/VSCodeProject/opencode_project$ opencode
Performing one time database migration, may take a few minutes...
Database migration complete.

                                   ▄
  █▀▀█ █▀▀█ █▀▀█ █▀▀▄ █▀▀▀ █▀▀█ █▀▀█ █▀▀█
  █  █ █  █ █▀▀▀ █  █ █    █  █ █  █ █▀▀▀
  ▀▀▀▀ █▀▀▀ ▀▀▀▀ ▀▀▀▀ ▀▀▀▀ ▀▀▀▀ ▀▀▀▀ ▀▀▀▀

  Session   New session - 2026-05-20T03:49:27.108Z
  Continue  opencode -s ses_1bc7f523bffeCNCwj3bF2MRaLg
```

# 退出

- **输入命令退出（最规范）**：直接输入 `/exit` 并回车，程序会保存会话并退出。
- **键盘快捷键**：按 `Ctrl + C` 可以中断当前操作并退出。如果正在生成回复，第一次 `Ctrl+C` 会停止生成，再按一次就会退出。
- **发送 EOF（文件结束符）**：按 `Ctrl + D` 发送 EOF，通常也会退出程序。