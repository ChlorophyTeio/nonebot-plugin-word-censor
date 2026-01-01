# nonebot-plugin-word-censor

✨ 基于 NoneBot2 的词汇与正则消息审查拦截插件 ✨

<a href="./LICENSE">
    <img src="https://img.shields.io/github/license/你的GitHub用户名/nonebot-plugin-word-censor.svg" alt="license">
</a>
<a href="https://pypi.python.org/pypi/nonebot-plugin-word-censor">
    <img src="https://img.shields.io/pypi/v/nonebot-plugin-word-censor.svg" alt="pypi">
</a>
<a href="https://www.python.org/">
    <img src="https://img.shields.io/badge/python-3.8+-blue.svg" alt="python">
</a>

## 📖 介绍

`nonebot-plugin-word-censor` 是一个用于审查 NoneBot2 机器人**发送内容**的插件。

它可以防止机器人因为意外触发某些关键词或匹配到特定正则表达式而发送不当消息。支持通过指令实时动态管理黑名单，并具备自动通知管理员（Superuser）的功能。

## 💿 安装

<details open>
<summary>使用 nb-cli 安装</summary>
<pre><code>nb plugin install nonebot-plugin-word-censor</code></pre>
</details>

<details>
<summary>使用 pip 安装</summary>
<pre><code>pip install nonebot-plugin-word-censor</code></pre>
</details>

## ⚙️ 配置

在 NoneBot2 项目的 `.env` 文件中添加以下配置（可选）：

| 配置项 | 类型 | 默认值 | 说明 |
|:-----|:----|:----|:----|
| `send_word_blacklist_file` | string | `./src/send_word_blacklist.json` | 黑名单数据存储路径 (JSON) |
| `send_word_priority` | int | `100` | 插件响应优先级 |

## 🎮 使用方法

**⚠️ 注意**：以下指令仅 **SUPERUSER** (超级用户) 可用。

### 1. 基础指令 (普通词汇)

| 指令 | 格式 | 说明 |
|:-----|:-----|:-----|
| 添加词汇 | `word blacklist add <内容>` | 将指定内容加入黑名单 |
| 删除词汇 | `word blacklist del <内容>` | 将指定内容移出黑名单 |

### 2. 高级指令 (正则表达式)

支持 Python `re` 模块的语法。

| 指令 | 格式 | 示例 |
|:-----|:-----|:-----|
| 添加正则 | `word blacklist add regex <表达式>` | `word blacklist add regex \d{11}` (拦截手机号) |
| 删除正则 | `word blacklist del regex <表达式>` | `word blacklist del regex \d{11}` |

### 3. 其他指令

- **查看列表**：`word blacklist list`
  - 查看当前生效的所有规则（普通词汇会脱敏显示）。
- **刷新配置**：`word blacklist refresh`
  - 如果你手动修改了 JSON 文件，可使用此指令热重载。
- **帮助**：`word blacklist help`

***

本项目使用 [MIT](./LICENSE) 许可证开源。
