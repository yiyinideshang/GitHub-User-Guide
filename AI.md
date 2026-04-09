# Claude Code

# Cursor

- 还没开始

# Codex

==[Codex 教程 | 菜鸟教程](https://www.runoob.com/codex/codex-tutorial.html)==

# 已经安装了Codex CLI 还没有ChatGPT账号

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

# 千问AI

## hello_qwen.mjs

在  `D:\nodejs\node_global\node_modules\test_qwen`下的`hello_qwen.mjs` 文件中:

```js
// hello_qwen.mjs
import OpenAI from 'openai';
import readline from 'readline';

const client = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
  baseURL: 'https://dashscope.aliyuncs.com/compatible-mode/v1',
});

// 对话历史
const messages = [
  { role: 'system', content: '你是一个有用的助手。' }
];

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

console.log('\n通义千问对话已启动。输入问题后按回车发送。输入 "exit" 或 "quit" 退出。\n');

async function askQuestion(userInput) {
  messages.push({ role: 'user', content: userInput });

  try {
    const stream = await client.chat.completions.create({
      model: 'qvq-max-2025-03-25',   
      // 可按需更换为 
      // qwen3.6-plus,
      // qwen3.5-plus,
      // qwen3.5-flash,
      // qwen3.5-35b-a3b,
      // qwen3-max, 
      // qwen-plus,
      // qvq-max-2025-03-25

      //qwen3-coder-plus
      //qwen3-coder-flash
      messages: messages,
      stream: true,
      stream_options: { include_usage: true },  // 注意：true 为小写
      extra_body: { enable_thinking: true },
    });

    let isAnswering = false;
    let assistantReply = '';
    let usage = null;

    console.log('\n' + '='.repeat(20) + '思考过程' + '='.repeat(20));
    for await (const chunk of stream) {
      // 处理 usage（可能出现在没有 choices 的最后一个 chunk 中）
      if (chunk.usage) {
        usage = chunk.usage;
      }

      // 处理正常内容（当 choices 存在时）
      if (chunk.choices && chunk.choices.length > 0) {
        const delta = chunk.choices[0].delta;
        // 阿里云扩展字段：思考内容
        if (delta.reasoning_content) {
          if (!isAnswering) {
            process.stdout.write(delta.reasoning_content);
          }
        }
        // 最终回复内容
        if (delta.content) {
          if (!isAnswering) {
            console.log('\n' + '='.repeat(20) + '完整回复' + '='.repeat(20));
            isAnswering = true;
          }
          process.stdout.write(delta.content);
          assistantReply += delta.content;
        }
      }
    }
    console.log('\n');

    // 打印 Token 用量
    if (usage) {
      console.log('📊 Token 用量统计:');
      console.log(`   - 输入 tokens (prompt_tokens): ${usage.prompt_tokens}`);
      console.log(`   - 输出 tokens (completion_tokens): ${usage.completion_tokens}`);
      console.log(`   - 总计 tokens (total_tokens): ${usage.total_tokens}`);
    } else {
      console.log('⚠️ 未获取到 usage 信息（可能模型或流式响应未返回）');
    }

    // 将助手的回复加入历史
    messages.push({ role: 'assistant', content: assistantReply });

  } catch (error) {
    console.error('发生错误:', error.message);
  }

  promptUser();
}

function promptUser() {
  rl.question('你: ', (input) => {
    if (input.toLowerCase() === 'exit' || input.toLowerCase() === 'quit') {
      console.log('再见！');
      rl.close();
      return;
    }
    askQuestion(input);
  });
}

//启动对话
promptUser();
```

1. **确保环境变量已设置**（在 PowerShell 中）：

   ```powershell
   $env:OPENAI_API_KEY = "sk-你的完整阿里云API Key"
   ```

   （如果之前设置过且当前窗口没关，可以跳过）

1. **运行脚本**：

   ```bash
   node hello_qwen.mjs
   ```

## 查看当前API调用的模型版本

根据之前的对话，你当前是通过API来调用模型的，这意味着你并没有在本地运行一个具体的模型。你代码中的 `model="qwen-turbo"`（或类似参数）决定了你在使用哪个版本的模型。

你可以通过以下几种方式来确认它：

- # **1. 直接查看你的代码**
  
  - **做法**：打开你的 `hello_qwen.mjs` 文件，找到 `client.chat.completions.create` 方法调用。
  - **关键参数**：查看其中 `model:` 后面的值，例如 `'qwen-turbo'`, `'qwen-plus'` 或 `'qwen3.6-plus'`。这个就是你正在使用的模型名称。
- **2. 通过API请求确认**
  
  - 如果你使用的是流式输出（`stream=True`），可以通过打印返回的第一个数据块（chunk）来查看 `model` 字段，这通常包含完整的模型标识符。
  - 但更直接的方式是，你可以发送一个同步请求，并打印完整的响应对象，在其中找到 `model` 字段。
- **3. 访问阿里云百炼控制台**
  - **做法**：登录[阿里云百炼控制台](https://bailian.console.aliyun.com/)。
  - **路径**：进入“模型广场”或“模型管理”页面。
  - **详情**：你可以在这里看到所有可用模型的列表及其详细信息，包括 `qwen-turbo`、`qwen-plus`、`qwen-max` 以及 `qwen3.6-plus` 等模型的具体说明和版本状态

## Cline上配置==千问==

https://bailian.console.aliyun.com/cn-beijing/?spm=5176.29619931.J_SEsSjsNv72yRuRFS2VknO.2.74cd10d7oW5oib&tab=doc#/doc/?type=model&url=2880898

# 智谱AI

## 智谱的使用指南

https://bigmodel.cn/usercenter/glm-coding/overview

## Cline上配置==智谱==

https://docs.bigmodel.cn/cn/coding-plan/tool/cline

## MCP服务器

# Cline - VSCode插件

![Cine](D:\Typora\typora_work\C++规划\Cline.png)

