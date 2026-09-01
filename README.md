# ColorOS Live Lyrics Bridge

<p align="center">
  <img src="GIF.gif" alt="ColorOS lock-screen lyrics demo" width="360">
</p>

## 简体中文

为 ColorOS / OPlus 原生锁屏与 AOD 歌词页面提供完整时间轴、逐字高亮、翻译、样式与兼容
增强。它不是悬浮窗；歌词界面仍由 SystemUI 绘制。

### v4.1.0

- 12 个独立 Provider 全部迁移到 libxposed API 102、静态作用域与 Remote Preferences；
  Provider 可以独立向 ColorOS SystemUI 发布标准 `lyricInfo`。
- Bridge 继续只作用于 `system` 和 `com.android.systemui`，加强 MediaController 生命周期、
  曲目身份、歌词发布代际、绘制失败回退、缓存所有权与日志隐私。
- 修复酷狗冷启动首曲的同曲 generation 抖动和非单调官方时间轴，《回家的路》第一句可
  正常显示，同时完整保留制作人员 credit。
- 修复酷我冷启动第一首歌封面降级为纯色的问题，异步结果严格绑定曲目身份与 generation。
- 修复 OPlus 媒体 action 图标兼容、Unicode 空白占位、负速率回绕以及设置页无障碍问题。
- 保留 4.0 的亮度渐隐、翻译按钮、配置备份/恢复、AOD 与完整外观能力。

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

### 安装与升级

1. 安装自己使用的 `ColorOS-Live-Lyrics-Provider-<Name>-v4.1.0.apk`，在 LSPosed 中只
   勾选对应播放器。
2. 安装 `ColorOS-Live-Lyrics-Bridge-v4.1.0.apk`，Bridge 作用域只保留 `system` 与
   `com.android.systemui`。
3. 不要让旧 Provider 与 4.1 Provider 同时 hook 一个播放器。
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

### v4.1.0

- All 12 Providers now use libxposed API 102, static scope, and Remote Preferences while publishing
  standard `lyricInfo` directly to ColorOS SystemUI.
- Bridge remains limited to `system` and `com.android.systemui` and hardens controller lifecycle,
  track identity, publication epochs, draw fallback, cache ownership, and diagnostic privacy.
- Fixed KuGou first-track generation churn and non-monotonic official timelines, preserving every
  production credit while showing the first real line on cold start.
- Fixed first-track KuWo artwork degrading to a solid color with identity/generation-bound recovery.
- Fixed OPlus media-action icon compatibility, Unicode blank placeholders, negative rewind, and
  settings accessibility while retaining all 4.0 appearance, AOD, translation, and backup features.

### Requirements

- Root and an LSPosed/LSP manager with **libxposed API 102** support; LSPosed 2.2.0+ is recommended.
- A ColorOS/OPlus ROM that already includes the native lock-screen lyric page.
- Current validation focuses on ColorOS 16; major system/player updates can require renewed support.

### Provider matrix

Salt, Cone/GP, KuWo, LX/Walnut, Poweramp, Metrolist, KuGou/Concept, QQ Music, NetEase/Honor/
modified 9.0.40, Apple Music, Spotify, and QiShui are shipped as separate Provider APKs. Metrolist
and Spotify currently have no translation; QQ Music HD is not in the 4.1 matrix.

### Install and upgrade

1. Install the required `ColorOS-Live-Lyrics-Provider-<Name>-v4.1.0.apk` and select only its player
   package in LSPosed.
2. Install `ColorOS-Live-Lyrics-Bridge-v4.1.0.apk`; keep only `system` and
   `com.android.systemui` in Bridge scope.
3. Do not let an old Provider and a 4.1 Provider hook the same player.
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
