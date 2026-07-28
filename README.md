# Agent Skills

个人分享的 AI Agent Skills 集合（作者：**bianya**）。

兼容 [skills.sh](https://skills.sh) / 官方 CLI [`npx skills`](https://github.com/vercel-labs/skills)（Vercel Labs）。发布为 **公开 GitHub 仓库** 后，即可用下面的方式安装到 Claude Code、Cursor、Codex、Copilot 等支持 Agent Skills 的工具。

[![skills.sh](https://skills.sh/b/biyan113/agent-skills)](https://skills.sh/biyan113/agent-skills)
[![微信公众号](https://img.shields.io/badge/%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%8F%B7-%E5%AD%97%E8%8A%82%E7%AC%94%E8%AE%B0%E6%9C%AC-07C160?style=flat&logo=wechat&logoColor=white)](https://www.bytenote.net)

## 安装（skills.sh / npx skills）

前置：已安装 Node.js（可用 `npx`）。

### 1. 查看仓库里有哪些 skill

```bash
npx skills add biyan113/agent-skills -l
```

### 2. 安装指定 skill

```bash
# 项目级（默认写入当前仓库的 agent skills 目录）
npx skills add biyan113/agent-skills --skill react-native-ios-ui-design
npx skills add biyan113/agent-skills --skill readme-badges

# 全局安装
npx skills add biyan113/agent-skills --skill readme-badges -g

# 安装全部 skill，并写入所有已检测的 agent
npx skills add biyan113/agent-skills --all
```

也支持完整 URL：

```bash
npx skills add https://github.com/biyan113/agent-skills --skill readme-badges
```

### 3. 更新已安装 skill

```bash
npx skills update
```

### 4. 其它常用命令

```bash
npx skills list          # 查看本机已安装
npx skills find [query]  # 搜索公开 skill
npx skills remove <name> # 卸载
```

> **说明**：`skills.sh` 的安装入口就是 `npx skills`。仓库结构只要包含 `skills/<name>/SKILL.md`（YAML frontmatter 含 `name` / `description`），CLI 就能发现并安装。无需额外 npm 发布。

## 仓库结构

```text
.
├── README.md
└── skills/
    └── <skill-name>/
        └── SKILL.md
```

## 可用 Skills

### `react-native-ios-ui-design`

- **路径**: `skills/react-native-ios-ui-design/`
- **版本**: 1.0.0
- **说明**: 面向 iOS 的 React Native 界面设计、审查与实现 Skill。适用于 Expo Router、Expo 和裸 React Native 项目，覆盖 iPhone、iPad、原生导航、设计令牌、安全区、键盘、手势、Dynamic Type、VoiceOver、深色模式、动画、性能与 App Store 发布前验收。

**安装**

```bash
npx skills add biyan113/agent-skills --skill react-native-ios-ui-design
```

### `readme-badges`

- **路径**: `skills/readme-badges/`
- **版本**: 1.0.0
- **说明**: 为 GitHub README 设计、生成并维护徽章。覆盖 skills.sh 安装徽章、Shields.io 静态/动态徽章、微信公众号与品牌自定义徽章（含「字节笔记本」配方）、中文 URL 编码、颜色与 logo 规范、徽章栏排版。

**安装**

```bash
npx skills add biyan113/agent-skills --skill readme-badges
```

### `app-store-screenshot-design`

- **路径**: `skills/app-store-screenshot-design/`
- **版本**: 1.0.0
- **说明**: 为 iOS/iPadOS 应用生成符合 2026 年主流审美与 App Store 转化逻辑的上架宣传截图方案，覆盖风格选择、截图排序、卖点文案、版式规范、本地化和验收清单。当用户要求设计 App Store 截图、规划宣传图、优化商店截图转化率或准备产品页 A/B 测试时使用。

**安装**

```bash
npx skills add biyan113/agent-skills --skill app-store-screenshot-design
```

### `firebase-analytics-integration`

- **路径**: `skills/firebase-analytics-integration/`
- **版本**: 1.0.0
- **说明**: Firebase Analytics 跨平台接入 Skill。覆盖 Flutter、React Native、Expo、iOS、Android、Web、Unity、C++ 等多端的 SDK 接入、原生配置、统一抽象层封装、事件字典设计、用户属性、内购订阅埋点、隐私合规与 ATT、Consent 同意流程、DebugView 验证和上线验收清单。当用户要求接入 Firebase Analytics、增加应用统计、设计埋点、统计用户行为、统一多端 Analytics 时使用。

**安装**

```bash
npx skills add biyan113/agent-skills --skill firebase-analytics-integration
```

### `codebase-documentation`

- **路径**: `skills/codebase-documentation/`
- **版本**: 1.0.0
- **说明**: 项目全量分析与文档化 Skill。对当前项目进行完整、深入、基于真实代码的分析，在 docs/ 下生成一套系统化的项目文档（含项目总览、本地启动、目录结构、系统架构、业务架构、Web 前端、管理后台、后端、移动端、接口、数据库、认证权限、状态与数据流、第三方服务、配置、测试、构建部署、监控日志、安全、故障排查、开发规范、实习生入门、术语表、风险与技术债务、功能追踪矩阵、维护检查清单等 25+ 篇文档及 Mermaid 图表）。所有结论需提供真实代码路径、类名、函数名作为依据，不编造、不泄露敏感信息。当用户要求分析项目、生成项目文档、代码库文档化、新人入门文档、架构梳理、输出项目知识库时使用。

**安装**

```bash
npx skills add biyan113/agent-skills --skill codebase-documentation
```

新增 skill 时：在 `skills/<name>/SKILL.md` 下添加文件，更新本 README 的「可用 Skills」列表后推送即可。

## 本地验证

```bash
git clone https://github.com/biyan113/agent-skills.git
cd agent-skills
npx skills add . -l
```

## License

各 skill 的 `SKILL.md` frontmatter 中可单独声明 `license`。未声明时默认 MIT。
