# Node.js（JavaScript 的后台运行环境）

- 本来 JavaScript 只能在浏览器里运行（做网页交互）。
- **Node.js 把它搬到了电脑本地**，让你不用打开浏览器，就能在终端/命令行里直接执行 `.js` 脚本，用来写后端服务、自动化工具、命令行程序等。
- 像你装的 `claude-code`，本质上就是一个用 Node.js 写的命令行工具。

# npm（Node Package Manager）

- 全称 **Node 包管理器**。
- 它是跟着 Node.js 一起装到电脑上的。
- 用来**下载、安装、管理**别人写好的 Node.js 代码包（比如你装的 `@anthropic-ai/claude-code`）。
- 你敲的那些 `npm install -g xxx` 命令，就是在让 npm 去网上仓库（https://www.npmjs.com/）把对应的功能模块拉取到本地。

------

**打个比方：**

- **Node.js** 像是一个**游戏引擎**（提供运行游戏所需的基础能力）。
- **npm** 像是**游戏 mod 管理器**（用来下载、安装各种游戏模组）。
- 你通过 `npm install` 安装的 `claude-code` 就是其中一个模组，安装后直接在命令行输入 `claude-code` 就能跑。

# 验证node版本

## `node -v`

- **作用**：显示当前安装的 **Node.js** 版本。
- `-v` 是 `--version` 的简写。

```powershell
PS C:\Users\Lenovo> node -v
v24.14.1
```

# 验证npm版本

## `npm -v`

- **作用**：显示当前安装的 **npm** 版本。
- 同样，`-v` 就是 `--version`。

```powershell
PS C:\Users\Lenovo> npm -v
11.16.0
```

# 更新npm

## 安装最新稳定版

```powershell
npm install -g npm@latest
```

- `-g`：全局安装（因为 npm 本身是全局工具）
- `npm@latest`：告诉 npm 去安装它自己的最新稳定版
- 运行后，它会把旧版 npm 替换成最新版

## 指定版本更新

```powershell
npm install -g npm@11.16.0
```

在 Windows 上使用 `npm install -g`（全局安装），包会被放到 npm 的全局目录中。

---

# 安装`claude-code`

```powershell
PS C:\Users\Lenovo> npm install -g @anthropic-ai/claude-code

added 2 packages in 8s
```

## 验证是否安装成功

```powershell
PS C:\Users\Lenovo> claude --version
2.1.167 (Claude Code)
```

如果显示版本号，说明全局安装完全正常。

# 1. 查看全局模块的根目录（包体所在）

```powershell
PS C:\Users\Lenovo> npm root -g
D:\nodejs\node_global\node_modules
```

你安装的 `@anthropic-ai/claude-code` 就位于这个目录下的 `\@anthropic-ai\claude-code` 文件夹里。

# 2. 查看可执行命令（快捷方式）的位置

```powershell
PS C:\Users\Lenovo> npm prefix -g
D:\nodejs\node_global
```

这个目录（不是 `node_modules`）里面存放了对应命令的 `.cmd` 和 `.ps1` 文件，比如 `claude-code.cmd`。**这个目录默认已经添加到系统的 PATH 环境变量中**，所以你在终端直接输入 `claude-code` 就能启动程序。

