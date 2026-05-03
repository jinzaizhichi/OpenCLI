# OpenCLI

> **把网站、浏览器会话和本地工具，变成统一 CLI。**  
> 复用已登录 Chrome，一条命令搞定浏览、提取、下载。100+ 站点开箱即用。

[![English](https://img.shields.io/badge/docs-English-1D4ED8?style=flat-square)](./README.md)
[![npm](https://img.shields.io/npm/v/@jackwener/opencli?style=flat-square)](https://www.npmjs.com/package/@jackwener/opencli)
[![Node.js Version](https://img.shields.io/node/v/@jackwener/opencli?style=flat-square)](https://nodejs.org)
[![License](https://img.shields.io/npm/l/@jackwener/opencli?style=flat-square)](./LICENSE)

## 亮点

- **网站变 CLI** — B站、知乎、小红书、Twitter、Reddit 等 100+ 站点，一条命令调用
- **复用登录态** — 不需要 API Key，不需要重新登录，直接复用 Chrome 里的已登录会话
- **零 LLM 成本** — 纯本地执行，不消耗 token，跑一万次也不花钱
- **AI Agent 友好** — 安装 skill 后，AI 可以替你操作任意网站
- **确定性输出** — 相同命令相同输出，支持 json/yaml/csv/table，可管道、可脚本

## 快速开始

### 1. 安装（需要 Node.js >= 21）

```bash
npm install -g @jackwener/opencli
```

### 2. 安装 Chrome 扩展

在 [Chrome Web Store](https://chromewebstore.google.com/detail/opencli/ildkmabpimmkaediidaifkhjpohdnifk) 安装 **OpenCLI** 扩展。

### 3. 验证

```bash
opencli doctor
```

### 4. 跑起来

```bash
opencli list                          # 看看有哪些命令
opencli hackernews top --limit 5      # HN 热帖
opencli bilibili hot --limit 5        # B站热门
opencli zhihu hot                     # 知乎热榜
opencli weibo hot                     # 微博热搜
```

## 我怎么用它

### 浏览信息流

```bash
opencli hackernews top --limit 10     # HackerNews 首页
opencli bilibili hot                  # B站热门
opencli zhihu hot                     # 知乎热榜
opencli weibo hot                     # 微博热搜
opencli reddit hot --subreddit python # Reddit 热帖
opencli twitter trending              # Twitter 趋势
opencli 36kr hot                      # 36氪热门
opencli producthunt today             # ProductHunt 今日
opencli v2ex hot                      # V2EX 热帖
```

### 搜索

```bash
opencli bilibili search "关键词"       # B站搜索
opencli zhihu search "关键词"          # 知乎搜索
opencli xiaohongshu search "关键词"    # 小红书搜索
opencli xianyu search "关键词"         # 闲鱼搜索
opencli bilibili search "关键词" -f json | jq .
```

### 社交媒体

```bash
opencli twitter timeline              # Twitter 时间线
opencli weibo feed                    # 微博动态
opencli xiaohongshu feed              # 小红书推荐流
opencli xiaohongshu note <url>        # 查看笔记详情
opencli twitter user <username>       # 查看用户
```

### 下载内容

```bash
opencli bilibili download BV1xxx                      # B站视频（需 yt-dlp）
opencli xiaohongshu download <url>                    # 小红书图片/视频
opencli zhihu download <文章url>                      # 知乎文章→Markdown
opencli twitter download <username> --limit 20        # Twitter 媒体
opencli weixin download --url <公众号文章url>          # 公众号→Markdown
```

### 输出格式

每条命令都支持 `--format` 或 `-f`：

```bash
opencli bilibili hot -f json    # JSON（给 AI/脚本用）
opencli bilibili hot -f csv     # CSV（给 Excel 用）
opencli bilibili hot -f table   # 表格（给人看，默认）
opencli bilibili hot -f yaml    # YAML
opencli bilibili hot -f md      # Markdown
opencli bilibili hot -v         # 详细调试模式
```

### AI Agent 用

告诉你的 AI Agent（Claude Code / Cursor / OpenCode）这些话，它会自己用 opencli 操作浏览器：

- "帮我看看小红书通知"
- "帮我把知乎热榜导出成 JSON"
- "搜一下 B站关于 AI 的视频"
- "帮我写一个抓取这个网页的适配器"

## 常用站点

运行 `opencli list` 查看完整列表。最常用的：

| 站点 | 命令 |
|------|------|
| bilibili | `hot` `search` `video` `feed` `download` |
| zhihu | `hot` `search` `question` `download` |
| xiaohongshu | `search` `feed` `note` `download` |
| weibo | `hot` `search` `feed` |
| twitter | `trending` `timeline` `search` `tweets` |
| reddit | `hot` `search` `subreddit` |
| hackernews | `top` `new` `best` `search` |
| v2ex | `hot` `latest` `topic` |
| 36kr | `hot` `news` `search` |
| xianyu | `search` `item` |
| douban | `search` `top250` `movie-hot` |
| youtube | `search` `video` `transcript` |
| github | `trending` `search` |
| xiaoyuzhou | `podcast` `episode` `download` `transcript` |

**→ [100+ 站点完整列表](./docs/adapters/index.md)**

### 外部 CLI 透传

把现有的命令行工具统一到 `opencli` 下：

```bash
opencli gh pr list --limit 5    # GitHub CLI
opencli docker ps               # Docker
opencli external register my-tool    # 注册你自己的工具
```

## 安装后做什么

```bash
# 列出所有可用命令
opencli list

# 查看某个站点有什么命令
opencli bilibili --help

# 查看某条命令的参数
opencli bilibili hot --help

# 验证环境
opencli doctor

# 更新
npm install -g @jackwener/opencli@latest
```

## 需要浏览器登录态的命令

大部分命令需要你在 Chrome 里登录了对应网站。如果返回空或报 Unauthorized：
→ 在 Chrome 里打开那个网站，确认登录状态正常，再试。

## 常见问题

**"Extension not connected"**
→ 确保 Chrome 扩展已安装且在 `chrome://extensions` 中已启用，Chrome 在运行中。

**返回空数据**
→ 先在 Chrome 里打开目标网站，确认已登录，再执行命令。

**Node 版本太低**
→ 需要 Node.js >= 21，`node --version` 检查。

**Daemon 问题**
→ `curl localhost:19825/status` 查看状态

## 进阶

- [扩展 OpenCLI](./docs/zh/guide/extending-opencli.md) — 写自己的适配器
- [插件指南](./docs/zh/guide/plugins.md) — 安装第三方命令
- [桌面应用适配器](./docs/adapters/desktop/) — 控制 Cursor、Notion 等
- [给 AI Agent 开发者](./skills/opencli-adapter-author/SKILL.md)

## License

[Apache-2.0](./LICENSE)
