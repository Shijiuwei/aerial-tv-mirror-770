# 小米 MiTV-MFTP0（codename alita）

- Android 9（SDK 28），build `23.5.23.6436`（incremental `6436`），MediaTek `m7642`（`ro.product.board=alita`），32 位 ARM（`armeabi-v7a,armeabi`），内存 2704376 kB（约 2.6 GB）。
- 1920×1080，320dpi（960×540dp）。
- 原桌面：`com.mitv.tvhome/.MainActivityUserMode`（PatchWall，`versionName=3.50.1621`）。
- 安装的 flavor：`standard`。普通的 `adb install` 就能装。
- 2026-10-21 在这台电视上实测，从 1.14（`versionCode=15`）升级到 1.16（`versionCode=18`）。

## 和 Android 14 机型的差异（重要）

这台是 **Android 9 / SDK 28**，比 [xiaomi-mitv-mftr0.md](xiaomi-mitv-mftr0.md) 和 [xiaomi-mitv-mffu0.md](xiaomi-mitv-mffu0.md) 记录的 Android 14 老很多。已知的 HOME 锁定结论大致沿用，但**升级后守护会失效**，而且**复活机制完全不工作**，见下文。

## 覆盖安装/升级后 Home Guard 绑不上（最关键）

升级 App 后桌面会退回 PatchWall，**不是被小米杀掉，是无障碍服务根本没绑上**。机理是 `system_server` 持有一条失效的 APK 路径。

升级到 1.16 后 logcat 里立刻出现：

```text
W ziparchive: Unable to open '/data/app/app.orchard.tv-EQ7BsdEf0tfGxmIzz_rO1g==/base.apk': No such file or directory
E system_server: Failed to open APK '/data/app/app.orchard.tv-EQ7BsdEf0tfGxmIzz_rO1g==/base.apk' I/O error
E ResourcesManager: failed to add asset path /data/app/app.orchard.tv-EQ7BsdEf0tfGxmIzz_rO1g==/base.apk
W PackageManager: Failure retrieving resources for app.orchard.tv
```

对照实际路径：

```text
错误里的路径 : /data/app/app.orchard.tv-EQ7BsdEf0tfGxmIzz_rO1g==/base.apk   ← 已被删除
真实的 codePath: /data/app/app.orchard.tv-3cMB0fugJLqv9l5cVlhkpA==/base.apk   ← 升级后的新目录
```

**怎么认出这个状态**：设置里有、`dumpsys` 里没有。

```text
$ adb shell settings get secure enabled_accessibility_services
...:app.orchard.tv/app.orchard.tv.guard.HomeGuardService        ← 设置里在

$ adb shell dumpsys accessibility | grep -iE "label=Aerial"
（空）                                                           ← 系统里没有
```

**排查时必须看 `dumpsys accessibility`，不要看 `enabled_accessibility_services`**——设置里的记录是好的，看它会误判成「已就绪」。

### 恢复方式（已验证有效，不需要重装）

```bash
adb shell am force-stop app.orchard.tv
sleep 3
adb shell am start -W -n app.orchard.tv/.MainActivity
```

执行后守护立刻绑定，前台回到 Aerial。`force-stop` 让系统重新走一遍资源加载，路径就对了。也不需要重启电视。

## 通知监听绑不上（Android 9 的第二个坑）

`RevivalListener`（被杀后拉回来的机制）**在这台上无法绑定**，所以一旦守护失效，没有任何东西能自动恢复。1.16 上复测仍然如此。

```text
$ adb shell cmd notification allow_listener app.orchard.tv/app.orchard.tv.guard.RevivalListener
EXIT=0                                            # ← 报告成功

$ adb shell dumpsys notification | grep -A4 "Notification listeners:"
  Notification listeners:
    Allowed notification listeners:               # ← 空
    All notification listeners (0) enabled for current profiles:
    Live notification listeners (0):
```

试过的其他写法，都不行：

| 写法 | 结果 |
| --- | --- |
| `cmd notification allow_listener <COMPONENT>` | `EXIT=0`，不写入 |
| `cmd notification allow_listener <COMPONENT> 0`（positional user_id） | `EXIT=0`，不写入 |
| `cmd notification allow_listener --user 0 <COMPONENT>` | `Invalid listener - must be a ComponentName`，`EXIT=255`（Android 9 不认这个写法） |
| `settings put secure enabled_notification_listeners <COMPONENT>` | **能写入并读回**，但 `NotificationManagerService` 从不加载它，`dumpsys` 里始终 0 绑定 |
| `appops set app.orchard.tv NOTIFICATION_LISTENER allow` | `Unknown operation string`（Android 9 没有这个 op） |
| `appops set app.orchard.tv BIND_NOTIFICATION_LISTENER_SERVICE allow` | `Unknown operation string` |
| `appops set app.orchard.tv ACCESS_NOTIFICATIONS allow` | `EXIT=0`，但对绑定无帮助 |

**电视上没有手动开启的入口**：`android.settings.NOTIFICATION_LISTENER_SETTINGS` 解析不到任何 Activity，用户没法在设置里自己打开。

对照 [xiaomi-mitv-mffu0.md](xiaomi-mitv-mffu0.md)（Android 14）「约 0.5 秒通知监听就被绑定」，**Android 9 上这条路径整个是失效的**。`install.sh` 里这行是 `|| true`，所以静默失败，安装时看不出来。

**根因（2026-10-21 查清）**：这台 `ro.config.low_ram=true`。Android 9 的 `NotificationManagerService.canUseManagedServices()` 在低内存设备上对所有非白名单包返回 false（白名单是 framework 资源 `config_allowedManagedServicesOnLowRamDevices`），`allow_listener`、`allow_dnd` 和读 `notification_policy.xml` 时都会过滤掉，所以通知监听、通知助手、勿扰条件提供者（ConditionProvider）三条路全部不通。`settings put secure enabled_notification_listeners` 也没用：Android 8.1 起已批准列表存在 `/data/system/notification_policy.xml`，这个设置只在迁移时读。`ro.*` 属性改不了，这条路在这台上没有办法。

## 待机强停后的复活：同步适配器（1.21）

改用 Android 的同步框架拉起 Aerial（`guard/RevivalSync.kt`）：

- Aerial 自己建一个账号（类型 `app.orchard.tv`，设置 → 账号里能看到「Aerial 桌面守护」），在 `settings` 这个 authority 上注册同步适配器，`supportsUploading=true`。
- `SettingsProvider` 每次写设置都 `notifyChange(uri, null, syncToNetwork=true)`，`ContentService` 因此对 `settings` 安排一次本地同步，延迟 `sync.local_sync_delay`（默认 30 秒）。
- 强停本身就会触发它：`AccessibilityManagerService.onHandleForceStop` 把 Aerial 从 `enabled_accessibility_services` 里删掉，这就是一次设置写入。小米唤醒时把 `adb_enabled` 改成 0 也是。
- 同步任务的 JobScheduler 任务属于系统 uid，`cancelJobsForPackageAndUid` 只取消 Aerial 自己 uid 的任务，所以强停不会把它取消；`SyncManager` 绑定适配器服务时，系统会为已强停的包启动进程。
- 适配器只在守护没在运行、且守护确实被删掉需要写回时才算「复活」，否则什么都不做（每次改设置都会跑一次）。熄屏时的复活不计入 LeTV 的「频繁被杀」判断（`GuardSetup.noteRevival`）。
- 需要联网（同步任务要求有网络）和系统的自动同步开着（默认开）。

验证情况：

| 环境 | 结果 |
| --- | --- |
| Android 9 模拟器（`system-images;android-28;default;arm64-v8a`，`setprop ro.config.low_ram true` 后重启框架，`meminfo` 显示 `low-ram`） | `allow_listener` 静默失败，复现了这台的现象。原生桌面在前台时 `am force-stop`：进程一直不在，49 秒后 `Start proc … for service app.orchard.tv/.guard.RevivalSyncService`，守护写回并接管桌面 |
| 同上，熄屏 → 强停 → 待机 60 秒 → 唤醒，连做 3 轮 | 每轮强停后约 30 秒复活，唤醒后 2 秒前台是 Aerial |
| 同上，只待机 15 秒，做 2 轮 | 唤醒后 22–25 秒才回到 Aerial（等同步的 30 秒延迟） |
| 小米 MFTR0（Android 14，固件同样有 `MitvActivityManagerService`），临时 `disallow_listener` 后 `am force-stop` | 46 秒后 `Start proc … for service RevivalSyncService`，**小米固件放行了同步框架拉起已强停的应用**。测完已 `allow_listener` 恢复 |
| 小米 MFTR0，通知监听开着时强停 | 照旧 0.3 秒由通知监听复活，同步适配器之后看到守护在运行，不重复动作 |

**这台 MFTP0 本身还没有用 1.21 实测**，需要用户在电视上验证真实遥控器待机。

## 两个缺陷叠加才会丢桌面

单独任何一个都不至于完全没救：

| 缺陷 | 后果 | 兜底 |
| --- | --- | --- |
| 升级后 APK 路径失效 → 守护绑不上 | HOME 退回 PatchWall | 重启 App 即可恢复 |
| 通知监听绑不上 | 被杀后无法立刻复活 | 1.21 起：同步适配器约 30–60 秒后复活 |

所以 1.20 及以前的症状是「装完正常、升级或待机后桌面消失、且再也回不来」。**`KEEP_ADB` 同样会丢**——它依赖 Aerial 的进程存活。1.21 起同步适配器复活后会先把 ADB 打开。

2026-10-21 一位用户在 1.18 上用实体遥控器测到了待机后的结果：唤醒后停在 PatchWall，ADB 被关（`Connection refused`），手动开 ADB 后 `stopped=true`、`enabled_accessibility_services` 里没有 Aerial、守护没在运行。这说明**这台的遥控器待机确实会强停 Aerial**（下文「待机与唤醒」那次没进入待机的测试不算数）。

## HOME 与守护

- 和 MFTR0 一样 HOME 被锁给 `com.mitv.tvhome`，靠桌面守护（无障碍）接管。绑定正常时日志是 `Service[label=Aerial 桌面守护, capabilities=8, eventTypes=TYPE_WINDOW_STATE_CHANGED]`。
- `enabled_accessibility_services` 用追加写入，原有的 `com.xiaomi.mitv.karaoke.service/...PuremicKeyService` 和 `com.xiaomi.voicecontrol/...VoiceAccessibilityService` 都要保留（这台有两个 voicecontrol 条目，写入时不要去重）。
- 用 `adb shell input keyevent KEYCODE_HOME` 注入的 HOME 不经过无障碍，**不能用来判断守护是否生效**，必须用实体遥控器。

## 开机自启

- `BootReceiver` + `RECEIVE_BOOT_COMPLETED: granted=true` 都在，**冷启动自启正常**。
- 2026-10-21 实测 `adb reboot`：约 33 秒后 `sys.boot_completed=1`，此时前台已经是 `app.orchard.tv/.MainActivity`，没有落到 PatchWall。
- 开机后 `settings get global adb_enabled` 仍是 `1`（装了 `KEEP_ADB=1`）。
- 未停用任何系统应用。

## 待机与唤醒（未测准）

**没有测成。** 2026-10-21 试了一次遥控器电源键待机再唤醒，但日志显示 `mWakefulness` 前后都是 `Awake`，logcat 时间戳连续无缺口（网络 ADB 没断），说明**电视并没有真正进入待机**。所以这台待机唤醒后会怎样，目前未知。

要测的话建议用 adb 触发，比遥控器可靠：

```bash
adb shell input keyevent KEYCODE_POWER   # 待机
sleep 60
adb shell input keyevent KEYCODE_POWER   # 唤醒
```

## 出厂就停用的两个包（不是安装造成的）

这台上 `pm list packages -d` 有两个包，**先于 Aerial 安装就已停用**，和 `install.sh` 无关（脚本全文没有 `pm disable` / `uninstall`，两个包名在 `install.sh` 和 `restore.sh` 里都搜不到）：

| 包 | 状态 | 依据 |
| --- | --- | --- |
| `com.mitv.care` | `enabled=2`（用户停用） | `firstInstallTime=2022-06-26`，且 `pkgFlags` **无 `SYSTEM`**，codePath 在 `/data/app/`，是个普通应用 |
| `com.xiaomi.mitv.remotecontroller.service` | `enabled=2` | `pkgFlags=[SYSTEM ... UPDATED_SYSTEM_APP]`，`lastUpdateTime=2023-01-27` |

`restore.sh` 不要去动它们——还原时保持原样即可。

## 屏保

- `screensaver_components` 写成 `app.orchard.tv/app.orchard.tv.dream.AerialDream` 有效，重启后仍然保持。
- 小米不用 Android 标准 Dream（同 MFTR0），注意不要依赖 `screensaver_enabled`。

## 其他

- ADB 调试打开路径：设置 → 关于 → **产品型号** 连续按确认键进入开发者模式，然后 设置 → **账号与安全** → **ADB 调试**。
- 网络 ADB 在待机时会断（`Connection refused`），唤醒后能重连。`ping` 一直是通的，所以 `ping` 通但 5555 拒绝 = ADB 被关，不是电视掉线。
- 唤醒后 `global adb_enabled` 会被系统改成 0，`KEEP_ADB=1` 由守护服务写回 1（同 MFTR0）。

## 安装记录

```bash
adb connect <电视IP>:5555
curl -fsSL https://aerial.qust.me/install.sh | KEEP_ADB=1 TV=<电视IP> bash
```

输出：

```text
Downloading aerial-standard.apk from the latest release
Installing /var/folders/.../aerial-standard.apk
Aerial's screensaver is now the TV's screensaver.
Aerial will switch ADB debugging back on whenever the TV turns it off.
The firmware still sends Home to com.mitv.tvhome/.MainActivityUserMode; turning on Aerial Home Guard.
Home Guard is running. Press Home on the remote.
```

装的是 1.14（`versionCode=15`）。安装输出**没有提到通知监听失败**，这一项是后来排查才发现的。后续覆盖升级到 1.16 时触发了上面的 APK 路径问题。


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

