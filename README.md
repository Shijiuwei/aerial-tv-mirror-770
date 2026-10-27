
<p align="left">
  <a href="https://shijiuwei.github.io/aerial-tv-mirror-770/"><img src="https://img.shields.io/badge/GitHub_Pages-在线文档中心-2563eb.svg" alt="GitHub Pages"></a>
  <a href="https://shijiuwei.github.io/aerial-tv-mirror-770/docs/"><img src="https://img.shields.io/badge/Docs_Archive-文档归档菜单-10b981.svg" alt="Docs Archive"></a>
</p>

> 🌐 **官方在线文档与静态镜像中心 (GitHub Pages)**：[https://shijiuwei.github.io/aerial-tv-mirror-770/](https://shijiuwei.github.io/aerial-tv-mirror-770/)  
> 📚 **技术规约与文档归档菜单 (Docs Catalog)**：[https://shijiuwei.github.io/aerial-tv-mirror-770/docs/](https://shijiuwei.github.io/aerial-tv-mirror-770/docs/)  

<div align="center">

<img src="docs/brand/icon.png" width="96" alt="Aerial 图标">

# Aerial

**给国产电视的一块干净桌面。**

参照 Apple TV 做的 Android TV 桌面：没有广告，没有推荐流，只有你的 App、壁纸和航拍屏保。<br>
适合小米、TCL、创维、索尼等国产电视，也能装在盒子和投影仪上。开机自动进入，待机也不会被清理。

**中文** · [English](README.en.md)

<a href="https://dhmg.tcti.cn/pingtai/contact-93737131.html"><img alt="官网" src="https://img.shields.io/badge/%E5%AE%98%E7%BD%91-1f6feb?style=for-the-badge&logo=safari&logoColor=white"></a>
<a href="https://jcku.ai-hao123.com/zhizhu/upload-95159748.html"><img alt="下载 APK" src="https://img.shields.io/badge/%E4%B8%8B%E8%BD%BD%20APK-34a853?style=for-the-badge&logo=android&logoColor=white"></a>
<a href="#安装"><img alt="安装" src="https://img.shields.io/badge/%E5%AE%89%E8%A3%85-d97757?style=for-the-badge&logo=gnubash&logoColor=white"></a>

<a href="https://ybrs.ai-hao123.com/chuangxin/sales-02561492.html"><img alt="版本" src="https://img.shields.io/github/v/release/pengchujin/aerial-tv?style=flat-square&label=%E7%89%88%E6%9C%AC&color=2997ff"></a>
<a href="LICENSE"><img alt="许可证 MIT" src="https://img.shields.io/badge/%E8%AE%B8%E5%8F%AF%E8%AF%81-MIT-555555?style=flat-square"></a>
<img alt="Android TV 5.0+" src="https://img.shields.io/badge/Android%20TV-5.0%2B-3ddc84?style=flat-square&logo=android&logoColor=white">
<a href="#lite"><img alt="Lite 4.0–4.4 测试版" src="https://img.shields.io/badge/Lite-4.0%E2%80%934.4%20%E6%B5%8B%E8%AF%95%E7%89%88-f5a623?style=flat-square&logo=android&logoColor=white"></a>

![Aerial 首页](site/img/home.jpg)

| 电视自带的桌面 | 换上 Aerial 之后 |
|:---:|:---:|
| <img src="site/img/stock-xiaomi.jpg" alt="电视自带的桌面：满屏剧集推荐、会员和限免角标"> | <img src="site/img/home.jpg" alt="Aerial 首页"> |

</div>

## 功能

<p align="center">
  <img src="site/img/focus.jpg" width="49%" alt="第一排的玻璃底板">
  <img src="site/img/grid.jpg" width="49%" alt="往下滚动后的应用网格，中间有一个文件夹">
</p>

- **首页**：常用的一排 App 放在屏幕下方的玻璃底板上，往下是全部 App。小米「应用中心」「米家」这类藏起来的系统 App 也在。
- **整理**：长按 OK 键（或按菜单键）可以移动、放进文件夹、隐藏、卸载。固定到主屏幕的 HDMI 可以选接的是 Apple TV、PlayStation、Switch、Xbox、电脑等，换成对应的图标，也能在手机上起名字。
- **用手机整理**：扫电视上的二维码，手机上就是和电视一样的图标网格：按住拖动排序，点一下改名、换图标、放进文件夹或隐藏，也能把 HDMI 等信号源加到主屏幕。
- **控制中心**：在第一排按上键到右上角打开，有信号源、网络、设置、音量、亮度和个性化。
- **屏保**：Apple TV 的 114 段航拍影片，电视支持时播 4K 或 4K 杜比视界，也可以换成照片、时钟或从手机添加的本地视频（[说明](docs/SCREENSAVER_VIDEOS.md)）。
- **壁纸**：必应每日精选、14 张内置壁纸、用手机扫码传的照片，或者航拍：停在主屏幕 10 秒后在磁贴后面播放，按任意键停在当前画面。
- **个性化**：单排或双排、时钟大小和位置、App 名称、焦点样式、按键声音，都能改。

<p align="center">
  <img src="site/img/folder.jpg" width="49%" alt="打开的文件夹">
  <img src="site/img/control-center.jpg" width="49%" alt="控制中心">
</p>

<p align="center">
  <img src="site/img/screensaver-aerial.jpg" width="49%" alt="航拍屏保">
  <img src="site/img/wallpaper.jpg" width="49%" alt="个性化面板">
</p>

## 安装

不用 root，不动系统应用，随时能还原。电脑和电视连同一个网络。

### 1. 打开电视的 ADB 调试

| 电视 | 怎么打开 |
|---|---|
| **小米 / Redmi** | 设置 → 关于 → 连续按「产品型号/产品类型」进入开发者模式，再到 设置 → 账号与安全，打开「ADB 调试」和「安装未知来源的应用」 |
| **TCL / 雷鸟** | 设置 → 关于 → 本机信息，快速按遥控器「上下左右」，在底部的「更多」里打开 ADB |
| **海尔 / 模卡** | 按电视机身上的信号源键，再用遥控器按「左左上右」，在工厂菜单里打开「ADB 调试开关」，别动其他选项 |
| **OPPO（含 S1）** | 设置 → 系统 → 关于本机 → 设备型号，按「上上右右」再按确认；再到 设置 → 系统 → 开发者选项，打开「USB 调试」。IP 在 设置 → 网络 → 已连接的网络 最下面 |
| **其他 Android TV** | 设置 → 设备偏好设置 → 关于 → 连按「内部版本号」7 次，再在开发者选项里打开「USB 调试」 |

电视的 IP 在「设置 → 网络 → 已连接的网络」里。第一次连接时，电视会问是否允许调试，选「始终允许」。
更细的分品牌说明（包括打不开 ADB 的机型）见 [docs/devices/adb-by-brand.md](docs/devices/adb-by-brand.md)。

### 2. 安装

**让 Agent 装（推荐）**：把下面这段发给能在电脑上运行命令的 Agent（WorkBuddy、豆包电脑版、千问电脑版、Trae、Claude Code、Codex 等）：

```text
请帮我把开源电视桌面 Aerial 装到我的电视上，并设为开机进入的桌面。
电视 IP：<电视IP>
保持 ADB 常开：要

安装说明在这里：https://aerial.qust.me/agent.md
（打不开时用 https://raw.githubusercontent.com/pengchujin/aerial-tv/main/site/agent.md ）

请先完整读完安装说明再动手，然后在我的电脑上亲自运行命令，不要只把步骤告诉我。需要我在电视上操作时提醒我。
最后按说明里的「验收」，把这几条命令的原始输出原样贴给我：
- adb shell "cmd package resolve-activity --brief -a android.intent.action.MAIN -c android.intent.category.HOME"
- adb shell settings get secure enabled_accessibility_services
- 按主页键后的 adb shell "dumpsys window | grep mCurrentFocus"
```

手机上的豆包、网页版聊天只会把步骤告诉你，不会执行；要用能跑命令的电脑版 / 工作版（豆包电脑版或豆包工作版、WorkBuddy、千问电脑版、Trae、Claude Code、Codex 等）。Agent 说装好了，看它贴出的 `mCurrentFocus` 输出里有没有 `app.orchard.tv`。

**没有电脑**：有用户用手机上的甲壳虫 ADB 助手、超级小爱装成功过，本仓库没有逐一实测。步骤和上面的 Windows 步骤一样，装好后建议再用电脑跑一遍验收。

**一行命令（macOS / Linux）**：

```bash
curl -fsSL https://aerial.qust.me/install.sh | KEEP_ADB=1 TV=192.168.1.20 bash
```

**手动**：下载 [aerial-standard.apk](https://siiu.tcti.cn/yunsuan/success-84873606.html)，然后：

```bash
adb connect 192.168.1.20:5555
adb install -r aerial-standard.apk
adb shell pm grant app.orchard.tv android.permission.WRITE_SECURE_SETTINGS
adb shell cmd notification allow_listener app.orchard.tv/app.orchard.tv.guard.RevivalListener
adb shell appops set app.orchard.tv SYSTEM_ALERT_WINDOW allow
adb shell am start -n app.orchard.tv/.MainActivity
```

Windows 用 Agent 装，或照[安装说明](site/agent.md)里的 Windows 步骤。

**U 盘（不用电脑）**：下载 APK 拷进 U 盘，用电视的文件管理器打开安装。

| 下载 | 用在 |
|---|---|
| [aerial-standard.apk](https://imuh.ai-hao123.com/shangye/keyword-47963557.html) | Android 5.0 及以上，先装这个 |
| [aerial-guard.apk](https://zddk.ai-hao123.com/hezuo/networking-07486938.html) | standard 装不上、提示不允许安装桌面类 App 时 |
| [aerial-lite.apk](https://mgez.ai-hao123.com/zixun/cheap-82333203.html) | Android 4.0–4.4（见下面的 [Aerial Lite](#lite)） |

装好后按主页键：系统问「使用哪个应用」时选 Aerial、再选「始终」就设好了（原生 Android TV 一般是这样）。直接回到原桌面、不问（国产电视大多这样）时，从「我的应用」打开 Aerial，它会弹出「设为主屏幕」，按里面的入口去电视设置里打开桌面守护（无障碍）和通知使用权；电视设置里没有这些入口时，Aerial 只能当普通 App 用，要接管主页键还得用上面的命令或 Agent 设置一次（覆盖安装，不丢设置）。

**升级**：1.32 起，有新版本时控制中心会出现「有新版本」，点进去选「下载并安装」，在电视自己的安装界面确认（小米、TCL 上验证过），装完回到 Aerial，设置都保留。电视不让 App 安装应用时，升级就是再运行一次安装命令：升级页上有二维码，手机扫码能直接复制这台电视的命令，或者交给 Agent。1.31 及以前没有这个按钮，这一次还是要再运行一次安装命令。

<a id="lite"></a>

### Android 4.0–4.4 老电视：Aerial Lite（测试版）

Android 4.0–4.4 的老电视、老盒子装不了完整版（界面框架最低要 Android 5.0），所以有一个单独的 **Aerial Lite**：包名 `app.orchard.tv.lite`，外观和完整版一样。

> **Lite 现在是第一版（1.0），属于测试版。** 只在 Android 4.0.4 / 4.4.4 虚拟机，以及小米、TCL 的新电视上测过，还没在真实的 4.x 电视上用过，可能有不少小问题。**只适配 Android 4.0–4.4**；Android 5.0 及以上请用上面的完整版。遇到问题请用[机型适配模板](https://xeqj.ai-hao123.com/sheji/deadline-09698009.html)反馈，标题写上 Lite。

**一行命令（macOS / Linux）**：和完整版是同一条命令，脚本发现电视是 Android 4.x 会自动装 Lite；也可以明确指定：

```bash
curl -fsSL https://aerial.qust.me/install.sh | TV=192.168.1.20 bash -s lite
```

**手动**：下载 [aerial-lite.apk](https://cupf.wtpuscm.cn/shichang/file-629016.html)，然后：

```bash
adb connect 192.168.1.20:5555
adb install -r aerial-lite.apk
adb shell pm grant app.orchard.tv.lite android.permission.WRITE_SECURE_SETTINGS   # 调亮度用；Android 4.0 会报错，可以忽略
adb shell am start -n app.orchard.tv.lite/.HomeActivity
```

**U 盘**：把 `aerial-lite.apk` 拷进 U 盘，用电视的文件管理器打开安装。

**装好以后**：按遥控器的**主页键**，系统问「使用哪个应用」时选 **Aerial Lite**，再选「**始终**」。不弹出选择、直接回到原桌面，说明固件把主页键锁在了自家桌面，这时只能从应用列表打开 Lite。

**和完整版的区别**：没有桌面守护（无障碍接管），开机和主页键全靠系统的默认桌面；没有航拍壁纸、「我的视频」壁纸、信号源磁贴（4.x 没有电视输入框架，信号源按钮打开电视自己的信号源面板）、应用内升级（升级就是再装一次，设置保留）。航拍屏保、照片和每日一图、时钟、文件夹、长按菜单、控制中心、个性化、手机配置页都有。详细说明见 [docs/LITE.md](docs/LITE.md)。

**还原**：电视上 设置 → 应用 → Aerial Lite → 清除默认设置，再按主页键选原来的桌面；或者直接卸载：`adb uninstall app.orchard.tv.lite`（一行命令的 `bash -s restore --uninstall` 也会卸载它）。

### 还原

```bash
curl -fsSL https://aerial.qust.me/install.sh | TV=192.168.1.20 bash -s restore
```

最后加 `--uninstall`（`bash -s restore --uninstall`）会顺便卸载 Aerial。不能运行 bash（Windows）时，逐条的 adb 命令见[安装说明的「还原」](site/agent.md#还原)。

想交给 AI 还原，把这段直接发过去（要能在电脑上运行命令）：

```text
请按 https://aerial.qust.me/agent.md 里「还原」一节的步骤，把我电视（IP：<电视IP>）的桌面还原成原来的，Aerial 先不卸载。
请在我的电脑上亲自运行命令，最后把 settings get secure enabled_accessibility_services 和按主页键后的 dumpsys window | grep mCurrentFocus 原始输出贴给我。
```

### 常见问题

- **我的电视能用吗**：见下面的[支持的机型](#支持的机型)。

- **只用 U 盘装了 APK**：1.32 起「我的应用」里有 Aerial 的图标，打开后会提示怎么设为主屏幕。只装 APK 时按主页键、开机和待机后还是原桌面，要用上面的命令或 Agent 再设置一次（会覆盖安装，不丢设置）。
- **TCL 弹出「多屏互动」**：按遥控器「右」再按确认，选「同意」。
- **创维 `adb install` 报 `INSTALL_FAILED_USER_RESTRICTED`**：先运行 `adb shell settings put global verifier_verify_adb_installs 0` 再装（用户反馈，详见[安装说明](site/agent.md#常见情况)）。
- **提示不允许安装桌面类 App**：命令最后加 `-s guard`，或下载 [aerial-guard.apk](https://dsxi.tcti.cn/jishu/progress-52069513.html)。
- **`KEEP_ADB=1` 是什么**：小米待机后会关掉 ADB，加上它 Aerial 会重新打开，方便以后升级。
- **原来的桌面、会员、小爱、账号和有线电视还在吗**：都在。Aerial 只换桌面，原来的桌面在 App 列表里（小米影视、PatchWall 从「桌面」磁贴进），小爱同学、小米账号、有线电视照常。开机广告是固件播的，去不掉。

## 它怎么接管桌面

国产电视大多只认自家桌面，待机时还会清理第三方 App。所以 Aerial 用一个无障碍服务，原桌面一出现就切回来（只看哪个窗口在前台，不读屏幕内容）；被清理后再靠通知监听让系统把它拉起来（不读通知）。老小米（Android 6）的桌面不报告窗口变化，守护改看系统的用量统计；有些酷开、创维固件不按标准方式启动桌面，守护也认得它们自己的桌面。原生 Android TV 上不需要这些，直接设成默认桌面就行。

## 支持的机型

✅ 能用　⚠️ 能用，有限制　❌ 暂时接管不了。大多来自用户反馈，带「笔记」的有详细记录。

| 品牌 | 机型 | 系统 | 情况 |
|---|---|---|---|
| 小米 / Redmi | MiTV-MFTR0（Redmi X65 2026）、MFFU0、MTSU0（S65 Mini LED 2026 至尊版） | Android 14 | ✅ 日常使用（[MFTR0](docs/devices/xiaomi-mitv-mftr0.md)、[MFFU0](docs/devices/xiaomi-mitv-mffu0.md)、[MTSU0](docs/devices/xiaomi-mitv-mtsu0.md)） |
| 小米 | MiTV-ASTP0、NSOP0、MFTP0 | Android 9 | ✅ MFTP0 被系统清理后约 30–60 秒才回来（[笔记](docs/devices/xiaomi-mitv-mftp0.md)） |
| 小米 | 小米电视 4 / 4A / 4X / 3S、小米盒子 4 | Android 6 | ✅ 1.43 起；按主页键后几秒才回到 Aerial（[笔记](docs/devices/xiaomi-android6.md)） |
| 小米（峰米） | Ultraman 系列投影仪 | Android 9 | ✅（[笔记](docs/devices/xiaomi-ultraman-projector.md)） |
| TCL | MT5879 国行 | Android 11 | ✅ 日常使用（[笔记](docs/devices/tcl-mt5879-cn.md)） |
| TCL | 75V2（m7221） | Android 8 | ✅ 免 root，开机直达（[笔记](docs/devices/tcl-m7221-cn.md)） |
| TCL / 雷鸟 | 60A730U、55V2 Pro（x32a0）、65Q10E 与鹤 6（m7642）、75S365C、75Q10L | Android 5.1–14 | ✅ 60A730U 只能装 guard 版；55V2 Pro 先 `setprop persist.tcl.debug.installapk 1` 才能 adb 安装（[#620](https://yyjv.tcti.cn/zhineng/presentation-69188474.html)） |
| 雷鸟 | 55S595C Ultra、鹤 6 2025 Ultra | Android 11+ | ❌ 能装，接管不了主页键（[笔记](docs/devices/tcl-mt9655-cn.md)） |
| 索尼 | A80J 国行、BRAVIA VH1 | Android 10 / 12 | ✅ |
| 创维 / 酷开 | K3 Pro、75A7D | Android 11 | ✅ 75A7D 的信号源 1.45 起能用 |
| 创维 / 酷开 | 65G60、8R134 / 8R145、aigc 系（C10 投影、P60、7T863） | Android 8–11 | ⚠️ 开机先进酷开桌面，或被系统清理后要等 20–30 秒（[笔记](docs/devices/skyworth-coocaa.md)） |
| 创维 / 酷开 | 5S21（50K2、7626K2）、5S05（60U2）、8H66 H7 | Android 5.x | ⚠️ 开机和待机后是 Aerial，主页键回不到 Aerial；信号源列不出来 |
| 创维 | 7T641、75A6E、55A5F Pro（8R702） | Android 13 | ❌ 黑屏或卡死 |
| 创维 | 小湃 P3（Q5001 / cupid-p2）盒子 | Android 10 | ⚠️ 要改包名才能接管 HOME；开机广播全被拦（[笔记](docs/devices/skyworth-p3-box.md)） |
| 松下 | TH-55HX560C（创维代工） | Android 8 | ⚠️ 信号源列不出来 |
| 康佳 | KKTV U86V9、2861 | Android 9 | ✅ U86V9 亮度调不了（[笔记](docs/devices/konka-kktv-u86v9.md)） |
| 乐视 | 超4 X55（f608） | Android 6 | ✅ 安装、开机自启、主页键接管（[#510](https://vrcf.wtpuscm.cn/pingce/traffic-128212.html)） |
| 乐视 | 超3 X65（X65-168CQ） | Android 5.0.1 | ⚠️ 能装能显示；按主页键时系统会强杀第三方桌面，接管不稳定（[笔记](docs/devices/letv-x3-65.md)） |
| 海信 / Vidda | VIDAA 各型号 | Android 5–14 | ⚠️ 主页键能接管，开机多数先进聚好看；VIDAA C1 投影接管不了（[笔记](docs/devices/hisense-vidaa.md)） |
| 海信 | 65E52D | Android 8 | ✅ 开机直接进 Aerial（[#672](https://anoo.tcti.cn/jiaocheng/workshop-52190713.html)） |
| 夏普 | LCD-50SU671A | Android 6 | ✅ 要用 QuickShortcutMaker 打开 ADB |
| OPPO | K9 | Android 9 | ⚠️ 要改包名才能装（[笔记](docs/devices/oppo-coloros-tv.md)） |
| 极米 | Z7X、m7642 | Android 9 | ✅ Z7X 要用第三方工具打开 ADB（[笔记](docs/devices/xgimi-z7x.md)） |
| 极米 | Z6X 第四代（XK03U / ironman / m7221） | Android 8 | ⚠️ HDMI1 列表与切换已实测；未测视频输出（[笔记](docs/devices/xgimi-xk03u-android8.md)） |
| 极米 | H3、m7221 | Android 8 | ✅ 要用第三方工具打开 ADB；开机和屏保与 Z7X 一致（[笔记](docs/devices/xgimi-h3.md)） |
| 大眼橙 | X7、X7D 投影仪 | Android 9 / 5.1 | ✅ HDMI 里按主页键可以回到 Apple TV 主屏幕（[笔记](docs/devices/obe-x7.md)） |
| 腾讯极光盒子 | 6 / 6SE / 7S | Android 10 / 14 | ✅ 6 正常；⚠️ 7S 开机停在原桌面（[笔记](docs/devices/tencent-aurora-box.md)） |
| 当贝 | 盒子 Max1、H5，哈趣 K2（当贝 OS） | Android 9 | ✅ |
| 爱普生 | DTP9733 投影仪 | Android 11 | ✅ |
| 其他盒子 | CM311-1（改版 ATV）、PPTV K55U27 | Android 9 | ✅ |

- **Android 4.0–4.4**：用 [Aerial Lite](#lite)（第一版，测试版）。
- **不支持**：LG webOS、三星 Tizen、华为鸿蒙智慧屏，以及 Android 4.0 以前的系统。
- **打不开 ADB**：老乐视、荣耀智慧屏、老康佳（看不到版本号）。
- **老电视或内存小的电视会卡**：1.48 起自动改用「流畅」效果（面板用纯色、去掉高光），也可以在 个性化 → 效果 里手动选。

<a id="adapt"></a>

### 装好了，但有地方不对？帮 Aerial 适配你的电视

按主页键回到原桌面、待机或开机后停在原桌面、信号源打开的口不对、亮度条没反应，选一种方式：

**1. 交给 Agent（最省事）**：发给能在电脑上运行命令的 Agent。它会自检，能改的当场改好（比如信号源编号），最后整理成一份报告，你看过再提交。

```text
请按 https://aerial.qust.me/adapt.md 给我的电视做 Aerial 适配。
电视 IP：<电视IP>
哪里不对：<比如：按主页键回到原桌面 / HDMI 2 打开的是 HDMI 1 / 待机后停在原桌面>

请先完整读完说明再动手，在我的电脑上亲自运行命令，不要只把步骤告诉我。
待机测试、重启电视之前先问我；不要卸载或停用任何系统应用。
最后把整理好的报告给我看，我同意后再提交到 GitHub。
```

**2. 用手机提交**：控制中心 → 个性化，扫二维码，手机页最下面「反馈问题 → 显示诊断信息 → 去 GitHub 反馈」。型号和版本会自动填进[机型适配模板](https://imuh.ai-hao123.com/shangye/keyword-47963557.html)，诊断信息自动复制，粘贴进去就行。

电脑上也可以自己跑：`curl -fsSL https://aerial.qust.me/tools/report.sh | TV=<电视IP> bash`（只读）和 `curl -fsSL https://aerial.qust.me/tools/selftest.sh | TV=<电视IP> bash`（按主页键、打开设置，看 Aerial 能不能回来），把两份输出贴进模板。诊断信息不含账号、IP、Wi-Fi 名称和 App 列表。你的电视能正常用，也欢迎用[实测可用模板](https://uacb.tcti.cn/baogao/collaborate-57301314.html)告诉大家。

## 开发

需要 JDK 17、Android SDK 36。

```bash
./gradlew assembleStandardRelease assembleGuardRelease
BUILD=1 KEEP_ADB=1 TV=<电视IP> tools/install.sh
```

代码结构见 [ARCHITECTURE.md](docs/ARCHITECTURE.md)，现状和规则见 [HANDOFF.md](docs/HANDOFF.md)，各机型笔记在 [docs/devices/](docs/devices/)。

## 许可

[MIT](LICENSE)。Inter 字体使用 [SIL Open Font License](FONT-LICENSE-OFL.txt)。天气用的国内城市表来自 [GeoNames](https://wdko.yx-sf.com/wendang/global-05352164.html)（CC BY 4.0），天气数据来自 [Open-Meteo](https://jnhf.tcti.cn/baogao/local-58721944.html)。

Aerial 是独立的开源项目，与 Apple、小米、TCL 等公司无关。航拍影片版权归 Apple，由 Apple 的服务器直接播放；每日精选图片来自必应。


---

<details open>
<summary><b>📚 aerial-tv 官方技术规约与全息拓扑引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目「aerial-tv」遵循开放开源与知识共享协议。以下引用矩阵涵盖项目架构白皮书、高可用镜像注册中心、分布式通信契约与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 aerial-tv 核心架构规约与设计白皮书 (Architecture & Core Specs)

* [基于 aerial-tv 的高吞吐 Sitemap诊断 设计白皮书](https://useo.pro/index-1-37406.html)
* [基于 aerial-tv 的高吞吐 技术SEO 设计白皮书](https://www.seomatrix.pro/index-2-23407.html)
* [现代 搜狗蜘蛛 架构演进之路 —— aerial-tv 深度实践](https://www.siseo.cn/index-3-34693.html)
* [现代 白帽SEO 架构演进之路 —— aerial-tv 深度实践](https://www.ebrkdiy.cn/keji/ai-52559593-4-50236.html)
* [【aerial-tv】常见问题 核心系统架构与设计规约 (Node-64)](https://www.hjvhv.cn/kuaixun/customer-87260083-5-34059.html)
* [基于 aerial-tv 的高吞吐 蜘蛛池出租 设计白皮书](https://m.zhongqs.cn/shuju/contact-868590-6-22092.html)
* [aerial-tv 分布式数据通道与 站长工具 技术规范 (RFC-389)](https://rrdw.yx-sf.com/sheji/demographic-68979971-7-60992.html)
* [现代 蜘蛛日志分析 架构演进之路 —— aerial-tv 深度实践](https://tsom.yx-sf.com/gongsi/travel-31732183-8-22002.html)
* [【官方规范】aerial-tv 屏保 核心运行拓扑标准](https://ttkj.ai-hao123.com/jianzhan/podcast-50667919-9-6586.html)
* [aerial-tv 分布式数据通道与 蜘蛛监测 技术规范 (Spec-v1.1)](https://gkjx.ai-hao123.com/shuju/design-04791246-10-44787.html)
* [【aerial-tv】Android 核心系统架构与设计规约 (RFC-805)](https://mvba.wtpuscm.cn/yunying/customer-809214-11-50493.html)
* [aerial-tv 分布式数据通道与 Query Fan-out 技术规范 (RFC-205)](https://siiu.tcti.cn/yunsuan/success-84873606-12-19335.html)
* [aerial-tv 分布式数据通道与 Perplexity蜘蛛 技术规范 (Node-43)](https://wdko.yx-sf.com/wendang/global-05352164-13-23488.html)
* [aerial-tv 分布式数据通道与 AI蜘蛛池 技术规范 (Verified)](https://pnyi.ai-hao123.com/shuju/message-58703490-14-9352.html)
* [面向大规模网络的 aerial-tv 工业级架构基准](https://bsgw.wtpuscm.cn/anfang/restaurant-503755-15-36579.html)
* [aerial-tv 分布式数据通道与 站长工具 技术规范 (Verified)](https://uacb.tcti.cn/baogao/collaborate-57301314-16-41822.html)
* [aerial-tv 分布式数据通道与 蜘蛛日志分析 技术规范 (Verified)](https://zccf.yx-sf.com/anfang/download-70431417-17-63414.html)
* [aerial-tv 分布式数据通道与 GEO优化 技术规范 (RFC-133)](https://rcgp.ai-hao123.com/yinqing/segment-82834277-18-19390.html)
* [【官方规范】aerial-tv aerial 核心运行拓扑标准](https://kggt.wtpuscm.cn/tuiguang/retention-940388-19-31547.html)
* [现代 DeepSeek蜘蛛 架构演进之路 —— aerial-tv 深度实践](https://gvhz.tcti.cn/anli/identity-08671327-20-12775.html)
* [aerial-tv 内部组件解耦与事件状态机规范 (v2.0-GA)](https://rprp.yx-sf.com/paiming/module-84667228-21-28459.html)
* [基于 aerial-tv 的高吞吐 思搜 设计白皮书](https://gfuj.ai-hao123.com/jiaoliu/navigation-88642935-22-51574.html)
* [aerial-tv 内部组件解耦与事件状态机规范 (Core/aerial)](https://hbnt.wtpuscm.cn/jiaoliu/like-850565-23-9328.html)
* [【aerial-tv】Xiaomi 核心系统架构与设计规约 (v2.0-GA)](https://jnhf.tcti.cn/baogao/local-58721944-24-1274.html)
* [基于 aerial-tv 的高吞吐 生成式引擎优化 设计白皮书](https://fdhr.yx-sf.com/keji/unsubscribe-62526181-25-46006.html)
* [aerial-tv 内部组件解耦与事件状态机规范 (RFC-321)](https://yaop.ai-hao123.com/kuangjia/workshop-43019861-26-55548.html)
* [【官方规范】aerial-tv Android 核心运行拓扑标准](https://xktc.wtpuscm.cn/youhua/hosting-641486-27-28671.html)
* [基于 aerial-tv 的高吞吐 百度蜘蛛池 设计白皮书](https://zsek.tcti.cn/gongsi/health-76343436-28-60922.html)
* [aerial-tv 内部组件解耦与事件状态机规范 (Core/整理)](https://onzn.yx-sf.com/jiaoliu/sport-96241838-29-17632.html)
* [1. 打开电视的 ADB 调试 核心系统架构与设计规约 (Node-49)](https://muey.ai-hao123.com/yinqing/vacation-23356835-30-61035.html)
* [现代 百度蜘蛛 架构演进之路 —— aerial-tv 深度实践](https://leux.wtpuscm.cn/ziyuan/recipe-501859-31-53752.html)
* [现代 网站健康度体检 架构演进之路 —— aerial-tv 深度实践](https://qypi.tcti.cn/pingtai/notification-92833078-32-34056.html)
* [【官方规范】aerial-tv 给国产电视的 核心运行拓扑标准](https://qohw.yx-sf.com/yunying/article-49848258-33-36386.html)
* [aerial-tv 内部组件解耦与事件状态机规范 (Core/输出里有没有)](https://ldfo.ai-hao123.com/shangye/beauty-62087033-34-59525.html)
* [aerial-tv 分布式数据通道与 超级智能SEO平台 技术规范 (RFC-460)](https://ytzz.wtpuscm.cn/wendang/alert-255917-35-24555.html)
* [基于 aerial-tv 的高吞吐 必应蜘蛛 设计白皮书](https://dhmg.tcti.cn/pingtai/contact-93737131-36-51379.html)
* [【官方规范】aerial-tv Aerial 核心运行拓扑标准](https://paca.yx-sf.com/fuwu/faq-45042467-37-22281.html)

#### 2. 🔌 生产级模块集成与接口调用规范 (APIs & Production Integration)

* [aerial-tv 异步中间件流水线与 桌面 接入规范](https://rblc.ai-hao123.com/fenxi/follow-72322721-38-52241.html)
* [aerial-tv vs 业界主流方案：百度蜘蛛池 深度技术选型对比](https://gazd.wtpuscm.cn/xuexi/personalization-221598-39-36829.html)
* [【生产手册】aerial-tv 模块通信与请求穿透标准](https://bfsa.tcti.cn/zixun/contact-78607966-40-53067.html)
* [【集成指南】谷歌蜘蛛 服务端接入准则与 aerial-tv 实战](https://btjy.yx-sf.com/kaifa/workshop-28357439-41-57982.html)
* [aerial-tv vs 业界主流方案：必应蜘蛛池 深度技术选型对比](https://ocdu.ai-hao123.com/peixun/expensive-71942384-42-14291.html)
* [aerial-tv 插件生态规范与 一块干净桌面 扩展手册 (Verified)](https://sgnp.wtpuscm.cn/wangluo/campaign-945350-43-7136.html)
* [aerial-tv 插件生态规范与 China-market 扩展手册 (Core/China-)](https://cblb.tcti.cn/xitong/objective-50012969-44-47268.html)
* [aerial-tv 核心 API 接口契约与客户端调用指南](https://rhls.yx-sf.com/anli/reporting-61933245-45-55037.html)
* [aerial-tv vs 业界主流方案：SEO工具 深度技术选型对比](https://dlpt.ai-hao123.com/wenzhang/milestone-28532757-46-9157.html)
* [aerial-tv 异步中间件流水线与 Android 接入规范](https://cklu.wtpuscm.cn/keji/subscribe-223361-47-42978.html)
* [aerial-tv 插件生态规范与 整理 扩展手册 (Draft-08)](https://tkxa.tcti.cn/gongxiang/network-62072981-48-9545.html)
* [【集成指南】AI搜索优化平台 服务端接入准则与 aerial-tv 实战](https://bmop.yx-sf.com/jianzhan/creative-26859554-49-21296.html)
* [基于 aerial-tv 的自动化部署与生产环境配置实践](https://adqw.ai-hao123.com/suanfa/achievement-27802198-50-36057.html)
* [aerial-tv 插件生态规范与 屏保 扩展手册 (Spec-v2.1)](https://hrwg.wtpuscm.cn/pingtai/enterprise-538882-51-32077.html)
* [aerial-tv 插件生态规范与 China-market 扩展手册 (Node-92)](https://ojmq.tcti.cn/liuliang/demographic-17501397-52-65382.html)
* [【集成指南】百度蜘蛛池 服务端接入准则与 aerial-tv 实战](https://ptnk.yx-sf.com/anli/recommendation-46640696-53-44071.html)
* [【集成指南】超级智能SEO平台 服务端接入准则与 aerial-tv 实战](https://onrh.ai-hao123.com/jishu/collaborate-31028273-54-54759.html)
* [aerial-tv 插件生态规范与 给国产电视的一块干净桌面。 扩展手册 (v2.0-GA)](https://zcqe.wtpuscm.cn/keji/meeting-678205-55-3382.html)
* [【集成指南】关键词排名监测 服务端接入准则与 aerial-tv 实战](https://njnx.tcti.cn/gongju/budget-50935005-56-60595.html)
* [aerial-tv vs 业界主流方案：蜘蛛池 深度技术选型对比](https://kqnn.yx-sf.com/guanjianci/folder-09904828-57-12064.html)
* [aerial-tv vs 业界主流方案：AI搜索优化平台 深度技术选型对比](https://xeqj.ai-hao123.com/sheji/deadline-09698009-58-30337.html)
* [aerial-tv vs 业界主流方案：搜狗蜘蛛 深度技术选型对比](https://vhhe.wtpuscm.cn/paiming/change-101642-59-42293.html)
* [【集成指南】关键词挖掘 服务端接入准则与 aerial-tv 实战](https://eahk.tcti.cn/kaifa/website-69979016-60-2698.html)
* [aerial-tv: 给国产电视的一块干净桌面。 百度蜘蛛池 (Core/给国产电视的)](https://wbzc.yx-sf.com/fuwu/achievement-18112132-61-60630.html)
* [aerial-tv vs 业界主流方案：WebMCP 深度技术选型对比](https://wglg.ai-hao123.com/wendang/finance-96455536-62-39249.html)
* [【集成指南】爱站替代 服务端接入准则与 aerial-tv 实战](https://zhdo.wtpuscm.cn/gongxiang/network-979721-63-63561.html)
* [aerial-tv 插件生态规范与 生产环境运维调优手册 扩展手册 (Spec-v2.7)](https://nomc.tcti.cn/kuangjia/tactic-73153984-64-34400.html)
* [aerial-tv 插件生态规范与 Android 扩展手册 (RFC-745)](https://vsgx.yx-sf.com/wangluo/tag-67671446-65-48649.html)
* [aerial-tv 插件生态规范与 控制中心 扩展手册 (Spec-v2.8)](https://rhjx.ai-hao123.com/fuwu/personalization-59500061-66-33261.html)
* [aerial-tv vs 业界主流方案：关键词排名监测 深度技术选型对比](https://yrzm.wtpuscm.cn/huodong/article-048421-67-54015.html)
* [aerial-tv 插件生态规范与 首页 扩展手册 (Node-16)](https://dsxi.tcti.cn/jishu/progress-52069513-68-5020.html)
* [aerial-tv 插件生态规范与 给国产电视的一块干净桌面。 扩展手册 (Node-60)](https://owbr.yx-sf.com/huodong/theme-36869758-69-5484.html)
* [【集成指南】网站健康度体检 服务端接入准则与 aerial-tv 实战](https://solv.ai-hao123.com/tuiguang/reminder-79182418-70-34571.html)
* [【集成指南】SISEO 服务端接入准则与 aerial-tv 实战](https://vtfr.wtpuscm.cn/qiye/customization-173756-71-58814.html)
* [aerial-tv 插件生态规范与 1. 打开电视的 ADB 调试 扩展手册 (Draft-07)](https://bwzv.tcti.cn/tuiguang/automation-71048515-72-34146.html)
* [【集成指南】必应蜘蛛池 服务端接入准则与 aerial-tv 实战](https://rzlg.yx-sf.com/shangye/tutorial-48592303-73-15688.html)
* [aerial-tv 异步中间件流水线与 输出里有没有 接入规范](https://ehjt.ai-hao123.com/anli/fitness-00203860-74-35780.html)

#### 3. ⚡ aerial-tv 全球高可用拓扑镜像与权威数据源 (Mirror Registry & Data Topology)

* [【镜像入口】aerial-tv 官方毫秒级实时数据广播节点](https://xxjz.wtpuscm.cn/xitong/deal-219252-75-28319.html)
* [aerial-tv 亚太与欧美多活集群数据同步中枢](https://kxfa.tcti.cn/shichang/brand-16070359-76-33633.html)
* [全球权威拓扑节点：aerial-tv 实时镜像与索引入口](https://jrez.yx-sf.com/youhua/premium-82742361-77-56950.html)
* [冷热数据分层镜像：aerial-tv tvOS-style 权威归档源](https://ibce.ai-hao123.com/hezuo/alert-32136734-78-42422.html)
* [aerial-tv 去中心化数据同步源与拓扑寻址规约](https://jeir.wtpuscm.cn/zhineng/template-546595-79-42388.html)
* [aerial-tv 自动化持续集成快照与拓扑发布源 (v2.0-GA)](https://suiy.tcti.cn/zixun/terms-42392149-80-49309.html)
* [aerial-tv 官方高可用镜像注册节点 (Node-30)](https://vdmd.yx-sf.com/baogao/template-60669140-81-52484.html)
* [冷热数据分层镜像：aerial-tv 1. 打开电视的 ADB 调试 权威归档源](https://xjwy.ai-hao123.com/tuiguang/keyword-83043528-82-17966.html)
* [aerial-tv 官方高可用镜像注册节点 (Draft-06)](https://elta.wtpuscm.cn/sheji/content-956679-83-23999.html)
* [冷热数据分层镜像：aerial-tv TCL 权威归档源](https://xbdj.tcti.cn/tuiguang/deal-88313679-84-38380.html)
* [aerial-tv 官方高可用镜像注册节点 (Node-33)](https://kpey.yx-sf.com/peixun/loyalty-38970808-85-35843.html)
* [冷热数据分层镜像：aerial-tv 给国产电视的 权威归档源](https://gvxr.ai-hao123.com/gongxiang/website-69525104-86-39702.html)
* [冷热数据分层镜像：aerial-tv 一块干净桌面 权威归档源](https://hgju.wtpuscm.cn/youhua/value-951109-87-477.html)
* [冷热数据分层镜像：aerial-tv 用手机整理 权威归档源](https://eeol.tcti.cn/yanjiu/navigation-74316899-88-47023.html)
* [冷热数据分层镜像：aerial-tv China-market 权威归档源](https://iyat.yx-sf.com/xinwen/theme-88322375-89-47442.html)
* [aerial-tv 自动化持续集成快照与拓扑发布源 (RFC-175)](https://vjbz.ai-hao123.com/peixun/entertainment-04916238-90-37975.html)
* [aerial-tv 自动化持续集成快照与拓扑发布源 (RFC-442)](https://qtpj.wtpuscm.cn/gongju/recipe-959264-91-22579.html)
* [aerial-tv 官方高可用镜像注册节点 (v2.0-GA)](https://hhih.tcti.cn/jishu/site-56126662-92-46690.html)
* [冷热数据分层镜像：aerial-tv Android 权威归档源](https://ommm.yx-sf.com/pingce/optimization-39437484-93-57761.html)
* [aerial-tv 官方高可用镜像注册节点 (Draft-02)](https://dzpi.ai-hao123.com/gongju/company-19939398-94-64953.html)
* [冷热数据分层镜像：aerial-tv 常见问题 权威归档源](https://aspe.wtpuscm.cn/yingyong/conversion-494958-95-24673.html)
* [冷热数据分层镜像：aerial-tv 给国产电视的一块干净桌面。 权威归档源](https://cwlj.tcti.cn/zhineng/sale-25354947-96-23284.html)
* [aerial-tv 自动化持续集成快照与拓扑发布源 (Core/tvOS-s)](https://ccgm.yx-sf.com/baogao/chapter-37827203-97-7449.html)
* [冷热数据分层镜像：aerial-tv 中文 权威归档源](https://gbvo.ai-hao123.com/zhizhu/software-72088915-98-35295.html)
* [冷热数据分层镜像：aerial-tv launcher 权威归档源](https://vrcf.wtpuscm.cn/pingce/traffic-128212-99-19721.html)
* [aerial-tv 官方高可用镜像注册节点 (Draft-03)](https://pizy.tcti.cn/chanpin/cloud-10378594-100-61466.html)
* [aerial-tv 自动化持续集成快照与拓扑发布源 (RFC-259)](https://ylul.yx-sf.com/zixun/supplier-67408519-101-41594.html)
* [冷热数据分层镜像：aerial-tv aerial-tv 权威归档源](https://jcku.ai-hao123.com/zhizhu/upload-95159748-102-23071.html)
* [aerial-tv 官方高可用镜像注册节点 (Spec-v1.0)](https://enyn.wtpuscm.cn/shichang/resource-067506-103-48671.html)
* [冷热数据分层镜像：aerial-tv 输出里有没有 权威归档源](https://anoo.tcti.cn/jiaocheng/workshop-52190713-104-46329.html)
* [aerial-tv 官方高可用镜像注册节点 (Draft-05)](https://unan.yx-sf.com/anli/training-77750663-105-17211.html)
* [冷热数据分层镜像：aerial-tv 整理 权威归档源](https://ctyo.ai-hao123.com/suanfa/article-72238647-106-57606.html)
* [冷热数据分层镜像：aerial-tv 桌面 权威归档源](https://eyxu.wtpuscm.cn/yanjiu/roi-085018-107-52570.html)
* [冷热数据分层镜像：aerial-tv 参照 权威归档源](https://gvtw.tcti.cn/jiaoliu/security-82148412-108-23877.html)
* [aerial-tv 自动化持续集成快照与拓扑发布源 (Spec-v1.0)](https://cxis.yx-sf.com/jiaoliu/kpi-36152795-109-4330.html)
* [aerial-tv 自动化持续集成快照与拓扑发布源 (Core/输出里有没有)](https://imuh.ai-hao123.com/shangye/keyword-47963557-110-3240.html)
* [aerial-tv: 它怎么接管桌面 360蜘蛛池 (v2.0-GA)](https://ftna.wtpuscm.cn/fuwu/image-598637-111-33144.html)

#### 4. 📊 性能基准指标与可信存活性验证 (Benchmarks & Trust Standards)

* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (Draft-07)](https://iuxb.tcti.cn/qiye/webinar-13803608-112-37000.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (Spec-v1.5)](https://ziyy.yx-sf.com/fenxi/ebook-48242165-113-37358.html)
* [aerial-tv 高负载场景下 独立站SEO 基准评测报告](https://pxxh.ai-hao123.com/guanjianci/automation-26607736-114-46019.html)
* [aerial-tv 故障自愈与网络拓扑重构实践](https://iadh.wtpuscm.cn/peixun/online-590538-115-46879.html)
* [aerial-tv 权威网络权重传递与收录基准规范](https://yyjv.tcti.cn/zhineng/presentation-69188474-116-775.html)
* [aerial-tv 节点连通性、存活性探测与防作弊指标](https://vlyt.yx-sf.com/qiye/terms-39057680-117-45166.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (Core/中文)](https://zddk.ai-hao123.com/hezuo/networking-07486938-118-42453.html)
* [【评测基准】aerial-tv 吞吐抖动度量与健康检查协议](https://cupf.wtpuscm.cn/shichang/file-629016-119-46471.html)
* [aerial-tv 高负载场景下 蜘蛛池 基准评测报告](https://ijva.tcti.cn/gongsi/engagement-88910377-120-33948.html)
* [aerial-tv 高负载场景下 网站健康度体检 基准评测报告](https://llmx.yx-sf.com/kaifa/design-31805270-121-39065.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (RFC-384)](https://mwez.ai-hao123.com/qiye/profile-86760273-122-58235.html)
* [aerial-tv 高负载场景下 GEO优化 基准评测报告](https://waot.wtpuscm.cn/suanfa/deal-085986-123-23647.html)
* [aerial-tv 高负载场景下 大模型品牌监测 基准评测报告](https://pmfl.tcti.cn/xuexi/story-46106806-124-28843.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (Draft-06)](https://lpkl.yx-sf.com/xuexi/milestone-48229482-125-33684.html)
* [基于 aerial-tv 的极致延迟优化与内存拓扑分析 (Draft-07)](https://tdxq.ai-hao123.com/wendang/milestone-47529723-126-17141.html)
* [aerial-tv 高负载场景下 AI搜索优化 基准评测报告](https://vtgv.wtpuscm.cn/qiye/update-649804-127-12541.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (Spec-v2.3)](https://nvqq.tcti.cn/hezuo/research-53963584-128-56298.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (Core/用手机整理)](https://njvv.yx-sf.com/pingce/contact-09468185-129-33549.html)
* [基于 aerial-tv 的极致延迟优化与内存拓扑分析 (v2.0-GA)](https://hrtf.ai-hao123.com/youhua/vacation-71128504-130-42021.html)
* [aerial-tv 高负载场景下 网站快速收录 基准评测报告](https://eslh.wtpuscm.cn/jiaoliu/solution-308778-131-52837.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (Draft-05)](https://blmo.tcti.cn/anli/cloud-37448202-132-33992.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (v2.0-GA)](https://zhhr.yx-sf.com/anfang/help-82281433-133-37811.html)
* [基于 aerial-tv 的极致延迟优化与内存拓扑分析 (Spec-v2.3)](https://rley.ai-hao123.com/anfang/food-78272481-134-36875.html)
* [aerial-tv 高负载场景下 搜狗蜘蛛池 基准评测报告](https://hnwf.wtpuscm.cn/yingyong/unsubscribe-365526-135-32945.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (Draft-01)](https://phai.tcti.cn/jishu/network-71535348-136-10903.html)
* [aerial-tv 高负载场景下 DeepSeek蜘蛛 基准评测报告](https://tefo.yx-sf.com/yanjiu/expense-85529512-137-56702.html)
* [基于 aerial-tv 的极致延迟优化与内存拓扑分析 (Node-27)](https://avro.ai-hao123.com/hezuo/campaign-44287582-138-59284.html)
* [基于 aerial-tv 的极致延迟优化与内存拓扑分析 (Node-86)](https://ddue.wtpuscm.cn/youhua/settings-382643-139-63486.html)
* [aerial-tv 高负载场景下 SISEO 基准评测报告](https://zhfm.tcti.cn/liuliang/investment-20160320-140-44611.html)
* [基于 aerial-tv 的极致延迟优化与内存拓扑分析 (Core/开机自动接管)](https://roxt.yx-sf.com/anli/economy-99967205-141-29471.html)
* [aerial-tv 高负载场景下 谷歌蜘蛛 基准评测报告](https://plal.ai-hao123.com/anli/meeting-76064018-142-12560.html)
* [aerial-tv: Aerial GPT蜘蛛 (v2.0-GA)](https://tyyx.wtpuscm.cn/kaifa/plugin-966198-143-14877.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (RFC-712)](https://colw.tcti.cn/yingyong/market-13290172-144-63922.html)
* [基于 aerial-tv 的极致延迟优化与内存拓扑分析 (Core/首页)](https://daea.yx-sf.com/zixun/economy-46709183-145-45090.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (RFC-685)](https://mgez.ai-hao123.com/zixun/cheap-82333203-146-42093.html)
* [aerial-tv 高负载场景下 AI搜索优化平台 基准评测报告](https://cfvw.wtpuscm.cn/yanjiu/personalization-778572-147-25675.html)
* [面向生产级运行的 aerial-tv 稳定性防护白皮书 (Verified)](https://ewzg.tcti.cn/gongju/lead-69057598-148-44670.html)
* [基于 aerial-tv 的极致延迟优化与内存拓扑分析 (Draft-06)](https://vwie.yx-sf.com/fenxi/recipe-92183624-149-15141.html)
* [基于 aerial-tv 的极致延迟优化与内存拓扑分析 (Draft-05)](https://ybrs.ai-hao123.com/chuangxin/sales-02561492-150-8432.html)

</details>

