# Studio Mini

原生 macOS 上的 Android 构建精简版：打开项目、构建 APK、安装到设备。

当前正式版为 **0.2.12**（构建号 24）。本仓库只发布已用 Apple Developer ID 签名并公证的安装包，以及 Sparkle 更新源，不包含应用源码。

## 功能

界面与行为以 0.2.12 安装包为准：

- **打开项目**：拖入或打开带 `gradlew` 的 Android 项目；也可从 HTTPS、`ssh://` 或 `git@host:path` 地址克隆。私有仓库使用本机已配置的 Git 凭据或 SSH 密钥。
- **快速构建**：识别主模块调试包，构建后安装到已选设备。
- **构建工作台**：读取 Gradle `assemble` 任务，可筛选变体，并支持 `--offline`、`--stacktrace` 和自定义任务。
- **设备与 ADB**：USB 调试、已创建的模拟器，以及 Android 11 及以上的无线调试（配对码或二维码）。设备以宫格卡片选择；可安装、覆盖安装、安装分包 APK、卸载、清除数据、重启、截图、查看日志、开关布局边界，以及设置抓包用的 HTTP 代理。
- **APK 文件**：构建后扫描产物，也可导入已有 APK，再安装、导出或移到废纸篓。
- **签名与加固**：构建时用 Gradle 属性注入 `versionName` / `versionCode`（不改项目文件）；可对产物做 `zipalign`、签名和校验。也可接外部加固脚本或 JAR（识别到 `NHPProtect.jar` 时套用易盾 fullapk 参数模板）。密钥库密码只留在当次运行的内存中，不写入设置或日志。
- **Git**：查看分支与提交，按名称搜索并筛选本地或远端分支；新建、切换、检出、拉取、推送、提交、合并，以及清理工作区。
- **开发环境**：检查本机 JDK、Android SDK 和 ADB。可把 Eclipse Temurin JDK 21 与 Android 命令行工具装到用户目录，不修改项目，也不安装 Android Studio 或模拟器。
- **构建历史**：保留最近 100 次构建记录和完整日志。
- **远程 Bridge**：在局域网、Tailscale 或 ZeroTier 等私网中，把 APK 推到另一台正在运行的 Studio Mini，由对方确认后安装。安装包内容为明文传输，只应在可信私网上使用。

## 环境要求

- Apple Silicon（arm64）
- macOS 14 或更高版本
- 要构建的项目需自带 Gradle Wrapper（`gradlew` 与 Gradle 配置）
- 安装到设备时需要可用的 ADB；可用 USB、模拟器或无线调试
- 应用可代为准备 JDK 与 Android SDK。已有 Android Studio 时，也可在「开发环境」里改用检测到的 JDK 和 SDK

## 安装

### 下载安装包

最新正式包见 [Releases](https://github.com/lageev/StudioMini/releases/latest)。每个版本附带：

- `Studio-Mini-<版本>.zip`：解压后得到 `Studio Mini.app`
- `appcast.xml`：Sparkle 更新源
- `SHA256SUMS-<版本>.txt`：上述文件的 SHA-256

解压后把 `Studio Mini.app` 拖到「应用程序」。

### Homebrew

```bash
brew install --cask lageev/tap/studio-mini
```

配方在 [lageev/homebrew-tap](https://github.com/lageev/homebrew-tap)。cask 版本可能暂时落后于 GitHub Releases；装好后可用应用内更新。

## 更新

正式包会通过 Sparkle 检查更新，更新源为：

https://github.com/lageev/StudioMini/releases/latest/download/appcast.xml

也可在「关于 Studio Mini」或应用菜单中手动检查。自动检查默认开启，间隔约 24 小时。Debug 构建不检查更新，以免线上版本覆盖本地调试包。

## 使用

1. 打开 Studio Mini。
2. 拖入项目文件夹，或使用「打开 Android 项目…」「从远端克隆 Android 项目…」。目录需要位于带 `gradlew` 的项目根，或在前 4 层内能找到这样的目录。
3. 打开「开发环境」，确认 JDK、Android SDK 和 ADB。缺少时可以一键安装推荐组件。
4. 在「设备与 ADB」连接手机：USB 需开启开发者选项和 USB 调试并在设备上授权；无线调试需与 Mac 在同一网络，并使用 Android 11 及以上的「无线调试」。
5. 在「快速构建」或「构建工作台」选择构建版本后开始。需要发布签名或加固时，先到「签名与加固」配好，再构建。
6. 构建产物出现在「APK 文件」。日志在底部面板，「构建历史」里保留完整日志。

Git 页面的拉取、推送和分支切换都作用在当前检出的本地分支上。工作区不干净时，切换和合并会停下来；拉取可以自动暂存并在更新后恢复。

## 从本仓库构建

不能。这里没有应用源码，也无法在本仓库里编译 Studio Mini。请安装上面的正式包；Android 项目的构建在应用内完成。

应用数据默认在：

- `~/Library/Application Support/AndroidBuildDesk`
- `~/Library/Preferences/com.fring.androidbuilddesk.plist`

## English

Studio Mini is a native macOS app for opening an Android project, building an APK, and installing it on a device.

The current release is **0.2.12** (build 24). This repository publishes the Apple Developer ID–signed and notarized app and its Sparkle update feed. It does not contain the application source.

### Features

Behavior below matches the 0.2.12 app:

- **Open a project.** Drop or open an Android project that contains `gradlew`, or clone an HTTPS, `ssh://`, or `git@host:path` URL. Private repositories use the Git credentials or SSH keys already configured on the Mac.
- **Quick build.** Detect the main module’s debug APK, build it, and install it on the selected device.
- **Build workspace.** Read Gradle `assemble` tasks, filter variants, and run with `--offline`, `--stacktrace`, or a custom task.
- **Devices and ADB.** USB debugging, an existing emulator, or wireless debugging on Android 11 or later (pairing code or QR code). Pick a device from the card grid, then install, reinstall, install split APKs, uninstall, clear data, reboot, take a screenshot, read logs, toggle layout bounds, or set an HTTP proxy for packet capture.
- **APK files.** Scan build outputs or import an existing APK, then install, export, or move it to the Trash.
- **Signing and packing.** Inject `versionName` / `versionCode` through Gradle properties without editing the project. Optionally `zipalign`, sign, and verify the APK, or run an external packing script or JAR (`NHPProtect.jar` uses the NetEase fullapk argument template). Keystore passwords stay in memory for that run and are not written to settings or logs.
- **Git.** Browse branches and commits, search and filter local or remote branches, and create, switch, check out, pull, push, commit, merge, or clean the worktree.
- **Toolchain.** Check the local JDK, Android SDK, and ADB. The app can install Eclipse Temurin JDK 21 and the Android command-line tools under the user home without changing the project or installing Android Studio or an emulator.
- **Build history.** Keep the last 100 builds and their full logs.
- **Remote Bridge.** Send an APK over a private network (LAN, Tailscale, or ZeroTier) to another running copy of Studio Mini. The receiver confirms before install. The APK is transferred in the clear, so use a trusted private network.

### Requirements

- Apple Silicon (arm64)
- macOS 14 or later
- A project with its own Gradle Wrapper (`gradlew` and a Gradle build file)
- ADB when installing to a device, over USB, an emulator, or wireless debugging
- A JDK and Android SDK, which the app can install. An existing Android Studio JDK or SDK can be selected in the toolchain page

### Install

Download the latest release from [Releases](https://github.com/lageev/StudioMini/releases/latest). Each release includes:

- `Studio-Mini-<version>.zip` — contains `Studio Mini.app`
- `appcast.xml` — Sparkle update feed
- `SHA256SUMS-<version>.txt` — SHA-256 checksums

Unzip the archive and move `Studio Mini.app` to Applications.

Homebrew:

```bash
brew install --cask lageev/tap/studio-mini
```

The cask is in [lageev/homebrew-tap](https://github.com/lageev/homebrew-tap). It may lag behind GitHub Releases; the installed app can update itself.

### Updates

Release builds check for updates with Sparkle:

https://github.com/lageev/StudioMini/releases/latest/download/appcast.xml

Check manually from About Studio Mini or the application menu. Automatic checks are on by default, about once every 24 hours. Debug builds do not check, so a release build does not replace a local debug build.

### Usage

1. Open Studio Mini.
2. Drop a project folder, or use Open Android Project or Clone Android Project from a Remote. The folder must be a project root with `gradlew`, or such a root must be within the first four directory levels.
3. Open the toolchain page and confirm the JDK, Android SDK, and ADB. Install the recommended components there if they are missing.
4. Connect a phone from Devices and ADB. USB needs Developer Options, USB debugging, and authorization on the device. Wireless debugging needs the same network as the Mac and Android 11 or later.
5. Choose a variant in Quick Build or the build workspace and start. Set up release signing or packing first when you need it.
6. Outputs are listed under APK Files. The bottom panel shows the live log; Build History keeps the full log.

Pull, push, and branch switching apply to the currently checked-out local branch. A dirty worktree stops a switch or merge. Pull can stash local changes and restore them after the update.

### Building from this repository

You cannot. This repository has no application source, so Studio Mini is not built here. Install a release above. Android project builds happen inside the app.

App data is stored at:

- `~/Library/Application Support/AndroidBuildDesk`
- `~/Library/Preferences/com.fring.androidbuilddesk.plist`
