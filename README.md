# 比恋 AI · Flutter-BilianAI

比恋 AI（Bilian AI）的 Flutter 移动端界面原型，面向中文 AI 聊天应用场景，包含登录、对话／发现切换、个人设置、反馈与账号注销等页面。

项目使用同一套 Dart 代码组织 Android 与 iOS 界面，并通过 `flutter_screenutil` 适配屏幕尺寸。当前公开源码处于前端原型阶段，适合用于了解页面结构、复用 UI 组件及继续开发移动端功能。

> **当前状态：** 应用从带计数器的调试首页启动，可通过 Login、Major、Setting 按钮浏览已实现页面。仓库尚未包含可用的 AI 对话服务、真实登录、微信分享或账号管理后端；页面上的相关入口不代表已接入这些服务。

## 目录

- [功能与实现状态](#功能与实现状态)
- [技术栈](#技术栈)
- [环境要求](#环境要求)
- [快速开始](#快速开始)
- [页面浏览](#页面浏览)
- [项目结构](#项目结构)
- [开发说明](#开发说明)
- [检查与测试](#检查与测试)
- [构建与发布](#构建与发布)
- [常见问题](#常见问题)
- [后续开发方向](#后续开发方向)
- [贡献与反馈](#贡献与反馈)
- [许可证](#许可证)

## 功能与实现状态

| 模块 | 已实现内容 | 当前边界 |
| --- | --- | --- |
| 调试首页 | 计数器、登录／主页面／设置页面跳转 | 仍保留 Flutter 示例入口 |
| 登录页 | 品牌图标、微信／手机号／Apple ID 登录入口、协议勾选状态 | 登录回调与协议链接尚未实现；勾选协议后微信按钮才可用 |
| 主页面 | “对话／发现”顶部切换、个人设置入口 | 切换状态通过流更新，页面主体仍为空 |
| 发现页 | 分类标签组件及选中样式 | 页面主体仍为空，未接入内容数据 |
| 对话与会话列表 | 对话页面占位与会话列表文件 | 对话页为空容器，会话列表文件为空；未实现消息交互 |
| 设置页 | 头像、昵称、分享、反馈、隐私、关于、注销和退出入口 | 头像与昵称为本地示例数据；分享、隐私、关于和退出回调为空 |
| 反馈页 | 多行输入、200 字限制、提交按钮 | 提交回调为空，未上传反馈 |
| 注销页 | 原因输入、提示文案、提交按钮 | 提交回调为空，不会执行真实账号删除 |
| 通用组件 | 主按钮、分隔线、导航切换与文本输入组件 | 供现有页面复用 |

ChatGPT、文心一言等模型服务属于后续接入方向，当前仓库没有对应请求逻辑、服务地址或 API Key 配置。

## 技术栈

| 技术／依赖 | 声明版本 | 用途 |
| --- | --- | --- |
| Flutter / Dart | Dart `>=3.2.3 <4.0.0` | 跨平台 UI 与业务代码 |
| Material 3 | Flutter 内置 | 主题与基础界面组件 |
| flutter_screenutil | `^5.9.0` | 尺寸与文字适配 |
| flutter_bloc | `^8.1.3` | 已声明的状态管理依赖；现有页面主要使用 `setState` 和流 |
| image_picker | `^1.0.7` | 已声明的图片选择依赖，尚未接入头像编辑 |
| cupertino_icons | `^1.0.2` | Cupertino 图标 |
| flutter_test / flutter_lints | SDK / `^2.0.0` | Widget 测试与静态检查 |

应用包名为 `bilian_xy`，版本为 `1.0.0+1`。依赖声明见 [pubspec.yaml](pubspec.yaml)，解析版本见 [pubspec.lock](pubspec.lock)。

## 环境要求

- 安装 Flutter SDK 并将 `flutter`、`dart` 加入 PATH；其内置 Dart 必须满足 `>=3.2.3 <4.0.0`。
- 安装 Git，用于获取源码。
- Android：安装 Android SDK、兼容的 JDK，以及模拟器或支持 USB 调试的真机。
- iOS：需要 macOS、Xcode 与 CocoaPods，以及模拟器或已配置签名的真机。

仓库仅包含 Android 与 iOS 平台工程，没有 Web 或桌面端工程。Android 工程使用 Gradle 7.5、Android Gradle Plugin 7.3.0 和 Kotlin 1.7.10；升级 Flutter 或 JDK 时应同时检查原生构建工具的兼容性。仓库未提供持续集成验证的 Flutter 版本矩阵。

可先查看 [Flutter 环境安装文档](https://docs.flutter.dev/get-started/install)，再运行：

```bash
flutter --version
dart --version
flutter doctor -v
```

## 快速开始

```bash
git clone https://github.com/Pannic17/Flutter-BilianAI.git
cd Flutter-BilianAI
flutter pub get
flutter devices
flutter run -d <device-id>
```

将 `<device-id>` 替换为 `flutter devices` 列出的设备标识；只有一个可用设备时也可直接执行 `flutter run`。

当前 UI 原型不需要后端、环境变量或 API Key。运行时可按 `r` 热重载，按 `R` 热重启，按 `q` 退出。

## 页面浏览

启动后会看到 Flutter 调试首页及计数器：

1. 点击 **Login** 查看比恋 AI 登录页，勾选协议可观察微信登录按钮的启用状态。
2. 点击 **Major** 查看“对话／发现”切换；右侧用户图标可进入设置页。主页面内容区域暂时为空。
3. 点击 **Setting** 直接进入设置页。
4. 在设置页点击“反馈与投诉，帮助我们改进”，进入反馈输入页。
5. 在设置页点击“注销账号”，进入注销原因输入页。

真实登录、反馈提交和账号注销均未实现；点击对应提交按钮不会发起业务请求。

## 项目结构

```text
Flutter-BilianAI/
├── android/                       # Android 原生工程与 Gradle 配置
├── ios/                           # iOS 原生工程与 Xcode 配置
├── asset/                         # 品牌、登录方式及设置图标
├── lib/
│   ├── main.dart                  # 应用入口、主题、屏幕适配与调试首页
│   ├── components/
│   │   ├── appbar.dart            # 对话／发现切换与设置入口
│   │   └── global.dart            # 主按钮、分隔线等通用组件
│   ├── pages/
│   │   ├── login.dart             # 登录 UI 与协议勾选
│   │   ├── major.dart             # 主页面与切换状态流
│   │   ├── discover.dart          # 发现页占位与分类标签组件
│   │   ├── dialog.dart            # 对话页占位
│   │   ├── chat_list.dart         # 会话列表占位（空文件）
│   │   ├── setting.dart           # 设置页
│   │   └── setting/
│   │       ├── setting_advice.dart # 反馈表单
│   │       └── setting_logout.dart # 注销表单与共享文本输入组件
│   └── test.dart                  # 开发试验代码
├── test/widget_test.dart          # 默认计数器 Widget 测试
├── analysis_options.yaml          # Dart lint 配置
├── pubspec.yaml                   # 包信息、依赖与资源声明
└── pubspec.lock                   # 依赖版本锁定
```

## 开发说明

### 屏幕适配与主题

`lib/main.dart` 使用 `ScreenUtilInit`，设计基准为 **375 × 812**，并启用 `minTextAdapt` 与 `splitScreenMode`。页面使用 `.w`、`.h`、`.r` 和 `.sp` 表达适配尺寸。主题启用 Material 3，主要品牌色为 `#6886FF`。

调整布局后，应检查小屏、横屏、系统文字放大和键盘弹出时的表现；现有登录与设置页包含固定高度布局。

### 导航与状态

页面通过 `Navigator.push` 与 `MaterialPageRoute` 跳转，尚未引入集中路由表。主页面使用广播 `StreamController<bool>` 配合 `StreamBuilder` 更新顶部切换状态，其他局部交互主要使用 `setState`。

### 资源与服务接入

图片存放在 `asset/`，并在 `pubspec.yaml` 的 `flutter.assets` 中声明。添加资源后重新执行 `flutter pub get`，必要时热重启。

若继续接入模型、登录或用户服务，需新增请求层、数据模型、错误处理及凭据管理。模型服务密钥应由受控后端管理，避免写入客户端源码。接入图片选择时，还需补齐相应平台权限及隐私用途说明。

## 检查与测试

安装依赖后，可执行：

```bash
flutter analyze
flutter test
dart format --output=none --set-exit-if-changed lib test
```

当前 `test/widget_test.dart` 只验证默认首页计数器从 0 增加到 1，不覆盖登录、设置、反馈或对话业务。上述命令是本地检查入口，不代表当前源码已经通过全部检查；历史代码包含未使用导入、占位实现及可能随 SDK 更新产生的弃用提示。

需要整理格式时可运行 `dart format lib test`，并检查产生的差异。

## 构建与发布

### Android

用于本地安装验证：

```bash
flutter build apk --debug
```

默认产物位于 `build/app/outputs/flutter-apk/app-debug.apk`。

准备正式发布配置后，可执行：

```bash
flutter build apk --release
flutter build appbundle --release
```

默认产物分别位于 `build/app/outputs/flutter-apk/app-release.apk` 和 `build/app/outputs/bundle/release/app-release.aab`。**当前 release 配置使用 debug 签名**，发布前需配置正式签名、替换 `com.example.bilian_xy` 应用 ID，并检查应用名称、图标、版本及所接入服务需要的权限。

### iOS

在 macOS 上可先验证无签名构建：

```bash
flutter build ios --no-codesign
```

真机安装或正式分发需要在 Xcode 中配置开发团队、Bundle Identifier 与签名；完成配置后可使用 `flutter build ipa`。当前 iOS 显示名称仍为 `Bilian Xy`，应在发布前确认。

## 常见问题

| 现象 | 说明／处理方式 |
| --- | --- |
| 启动后是 Flutter 示例首页 | 当前入口就是调试首页，通过 Login、Major、Setting 浏览页面 |
| 微信按钮不可用 | 先勾选协议；按钮启用后仍只有占位回调 |
| 切换“对话／发现”后没有内容 | 主页面与对应内容页尚未实现 |
| 提交反馈、注销或点击分享没有响应 | 当前没有接入对应业务服务 |
| Dart 版本不满足约束 | 检查 Flutter 内置 Dart 版本与 `pubspec.yaml` |
| Android 构建出现 Gradle／JDK 错误 | 核对旧版 Gradle、AGP、Kotlin 与本机 JDK、Flutter 的兼容性 |
| 图片加载失败 | 检查路径大小写、资源声明，并重新获取依赖／热重启 |
| 无法构建 Web 或桌面端 | 仓库未包含这些平台工程，部分共享代码还引入 `dart:ffi`；需要先处理平台兼容性 |
| Windows 上无法构建 iOS | iOS 构建需在 macOS 与 Xcode 环境中执行 |

## 后续开发方向

以下为可继续完善的方向，不表示已实现或承诺交付：

- 将调试首页替换为正式启动与登录流程。
- 完成对话 UI、会话列表、消息存储及模型服务接入。
- 为发现页接入内容数据与分类筛选。
- 完成真实登录、用户资料编辑、反馈和账号注销流程。
- 接入分享与协议页面，完善权限和隐私配置。
- 为页面导航、表单交互和服务错误状态补充测试。
- 整理组件、状态管理、布局适配与发布配置。

## 贡献与反馈

欢迎通过 [Issues](https://github.com/Pannic17/Flutter-BilianAI/issues) 提交问题或建议。报告问题时请附上系统、设备、Flutter／Dart 版本、复现步骤及相关日志；避免包含个人数据或密钥。

提交代码时，请说明修改范围、实际实现的功能与验证结果，并尽量保持改动聚焦。涉及界面的修改建议提供截图。

## 许可证

当前仓库未提供 LICENSE 文件。公开源码不等同于授予开放源代码许可；如需分发、商用或复用代码及图形资源，请先与作者确认授权。第三方依赖遵循各自的许可证。
