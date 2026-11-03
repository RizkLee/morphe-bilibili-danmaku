# YouTube Morphe — Bilibili 弹幕补丁 2.0.3

更新于 2026-11-04。此补丁为 Morphe Patches 的个人分支，提供 YouTube 内 B 站弹幕。源码在 `patches/src/main/kotlin/app/morphe/patches/youtube/layout/bilibilidanmaku/`，播放器扩展在 `extensions/youtube/src/main/java/app/morphe/extension/youtube/bilibilidanmaku/`。

## 获取补丁与兼容 APK

- **补丁源 URL**：`https://github.com/RizkLee/morphe-patches`（可直接粘贴到 Morphe Manager 的 Sources / 来源 → + → Remote / 远程）。
- **元数据直链**：`https://raw.githubusercontent.com/RizkLee/morphe-patches/main/patches-bundle.json`
- **发布页**：`https://github.com/RizkLee/morphe-patches/releases/tag/v2.0.3`
- **补丁包**：`https://github.com/RizkLee/morphe-patches/releases/download/v2.0.3/patches-2.0.3.mpp`
- **确认适配的 YouTube 原版**：`com.google.android.youtube` **21.30.216**，下载为未经任何修改的 Google 原始单文件 APK；此前用该版本在本机完成过 APK 的实际修补和签名。输入已修补的 YouTube Morphe APK 会因 `MORPHE_BRANDING.TXT` 冲突而失败。
- 2.0.3 的 Android 扩展已重新构建并执行单元测试，但**本版的最后一轮改动尚未经 Android 手机实际体验验证**。

本分支的 `.mpp` 包含 Morphe 官方的补丁实现及 Bilibili danmaku，原因是弹幕补丁依赖播放器按钮注入、视频 ID、播放器时间、扩展资源等共享补丁。**可以在专家模式仅勾选「Bilibili danmaku」及自动选择的依赖项；目前不能把它当作与任意新版官方来源拼接的独立单补丁包。** 官方补丁要更新时，需要同步上游代码并重建此分支。Morphe Manager 对同一个 App 使用多个来源存在兼容风险，建议 YouTube 一次只从本来源修补，其他 App 仍可使用官方源。

## Morphe Manager 安装步骤

1. 打开 **Morphe Manager → 来源（Sources）→ + → 远程（Remote）**，粘贴上述 **补丁源 URL** 并添加，等待载入。也可在手机浏览器打开 `https://morphe.software/add-source?github=RizkLee/morphe-patches&app=com.google.android.youtube`。
2. 若当前 YouTube 被设置为仅使用官方来源，请在来源/App 选择里允许本分支用于 YouTube。**修补 YouTube 时选择本分支作为唯一来源**，减少混用来源造成的冲突。其他 App 可继续使用官方源。
3. 在补丁选择界面开启**专家模式**，选中 **Bilibili danmaku**；若 Manager 自动要求共享前置补丁，请保留前置依赖。其他补丁可按需勾选。
4. 提供 **21.30.216 未修改的原版 YouTube APK**。不要使用手机里已安装的 Morphe 或此前改过的 APK。按 Manager 提示修补、签名与安装。
5. 如果提示签名不一致，先备份原应用的数据与设置，并检查是否能使用原 Manager 的签名；卸载旧应用可能导致数据丢失。Android 安装签名密钥与本补丁包的 Ed25519 来源密钥是两回事。
6. 此来源托管于 GitHub Releases，后续发布版本时可在 Morphe Manager 的来源卡片点击「更新」，也可使用本地 `.mpp` 文件导入。

## 开启、选择弹幕及界面

在 YouTube Morphe 内打开 **设置 → Morphe → 播放器 → 悬浮按钮**，启用 Bilibili danmaku。打开普通视频，在播放器悬浮按钮栏点弹幕图标以打开设置抽屉。最新源码已对接共享的原生 SlideDialog，以恢复上滑入场、下滑退出与下拉关闭；竖屏菜单以播放器下缘定位，横屏使用有限宽度的侧边抽屉。

菜单开关下方显示当前匹配 B 站视频的封面、标题及 BV 号，选视频的候选列表也带封面。菜单内可搜索 B 站视频、手动粘贴 BV/av/番剧/分 P 链接、导入 XML、设置字幕字体/透明度、时间偏移与倍率、过滤弹幕等。全屏和非全屏字体大小会按视频区域尺寸归一化，竖屏全屏减少并行弹幕的纵向间隔，顶部/底部固定弹幕锚定于相应边缘。

如果尚无登录凭证，弹幕菜单里的「B 站账号」会引导导入 `SESSDATA`。登录不是每个公开资源都必需，但未登录时部分 B 站接口会拒绝访问；本地 XML 导入不需要登录。

## 安全导入 SESSDATA

**SESSDATA 相当于 B 站账号登录会话，不要把它发给陌生人、贴进聊天或公开网盘。**

1. 电脑 Chrome / Edge 打开 `https://www.bilibili.com/`，确认已经登录。
2. 按 F12 打开开发者工具，进入 **Application（应用程序）→ Cookies → https://www.bilibili.com**。
3. 搜索 `SESSDATA`，仅复制其 **Value**；通过可信的私人方式发送到自己的手机。
4. 打开弹幕菜单 → **B 站账号**，按引导粘贴 `SESSDATA`，点击验证并保存。过期后可重新导入，或在菜单中清除。
5. 普通 JS 收藏夹脚本只能读 `document.cookie`，但 `SESSDATA` 一般是 `HttpOnly`，因此无法保证通过「点击书签导出 Cookie」取得它。使用浏览器开发者工具更可靠。账号数据在手机本地使用 Android Keystore 加密，按需发送给 B 站 HTTPS 接口。

## 构建与签名

项目运行 JDK 21 / Gradle。先准备 Android SDK、依赖，再执行：

```powershell
cd 'E:\Code\morphe-patches'
$env:JAVA_HOME = 'D:\Program Files\Eclipse Adoptium\jdk-21.0.9.10-hotspot'
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
$env:GITHUB_ACTOR = (gh api user --jq .login)
$env:GITHUB_TOKEN = (gh auth token)
.\gradlew.bat :extensions:youtube:testDebugUnitTest :patches:buildAndroid '-Dmaven.repo.local=E:/Code/morphe-patches/build/local-m2' --no-daemon
```

输出 `patches/build/libs/patches-2.0.3.mpp`，内含签名（ZIP comment 的 `MorpheSig1`）。`morphe-signing.pub` 是本分支的公开验证密钥，`morphe-signing.key` 是忽略 Git 跟踪的私钥；**私钥不可公开，也不能丢失，否则不能继续用同一个来源签名后续版本**。发布时同步更新 `patches-bundle.json` 的版本、下载链接及对应公开密钥。

需要本机完整修补测试时，可使用 `scripts/patch-youtube-bilibili.ps1`；它依赖 Android SDK 构建工具以及原始的 Google APK。不要把补丁包重新用于已修补的 APK。