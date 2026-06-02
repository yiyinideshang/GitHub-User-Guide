# 1. Claude Code

[Claude Code官网](https://claude.com/)Anthropic

- 还没开始

# 2. ChatGPT 

[ChatGPT](https://chatgpt.com/)

## [OpenAI官网](https://openai.com/zh-Hans-CN/)

旗下产品：

- Codex

- ChatGPT

# 3. Deepseek

[DeepSeek | 深度求索](https://www.deepseek.com/)

- API开放平台：[DeepSeek 开放平台](https://platform.deepseek.com/usage)

Deepseek开放平台：https://platform.deepseek.com/usage

DeepseekAPI文档：https://api-docs.deepseek.com/zh-cn/

- 👤 **DeepSeek Chat (非思考模式)**：定位为日常全能助手，反应快且不展示思考过程，适合**日常对话、内容创作、翻译、客服**等需要快速响应的常规任务。不适用于需要深入多步推理的复杂任务（如复杂数学证明）。
- 🧠 **DeepSeek Reasoner (思考模式)**：定位为深度推理专家，会展示包含“分析问题→推导过程”在内的完整思维链（CoT），适合处理**高阶编程、复杂数学、逻辑分析、辅助决策**等难题。不适用于对响应速度要求高或仅需简单闲聊的场合。
- ⚙️ **性能与成本**：Chat 追求速度与低延迟，成本较低；Reasoner 则偏向慢速深度思考，生成详细思维链导致成本更高。Reasoner默认输出长度更长（可达32K-64K tokens），且不支持调整`temperature`等参数。

# 4. Kimi

[Kimi ](https://www.kimi.com/)

[Moonshot AI(月之暗面官网)](https://www.moonshot.cn/)

- API开放平台：[Kimi API 开放平台](https://platform.kimi.com/)

# 5. 千问

 [千问.md](千问.md) 

[千问](https://www.qianwen.com/)

- [阿里云](https://www.aliyun.com/)

# 6. 智谱

 [智谱.md](智谱.md) 

[bigmodel智谱官网](https://bigmodel.cn/)

- [智谱AI开放平台](https://bigmodel.cn/)

# Cursor

- 还没开始

# [Chatbox AI](https://chatboxai.app/zh/)

[Chatbox 官网](https://chatboxai.app/zh/)

网页版：https://web.chatboxai.app/guide

Chatbox AI 是一款 AI 客户端应用和智能助手，支持众多先进的 AI 模型和 API，可在 Windows、MacOS、Android、iOS、Linux 和网页版上使用。

# OpenCode

[OpenCode官网](https://opencode.ai/zh)

[OpenCode工作台](https://opencode.ai/auth)

# Codex

[OpenAI Developers](https://developers.openai.ac.cn/codex)

==[Codex 教程 | 菜鸟教程](https://www.runoob.com/codex/codex-tutorial.html)==

## 已经安装了Codex CLI 还没有ChatGPT账号

https://www.runoob.com/codex/codex-install.html

## 已使用 npm 安装了Codex CLI

````c++
sudo npm install -g @openai/codex

# 使用国内镜像安装更快
sudo npm install -g @openai/codex --registry=https://registry.npmmirror.com
````

安装位置：`D:\nodejs\node_global\node_modules\@openai`

安装完成后**在`powershell`中运行**：

```powershell
codex
```

即可启动 Codex。

## ==登录 Codex==

首次运行需要登录。

有两种方式：

### 方法一：ChatGPT 登录（推荐）

```powershell
codex
```

选择：

```powershell
Sign in with ChatGPT
```

然后浏览器会打开登录页面。

登录完成即可使用。

### 方法二：API Key 登录

如果是开发者模式，可以使用 API Key：

```powershell
# macOS / Linux - 临时设置（仅当前终端会话有效）
export OPENAI_API_KEY="sk-你的API密钥"

# 永久配置（添加到 ~/.bashrc 或 ~/.zshrc）
echo 'export OPENAI_API_KEY="sk-你的API密钥"' >> ~/.zshrc
source ~/.zshrc

# Windows PowerShell
$env:OPENAI_API_KEY="sk-你的API密钥"

# 配置后启动（指定模型）
codex --model gpt-5-codex
```

然后运行：

```powershell
codex
```

### 方式三：auth.json 文件配置

手动编辑认证文件, 创建目录:

```powershell
mkdir -p ~/.codex
```

写入 API key:

```js
cat > ~/.codex/auth.json << 'EOF'
{
  "OPENAI_API_KEY": "sk-你的API密钥"
}
EOF
```

## 第一次运行 Codex

进入项目目录：

```powershell
cd my-project
```

启动 Codex：

```powershell
codex
```

然后输入：

```powershell
分析下当前的项目结构
```

Codex 会自动：

1. 扫描代码库
2. 分析项目结构
3. 输出系统架构说明

例如，我们创建一个目录：

```powershell
mkdir codex-runoob-test
```

进入目录：

```powershell
cd codex-runoob-test
```

新建 test.py 文件，代码如下：

```powershell
print("Hello Runoob!")
```

启动 Codex：

```powershell
codex
```

选第一个 **Yes, continue** 回车，这样就可以开始使用 Codex Cli 开始写代码了:

![img](https://www.runoob.com/wp-content/uploads/2026/03/8afd5f03-d5bb-452b-b66a-537aaafb8aa6.png)

## Codex 的三种运行模式

Codex CLI 提供三种安全模式。

| 模式      | 功能             |
| :-------- | :--------------- |
| Suggest   | 只建议修改       |
| Auto Edit | 自动修改文件     |
| Full Auto | 自动执行所有操作 |

默认模式：

```powershell
Suggest
```

切换模式：

```powershell
codex --auto-edit
```

或者：

```powershell
codex --full-auto
```

Full Auto 模式可以自动执行代码修复和任务。

### 更新与卸载

```powershell
# 更新到最新版本
npm update -g @openai/codex

# 或强制重装最新版
npm install -g @openai/codex@latest

# 卸载
npm uninstall -g @openai/codex

# Homebrew 卸载
brew uninstall --cask codex
```

# MCP服务器

# Cline - VSCode插件

## Cline上配置==智谱==

参见： [智谱.md ->Cline上配置智谱](智谱.md) 

## Cline上配置==千问==

- 参见： [千问.md ->Cline上配置千问](千问.md) 

# OpenRouter

[OpenRouter官网](https://openrouter.ai/)

免费可用模型：

- z-ai/glm-4.5-air:free

- google/gemma-4-31b-it:free

