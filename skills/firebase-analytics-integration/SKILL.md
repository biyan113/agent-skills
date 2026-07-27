---
name: firebase-analytics-integration
version: 1.0.0
description: Firebase Analytics 跨平台接入 Skill。覆盖 Flutter、React Native、Expo、iOS、Android、Web、Unity、C++ 等多端的 SDK 接入、原生配置、统一抽象层封装、事件字典设计、用户属性、内购订阅埋点、隐私合规与 ATT、Consent 同意流程、DebugView 验证和上线验收清单。当用户要求接入 Firebase Analytics、增加应用统计、设计埋点、统计用户行为、统一多端 Analytics 时使用。
license: MIT
---

# Firebase Analytics 跨平台接入 Skill

## 1. 使用目标

当用户要求“接入 Firebase Analytics”“增加应用统计”“设计埋点”“统计用户行为”“统一多端 Analytics”时，执行本 Skill。

最终交付必须包含：

- 已识别的平台、框架、包管理器和路由方案。
- Firebase 项目与各平台 App 注册清单。
- 当前平台所需依赖和原生配置。
- 一个业务无关的 Analytics 抽象层。
- 一份统一事件字典。
- 页面访问、登录、核心功能、付费漏斗埋点。
- 用户同意与关闭统计能力。
- DebugView 或本地日志验证结果。
- 上线前验收清单。
- 实际修改文件列表和仍需用户在控制台完成的步骤。

不要只安装 SDK。必须保证至少一个测试事件能在 DebugView 中被验证。

## 2. 支持范围

优先支持：

- React Native CLI
- React Native + Expo Development Build / EAS Build
- Flutter iOS / Android
- iOS / iPadOS 原生：SwiftUI、UIKit、Swift、Objective-C
- Android 原生：Kotlin、Java
- Web：JavaScript、TypeScript、React、Vue、Next.js、Nuxt
- Unity：C#
- Firebase C++ SDK

条件支持：

- React Native Web：Web 端使用 Firebase JavaScript SDK，iOS/Android 使用 React Native Firebase。
- Flutter Web：先确认当前 firebase_analytics 插件和项目目标支持情况。
- Electron：渲染进程可按 Web SDK 接入，但必须处理 SSR、CSP 和多窗口重复初始化。
- macOS / Catalyst：按 Firebase Apple platforms SDK 的当前支持情况接入并单独验证。
- Tauri、Wails、Windows、Linux 原生桌面：不要假定存在官方 Firebase Analytics 原生 SDK。可在 WebView 前端使用 Web SDK，或改用 Measurement Protocol、PostHog、Sentry、自建统计。

不支持或不应强行接入：

- Expo Go 中的原生 Firebase Analytics。
- watchOS。
- 服务端直接伪装成移动端 SDK 事件。
- 将 Crashlytics 当作 Analytics，或将 Analytics 当作崩溃监控。
- 未取得授权时收集敏感数据、广告标识符或跨应用追踪数据。

## 3. 执行原则

### 3.1 先检查项目，不盲改

执行前检查：

```bash
find . -maxdepth 3 \( \
  -name "pubspec.yaml" -o \
  -name "package.json" -o \
  -name "*.xcodeproj" -o \
  -name "*.xcworkspace" -o \
  -name "Podfile" -o \
  -name "build.gradle" -o \
  -name "build.gradle.kts" -o \
  -name "app.json" -o \
  -name "app.config.*" -o \
  -name "ProjectSettings" \
\) -print
```

识别以下信息：

- 应用名称
- Bundle ID / Application ID
- iOS targets
- Android product flavors / build types
- React Native CLI 或 Expo
- FlutterFire 是否已配置
- CocoaPods 或 Swift Package Manager
- Gradle Groovy 或 Kotlin DSL
- 路由框架
- 登录系统
- 内购系统
- 是否存在隐私同意页
- 是否已有其他 Analytics SDK
- 是否已有 GoogleService-Info.plist
- 是否已有 google-services.json
- 是否区分 dev / staging / production

如果用户没有提供 Firebase 控制台访问能力，不阻塞代码改造；生成精确的控制台操作清单和配置文件放置位置。

### 3.2 不固定过时版本

不得凭记忆写死 Firebase SDK、BoM、CocoaPods、Gradle 插件或 npm 包版本。

执行时：

- 优先使用包管理器安装当前兼容版本。
- Android 优先使用 Firebase BoM。
- Apple 原生优先使用 Swift Package Manager。
- Flutter 优先使用 FlutterFire CLI。
- React Native 优先使用 React Native Firebase 模块化 API。
- 若项目已有锁定版本，先检查兼容性，再升级。
- 不擅自覆盖用户现有 Firebase 原生 SDK 版本。

### 3.3 业务代码不得直接依赖 Firebase

业务页面只能调用统一抽象层：

```text
UI / ViewModel / Controller
          |
          v
AnalyticsClient
          |
          v
FirebaseAnalyticsAdapter
```

这样后续可以替换或并行接入：

- PostHog
- Amplitude
- Mixpanel
- 自建 Analytics
- 测试环境 No-op 实现

## 4. 平台决策表

| 项目类型 | 推荐方案 | 关键限制 |
|---|---|---|
| Flutter iOS/Android | firebase_core + firebase_analytics + FlutterFire CLI | 重新运行 flutterfire configure |
| React Native CLI | @react-native-firebase/app + @react-native-firebase/analytics | 必须完成原生配置并重新编译 |
| Expo | React Native Firebase + Development Build | Expo Go 不可用 |
| iOS Swift/SwiftUI | Firebase Apple SDK + FirebaseAnalytics | 默认优先 SPM |
| iOS Objective-C | Firebase Apple SDK | 新文档以 Swift 为主，Objective-C API 仍可用 |
| Android Kotlin/Java | Firebase Android SDK + BoM | 应用模块启用 Google Services 插件 |
| Web | Firebase JS SDK 模块化 API | SSR 时只能在浏览器初始化 |
| React Native Web | Native 使用 RNFirebase，Web 使用 JS SDK | 不混用初始化对象 |
| Unity | Firebase Unity SDK | 导入 FirebaseAnalytics.unitypackage |
| C++ | Firebase C++ SDK | 仅使用当前官方支持目标 |
| Electron | Web SDK | 防止多窗口重复初始化 |
| Tauri/Wails/桌面原生 | Web SDK 或其他统计方案 | 不假定存在官方原生 Analytics SDK |

## 5. Firebase 控制台准备

### 5.1 创建或选择项目

确认：

- 已启用 Google Analytics。
- 使用正确的 Google Analytics 账号和数据区域。
- dev、staging、production 是否需要隔离。

推荐隔离策略：

```text
小型独立应用
一个 Firebase Project
├── iOS production
├── Android production
└── Web production

有测试环境的正式产品
your-app-dev
your-app-staging
your-app-production
```

不要把生产数据和开发测试数据长期混在同一数据流中。

### 5.2 注册平台应用

iOS：

- Bundle ID 必须与 Xcode target 完全一致，区分大小写。
- 下载 GoogleService-Info.plist。
- 多 target / 多环境分别注册 Firebase App。

Android：

- Package name / Application ID 必须与最终构建变体一致。
- 下载 google-services.json。
- 有 flavor 时按 src/<flavor>/google-services.json 管理。

Web：

- 注册 Web App。
- 保存 Firebase config。
- 启用 Analytics 后确认 Web data stream 已创建。

Unity / C++：

- 分别注册 iOS 和 Android App。
- 使用官方 SDK 要求的配置文件和目录。

## 6. 统一事件规范

### 6.1 命名

事件名和参数名统一使用英文 snake_case：

```text
onboarding_started
onboarding_completed
feature_used
export_started
export_completed
subscription_started
purchase_completed
```

禁止：

```text
OnboardingStarted
onboarding-started
用户点击翻译
button_1
test_event_final_2
```

事件名称必须表达业务动作，而不是 UI 实现。

推荐：

```text
translation_started
```

不推荐：

```text
translate_button_clicked
```

按钮位置放参数：

```json
{
  "entry_point": "home_toolbar"
}
```

### 6.2 平台限制

设计事件时遵守当前 GA4 App 数据限制：

- 事件名最多 40 个字符。
- 每个事件最多 25 个参数。
- 参数名最多 40 个字符。
- 普通参数值最多 100 个字符。
- 每个项目最多 25 个用户属性。
- 用户属性名最多 24 个字符。
- 用户属性值最多 36 个字符。
- 每个 App 用户最多处理 500 个不同事件名称。
- 事件名大小写敏感。
- 不创建动态事件名。

错误：

```text
document_opened_123456
screen_view_user_987
purchase_product_com.example.pro.monthly
```

正确：

```text
document_opened { document_type: "pdf" }
screen_view { screen_name: "editor" }
purchase { item_id: "pro_monthly" }
```

### 6.3 优先使用推荐事件

能使用 Firebase / GA4 推荐事件时，不创建同义自定义事件：

| 场景 | 推荐事件 |
|---|---|
| 登录 | login |
| 注册 | sign_up |
| 搜索 | search |
| 分享 | share |
| 选择内容 | select_content / select_item |
| 开始结账 | begin_checkout |
| 购买 | purchase |
| 查看商品 | view_item |
| 教程开始 | tutorial_begin |
| 教程完成 | tutorial_complete |
| 页面访问 | screen_view |

只有推荐事件无法表达业务含义时才增加自定义事件。

### 6.4 禁止采集的数据

任何事件、参数、用户属性中不得发送：

- 姓名
- 邮箱
- 电话号码
- 身份证件
- 精确地址
- 精确经纬度
- 用户输入的原文
- 剪贴板内容
- 文件名和本地绝对路径
- 文档正文
- 图片、音频、视频内容
- 搜索原词，除非已确认不包含敏感内容且业务确有需要
- 密码、Token、API Key、Cookie
- 广告标识符之外的自建设备指纹
- 健康、宗教、政治、性取向等敏感属性
- 可直接识别自然人的 User ID

setUserId 只能使用内部生成、不可反查个人身份的稳定 ID。退出登录时必须清空。

### 6.5 参数设计

参数应低基数、可聚合：

```json
{
  "feature_name": "image_export",
  "entry_point": "editor_toolbar",
  "result": "success",
  "plan_tier": "pro",
  "platform": "ios"
}
```

不要把错误堆栈、长文本或随机 UUID 作为 Analytics 参数。错误详情交给 Crashlytics 或 Sentry。

### 6.6 统一基础事件

每个应用至少规划：

```text
onboarding_started
onboarding_completed
screen_view
feature_used
export_started
export_completed
paywall_viewed
trial_started
purchase
subscription_restored
settings_changed
permission_result
```

示例参数：

```yaml
feature_used:
  feature_name: image_translate
  entry_point: editor
  result: success

export_completed:
  export_type: image
  format: png
  duration_bucket: 1_3s

paywall_viewed:
  entry_point: export_limit
  variant: default

permission_result:
  permission_type: photos
  result: granted
```

持续时长应使用区间，避免发送过细数据：

```text
lt_1s
1_3s
3_10s
10_30s
gt_30s
```

## 7. 统一抽象接口

不同语言实现相同语义：

```text
initialize()
setCollectionEnabled(enabled)
logEvent(name, parameters)
logScreen(screenName, screenClass?)
setUserId(userId?)
setUserProperty(name, value?)
resetUser()
```

实现要求：

- Release 默认发送。
- 单元测试使用 No-op 或 Fake 实现。
- Debug 构建可打印事件，但不要只打印而不发送测试事件。
- 参数为空时不发送 null。
- 对事件名、参数名和长度做本地校验。
- Analytics 调用失败不得中断核心业务。
- 不在高频动画、滚动、音频帧或每次键盘输入时埋点。
- 同一用户动作只记录一次。
- 防止页面重建造成重复 screen_view。

## 8. Flutter 接入

### 8.1 安装和配置

```bash
firebase login
dart pub global activate flutterfire_cli

flutter pub add firebase_core
flutter pub add firebase_analytics

flutterfire configure
```

`flutterfire configure` 应生成：

```text
lib/firebase_options.dart
```

新增平台、Firebase 产品、Bundle ID 或 Application ID 后重新运行：

```bash
flutterfire configure
```

### 8.2 初始化

```dart
import 'package:firebase_core/firebase_core.dart';
import 'package:flutter/widgets.dart';
import 'firebase_options.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();

  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );

  runApp(const App());
}
```

### 8.3 Analytics Service

```dart
import 'package:firebase_analytics/firebase_analytics.dart';

abstract interface class AnalyticsClient {
  Future<void> setCollectionEnabled(bool enabled);

  Future<void> logEvent(
    String name, {
    Map<String, Object>? parameters,
  });

  Future<void> logScreen(
    String screenName, {
    String? screenClass,
  });

  Future<void> setUserId(String? userId);

  Future<void> setUserProperty(String name, String? value);
}

final class FirebaseAnalyticsClient implements AnalyticsClient {
  FirebaseAnalyticsClient({
    FirebaseAnalytics? analytics,
  }) : _analytics = analytics ?? FirebaseAnalytics.instance;

  final FirebaseAnalytics _analytics;

  @override
  Future<void> setCollectionEnabled(bool enabled) {
    return _analytics.setAnalyticsCollectionEnabled(enabled);
  }

  @override
  Future<void> logEvent(
    String name, {
    Map<String, Object>? parameters,
  }) {
    return _analytics.logEvent(
      name: name,
      parameters: parameters,
    );
  }

  @override
  Future<void> logScreen(
    String screenName, {
    String? screenClass,
  }) {
    return _analytics.logScreenView(
      screenName: screenName,
      screenClass: screenClass,
    );
  }

  @override
  Future<void> setUserId(String? userId) {
    return _analytics.setUserId(id: userId);
  }

  @override
  Future<void> setUserProperty(
    String name,
    String? value,
  ) {
    return _analytics.setUserProperty(
      name: name,
      value: value,
    );
  }
}
```

### 8.4 页面追踪

Navigator 1.x：

```dart
import 'package:firebase_analytics/firebase_analytics.dart';

MaterialApp(
  navigatorObservers: [
    FirebaseAnalyticsObserver(
      analytics: FirebaseAnalytics.instance,
    ),
  ],
);
```

go_router、auto_route 或自定义路由：

- 优先挂接路由 observer。
- 无法稳定获取业务页面名时手动调用 logScreenView。
- 页面名使用稳定业务名，不使用运行时类名或动态 ID。
- 避免 Widget rebuild 重复发送。

### 8.5 登录和退出

```dart
await analytics.setUserId(internalUserId);
await analytics.setUserProperty('plan_tier', 'pro');

// 退出登录
await analytics.setUserId(null);
await analytics.setUserProperty('plan_tier', null);
```

### 8.6 事件示例

```dart
await analytics.logEvent(
  'export_completed',
  parameters: {
    'export_type': 'image',
    'format': 'png',
    'result': 'success',
  },
);
```

### 8.7 Flutter 验收

```bash
flutter analyze
flutter test
flutter run
```

iOS DebugView：

在 Xcode Scheme 的 Arguments Passed On Launch 添加 `-FIRDebugEnabled`。

Android DebugView：

```bash
adb shell setprop debug.firebase.analytics.app com.example.app
```

关闭：

```bash
adb shell setprop debug.firebase.analytics.app .none.
```

## 9. React Native CLI 接入

### 9.1 安装

```bash
npm install @react-native-firebase/app
npm install @react-native-firebase/analytics
```

或：

```bash
yarn add @react-native-firebase/app
yarn add @react-native-firebase/analytics
```

React Native 0.60+ 使用 autolinking，但仍然需要重新编译原生 App。

### 9.2 Android 配置

放置：

```text
android/app/google-services.json
```

确认 Google Services Gradle 插件已配置。优先遵循当前 React Native Firebase 文档和现有 Gradle DSL，不凭记忆覆盖项目配置。

重建：

```bash
npx react-native run-android
```

### 9.3 iOS 配置

使用 Xcode 将文件加入正确 target：

```text
ios/<ProjectName>/GoogleService-Info.plist
```

然后：

```bash
cd ios
pod install --repo-update
cd ..
npx react-native run-ios
```

不要只把 plist 复制进目录而不加入 Xcode target membership。

### 9.4 使用模块化 API

React Native Firebase 的 namespaced API 正在弃用。新代码使用模块化 API：

```ts
import {
  getAnalytics,
  logEvent,
  logScreenView,
  setAnalyticsCollectionEnabled,
  setUserId,
  setUserProperty,
} from '@react-native-firebase/analytics';

const analytics = getAnalytics();

export const analyticsClient = {
  setCollectionEnabled(enabled: boolean) {
    return setAnalyticsCollectionEnabled(analytics, enabled);
  },

  logEvent(
    name: string,
    parameters?: Record<string, string | number | boolean>,
  ) {
    return logEvent(analytics, name, parameters);
  },

  logScreen(screenName: string, screenClass?: string) {
    return logScreenView(analytics, {
      screen_name: screenName,
      screen_class: screenClass ?? screenName,
    });
  },

  setUserId(userId: string | null) {
    return setUserId(analytics, userId);
  },

  setUserProperty(name: string, value: string | null) {
    return setUserProperty(analytics, name, value);
  },
};
```

### 9.5 React Navigation 页面追踪

在 NavigationContainer 状态变化时发送：

```tsx
import { NavigationContainer } from '@react-navigation/native';
import { useRef } from 'react';
import { analyticsClient } from './analyticsClient';

export function AppNavigation() {
  const navigationRef = useRef<any>(null);
  const routeNameRef = useRef<string | undefined>(undefined);

  return (
    <NavigationContainer
      ref={navigationRef}
      onReady={() => {
        routeNameRef.current =
          navigationRef.current?.getCurrentRoute()?.name;
      }}
      onStateChange={async () => {
        const previous = routeNameRef.current;
        const current =
          navigationRef.current?.getCurrentRoute()?.name;

        if (current && current !== previous) {
          await analyticsClient.logScreen(current);
        }

        routeNameRef.current = current;
      }}
    >
      {/* navigators */}
    </NavigationContainer>
  );
}
```

将技术路由名映射为稳定业务名：

```ts
const SCREEN_NAMES: Record<string, string> = {
  HomeStack: 'home',
  EditorScreen: 'editor',
  SubscriptionModal: 'paywall',
};
```

## 10. Expo 接入

### 10.1 重要限制

原生 Firebase Analytics 依赖原生代码，因此：

- Expo Go 不可用。
- 必须使用 Expo Development Build、expo run:* 或 EAS Build。
- Firebase JS SDK 在 React Native 环境中不提供原生移动端 Analytics。
- 不得继续使用已废弃的 expo-firebase-analytics。

### 10.2 安装

```bash
npx expo install expo-dev-client
npx expo install @react-native-firebase/app
npx expo install @react-native-firebase/analytics
npx expo install expo-build-properties
```

### 10.3 app.json 示例

```json
{
  "expo": {
    "ios": {
      "bundleIdentifier": "com.example.app",
      "googleServicesFile": "./GoogleService-Info.plist"
    },
    "android": {
      "package": "com.example.app",
      "googleServicesFile": "./google-services.json"
    },
    "plugins": [
      "@react-native-firebase/app",
      [
        "@react-native-firebase/analytics",
        {
          "ios": {
            "withoutAdIdSupport": true
          }
        }
      ],
      [
        "expo-build-properties",
        {
          "ios": {
            "useFrameworks": "static",
            "forceStaticLinking": [
              "RNFBApp",
              "RNFBAnalytics"
            ]
          }
        }
      ]
    ]
  }
}
```

说明：

- `withoutAdIdSupport` 适合不使用 IDFA 的普通工具类 App。
- 使用 AdMob、广告归因或确实需要 IDFA 时，不能机械开启该选项，应重新评估 ATT。
- `forceStaticLinking` 是否必须取决于当前 React Native / Expo / RNFirebase 版本；执行时以当前官方文档和实际构建结果为准。

### 10.4 重新生成并运行

安装原生模块后必须重建：

```bash
npx expo prebuild --clean
npx expo run:ios
npx expo run:android
```

EAS：

```bash
eas build --profile development --platform ios
eas build --profile development --platform android
```

如果出现 RNFBAppModule not found：

- 卸载设备上的旧 Development Build。
- 执行 `npx expo prebuild --clean`。
- 重新安装构建产物。

## 11. iOS / iPadOS 原生接入

### 11.1 Swift Package Manager

Xcode：

```text
File
→ Add Package Dependencies
→ firebase-ios-sdk
→ FirebaseAnalytics
```

不在 Skill 中写死 SDK 版本，默认选择当前兼容版本。

将 GoogleService-Info.plist：

- 加入 Xcode 工程。
- 勾选正确 target membership。
- 确认文件名没有 (1)、(2) 后缀。

### 11.2 SwiftUI 初始化

```swift
import SwiftUI
import FirebaseCore

final class AppDelegate: NSObject, UIApplicationDelegate {
    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions:
            [UIApplication.LaunchOptionsKey: Any]? = nil
    ) -> Bool {
        FirebaseApp.configure()
        return true
    }
}

@main
struct ExampleApp: App {
    @UIApplicationDelegateAdaptor(AppDelegate.self)
    private var appDelegate

    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

### 11.3 UIKit 初始化

```swift
import UIKit
import FirebaseCore

@main
final class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions:
            [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
        FirebaseApp.configure()
        return true
    }
}
```

### 11.4 Swift Analytics Client

```swift
import FirebaseAnalytics

protocol AnalyticsClient {
    func setCollectionEnabled(_ enabled: Bool)
    func logEvent(_ name: String, parameters: [String: Any]?)
    func logScreen(name: String, screenClass: String?)
    func setUserID(_ userID: String?)
    func setUserProperty(name: String, value: String?)
}

final class FirebaseAnalyticsClient: AnalyticsClient {
    func setCollectionEnabled(_ enabled: Bool) {
        Analytics.setAnalyticsCollectionEnabled(enabled)
    }

    func logEvent(
        _ name: String,
        parameters: [String: Any]? = nil
    ) {
        Analytics.logEvent(name, parameters: parameters)
    }

    func logScreen(
        name: String,
        screenClass: String? = nil
    ) {
        Analytics.logEvent(
            AnalyticsEventScreenView,
            parameters: [
                AnalyticsParameterScreenName: name,
                AnalyticsParameterScreenClass:
                    screenClass ?? name
            ]
        )
    }

    func setUserID(_ userID: String?) {
        Analytics.setUserID(userID)
    }

    func setUserProperty(
        name: String,
        value: String?
    ) {
        Analytics.setUserProperty(
            value,
            forName: name
        )
    }
}
```

### 11.5 Objective-C

初始化：

```objc
@import FirebaseCore;

- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  [FIRApp configure];
  return YES;
}
```

事件：

```objc
@import FirebaseAnalytics;

[FIRAnalytics logEventWithName:@"export_completed"
                    parameters:@{
  @"export_type": @"image",
  @"format": @"png",
  @"result": @"success"
}];
```

用户：

```objc
[FIRAnalytics setUserID:internalUserID];
[FIRAnalytics setUserPropertyString:@"pro"
                            forName:@"plan_tier"];
```

### 11.6 不使用 IDFA 的配置

普通工具类 App 默认不应为了 Analytics 主动引入广告标识符。

原则：

- 不需要广告归因、AdMob 或再营销时，优先使用不包含 IDFA 收集能力的 Analytics 组件。
- Swift Package Manager 可按官方文档选择 FirebaseAnalyticsCore。
- CocoaPods 可按官方文档使用 FirebaseAnalytics/Core。
- React Native Firebase 可使用 withoutAdIdSupport。
- ATT 是否需要弹窗取决于整个 App 的数据使用方式，不只取决于是否安装 Firebase。
- 不要为了“审核安全”盲目弹 ATT；也不要在实际跨应用追踪时绕过 ATT。

### 11.7 默认关闭，用户同意后开启

Info.plist：

```xml
<key>FIREBASE_ANALYTICS_COLLECTION_ENABLED</key>
<false/>
```

用户同意后：

```swift
Analytics.setAnalyticsCollectionEnabled(true)
```

该设置会持久化。用户撤回同意时：

```swift
Analytics.setAnalyticsCollectionEnabled(false)
Analytics.setUserID(nil)
```

## 12. Android 原生接入

### 12.1 配置文件

放置：

```text
app/google-services.json
```

有 flavor：

```text
app/src/dev/google-services.json
app/src/staging/google-services.json
app/src/release/google-services.json
```

### 12.2 Gradle

优先使用 Firebase BoM，不在 Skill 中固定版本：

```groovy
dependencies {
    implementation(
        platform(
            "com.google.firebase:firebase-bom:<CURRENT_VERSION>"
        )
    )

    implementation(
        "com.google.firebase:firebase-analytics"
    )
}
```

项目还必须应用当前版本的：

```text
com.google.gms.google-services
```

执行时根据现有项目的 Gradle Groovy / Kotlin DSL 和 Android Gradle Plugin 版本生成正确写法。

### 12.3 Kotlin

```kotlin
import android.os.Bundle
import com.google.firebase.analytics.FirebaseAnalytics
import com.google.firebase.analytics.ktx.analytics
import com.google.firebase.ktx.Firebase

class AnalyticsClient {
    private val analytics: FirebaseAnalytics by lazy {
        Firebase.analytics
    }

    fun setCollectionEnabled(enabled: Boolean) {
        analytics.setAnalyticsCollectionEnabled(enabled)
    }

    fun logEvent(
        name: String,
        params: Bundle? = null
    ) {
        analytics.logEvent(name, params)
    }

    fun logScreen(
        screenName: String,
        screenClass: String = screenName
    ) {
        val params = Bundle().apply {
            putString(
                FirebaseAnalytics.Param.SCREEN_NAME,
                screenName
            )
            putString(
                FirebaseAnalytics.Param.SCREEN_CLASS,
                screenClass
            )
        }

        analytics.logEvent(
            FirebaseAnalytics.Event.SCREEN_VIEW,
            params
        )
    }

    fun setUserId(userId: String?) {
        analytics.setUserId(userId)
    }

    fun setUserProperty(
        name: String,
        value: String?
    ) {
        analytics.setUserProperty(name, value)
    }
}
```

事件：

```kotlin
val params = Bundle().apply {
    putString("export_type", "image")
    putString("format", "png")
    putString("result", "success")
}

analytics.logEvent("export_completed", params)
```

也可以使用 Kotlin KTX builder：

```kotlin
firebaseAnalytics.logEvent("export_completed") {
    param("export_type", "image")
    param("format", "png")
    param("result", "success")
}
```

### 12.4 Java

```java
FirebaseAnalytics analytics =
    FirebaseAnalytics.getInstance(context);

Bundle params = new Bundle();
params.putString("export_type", "image");
params.putString("format", "png");
params.putString("result", "success");

analytics.logEvent("export_completed", params);
```

用户：

```java
analytics.setUserId(internalUserId);
analytics.setUserProperty("plan_tier", "pro");
```

### 12.5 默认关闭

在 AndroidManifest.xml 的 `<application>` 内：

```xml
<meta-data
    android:name="firebase_analytics_collection_enabled"
    android:value="false" />
```

用户同意后：

```kotlin
Firebase.analytics
    .setAnalyticsCollectionEnabled(true)
```

### 12.6 Android 调试

DebugView：

```bash
adb shell setprop debug.firebase.analytics.app com.example.app
```

查看详细日志：

```bash
adb shell setprop log.tag.FA VERBOSE
adb shell setprop log.tag.FA-SVC VERBOSE
adb logcat -v time -s FA FA-SVC
```

关闭 Debug 模式：

```bash
adb shell setprop debug.firebase.analytics.app .none.
```

## 13. Web 接入

### 13.1 安装

```bash
npm install firebase
```

### 13.2 初始化

```ts
import { getApp, getApps, initializeApp } from 'firebase/app';
import {
  getAnalytics,
  isSupported,
  logEvent,
  setAnalyticsCollectionEnabled,
  setUserId,
  setUserProperties,
  type Analytics,
} from 'firebase/analytics';

const firebaseConfig = {
  apiKey: process.env.NEXT_PUBLIC_FIREBASE_API_KEY,
  authDomain: process.env.NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.NEXT_PUBLIC_FIREBASE_PROJECT_ID,
  storageBucket:
    process.env.NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET,
  messagingSenderId:
    process.env.NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.NEXT_PUBLIC_FIREBASE_APP_ID,
  measurementId:
    process.env.NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID,
};

const app =
  getApps().length > 0
    ? getApp()
    : initializeApp(firebaseConfig);

let analyticsPromise: Promise<Analytics | null> | null = null;

export function getBrowserAnalytics() {
  if (typeof window === 'undefined') {
    return Promise.resolve(null);
  }

  analyticsPromise ??= isSupported().then((supported) =>
    supported ? getAnalytics(app) : null,
  );

  return analyticsPromise;
}

export async function trackEvent(
  name: string,
  parameters?: Record<string, unknown>,
) {
  const analytics = await getBrowserAnalytics();

  if (!analytics) {
    return;
  }

  logEvent(analytics, name, parameters);
}
```

### 13.3 SSR 框架

Next.js / Nuxt / Remix：

- 只在浏览器初始化 Analytics。
- 不在 Server Component、SSR middleware 或 Node.js 服务端调用 getAnalytics。
- 防止 HMR 重复初始化。
- 页面追踪应监听路由完成事件。
- Cookie / consent 未同意前保持关闭。

### 13.4 Web 同意

```ts
const analytics = await getBrowserAnalytics();

if (analytics) {
  setAnalyticsCollectionEnabled(
    analytics,
    userAcceptedAnalytics,
  );
}
```

如项目需要 GDPR / EEA Consent Mode，应使用当前 Google Consent Mode API，而不是只依赖本地布尔值。

## 14. React Native Web 双实现

不要在 iOS / Android 使用 Firebase JS SDK 的 Web Analytics。

推荐目录：

```text
src/analytics/
├── analytics.native.ts
├── analytics.web.ts
├── analytics.types.ts
└── index.ts
```

Native：

```ts
// analytics.native.ts
export {
  analyticsClient
} from './rnfirebaseAnalyticsClient';
```

Web：

```ts
// analytics.web.ts
export {
  analyticsClient
} from './firebaseWebAnalyticsClient';
```

保持统一接口，分别使用：

- iOS / Android：`@react-native-firebase/analytics`
- Web：`firebase/analytics`

## 15. Unity 接入

### 15.1 安装

导入 Firebase Unity SDK：

```text
FirebaseAnalytics.unitypackage
```

同时完成 iOS 和 Android App 注册，并放置 Firebase 配置文件。

### 15.2 初始化依赖

```csharp
using Firebase;
using Firebase.Analytics;
using UnityEngine;

public sealed class FirebaseBootstrap : MonoBehaviour
{
    private async void Awake()
    {
        var status =
            await FirebaseApp.CheckAndFixDependenciesAsync();

        if (status != DependencyStatus.Available)
        {
            Debug.LogError(
                $"Firebase unavailable: {status}"
            );
            return;
        }

        FirebaseAnalytics.SetAnalyticsCollectionEnabled(
            true
        );
    }
}
```

### 15.3 事件

```csharp
FirebaseAnalytics.LogEvent(
    "level_completed",
    new Parameter("level_name", "forest_01"),
    new Parameter("result", "success")
);
```

用户：

```csharp
FirebaseAnalytics.SetUserId(internalUserId);
FirebaseAnalytics.SetUserProperty(
    "plan_tier",
    "pro"
);
```

## 16. C++ 接入

仅在 Firebase C++ SDK 当前支持的平台使用。

初始化：

```cpp
#include "firebase/app.h"
#include "firebase/analytics.h"

namespace analytics = ::firebase::analytics;

firebase::App* app = nullptr;

#if defined(__ANDROID__)
app = firebase::App::Create(
    GetJniEnv(),
    GetActivity()
);
#else
app = firebase::App::Create();
#endif

analytics::Initialize(*app);
analytics::SetAnalyticsCollectionEnabled(true);
```

事件：

```cpp
const analytics::Parameter params[] = {
    analytics::Parameter(
        "export_type",
        "image"
    ),
    analytics::Parameter(
        "format",
        "png"
    ),
    analytics::Parameter(
        "result",
        "success"
    ),
};

analytics::LogEvent(
    "export_completed",
    params,
    sizeof(params) / sizeof(params[0])
);
```

结束：

```cpp
analytics::Terminate();
delete app;
```

## 17. 内购与订阅统计

### 17.1 原则

- 优先使用推荐的 `purchase` 事件和规定参数。
- 不把“点击购买按钮”记录成购买成功。
- 购买成功必须以 StoreKit / Google Play Billing / RevenueCat 等确认结果为准。
- 恢复购买使用独立事件，例如 `subscription_restored`。
- 服务端订阅续费、退款、取消应由 App Store Server Notifications、Google Play RTDN 或订阅平台服务端处理。
- 不手动重复发送 SDK 已自动采集的 in_app_purchase，避免重复收入。
- 货币使用 ISO 4217，例如 CNY、USD。
- 金额使用实际支付金额，不使用格式化字符串。

### 17.2 标准漏斗

```text
paywall_viewed
plan_selected
begin_checkout
purchase
subscription_restored
```

参数：

```yaml
paywall_viewed:
  entry_point: export_limit
  variant: default

plan_selected:
  item_id: pro_monthly
  billing_period: monthly

purchase:
  transaction_id: non_pii_transaction_id
  currency: CNY
  value: 18
  items:
    - item_id: pro_monthly
      item_name: Pro Monthly
```

不要在 Analytics 中发送完整 App Store 收据或 purchase token。

## 18. 页面统计

页面名应稳定、跨版本可比：

```text
home
editor
history
settings
paywall
onboarding_welcome
onboarding_permissions
```

禁止：

```text
ContentView
UIViewController123
Route_9A7B
/editor/7d604...
```

对于弹窗、Sheet 和 Overlay：

- 只有它代表独立业务页面时才记 screen_view。
- 短暂 toast、普通确认框不记页面事件。
- 付费墙记 paywall_viewed，必要时同时记 screen_view。

## 19. 用户属性

建议最多保留少量、低基数、长期稳定属性：

```text
plan_tier: free | trial | pro
onboarding_state: new | completed
preferred_language: zh_cn | en_us
account_type: guest | registered
```

不应作为用户属性：

- 用户 ID
- 邮箱
- 每次变化的页面
- 订单号
- 文件数量精确值
- 设备剩余空间
- 任意自由文本

退出登录时：

- `setUserId(null)`
- 清空账号相关 user properties
- 保留与设备级体验有关且合规的属性

## 20. 隐私、ATT 与商店申报

### 20.1 通用要求

必须同步更新：

- 隐私政策
- App Store Connect App Privacy
- Google Play Data Safety
- 应用内统计开关
- 用户撤回同意流程
- 数据删除说明
- 儿童类 App 的额外限制

### 20.2 ATT

Firebase Analytics 本身不等于一定需要 ATT。

判断标准是 App 是否：

- 访问 IDFA。
- 将数据与第三方 App / 网站数据关联用于定向广告或广告衡量。
- 进行 Apple 定义的跨 App / 网站追踪。

普通无广告工具类 App：

- 优先不引入 IDFA 支持。
- 不需要时不要添加 AdSupport。
- 不要为了“装了 Firebase”就机械弹 ATT。
- 仍需正确申报 Analytics 收集的数据。

使用 AdMob、广告归因、再营销：

- 按当前 Apple ATT 和 Google Ads 要求实施。
- 不得用技术手段规避授权。

### 20.3 Consent First 模式

对需要先征得同意的地区或产品：

```text
首次启动
  ↓
Analytics 默认关闭
  ↓
展示隐私说明
  ↓
用户允许
  ↓
开启 Analytics
```

用户拒绝：

- 不发送业务事件。
- 不设置 User ID。
- 不因拒绝统计阻断核心功能，除非法律允许且业务确有必要。
- 保留本地 consent 状态。

## 21. 多环境与多应用管理

推荐：

```text
config/
├── dev
├── staging
└── production
```

iOS：

```text
GoogleService-Info-Dev.plist
GoogleService-Info-Staging.plist
GoogleService-Info-Production.plist
```

构建阶段只复制当前环境文件为：

```text
GoogleService-Info.plist
```

Android：

```text
app/src/dev/google-services.json
app/src/staging/google-services.json
app/src/release/google-services.json
```

Flutter：

- 为每个环境运行正确的 `flutterfire configure`。
- 生成不同 firebase_options_*.dart，或按 flavor 管理。
- 不在运行时把生产和测试配置混用。

React Native / Expo：

- 使用 .env 只能管理 Web config 或选择逻辑。
- 原生配置文件仍由对应构建环境提供。
- 不把 GoogleService-Info.plist 和 google-services.json 当成服务器密钥；它们包含标识信息，但仍应避免环境混放。

## 22. DebugView 验证

### 22.1 iOS

在 Xcode Scheme：

```text
Run
→ Arguments
→ Arguments Passed On Launch
→ -FIRDebugEnabled
```

关闭：

```text
-FIRDebugDisabled
```

### 22.2 Android

开启：

```bash
adb shell setprop debug.firebase.analytics.app PACKAGE_NAME
```

关闭：

```bash
adb shell setprop debug.firebase.analytics.app .none.
```

### 22.3 验证内容

Firebase Console：

```text
Analytics
→ DebugView
```

必须验证：

- 测试设备出现。
- screen_view 出现。
- 一个核心业务事件出现。
- 参数值正确。
- User ID 设置和清空正确。
- 用户属性正确。
- 用户拒绝统计后不再发送。
- 没有 error 或 firebase_error 参数。
- 同一动作没有重复事件。

普通报表可能批量延迟上传，接入验收以 DebugView 和设备日志为主。

## 23. 自动化测试和本地校验

为统一封装增加校验：

```ts
const EVENT_NAME = /^[a-z][a-z0-9_]{0,39}$/;
const PARAM_NAME = /^[a-z][a-z0-9_]{0,39}$/;

export function validateAnalyticsEvent(
  name: string,
  parameters?: Record<string, unknown>,
) {
  if (!EVENT_NAME.test(name)) {
    throw new Error(
      `Invalid analytics event name: ${name}`,
    );
  }

  const entries = Object.entries(parameters ?? {});

  if (entries.length > 25) {
    throw new Error(
      `Too many analytics parameters: ${entries.length}`,
    );
  }

  for (const [key, value] of entries) {
    if (!PARAM_NAME.test(key)) {
      throw new Error(
        `Invalid analytics parameter: ${key}`,
      );
    }

    if (
      typeof value === 'string' &&
      value.length > 100
    ) {
      throw new Error(
        `Analytics parameter too long: ${key}`,
      );
    }
  }
}
```

测试至少包含：

- 合法事件通过。
- 大写或连字符事件被拒绝。
- 超过 40 字符被拒绝。
- 超过 25 个参数被拒绝。
- 长字符串被拒绝。
- PII 黑名单字段被拒绝。
- Analytics 异常不影响核心业务。
- 用户不同意时使用 No-op。

## 24. 推荐目录结构

跨平台共享概念：

```text
analytics/
├── analytics_client
├── firebase_analytics_adapter
├── analytics_events
├── analytics_properties
├── analytics_validator
├── analytics_consent
├── noop_analytics_client
└── README.md
```

事件定义集中管理，不允许散落魔法字符串。

TypeScript 示例：

```ts
export const AnalyticsEvents = {
  onboardingStarted: 'onboarding_started',
  onboardingCompleted: 'onboarding_completed',
  featureUsed: 'feature_used',
  exportCompleted: 'export_completed',
  paywallViewed: 'paywall_viewed',
  purchase: 'purchase',
} as const;
```

Dart 示例：

```dart
abstract final class AnalyticsEvents {
  static const onboardingStarted =
      'onboarding_started';

  static const onboardingCompleted =
      'onboarding_completed';

  static const featureUsed =
      'feature_used';

  static const exportCompleted =
      'export_completed';

  static const paywallViewed =
      'paywall_viewed';
}
```

Swift 示例：

```swift
enum AnalyticsEventName {
    static let onboardingStarted =
        "onboarding_started"

    static let onboardingCompleted =
        "onboarding_completed"

    static let featureUsed =
        "feature_used"

    static let exportCompleted =
        "export_completed"

    static let paywallViewed =
        "paywall_viewed"
}
```

Kotlin 示例：

```kotlin
object AnalyticsEvents {
    const val ONBOARDING_STARTED =
        "onboarding_started"

    const val ONBOARDING_COMPLETED =
        "onboarding_completed"

    const val FEATURE_USED =
        "feature_used"

    const val EXPORT_COMPLETED =
        "export_completed"

    const val PAYWALL_VIEWED =
        "paywall_viewed"
}
```

## 25. 常见错误处理

### DebugView 无事件

检查：

- Firebase 项目是否启用 Google Analytics。
- Bundle ID / Application ID 是否完全一致。
- 配置文件是否属于当前 Firebase App。
- iOS plist 是否加入正确 target。
- Android Google Services 插件是否生效。
- 是否使用了旧安装包。
- Expo 是否仍在 Expo Go。
- 是否开启了 Debug 模式。
- 用户是否关闭了 Analytics collection。
- 事件名或参数是否被拒绝。
- 是否在模拟器无网络环境运行。
- Android emulator 是否包含 Google Play 服务。
- 当前构建是否加载了错误环境配置。

### iOS 构建失败

检查：

- CocoaPods 与 use_frameworks! 冲突。
- React Native Firebase 与 Flipper 冲突。
- SPM 与 Pods 重复引入 Firebase。
- plist 重复加入 Copy Bundle Resources。
- 最低 iOS / Xcode / Swift 版本不满足当前 SDK。
- Expo Development Build 未重新生成。

### Android 构建失败

检查：

- Gradle plugin DSL 写法是否与项目版本匹配。
- google-services.json 路径是否正确。
- Application ID 与 Firebase package name 是否一致。
- flavor 是否使用正确配置文件。
- 是否混用不同 Firebase 依赖版本。
- 是否绕过 BoM 手动指定不兼容版本。

### 事件已发送但报表看不到参数

- DebugView 先确认原始参数。
- GA4 常规报表中的自定义参数通常需要创建 Custom Definition。
- 参数类型必须稳定，不能同一名称有时传字符串、有时传数字。
- 高基数参数可能不适合常规报表。

### 数据重复

检查：

- 页面重建触发多次。
- Navigation observer 和手动追踪同时启用。
- 客户端和服务端重复发送购买。
- SDK 自动事件与自定义同义事件同时存在。
- React Strict Mode 开发环境导致副作用执行两次。

## 26. 上线前验收清单

### 基础配置

- [ ] Firebase 项目启用 Google Analytics
- [ ] iOS Bundle ID 正确
- [ ] Android Application ID 正确
- [ ] Web data stream 正确
- [ ] 各环境配置隔离
- [ ] 配置文件属于当前环境
- [ ] SDK 使用当前兼容版本

### 代码

- [ ] Analytics 抽象层已创建
- [ ] 事件常量集中管理
- [ ] 无 UI 直接调用 Firebase
- [ ] 页面追踪已接入
- [ ] 登录后设置内部 User ID
- [ ] 退出后清空 User ID
- [ ] 用户属性低基数
- [ ] 内购不重复统计
- [ ] Analytics 异常不影响主流程
- [ ] 单元测试覆盖事件校验

### 隐私

- [ ] 不采集 PII
- [ ] 不发送用户内容
- [ ] 统计开关可关闭
- [ ] 用户撤回同意后停止收集
- [ ] iOS IDFA / ATT 策略明确
- [ ] App Store Privacy 已更新
- [ ] Google Play Data Safety 已更新
- [ ] 隐私政策已更新
- [ ] 儿童类 App 未接入 IDFA

### 验证

- [ ] iOS DebugView 验证完成
- [ ] Android DebugView 验证完成
- [ ] screen_view 正常
- [ ] 核心事件正常
- [ ] 参数正常
- [ ] 用户属性正常
- [ ] 购买事件正常
- [ ] 无重复事件
- [ ] 无 Analytics error 事件
- [ ] Release 构建完成一次真机验证

## 27. 执行完成后的输出格式

完成接入后必须输出：

```markdown
# Firebase Analytics 接入结果

## 已识别项目

- 平台：
- 框架：
- 包管理器：
- 路由：
- 环境：
- Bundle ID / Application ID：

## 已修改

- `path/to/file`
- `path/to/file`

## 已接入事件

| 事件 | 触发位置 | 参数 |
|---|---|---|
| screen_view | 路由切换 | screen_name |
| feature_used | 核心功能成功 | feature_name, result |
| purchase | 商店确认购买成功 | currency, value, item_id |

## 验证结果

- iOS DebugView：
- Android DebugView：
- Web Debug：
- 重复事件检查：
- Consent 检查：

## 仍需人工完成

1. 在 Firebase Console 注册对应 App。
2. 放入正确配置文件。
3. 更新商店隐私申报。
4. 创建必要的 GA4 Custom Definitions。

## 风险

- 当前未验证项：
- 隐私注意项：
- 版本兼容注意项：
```

不得只回复“已接入完成”。必须说明验证依据和未完成项。

## 28. 官方资料索引

执行时优先检查官方最新文档：

- Firebase Analytics 总览
  https://firebase.google.com/docs/analytics
- FlutterFire 初始化
  https://firebase.google.com/docs/flutter/setup
- Flutter Analytics
  https://firebase.google.com/docs/analytics/flutter/get-started
- Apple platforms Firebase 配置
  https://firebase.google.com/docs/ios/setup
- Apple Analytics 数据收集控制
  https://firebase.google.com/docs/analytics/ios/configure-data-collection
- Android Analytics
  https://firebase.google.com/docs/analytics/android/get-started
- Web Analytics
  https://firebase.google.com/docs/analytics/web/get-started
- DebugView
  https://firebase.google.com/docs/analytics/debugview
- React Native Firebase
  https://rnfirebase.io/
- React Native Firebase Analytics
  https://rnfirebase.io/analytics/usage
- Expo Firebase 指南
  https://docs.expo.dev/guides/using-firebase/
- Unity Analytics
  https://firebase.google.com/docs/analytics/unity/get-started
- C++ Analytics
  https://firebase.google.com/docs/analytics/cpp/events
- GA4 事件限制
  https://support.google.com/analytics/answer/9267744

## 29. 最终准则

接入成功的定义不是“依赖安装成功”，而是：

```text
配置正确
+
统一封装完成
+
事件字典完成
+
隐私策略明确
+
DebugView 验证通过
+
上线验收完成
```

只做 SDK 初始化而没有事件规范、Consent、DebugView 和验收，不算完成。
