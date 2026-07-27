---
name: readme-badges
description: 为 GitHub README 设计、生成并维护徽章（badges）。覆盖 skills.sh 安装徽章、Shields.io 静态/动态徽章、微信公众号与品牌自定义徽章、中文 URL 编码、颜色与 logo 规范、徽章栏排版。当用户要求加 badge、自定义徽章、微信公众号徽章、skills.sh 徽章、README 顶部标识、shields.io 或 Markdown 徽章时使用。
version: 1.0.0
license: MIT
---

# README 徽章 Skill

当用户提出以下需求时使用本 Skill：

- 给 README 加徽章 / badge
- 自定义徽章文案、颜色、logo
- 添加 **skills.sh** 安装徽章
- 添加 **微信公众号** 或其它品牌徽章（如「字节笔记本」）
- 整理 README 顶部徽章栏
- 生成 Shields.io 静态或动态徽章 Markdown
- 修复中文徽章不显示、编码错误、链接错误

目标：输出**可直接粘贴到 README 的 Markdown**，风格统一、可点击、可维护。

---

# 1. 工作流程

## 1.1 先收集信息

动手前尽量确认：

1. 徽章用途：安装入口 / 状态 / 品牌 / 社交 / 许可证
2. 文案：左侧 label、右侧 message（或单段文案）
3. 点击跳转 URL
4. 颜色：品牌色或语义色
5. logo：simple-icons slug 或不要 logo
6. 放置位置：标题下 / 安装区 / 文档底部
7. 是否已有徽章栏（保持同一 `style`）

信息不全时：

- 按常见开源 README 惯例推断
- 简短说明假设
- 给出可粘贴 Markdown，而不是空等

## 1.2 默认风格

除非用户指定，否则使用：

| 项 | 默认 |
|---|---|
| 服务 | [Shields.io](https://img.shields.io) 静态徽章 |
| style | `flat` |
| logoColor | `white`（深色底时） |
| 排列 | 标题下方，一行多个，空格或换行分隔 |
| 链接 | 每个徽章都必须可点击（有合理 URL） |

## 1.3 交付格式

始终按以下结构交付：

1. **假设**（若有）
2. **徽章预览说明**（label / message / color / link）
3. **可粘贴 Markdown**（完整代码块）
4. **放置建议**（插在 README 哪一段之后）
5. **可选变体**（flat-square / for-the-badge 等，仅当有用时）

用户要求改文件时，直接编辑 `README.md`（或指定文件），不要只给示例却不落地。

---

# 2. Markdown 模板

## 2.1 通用可点击徽章

```markdown
[![替代文本](徽章图片URL)](点击跳转URL)
```

## 2.2 Shields.io 静态徽章（推荐）

路径格式：

```text
https://img.shields.io/badge/<LABEL>-<MESSAGE>-<COLOR>?style=flat&logo=<LOGO>&logoColor=white
```

完整示例：

```markdown
[![MIT](https://img.shields.io/badge/license-MIT-blue?style=flat)](LICENSE)
```

### 路径参数编码规则（关键）

Shields 静态路径用 `-` 分段，特殊字符必须编码：

| 字符 | 编码 |
|---|---|
| 空格 | `_` 或 `%20` |
| `-`（字面量连字符） | `--` |
| `_`（字面量下划线） | `__` |
| `/` | `%2F` |
| 中文与其它非 ASCII | **必须 URL 编码**（UTF-8 percent-encoding） |

**中文务必编码**，不要直接把汉字塞进路径：

```bash
# 示例：生成「微信公众号 / 字节笔记本」编码
python3 - <<'PY'
from urllib.parse import quote
print(quote("微信公众号", safe=""))
print(quote("字节笔记本", safe=""))
PY
```

编码后示例路径片段：

```text
%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%8F%B7-%E5%AD%97%E8%8A%82%E7%AC%94%E8%AE%B0%E6%9C%AC-07C160
```

### 查询参数

常用：

- `style=flat` | `flat-square` | `plastic` | `for-the-badge` | `social`
- `logo=<simple-icons-slug>`（见 [simpleicons.org](https://simpleicons.org/)）
- `logoColor=<color>`
- `labelColor=<color>`（左侧底色）
- `cacheSeconds=<n>`（动态徽章可调缓存）

颜色可用：

- 具名：`brightgreen` `green` `yellowgreen` `yellow` `orange` `red` `blue` `lightgrey` `success` `important` `critical` `informational` `inactive`
- 十六进制：**不带 `#`**，如 `07C160`、`111111`

---

# 3. 场景配方库

## 3.1 skills.sh 安装徽章

适用于 Agent Skills 公开仓库（`owner/repo`）：

```markdown
[![skills.sh](https://skills.sh/b/<owner>/<repo>)](https://skills.sh/<owner>/<repo>)
```

示例：

```markdown
[![skills.sh](https://skills.sh/b/biyan113/agent-skills)](https://skills.sh/biyan113/agent-skills)
```

规则：

- 仓库必须是 **public**
- slug 与 GitHub `owner/repo` 一致
- 徽章本身由 skills.sh 托管，**不要**改成 shields 仿制，除非 skills.sh 不可用

配套安装命令（可写在徽章附近或安装章节）：

```bash
npx skills add <owner>/<repo> -l
npx skills add <owner>/<repo> --skill <skill-name>
npx skills add <owner>/<repo> --all
```

## 3.2 微信公众号徽章

品牌绿建议：`07C160`（微信主色近似），logo：`wechat`。

```markdown
[![微信公众号](https://img.shields.io/badge/%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%8F%B7-%E5%AD%97%E8%8A%82%E7%AC%94%E8%AE%B0%E6%9C%AC-07C160?style=flat&logo=wechat&logoColor=white)](https://www.bytenote.net)
```

渲染效果对应：

| label | message | color | logo | link |
|---|---|---|---|---|
| 微信公众号 | 字节笔记本 | `07C160` | wechat | 站点或介绍页 |

换公众号名时：

1. 用 URL 编码生成 message
2. 替换跳转链接（官网 / 介绍文 / 二维码页）
3. 保持绿色与 wechat logo，除非品牌另有规范

通用模板（替换编码后的名称与链接）：

```markdown
[![微信公众号](https://img.shields.io/badge/<ENCODED_LABEL>-<ENCODED_NAME>-07C160?style=flat&logo=wechat&logoColor=white)](<URL>)
```

## 3.3 许可证

```markdown
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat)](LICENSE)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=flat)](LICENSE)
```

## 3.4 语言 / 技术栈

```markdown
[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![React Native](https://img.shields.io/badge/React_Native-61DAFB?style=flat&logo=react&logoColor=black)](https://reactnative.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
```

## 3.5 GitHub 动态徽章

```markdown
[![Stars](https://img.shields.io/github/stars/<owner>/<repo>?style=flat&logo=github)](https://github.com/<owner>/<repo>/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/<owner>/<repo>?style=flat)](https://github.com/<owner>/<repo>/commits)
[![Issues](https://img.shields.io/github/issues/<owner>/<repo>?style=flat)](https://github.com/<owner>/<repo>/issues)
[![Release](https://img.shields.io/github/v/release/<owner>/<repo>?style=flat)](https://github.com/<owner>/<repo>/releases)
```

## 3.6 npm / CI

```markdown
[![npm](https://img.shields.io/npm/v/<package>?style=flat)](https://www.npmjs.com/package/<package>)
[![CI](https://img.shields.io/github/actions/workflow/status/<owner>/<repo>/<workflow-file>?style=flat&label=CI)](https://github.com/<owner>/<repo>/actions)
```

## 3.7 社交与主页

```markdown
[![X](https://img.shields.io/badge/X-000000?style=flat&logo=x&logoColor=white)](https://x.com/<handle>)
[![Blog](https://img.shields.io/badge/Blog-FF5722?style=flat&logo=rss&logoColor=white)](https://example.com)
[![Docs](https://img.shields.io/badge/Docs-blue?style=flat&logo=gitbook&logoColor=white)](https://example.com/docs)
```

---

# 4. Agent Skills 仓库推荐徽章栏

公开 skill 仓库标题下建议至少两枚：

1. **skills.sh**（安装与发现）
2. **品牌/公众号**（作者来源，可选）

示例（`biyan113/agent-skills` + 字节笔记本）：

```markdown
# Agent Skills

个人分享的 AI Agent Skills 集合（作者：**bianya**）。

[![skills.sh](https://skills.sh/b/biyan113/agent-skills)](https://skills.sh/biyan113/agent-skills)
[![微信公众号](https://img.shields.io/badge/%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%8F%B7-%E5%AD%97%E8%8A%82%E7%AC%94%E8%AE%B0%E6%9C%AC-07C160?style=flat&logo=wechat&logoColor=white)](https://www.bytenote.net)
```

原则：

- 徽章放在 **H1 与第一段说明之后、第一个 `##` 之前**
- 同一行或连续两行，不要夹杂大段正文
- 不要堆超过 6～8 枚无信息量徽章
- 动态徽章（stars 等）可放次要位置，避免喧宾夺主

---

# 5. 排版与可访问性

1. **alt 文本**写清含义：`微信公众号`、`skills.sh`、`License MIT`，不要写 `badge` 或 `image`
2. **每个徽章都有链接**；无链接时至少链到仓库相关路径
3. **颜色对比**：浅色字配深色底；浅色底用深 `logoColor`
4. **风格统一**：同一 README 只用一种 `style`
5. **中英文混排**：中文 label + 英文 logo 可以；避免 message 过长导致徽章超宽
6. **不要用 HTML `<img>`** 除非 Markdown 无法满足（GitHub README 优先纯 Markdown）
7. **不要把徽章放进表格单元格当正文**，顶部横排更清晰

---

# 6. 生成自定义徽章的标准步骤

用户说「做一个 xxx 徽章」时：

### 步骤 A：定文案

- label（左）/ message（右）
- 或单侧强调（message 为主，label 为品类）

### 步骤 B：定视觉

- 品牌色优先；否则语义色（文档=蓝，成功=绿，警告=黄，危险=红）
- logo 从 simple-icons 选最接近的 slug；没有就不加，**不要用 emoji 当 logo 参数**

### 步骤 C：编码

```bash
python3 -c "from urllib.parse import quote; print(quote('文案', safe=''))"
```

### 步骤 D：拼 URL 与 Markdown

```text
https://img.shields.io/badge/<label>-<message>-<color>?style=flat&logo=<logo>&logoColor=white
```

```markdown
[![alt](img-url)](link-url)
```

### 步骤 E：验证

- 本地 README 预览或打开图片 URL 确认渲染
- 中文是否正常（未编码会裂图）
- 链接是否 200 / 是否为用户期望落地页

### 步骤 F：写入仓库

- 更新 `README.md` 徽章栏
- 如同时发布 skill 列表，同步「可用 Skills」说明（若仓库有该章节）

---

# 7. 常见问题排查

| 现象 | 原因 | 处理 |
|---|---|---|
| 徽章裂图 | 中文未编码、路径 `-` 分段错误 | URL encode；字面量 `-` 写成 `--` |
| 颜色不对 | 带了 `#` | 去掉 `#`，用 `07C160` |
| logo 不显示 | slug 错误 | 查 simple-icons 官方 slug |
| skills.sh 徽章空白 | 私有仓 / slug 错 / 尚未公开索引 | 确认 public 与 `owner/repo` |
| 徽章过大 | `for-the-badge` + 长中文 | 改 `flat`，缩短 message |
| 点击无反应 | 缺少外层 `[](url)` | 补全可点击 Markdown |
| 缓存旧图 | CDN 缓存 | 改文案触发新 URL，或动态徽章调 `cacheSeconds` |

---

# 8. 禁止事项

不要：

- 伪造官方认证徽章（如假的「Verified」「Official」）
- 用徽章堆砌替代真实文档
- 在私有仓库承诺 skills.sh 公共安装徽章一定可用
- 把密钥、token、内网地址写进徽章链接
- 混用 3 种以上 style
- 用截图假徽章代替 SVG/服务徽章（难以维护）
- 不编码直接粘贴中文到 shields 路径

---

# 9. 快速对照：字节笔记本微信徽章

完整可复制：

```markdown
[![微信公众号](https://img.shields.io/badge/%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%8F%B7-%E5%AD%97%E8%8A%82%E7%AC%94%E8%AE%B0%E6%9C%AC-07C160?style=flat&logo=wechat&logoColor=white)](https://www.bytenote.net)
```

字段：

- label: 微信公众号
- message: 字节笔记本
- color: `07C160`
- logo: `wechat`
- link: `https://www.bytenote.net`

---

# 10. 参考

- Shields.io：https://shields.io/
- Shields 静态徽章：https://img.shields.io/badge/
- simple-icons：https://simpleicons.org/
- skills.sh：https://skills.sh/
- skills CLI：https://github.com/vercel-labs/skills
