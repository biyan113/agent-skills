---
name: react-native-ios-ui-design
description: 面向 iOS 的 React Native 界面设计、审查与实现 Skill。适用于 Expo Router、Expo 和裸 React Native 项目，覆盖 iPhone、iPad、原生导航、设计令牌、安全区、键盘、手势、Dynamic Type、VoiceOver、深色模式、动画、性能与 App Store 发布前验收。
version: 1.0.0
license: MIT
---

# React Native iOS 界面设计 Skill

当用户提出以下需求时使用本 Skill：

- 设计 React Native iOS 应用
- 重构或优化现有 iPhone、iPad 界面
- 将需求文档、截图、草图或 Figma 设计实现为 React Native 代码
- 改善 Expo 或 React Native 项目的 iOS 原生感
- 建立 iOS 专用设计系统和组件库
- 审查视觉一致性、交互、无障碍、性能或 App Store 上架质量
- 同时适配 iPhone 与 iPad
- 输出可运行、可维护、可发布的实现，而不是静态原型

本 Skill 只面向 iOS。

除非用户明确要求，否则不要加入：

- Android 设计规范
- Material Design
- Android 返回键逻辑
- Android 系统栏适配
- Android 权限
- Android 平台分支
- Android 专用组件

目标不是简单地让页面“更现代”，而是做出：

- 符合 iOS 使用习惯
- 视觉一致
- 交互自然
- 支持无障碍
- 性能可靠
- 易于维护
- 可以进入发布流程

的 React Native iOS 应用。

---

# 1. 基本工作原则

## 1.1 先检查项目，再开始设计

修改项目之前，先确认：

1. 项目使用 Expo 还是裸 React Native。
2. 导航使用 Expo Router、React Navigation 还是其他方案。
3. 当前样式方案：
   - `StyleSheet`
   - React Native 内联样式
   - NativeWind
   - Tamagui
   - React Native Paper
   - Restyle
   - 自研设计系统
4. 当前已有：
   - UI 组件
   - 主题
   - 颜色系统
   - 字体
   - 图标库
   - 图片资源
5. 当前状态管理和数据请求方案。
6. 是否存在 `.ios.tsx` 等 iOS 专用实现。
7. 应用支持范围：
   - iPhone
   - iPad
   - 竖屏
   - 横屏
   - Stage Manager
   - 外接显示器
8. 是否已经支持：
   - 深色模式
   - 多语言
   - RTL
   - Dynamic Type
   - VoiceOver
9. 是否包含：
   - Swift
   - Objective-C
   - CocoaPods
   - Expo Config Plugin
   - 原生模块
10. 最低支持的 iOS 版本。

不要仅仅因为更熟悉另一套技术，就替换项目现有架构。

## 1.2 尊重项目已有规范

- 不要无理由引入第二套样式系统。
- 不要随意替换导航库。
- 不要为了一个页面引入大型 UI 框架。
- 不要重复实现项目已经存在的组件。
- 不要在已有项目中重新初始化应用。
- 不要在 UI 任务中顺便重构无关业务代码。
- 优先进行小范围、可验证、可回退的改动。
- 优先复用项目现有字体、图标、动画和状态管理方案。

## 1.3 做原生 iOS 应用，不是手机尺寸网页

除非项目明确使用 Expo DOM 或 WebView，否则不要在原生界面中使用：

- `div`
- `span`
- `button`
- CSS 布局
- DOM API
- Web 专用组件

优先使用 iOS 常见交互：

- 原生 Stack 导航
- 左缘滑动返回
- 大标题导航
- 底部 Tab Bar
- Sheet
- Context Menu
- Action Sheet
- Pull to Refresh
- 列表滑动操作
- 原生搜索
- Safe Area
- 键盘适配
- 原生按压反馈
- 选择、禁用、加载、危险操作状态
- 适度触觉反馈

## 1.4 信息不完整时合理推断

当需求不完整时：

- 按常见 iOS 产品模式推断
- 简短说明重要假设
- 继续完成设计和实现
- 只有真正未知的数据才使用占位符
- 不要因为细节缺失停住整个任务

仅在缺少的信息会明显影响以下内容时才提问：

- 产品结构
- 支付
- 权限
- 隐私
- 账号安全
- 核心用户流程

---

# 2. iOS 设计流程

## 第一步：明确页面任务

每个页面都要明确：

- 用户目标
- 页面主操作
- 次要操作
- 必须展示的信息
- 页面入口
- 页面出口
- 成功状态
- 失败状态
- 页面属于：
  - Stack
  - Tab
  - Sheet
  - Full Screen Modal

一个页面通常只能有一个视觉上最重要的主操作。

## 第二步：建立信息架构

多页面产品在实现前先输出页面结构。

示例：

```text
启动流程
├── 欢迎页
├── 权限说明
└── 登录

主 Tab
├── 首页
│   ├── 详情页
│   └── 创建 Sheet
├── 搜索
│   └── 搜索结果详情
└── 我的
    ├── 设置
    └── 订阅
```

不要把低频功能、辅助功能或设置项放进主 Tab。

## 第三步：确定视觉方向

使用三个具体形容词描述设计方向。

例如：

```text
克制、编辑感、精确
```

然后将形容词转化为实际设计规则：

- 克制：
  - 少量强调色
  - 留白充足
  - 动效不过度
- 编辑感：
  - 强标题层级
  - 图片占主导
  - 内容结构清晰
- 精确：
  - 数字对齐
  - 控件紧凑
  - 间距统一

不要只写：

- 高级
- 现代
- 科技感
- Premium

必须说明这些词对应的具体视觉规则。

## 第四步：建立设计令牌

在制作多个页面之前，先定义：

- 颜色
- 间距
- 圆角
- 字体
- 边框
- 分割线
- 阴影
- 透明度
- 动画时长
- 控件尺寸
- 内容最大宽度
- Safe Area 间距

不要在每个页面里散落原始数值。

## 第五步：建立基础组件

优先创建或复用：

- `Screen`
- `AppText`
- `NavigationTitle`
- `Button`
- `IconButton`
- `TextField`
- `SearchField`
- `Card`
- `ListItem`
- `InsetGroupedSection`
- `Divider`
- `Badge`
- `EmptyState`
- `ErrorState`
- `LoadingState`
- `Sheet`
- `Avatar`
- `SectionHeader`
- `Toolbar`
- `FloatingBottomBar`

只使用一次的布局不要过度抽象。

## 第六步：覆盖所有状态

数据页面必须考虑：

- 初始加载
- 下拉刷新
- 正常内容
- 空状态
- 部分数据
- 可恢复错误
- 阻断错误
- 离线状态
- 禁用状态
- 乐观更新
- 成功反馈
- 权限未询问
- 权限被拒绝
- 权限受限制
- 有限照片权限

不要只实现正常状态。

## 第七步：在真实 Apple 设备尺寸上检查

至少检查：

- 小尺寸 iPhone
- 大尺寸 iPhone
- iPhone 横屏
- iPad 分屏
- iPad 常规宽度
- Stage Manager 窗口缩放
- 浅色模式
- 深色模式
- 大字体
- 键盘弹出
- 长文本
- VoiceOver 顺序
- 减少动态效果
- 增强对比度

---

# 3. iOS 视觉质量规则

## 3.1 页面层级

每个页面必须让用户立刻知道：

1. 当前在哪里
2. 页面是做什么的
3. 最重要的操作是什么
4. 下一步能做什么
5. 当前状态是否可编辑、临时或危险

通过以下方式建立层级：

- 字号
- 字重
- 对比度
- 间距
- 位置
- 导航栏
- 分组结构

不要只依赖颜色。

## 3.2 避免典型 AI 模板感

不要默认使用：

- 紫蓝渐变
- 过度玻璃拟态
- 所有内容都放圆角卡片
- 所有控件都使用超大圆角
- 大面积装饰阴影
- 多个悬浮按钮
- 随机颜色胶囊
- 内容页大面积居中排版
- 无意义的指标卡片
- 占位图表
- Emoji 图标
- 装饰性背景光斑
- 假 Dynamic Island
- 假 iOS 系统弹窗
- 粗糙模仿 Apple 系统界面

只有以下情况适合使用卡片：

- 内容需要分组
- 内容可点击
- 内容需要层级
- 内容是独立预览单元

简单信息优先使用：

- 留白
- Section Header
- Inset Grouped
- Divider
- 背景层级

## 3.3 页面密度

根据产品类型选择密度：

- 消费类应用：
  - 更多图片
  - 更多留白
- 效率工具：
  - 紧凑但清晰
- 金融和监控：
  - 数字对齐
  - 减少装饰
- 内容流：
  - 内容优先
  - 减少边框
- 设置页：
  - 稳定的分组列表
- 表单：
  - 标签、说明、错误靠近输入框

不要让所有页面都很空。

也不要让所有页面都很挤。

## 3.4 文案

使用真实界面文案，不要使用 Lorem Ipsum。

文案要求：

- 简洁
- 具体
- 动作明确
- 术语统一
- 能说明操作后果

按钮文案优先使用：

```text
保存更改
创建提醒
继续
删除账号
重试
允许访问
打开设置
```

能写清楚时，不要只写“确定”。

危险操作必须明确写出动作。

---

# 4. iOS 平台规范

## 4.1 导航

优先使用：

- 原生 Stack 动画
- 左缘滑动返回
- 顶级页面使用 Large Title
- 详情页使用 Inline Title
- 工具栏放次要操作
- 底部 Tab 放主要目的地
- Sheet 放临时任务
- Context Menu 放对象操作
- 搜索集成到导航区域

避免：

- 与系统返回冲突的自定义返回按钮
- 嵌套多个 Tab Bar
- 普通应用使用长期固定汉堡菜单
- 网页式面包屑
- 把重要功能只放在长按里
- 不必要地伪造导航栏

## 4.2 Large Title

适合使用 Large Title：

- 顶级页面
- 内容可以滚动
- 页面需要明显定位感

适合使用 Inline Title：

- 详情页
- 任务流程页
- 空间有限
- 页面内容已经包含大标题

不要每个页面都使用 Large Title。

## 4.3 Tab Bar

Tab Bar 用于稳定的一级入口。

规则：

- 通常使用 3 到 5 个 Tab
- 标签保持简短
- 图标风格统一
- 保留各 Tab 的导航状态
- 当前选中状态清晰
- 不要把临时操作放进 Tab
- 不要随意隐藏 Tab Bar
- 不要把删除等危险操作放进 Tab

## 4.4 Sheet

适合使用 Sheet：

- 创建内容
- 编辑内容
- 筛选
- 分享
- 选择选项
- 短流程
- 账号操作

Sheet 必须：

- 有清晰标题或上下文
- 安全时允许下拉关闭
- 防止误丢失未保存内容
- 正确处理键盘
- 尊重 Safe Area
- 必要时提供取消或关闭
- 合理使用 Detent

避免嵌套多个 Sheet。

## 4.5 Context Menu

适合放：

- 重命名
- 分享
- 复制
- 归档
- 删除
- 预览

不要把唯一的重要入口藏在 Context Menu 中。

## 4.6 列表滑动操作

适合：

- 删除
- 归档
- 标记已读
- 置顶
- 收藏

规则：

- 操作数量少
- 危险操作明显
- 易误触删除应提供撤销
- 关键功能不能只依赖滑动手势

---

# 5. iPhone 与 iPad 布局

## 5.1 布局基础

优先使用：

- Flexbox
- `gap`
- 弹性宽度
- `minWidth`
- `maxWidth`
- `aspectRatio`
- `useWindowDimensions`
- Safe Area Insets
- 内容最大宽度
- 类 Size Class 的布局判断
- iPad Split View

避免：

- 用绝对定位搭建主布局
- 固定屏幕宽度
- 写死状态栏高度
- 写死 Home Indicator 高度
- 模块初始化时只读取一次 `Dimensions.get()`
- 按设备型号判断布局
- 所有像素等比例缩放
- 把 iPad 当成放大的 iPhone

## 5.2 宽度分类

只有在布局结构需要变化时，才使用宽度分类。

建议：

```ts
compact: width < 600
regular: width >= 600 && width < 900
expanded: width >= 900
```

对应策略：

- compact：
  - 单列
- regular：
  - 更宽内容
  - 更大边距
- expanded：
  - Sidebar
  - Split View
  - Master Detail

## 5.3 iPad 规则

iPad 上：

- 主从结构优先使用 Split View
- 支持 Stage Manager 动态尺寸
- 控制正文最大宽度
- 表单居中并限制宽度
- 大型信息架构可用 Sidebar
- 必要时支持硬件键盘
- iPad 上优先考虑 Popover，而不是手机式 Bottom Sheet
- Popover 必须有正确锚点
- 交互需要时支持 Pointer Hover

## 5.4 横屏

只有产品确实需要时才支持横屏。

横屏时：

- 重新组合布局
- 不要只是旋转竖屏界面
- 检查危险区域
- 检查 Sheet
- 检查键盘
- 检查编辑器
- 检查媒体页面
- 检查仪表盘

---

# 6. Safe Area 与系统界面

优先使用项目现有 Safe Area 方案。

大多数项目推荐：

```text
react-native-safe-area-context
```

不要给所有容器盲目加 Safe Area Padding。

先确认是否已经由以下元素处理：

- Native Stack Header
- Bottom Tabs
- Sheet
- Scroll Inset
- 全屏媒体容器
- Keyboard Accessory
- 自定义 Toolbar

沉浸式页面需要：

- 设置正确的 Status Bar 样式
- 保证控件可点击
- 保证文字和图标对比度
- 离开页面时恢复系统界面
- 处理 Home Indicator
- 危险操作不要贴近底部边缘

---

# 7. 键盘和表单

表单必须在键盘弹出时仍然可用。

需要处理：

- Keyboard Avoidance
- 自动滚动到当前输入框
- Return Key 顺序
- 键盘关闭
- 多行输入
- 底部操作栏
- Input Accessory View
- 密码输入
- Autofill
- 验证码自动填充
- 密码管理器
- 硬件键盘焦点
- Sheet 中的键盘和 Detent

主操作不能被键盘遮住。

推荐：

- 中间输入框使用 `next`
- 最后一个输入框使用：
  - `done`
  - `go`
  - `search`
  - `send`
- 合适时点击背景关闭键盘
- 请求失败后保留用户输入
- 错误提示靠近对应字段
- 输入法仍在组合文本时不要误提交

---

# 8. 设计令牌

使用语义命名，而不是直接使用视觉名称。

## 8.1 颜色

建议：

```ts
export const colors = {
  background: {
    primary: "...",
    secondary: "...",
    grouped: "...",
    elevated: "...",
  },
  text: {
    primary: "...",
    secondary: "...",
    tertiary: "...",
    quaternary: "...",
    inverse: "...",
    link: "...",
  },
  separator: {
    opaque: "...",
    translucent: "...",
  },
  fill: {
    primary: "...",
    secondary: "...",
    tertiary: "...",
    quaternary: "...",
  },
  action: {
    primary: "...",
    pressed: "...",
    disabled: "...",
  },
  feedback: {
    success: "...",
    warning: "...",
    danger: "...",
    info: "...",
  },
};
```

组件中不要直接使用 `gray500` 之类的基础色名称。

深色模式不能只做颜色反转。

需要重新检查：

- 背景层级
- Separator 可见度
- Elevated Surface
- Disabled 状态
- 图片显示
- 危险色
- Overlay
- 键盘外观
- Status Bar
- Tab Bar

## 8.2 间距

建议使用统一比例：

```ts
export const spacing = {
  0: 0,
  1: 4,
  2: 8,
  3: 12,
  4: 16,
  5: 20,
  6: 24,
  8: 32,
  10: 40,
  12: 48,
};
```

不要每个组件使用不同的随机间距。

## 8.3 圆角

建议：

```ts
export const radius = {
  sm: 8,
  md: 12,
  lg: 16,
  xl: 24,
  full: 999,
};
```

胶囊形仅用于：

- 标签
- Capsule
- Segmented Control
- 状态提示
- 圆形图标按钮

## 8.4 字体

定义语义层级：

```ts
export const typography = {
  largeTitle: {},
  title1: {},
  title2: {},
  title3: {},
  headline: {},
  body: {},
  callout: {},
  subheadline: {},
  footnote: {},
  caption1: {},
  caption2: {},
  button: {},
  mono: {},
};
```

每种字体角色包含：

- 字体
- 字号
- 行高
- 字重
- 必要时的字距
- 缩放行为

默认优先使用 iOS 系统字体。

只有品牌要求明确时才使用自定义字体。

中文界面：

- 行高要足够
- 不要增加过多字距
- 避免超细字体
- 检查中英文混排
- 检查中文和数字混排
- 避免重要文字被截断
- 检查 Dynamic Type 下的中文布局

数字表格可使用等宽数字。

---

# 9. 组件规范

## 9.1 按钮

至少支持：

- Primary
- Secondary
- Tertiary
- Destructive
- Icon Only
- Loading
- Disabled

按钮必须包含：

- 默认状态
- 按下状态
- 焦点状态
- 禁用状态
- 加载状态
- 无障碍名称
- 足够点击面积
- 必要时的触觉反馈

建议最小点击区域：

```text
44 × 44 pt
```

图标看起来可以很小，但实际点击区域不能过小。

## 9.2 输入框

输入框应支持：

- Label
- Placeholder
- Value
- Helper Text
- Error Text
- Disabled
- Read Only
- Clear
- Prefix
- Suffix
- Secure Entry
- Keyboard Type
- Auto Capitalization
- Text Content Type
- Autocomplete
- Return Key

重要表单不要只使用 Placeholder 充当 Label。

## 9.3 列表项

推荐结构：

```text
前置内容
标题
说明
元信息
尾部操作或 Disclosure
```

不要同时放：

- Disclosure 箭头
- “查看”按钮

这会造成重复。

设置类页面才使用 Settings 风格分组列表。

## 9.4 卡片

适用于：

- 内容分组
- 可点击对象
- Elevated Content
- 独立预览

不要让每一段内容都变成卡片。

## 9.5 Modal 和 Sheet

优先使用导航系统管理。

必须：

- 上下文明确
- 有关闭路径
- 保护未保存内容
- 处理键盘
- 处理 Safe Area
- 危险操作明显
- 避免嵌套
- 支持 Reduce Motion

## 9.6 空状态

空状态要说明：

1. 当前缺少什么
2. 为什么有影响
3. 用户可以做什么

不要只放插画。

## 9.7 错误状态

字段错误靠近字段。

请求错误需要：

- 保留输入
- 说明错误
- 可重试时提供重试
- 不显示原始服务器错误
- 区分：
  - 离线
  - 超时
  - 登录失效
  - 无权限
  - 服务异常
- 权限被拒绝时提供“打开设置”

## 9.8 Segmented Control

适合少量同级视图切换。

不适合：

- 很长的文字
- 太多选项
- 层级导航
- 操作按钮
- 多选筛选

## 9.9 Picker 和 Menu

使用建议：

- Menu：
  - 紧凑操作选择
- Wheel：
  - 熟悉且高效的连续值
- Date Picker：
  - 日期和时间
- Inline Picker：
  - 需要保留表单上下文
- Popover：
  - iPad 上的轻量选择

不要为一个简单选项强制打开全屏页面。

---

# 10. 导航实现

## 10.1 优先使用现有方案

现有项目优先沿用已有导航。

新 Expo 项目可以优先考虑 Expo Router。

裸 React Native 项目沿用已有 React Navigation 或明确选定的导航方案。

## 10.2 导航结构

优先：

- Native Stack
- Bottom Tabs
- Sheet
- Full Screen Modal
- iPad Sidebar

避免过深嵌套。

## 10.3 Route 文件职责

Route 文件应保持轻量。

主要负责：

- 读取参数
- 配置导航
- 组合 Feature
- 连接页面数据
- 渲染状态

不要把大量业务逻辑和可复用组件全部放在 Route 目录中。

## 10.4 返回行为

检查：

- 左缘滑动返回
- 返回按钮标题
- Modal 关闭
- 未保存内容保护
- Deep Link
- Tab 历史
- Nested Navigator
- 导航状态恢复

不要无理由拦截原生返回行为。

---

# 11. 图片、图标和媒体

## 11.1 图片

使用项目已有图片方案。

Expo 项目可优先使用：

```text
expo-image
```

裸 React Native 使用：

```text
Image
```

或项目已有图片组件。

规则：

- 保持原始比例
- 禁止横向或纵向拉伸
- 只有允许裁剪时使用 `cover`
- 必须完整显示时使用 `contain`
- 响应式图片使用 `aspectRatio`
- 不要随意裁剪：
  - 人脸
  - 文字
  - 图表
  - 二维码
  - 产品细节
- Placeholder 不应造成布局跳动
- 提前预留图片空间
- 重要图片提供无障碍描述
- 支持 Retina 清晰度
- 缩略图不要解码超大原图

编辑或替换已有图片时，默认保持原比例。

## 11.2 图标

项目支持时优先使用 SF Symbols。

否则保持现有图标库一致。

规则：

- 不混用不同风格图标
- 不使用 Emoji 充当功能图标
- Filled 和 Outline 有统一规则
- Icon Button 必须有 Accessibility Label
- 导航和系统操作优先使用熟悉图标
- 不要使用视觉上不匹配的仿 SF Symbol 图标
- 检查 SF Symbol 是否支持最低 iOS 版本

## 11.3 视频和音频

需要处理：

- 加载
- 缓冲
- 播放失败
- 来电或中断
- 音频路由变化
- AirPlay
- 后台
- 静音
- 字幕
- 横竖屏
- Now Playing
- 锁屏控制
- Picture in Picture
- 权限

---

# 12. 动画和反馈

动画用于解释状态变化，不用于堆装饰。

适合：

- 页面切换
- 列表插入删除
- 展开收起
- 状态切换
- 拖拽
- 成功反馈
- 加载连续性
- 必要的匹配转场

建议时长：

```text
按压反馈：80–150ms
小状态切换：150–250ms
较大布局变化：250–400ms
```

这些是起点，不是绝对值。

规则：

- 支持 Reduce Motion
- 不要为装饰延迟导航
- 不要大面积动画列表和图片
- 动画曲线统一
- 常规动画优先使用 Opacity 和 Transform
- 不要为了一个淡入引入大型动画库
- 页面失焦后停止无意义动画
- 避免持续运动影响阅读

触觉反馈适合：

- 创建成功
- 选择变化
- 拖拽到阈值
- 危险确认
- 重要状态切换

不要每次点击都震动。

---

# 13. 无障碍

无障碍不是最后才检查的附加项。

每个交互控件必须有：

- Role
- Label
- Value 或 State
- 正确焦点顺序
- 足够点击区域
- 明确按压、选中、禁用和错误状态

示例：

```tsx
<Pressable
  accessibilityRole="button"
  accessibilityLabel="保存更改"
  accessibilityState={{
    disabled: isDisabled,
    busy: isSaving,
  }}
  disabled={isDisabled}
  onPress={handleSave}
>
  <Text>保存更改</Text>
</Pressable>
```

必须检查：

- VoiceOver
- Dynamic Type
- Bold Text
- Reduce Motion
- Increase Contrast
- Differentiate Without Color
- Button Shapes
- Full Keyboard Access
- Switch Control
- 阅读顺序
- Modal 焦点
- 异步状态播报

不要只用颜色表达成功、警告和错误。

避免：

```tsx
allowFontScaling={false}
```

除非是极少数图形标签，且含义在其他地方可以访问。

## 13.1 Dynamic Type

大字体下布局必须可用。

规则：

- 文本容器不要写死高度
- 重要文案允许换行
- 主操作不能被截断
- 大字体下点击区域仍可用
- 测试无障碍字号
- 水平布局必要时改为纵向
- 保持页面可滚动
- Modal 内容可访问

## 13.2 VoiceOver

检查：

- 装饰元素隐藏
- 必要时合理组合元素
- 标题有 Heading 语义
- 图片描述强调含义
- Icon Button 有名称
- 加载、错误和成功状态可播报
- Modal 打开关闭后焦点正确
- 重要滑动操作有可发现的替代方式

---

# 14. 多语言与 RTL

为文字扩展预留空间。

规则：

- 不写死文字容器宽度
- 不把文字放进位图
- 支持多行
- 使用 Start 和 End
- 需要时镜像方向性图标
- 不随意镜像通用媒体图标
- 日期、数字、货币、复数使用本地化工具
- 至少测试：
  - 中文
  - 英文
  - 一个长单词语言
- 不拼接翻译片段
- 不依赖英文缩写

中文界面还要检查：

- 标点换行
- 单字孤行
- 行高
- 中英文混排
- 简体和繁体
- 大写英文对中文视觉的影响

---

# 15. 权限和隐私

iOS 权限必须在用户需要功能时再请求。

请求前：

- 说明为什么需要
- 说明用户能获得什么
- 只在进入对应功能时请求
- 尽可能提供非阻塞路径

必须处理：

- Not Determined
- Granted
- Denied
- Restricted
- Limited

不要在用户拒绝后反复弹窗。

需要用户去系统设置时，提供：

```text
打开设置
```

根据功能配置对应隐私说明：

- 相机
- 照片
- 麦克风
- 语音识别
- 定位
- 联系人
- 日历
- 提醒事项
- 蓝牙
- 本地网络
- 运动
- Face ID

如果某个功能没有权限就无法工作，不要把权限描述成可选。

---

# 16. 登录与安全操作

登录和敏感操作需要：

- 密码管理器 Autofill
- 正确的 Text Content Type
- 验证码自动填充
- 请求失败后保留表单
- Face ID 或 Touch ID 只用于合适的重新验证
- 说明为什么需要生物识别
- 尽可能提供替代方式
- 隐藏敏感数据
- 不在日志输出密钥
- 高风险操作需要合理确认

当产品或审核规则需要时，支持 Sign in with Apple。

---

# 17. 性能设计

界面必须在真实 iPhone 和 iPad 上流畅。

## 17.1 列表

- 长列表使用 `FlatList` 或 `SectionList`
- 条目高度尽量稳定
- 不渲染大量隐藏内容
- 避免同方向嵌套滚动
- 大数据分页
- Key 保持稳定
- 只有确有需要时使用优化列表库
- 在较老支持设备上测试

## 17.2 图片

- 请求合适尺寸
- 缩略图不解码完整大图
- 使用缓存
- 只预取高价值资源
- Placeholder 保持布局
- 滚动区域避免大量 Blur

## 17.3 渲染

- 高频状态不要全部放在页面根节点
- 大页面拆成专注组件
- 只有测量确认后才 Memo
- 列表 Render Item 中避免创建昂贵对象
- 避免大面积透明和阴影
- 避免持续测量循环

## 17.4 动画

- 使用项目已有动画方案
- 不同时动画大量列表项
- 页面不可见后停止动画
- 必要时检查 60Hz 和高刷新率设备
- 没有 Profile 不要声称已优化

---

# 18. 项目结构

优先沿用已有结构。

示例：

```text
src/
├── components/
│   ├── ui/
│   └── shared/
├── features/
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── screens/
│   │   ├── services/
│   │   └── types.ts
│   └── home/
├── navigation/
├── theme/
│   ├── colors.ts
│   ├── spacing.ts
│   ├── typography.ts
│   ├── radius.ts
│   └── index.ts
├── hooks/
├── services/
├── utils/
└── types/
```

Expo Router 示例：

```text
app/
├── _layout.tsx
├── (tabs)/
│   ├── _layout.tsx
│   ├── index.tsx
│   └── profile.tsx
└── item/
    └── [id].tsx

src/
├── features/
├── components/
├── theme/
└── services/
```

不要把所有组件、Hook、Service 和 Type 都放进 Route 目录。

只有完整行为确实需要 iOS 专用实现时，才使用 `.ios.tsx`。

本 Skill 只面向 iOS，不要创建空的 Android 文件。

---

# 19. TypeScript 和代码质量

优先使用严格 TypeScript。

规则：

- 避免 `any`
- Route 参数必须有类型
- 组件 Variant 必须有类型
- 复杂状态使用 Discriminated Union
- 必要时区分 API Model 和 View Model
- 页面组件职责单一
- 提取重复布局
- 删除未使用 Import
- 删除无效样式
- 清理生产环境 Console
- 修复 Warning，不要只隐藏
- 简单功能不要增加依赖
- Strict Mode 未开启时，不要声称类型安全

示例：

```ts
type ButtonVariant =
  | "primary"
  | "secondary"
  | "tertiary"
  | "destructive";

type ButtonProps = {
  label: string;
  variant?: ButtonVariant;
  loading?: boolean;
  disabled?: boolean;
  onPress: () => void;
};
```

---

# 20. 样式策略

沿用项目当前样式系统。

## 使用 StyleSheet 时

- 令牌集中管理
- 静态样式使用 `StyleSheet.create`
- 条件样式使用数组
- 动态值保持局部
- 避免过深 Style Factory

## 使用内联样式时

- 语义令牌放外部
- 重复对象抽离
- 避免超长 JSX Style
- 不混用原始值和主题值

## 使用 NativeWind 时

- 遵循当前安装版本
- Class Name 保持可读
- 使用主题令牌
- 原生行为使用 RN Style
- 不假设所有 Web Tailwind 能在 RN 工作

## 使用 UI 库时

- 优先扩展 Theme
- 只有重复产品行为才封装
- 不要用大量 Override 对抗组件库
- 不要混用多个设计系统
- 保证最终表现符合 iOS

---

# 21. 状态和数据

状态要明确。

建议：

```ts
type ScreenState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "empty" }
  | { status: "error"; message: string; retryable: boolean };
```

Mutation 需要：

- 立即反馈
- 防止重复提交
- 失败保留输入
- 显示进度
- 只有可安全回滚时使用乐观更新
- 危险操作合理确认
- 成功后不无意义阻塞
- VoiceOver 播报重要变化
- 可逆删除提供撤销

---

# 22. 原生模块边界

只有 React Native 或 Expo 无法可靠实现时才加入原生 iOS 代码。

加入 Swift 或 Objective-C 前确认：

1. 当前能力无法通过现有 API 完成
2. 项目是否已有原生模块模式
3. 最低 iOS 版本
4. JS 与 Native 接口
5. 错误行为
6. 权限
7. 生命周期
8. 测试方案

原生模块保持窄范围。

不要为了“看起来更原生”把普通 UI 布局移到 Swift。

适合原生模块的能力：

- 高级后台任务
- 特殊系统集成
- Expo 或 RN 未暴露的 API
- 高性能媒体处理
- App Intents
- Widgets
- Live Activities
- Share Extension
- Notification Service Extension

当能力不能完全在 React Native 中实现时，要明确说明。

---

# 23. iOS 发布前检查

发布前检查：

- App Icon
- 启动体验
- Status Bar
- 深色模式
- 权限弹窗
- Privacy Usage Description
- Loading
- Empty
- Error
- Offline
- VoiceOver
- Dynamic Type
- Reduce Motion
- 键盘
- 小尺寸 iPhone
- 大尺寸 iPhone
- iPad
- 横竖屏
- Safe Area
- StoreKit 状态
- 登录失败
- Deep Link
- Push Permission
- 前后台切换
- 页面恢复
- 截图不泄露隐私
- 移除 Debug Menu
- 移除占位文案和素材
- 不使用假系统弹窗

没有实际检查项目配置时，不要声称已经满足 App Store 上架要求。

---

# 24. 设计审查评分

每项 0 到 2 分：

```text
0 = 缺失或较差
1 = 可用但不一致
2 = 完整且质量较高
```

评分项：

1. 产品层级
2. 导航清晰度
3. iOS 原生适配
4. 视觉一致性
5. 字体
6. 间距和对齐
7. 组件状态
8. 无障碍
9. iPhone 和 iPad 适配
10. 性能可行性

结果：

```text
17–20：可进入实现或发布审查
13–16：整体可靠，需要局部修改
9–12：不一致，需要重新设计
0–8：产品结构或流程尚未成熟
```

审查现有界面时，优先指出影响最大的三个问题。

---

# 25. 必须使用的交付格式

当用户要求设计或实现 iOS React Native 应用时，按以下顺序输出。

## A. 假设

简要说明：

- Expo 或裸 React Native
- 导航方案
- 最低 iOS 版本
- 是否支持 iPad
- 关键产品推断

## B. 产品结构

提供：

- 页面地图
- 核心用户流程

## C. 设计方向

提供：

- 三个设计形容词
- 颜色策略
- 字体策略
- 间距和密度策略
- iPhone 和 iPad 适配策略
- 导航和弹层策略

## D. 组件清单

列出：

- 可复用组件
- 组件状态
- 页面状态

## E. 实现

输出或修改可运行的 React Native 文件。

用户要求工作代码时，不要只给伪代码。

## F. 验证

只报告真正完成的检查：

- Type Check
- Lint
- iOS Build
- Bundle
- 导航
- Swipe Back
- 深色模式
- 小尺寸 iPhone
- 大尺寸 iPhone
- iPad
- 键盘
- Loading
- Empty
- Error
- Offline
- Permission
- VoiceOver
- Dynamic Type
- Reduce Motion
- 图片比例
- Safe Area

## G. 剩余风险

只写真实存在的问题。

---

# 26. 完成标准

React Native iOS 设计任务只有满足以下条件才算完成：

- 核心流程清晰
- 导航符合 iOS 习惯
- 滑动返回正常
- 设计令牌统一
- 组件一致
- Loading、Empty、Error、Disabled、Permission、Success 状态完整
- Safe Area 正确
- 键盘正确
- 深色模式可用
- Dynamic Type 不破坏布局
- 点击区域足够
- 非文本控件有 VoiceOver Label
- 图片保持正确比例
- 长列表虚拟化
- iPhone 布局完成
- 范围内的 iPad 布局完成
- 没有明显 Warning
- 没有断开的路由
- 没有无效操作
- 界面明显是为 iOS 设计
- 没有粗糙模仿系统界面

---

# 27. 禁止事项清单

不要：

- 加入 Android 设计规范
- 加入 Material Design
- 创建 Android 平台分支
- 只适配单一 iPhone 尺寸
- 拉伸图片
- 使用 Emoji 作为功能图标
- 每个区域都使用卡片
- 用渐变替代层级
- 仅使用透明度表达禁用
- 把关键功能只藏在手势中
- 让键盘挡住主操作
- 无理由替换原生导航
- 每个页面重新发明设计系统
- 在组件中大量使用原始 Hex
- 无替代方案地截断重要内容
- 忽略 Swipe Back
- 忽略深色模式
- 忽略 Dynamic Type
- 忽略 VoiceOver
- 忽略 Reduce Motion
- 忽略 Loading、Empty、Error 和 Permission
- 删除无障碍标签来消除警告
- 伪造 iOS 系统弹窗
- 使用最低系统不支持的 SF Symbol
- 不说明风险就引入实验 API
- 在 UI 任务中重构无关业务
- 声称完成了并未实际执行的验证

---

# 28. 参考资料

当 API 或平台行为可能发生变化时，优先查阅官方资料：

- React Native 官方文档
- React Native Accessibility 文档
- Expo 官方文档
- Expo Router 官方文档
- React Navigation 官方文档
- Apple Human Interface Guidelines
- Apple Accessibility 文档
- Apple SF Symbols 文档
- App Store Review Guidelines
- Apple Privacy 文档
- UIKit 文档
- SwiftUI 文档
