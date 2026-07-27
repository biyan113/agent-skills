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

新增 skill 时：在 `skills/<name>/SKILL.md` 下添加文件，更新本 README 的「可用 Skills」列表后推送即可。

## 本地验证

```bash
git clone https://github.com/biyan113/agent-skills.git
cd agent-skills
npx skills add . -l
```

## License

各 skill 的 `SKILL.md` frontmatter 中可单独声明 `license`。未声明时默认 MIT。
