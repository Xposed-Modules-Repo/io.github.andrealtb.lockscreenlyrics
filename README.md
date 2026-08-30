# ColorOS Live Lyrics Bridge

<p align="center">
  <img src="GIF.gif" alt="ColorOS lock-screen lyrics demo" width="360">
</p>

## 简体中文

为 ColorOS / OPlus 原生锁屏与 AOD 歌词页面提供完整时间轴、逐字高亮、翻译、样式与兼容
增强。它不是悬浮窗；歌词界面仍由 SystemUI 绘制。

### v4.0.0

- Bridge 只作用于 `system` 和 `com.android.systemui`，不再进入播放器进程。
- 12 个独立 Provider 从播放器自己的 MediaSession 发布标准 `lyricInfo`；只安装 Provider
  也可以让受支持的 ColorOS 显示官方样式锁屏歌词。
- 新增独立“歌词亮度与渐隐”页面：实时/未高亮/翻译/非实时亮度、上下边缘渐隐和额外
  非活动行淡化。
- 翻译按钮即时刷新，并修复跨播放器图标颜色与大小。
- 修复居中/右对齐歌词从错误左侧 pivot 缩放的问题。
- 新增完整 Bridge 配置备份/恢复与确认后全量重置。
- Bridge 巨型方法、重复解析和热路径扫描已完成 Phase 6 治理。

### 使用条件

- 已 Root，并安装支持 **libxposed API 102** 的 LSPosed / LSP 管理器。
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

1. 安装自己使用的 `ColorOS-Live-Lyrics-Provider-<Name>-v4.0.0.apk`，在 LSPosed 中只
   勾选对应播放器。
2. 安装 `ColorOS-Live-Lyrics-Bridge-v4.0.0.apk`，Bridge 作用域只保留 `system` 与
   `com.android.systemui`。
3. 不要让旧 Provider 与 4.0 Provider 同时 hook 一个播放器。
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

### v4.0.0

- Bridge is scoped only to `system` and `com.android.systemui` and no longer enters player
  processes.
- Twelve independent Providers publish standard `lyricInfo` from player-owned MediaSessions.
  Provider-only installation can drive the stock ColorOS lyric page on supported systems.
- Added independent brightness controls for active/unrevealed/translation/inactive lanes, native
  top/bottom fading, and optional inactive-row fading.
- Translation actions update immediately with consistent cross-player icon presentation.
- Center/right lyric scaling now uses the correct alignment pivot.
- Added complete Bridge configuration backup/restore and confirmed full reset.
- Phase 6 reduced repeated parsing/scans and split large model/draw responsibilities.

### Requirements

- Root and an LSPosed/LSP manager with **libxposed API 102** support.
- A ColorOS/OPlus ROM that already includes the native lock-screen lyric page.
- Current validation focuses on ColorOS 16; major system/player updates can require renewed support.

### Provider matrix

Salt, Cone/GP, KuWo, LX/Walnut, Poweramp, Metrolist, KuGou/Concept, QQ Music, NetEase/Honor/
modified 9.0.40, Apple Music, Spotify, and QiShui are shipped as separate Provider APKs. Metrolist
and Spotify currently have no translation; QQ Music HD is not in the 4.0 matrix.

### Install and upgrade

1. Install the required `ColorOS-Live-Lyrics-Provider-<Name>-v4.0.0.apk` and select only its player
   package in LSPosed.
2. Install `ColorOS-Live-Lyrics-Bridge-v4.0.0.apk`; keep only `system` and
   `com.android.systemui` in Bridge scope.
3. Do not let an old Provider and a 4.0 Provider hook the same player.
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
