# WSL

安装WSL2：https://zhuanlan.zhihu.com/p/2017602632177427017

WSL2：https://zhuanlan.zhihu.com/p/2017602632177427017

WSL2github:[发行作品 ·微软/WSL](https://github.com/microsoft/WSL/releases)

## 查看版本

- 较旧的 WSL内核版本 不支持 `--version` 参数

```powershell
PS C:\Users\Lenovo> wsl --status
默认分发: Ubuntu
默认版本: 2
PS C:\Users\Lenovo> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
```

## 更新WSL

法1：使用 `--web-download` 强制更新（推荐，最简单）

这个命令不依赖 Windows Update，直接通过网络下载安装：

```powershell
wsl --update --web-download
```

法二：手动从 GitHub 下载安装

1. 打开 [WSL GitHub Releases](https://github.com/microsoft/WSL/releases)

2. 下载 `Microsoft.WSL_<最新版本>_x64.msixbundle` [Microsoft.WSL_<2.7.3.0>_x64.msixbundle](https://github.com/microsoft/WSL/releases/download/2.7.3/Microsoft.WSL_2.7.3.0_x64_ARM64.msixbundle)
3. 运行安装命令 或 **双击安装包** 

```powershell
Add-AppxPackage .\Microsoft.WSL_2.7.3.0_x64.msixbundle
```

```powershell
PS C:\Users\Lenovo> wsl --version
WSL 版本: 2.7.3.0
内核版本: 6.6.114.1-1
WSLg 版本: 1.0.73
MSRDC 版本: 1.2.6676
Direct3D 版本: 1.611.1-81528511
DXCore 版本: 10.0.26100.1-240331-1435.ge-release
Windows: 10.0.22631.5189
PS C:\Users\Lenovo>
```

# 现代终端WezTerm 

**现代终端**：https://wezterm.org/#features

## 安装

### **📥 方法一：官网下载安装包 (setup.exe) - 推荐新手**

1. 访问 WezTerm 官网的 [Windows 安装页面](http://wezterm.org/install/windows.html)。
2. 在页面中找到 **“Windows”** 部分，点击下载 `setup.exe` 风格的安装程序。
3. 下载完成后，双击运行安装程序，按照指引完成安装。

### **🚀 方法二：使用 winget 包管理器安装 - 推荐开发者**

```
PS D:\WSL> winget install wez.wezterm
搜索源时失败: msstore
执行此命令时发生意外错误：
0x8a15005e : 服务器证书与任何预期值都不匹配。

在工作源中找到以下包。
若要继续操作，请使用--source选项指定其中一个。
名称    ID          源
--------------------------
WezTerm wez.wezterm winget
```

- `winget` 默认尝试连接 `msstore`（微软商店源）时证书验证失败，但社区源 `winget` 是可用的。根据提示，你需要**指定使用 `winget` 源**来安装。
  - `msstore` 源需要与微软商店服务器建立安全连接，但你的当前环境（可能是代理、VPN、或系统时间不准确）导致证书验证失败。
  - 而 `winget` 源（社区仓库，托管在 GitHub 等 CDN 上）不受此影响，可以正常使用。
- 直接用`winget install wez.wezterm`可能会出错,需要指定源安装

```powershell
#指定源安装
winget install --source winget wez.wezterm
#或
winget install wez.wezterm --source winget
```

这样就会从可用的 `winget` 源下载并安装 WezTerm，不会再尝试访问 `msstore`。

## 配置

C:\Users\Lenovo\在这里创建一个`.wezterm.lua`文件

确保设置为 **UTF-8**

- **用记事本打开，右上角文件->另存为->下方编码格式选择:UTF-8，保存到C:\Users\Lenovo\当前位置进行覆盖**

- **或者 在vscode下方点击编码格式选择编码->通过编码保存->选择UTF-8**

`C:\Users\Lenovo\.wezterm.lua`

```lua
local wezterm = require 'wezterm'
local config = wezterm.config_builder()

-- 设置默认启动的程序为 WSL Ubuntu
config.default_prog = { 'wsl.exe', '-d', 'Ubuntu' }   -- 修正：用 wsl.exe 而不是 powershell.exe

-- 添加快捷键：Ctrl+Shift+U 启动 WSL Ubuntu（在已有 WezTerm 窗口中新建标签页）
config.keys = {
    {
        key = 'U',
        mods = 'CTRL|SHIFT',
        action = wezterm.action.SpawnCommandInNewTab {
            label = 'WSL - Ubuntu',
            args = { 'wsl.exe', '-d', 'Ubuntu' },
        },
    },
}

return config
```

## 启动

### 从「开始」菜单

1. 点击屏幕左下角的「开始」按钮。
2. 在程序列表中找到 **WezTerm** 的文件夹或图标。
3. 点击 **WezTerm** 图标即可启动

直接运行 `wezterm` 即可启动

### 通过命令行

1. 按下 `Win + R` 键，输入 `cmd` 或 `powershell` 后回车。
2. 在打开的窗口中，直接输入 `wezterm` 并回车，终端窗口就会弹出。

## 退出

```bash
exit
```

## 新建对话

- **快捷键 `Ctrl+Shift+U`**：在 WezTerm 中按下该组合键，会在当前窗口中新建一个标签页并启动另一个 Ubuntu 会话，方便多任务操作。

# 图形界面vcxsrv

图形界面vcxsrv:https://developer.baidu.com/article/details/2892416

vcxsrv下载安装https://blog.csdn.net/canyuemanyue/article/details/148304354

github:https://github.com/marchaesen/vcxsrv/releases

https://blog.csdn.net/skysafari/article/details/131070710

