# ColorOS Live Lyrics Bridge

<p align="center">
  <img src="GIF.gif" alt="ColorOS lock-screen lyrics demo" width="360">
</p>

## 简体中文

为 ColorOS / OPlus 原生锁屏与 AOD 歌词页面提供完整时间轴、逐字高亮、翻译、样式与兼容
增强。它不是悬浮窗；歌词界面仍由 SystemUI 绘制。

### v4.3.1

- 修复 QQ 音乐媒体卡控制按钮错位：不再为该播放器强制 OPlus Rule0 或重新下发规则表，保留
  系统原生按钮行；翻译按钮继续通过收藏槽原地替换呈现。
- 翻译按钮接入增强：既无 `PlaybackState.CustomAction` 也无 OPlus heart 的播放器，由 Bridge 合成
  framework 同类型动作兜底，点击仍在 Bridge 内处理。
- 歌词时钟平滑：播放中小于 600 ms 的位置偏差按本地时钟修正处理，不再误判为跳变或回退。
- KuWo Provider 1.2.0 (3)：只往宿主 metadata 追加 `lyricInfo`，不再重建 metadata、不再改写
  封面通道（删除联网补封面与 240px 重绘），锁屏封面与歌词同时正确。
- 仍包含 v4.3.0 的字符上浮动画（默认关闭，幅度 0–200% 可调）；Bridge 继续只作用于
  `system` 和 `com.android.systemui`，不进入播放器进程、不创建额外 MediaSession。

### 使用条件

- 已 Root，并安装支持 **libxposed API 102** 的 LSPosed / LSP 管理器；建议 LSPosed 2.2.0+。
- 系统本身包含 ColorOS / OPlus 原生锁屏歌词页面。
- 当前主要围绕 ColorOS 16 验证；系统和播放器大版本更新可能需要重新适配。

### Provider 矩阵

| 播放器 | 额外模块 | 能力 |
|---|---|---|
| Salt Player | `Provider-Salt` | 逐字、翻译 |
| ConePlayer / GP | `Provider-Cone` | 完整时间轴、翻译 |
| 酷我 | `Provider-KuWo` | 官方 payload 追加逐字/翻译 |
| LX / Walnut | `Provider-LX` | 逐字、翻译、蓝牙身份与封面兼容 |
| Poweramp | `Provider-Poweramp` | sidecar / 内嵌歌词、翻译 |
| Metrolist | `Provider-Metrolist` | 多歌词源，不支持翻译 |
| 酷狗 / 概念版 | `Provider-KuGou` | 官方 payload 追加逐字/翻译 |
| QQ 音乐官方版 | `Provider-QQ` | QRC 逐字、翻译 |
| 网易云 / 荣耀 / 9.0.40 | `Provider-NetEase` | 官方追加或构造逐字/翻译 |
| Apple Music | `Provider-Apple` | TTML 逐字、翻译 |
| Spotify | `Provider-Spotify` | 逐行/逐字，不支持翻译 |
| 汽水音乐 | `Provider-QiShui` | TrackLyric / 缓存逐字、翻译 |
| 通用播放器 | `Provider-Universal` | 在设置 App 中选择目标播放器；system_server MediaSession 补全 |
| Readify AI | `Provider-Readify` | Readify 3.1.0 的事件驱动 TTS 句子窗口 |

### 通用播放器 Provider

`Provider-Universal 1.0.0 (1)` 为没有专属适配、但能创建 Android `MediaSession` 的播放器提供
受控的歌词补全。它只在 `system_server` 运行；用户必须在设置 App 中主动选择目标包，未选择的音乐、
视频和其他媒体 App 不会被观察或写入歌词。

安装后打开 **Universal Player Provider**，选择播放器并配置歌词来源、逐字、翻译、原始歌词和诊断。
它保留宿主 metadata，只向选中且活跃的会话附加标准 `MediaMetadata["lyricInfo"]`；Bridge 继续只在
`system` 与 `com.android.systemui` 中提供 SystemUI 外观、AOD 和翻译按钮增强。不要对同一播放器同时
使用专属 Provider 与通用 Provider。

使用通用 Provider 时关闭蓝牙歌词、车载歌词或其他会用当前歌词覆写媒体标题的功能，否则首曲匹配、
缓存与切歌识别可能不可靠。反馈时仅提供脱敏日志、目标包名和复现步骤，不要上传完整歌词、cookie、
token 或私人媒体路径。

### 安装与升级

1. 安装自己使用的 `ColorOS-Live-Lyrics-Provider-<Name>-v4.3.0.apk`；通用 Provider 在其设置
   App 中选择目标播放器，专属 Provider 在 LSPosed 中只勾选对应播放器。
2. 安装 `ColorOS-Live-Lyrics-Bridge-v4.3.0.apk`，Bridge 作用域只保留 `system` 与
   `com.android.systemui`。
3. 不要让旧 Provider 与 4.3.0 专属 Provider 同时 hook 一个播放器；通用 Provider 使用时关闭
   蓝牙歌词、车载歌词或其他会覆盖媒体标题的功能。
4. 重启播放器和 SystemUI；首次安装或改变 scope 后建议重启设备。

[3.8.x → 4.0 迁移指南](https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge/blob/4.0/docs/4.0/MIGRATION-3.8-TO-4.0.zh-CN.md)

本项目不分发词幕 Provider。需要词幕时请从
[LyricProvider 原项目](https://github.com/tomakino/LyricProvider) 获取，并向原项目反馈
词幕显示/产品问题。

### 排错

出现问题时提供机型、ROM/SystemUI 版本、播放器版本、Bridge/Provider 版本、LSPosed
scope、复现步骤和脱敏日志。不要上传 token、cookie、完整私人歌词或个人媒体路径。

[源码与详细文档](https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge) ·
[问题反馈](https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge/issues)

---

## English

Enhances the native ColorOS / OPlus lock-screen and AOD lyric page with complete timelines,
word-by-word highlighting, translations, appearance controls, and compatibility handling. It is
not a floating overlay; SystemUI still owns the lyric surface.

### v4.3.0

- The 12 dedicated Providers, Universal Player Provider, and Readify TTS Provider continue to use
  libxposed API 102 and static scope. Universal Provider enriches standard `lyricInfo` from
  `system_server` for user-selected apps.
- An optional Salt Player-style character float-up effect moves unsung graphemes below the baseline
  and returns them along a cosine step as the reveal front passes; it stays synchronized with the
  existing word reveal, feather, and glow.
- **Character float amount** supports 0–200%. Ordinary LRC-style lyrics receive the same whole-line
  front when **Line-timed lyric progress** is enabled.
- Wrapped multi-segment rows now sum revealed advances for one continuous front. A lift draw failure
  safely falls back and emits a `CHAR_LIFT_UNAVAILABLE` diagnostic.
- Bridge remains limited to `system` and `com.android.systemui`; no player-process Bridge hook,
  additional MediaSession, or legacy private lyric transport is restored.

### Requirements

- Root and an LSPosed/LSP manager with **libxposed API 102** support; LSPosed 2.2.0+ is recommended.
- A ColorOS/OPlus ROM that already includes the native lock-screen lyric page.
- Current validation focuses on ColorOS 16; major system/player updates can require renewed support.

### Provider matrix

Salt, Cone/GP, KuWo, LX/Walnut, Poweramp, Metrolist, KuGou/Concept, QQ Music, NetEase/Honor/
modified 9.0.40, Apple Music, Spotify, QiShui, Universal Player, and Readify AI are shipped as separate Provider
APKs. Universal Player selects its target apps in its settings UI and runs from `system_server`.

### Universal Player Provider

`Provider-Universal 1.0.0 (1)` supplies controlled lyric enrichment for players without a dedicated
adapter that still create an Android `MediaSession`. It runs only in `system_server`; users explicitly
select target packages in its settings UI, while unselected music, video, and other media apps are
not observed or written with lyrics.

Open **Universal Player Provider** after installation to select players and configure lyric sources,
word timing, translations, raw lyrics, and diagnostics. It preserves host metadata and appends only
standard `MediaMetadata["lyricInfo"]` to a selected active session. Bridge remains limited to `system`
and `com.android.systemui` for SystemUI styling, AOD, and translation controls. Do not enable a
dedicated Provider and Universal Provider for the same player.

Disable Bluetooth, car-lyrics, or similar features that overwrite media titles with current lyric
lines while using Universal Provider; otherwise first-track matching, caching, and track-change
recognition can be unreliable. Reports should contain sanitized logs, the selected package, and
reproduction steps, never complete lyrics, cookies, tokens, or personal media paths.

### Install and upgrade

1. Install the required `ColorOS-Live-Lyrics-Provider-<Name>-v4.3.0.apk`. Select target apps in the
   Universal Provider UI, or select only the dedicated Provider's player package in LSPosed.
2. Install `ColorOS-Live-Lyrics-Bridge-v4.3.0.apk`; keep only `system` and
   `com.android.systemui` in Bridge scope.
3. Do not let an old Provider and a 4.3.0 dedicated Provider hook the same player. Disable Bluetooth,
   car-lyrics, or similar media-title-overwrite features while using Universal Provider.
4. Restart the player and SystemUI; reboot after the first install or a scope change.

[3.8.x → 4.0 migration guide](https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge/blob/4.0/docs/4.0/MIGRATION-3.8-TO-4.0.md)

Lyricon Providers are distributed by the
[original LyricProvider project](https://github.com/tomakino/LyricProvider). Report Lyricon
display/product issues there.

For a useful issue, include device/ROM/SystemUI/player versions, Bridge/Provider versions and
scopes, reproduction steps, and sanitized logs. Never upload tokens, cookies, complete private
lyrics, or personal media paths.

[Source and documentation](https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge) ·
[Report an issue](https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge/issues)

### Support

<p align="center">
  <img src="PY_QR.png" alt="WeChat and Alipay support QR code" width="600" height="400">
</p>

### 致谢 / Acknowledgements

感谢 [Lyrico](https://github.com/Replica0110/Lyrico) 与
[Lyrico-Plugins](https://github.com/Replica0110/Lyrico-Plugins) 在本地音乐元数据、歌词管理和
插件化歌词源方面提供的开源工作与启发。 Thanks to Lyrico and Lyrico-Plugins for their open-source
work and inspiration around local music metadata, lyric management, and plugin-based lyric sources.
