# ColorOS Live Lyrics Bridge

## 简体中文

ColorOS Live Lyrics Bridge 是一个基于 LSPosed/libxposed API 102 的模块，用来把受支持音乐播放器的时间轴歌词桥接到 ColorOS/OPlus 锁屏岛-锁屏歌词管线。

项目源码：

https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge

### 功能

- 内置 Salt Player 兼容适配器。
- 内置 ConePlayer 兼容适配器，包含正式包和 Google Play 包名。
- 支持播放器主动发布 `lyricInfo` 接入协议。
- `rawLyric` 可用时支持逐字歌词绘制。
- 优化 ColorOS 锁屏歌词排版、长句换行、重复歌词定位、翻译行识别和切歌显示稳定性。
- 在官方锁屏歌词 UI 实际可见且仍处于锁屏界面时，阻止屏幕按系统超时时间自动熄灭。
- 支持锁屏歌词翻译按钮；当 Salt Player 等播放器没有发布旧版桌面歌词 action 时，模块会注入公开翻译开关 action，方便 ColorOS/SystemUI 构建翻译切换按钮。

### 作用域

本仓库的 `SCOPE` 文件列出了需要由 LSPosed 启用的静态作用域：

```text
com.salt.music
ink.trantor.coneplayer
ink.trantor.coneplayer.gp
com.android.systemui
```

Salt Player 和 ConePlayer 属于兼容适配器，需要模块进入播放器进程抓取歌词。`com.android.systemui` 用于锁屏歌词绘制、翻译按钮显示和屏幕超时保活。

### 播放器主动接入协议

如果播放器本身已经拥有时间轴歌词，推荐直接在当前媒体会话中发布 `MediaMetadata["lyricInfo"]` JSON。主动接入的播放器通常不需要加入模块 APK 依赖，也不需要加入 LSPosed 播放器作用域；模块会在 SystemUI 侧从媒体会话中动态识别。

基础 payload 示例：

```json
{
  "songName": "...",
  "artist": "...",
  "songId": "lockscreen-lyrics-...",
  "lyric": "[00:00.00]...",
  "rawLyric": "[00:00.000]word[00:00.120]..."
}
```

- `lyric`：带时间戳的逐行 LRC，用于 ColorOS/OPlus 官方锁屏歌词列表。
- `rawLyric`：可选，包含逐字时间轴；提供后模块会启用逐字歌词绘制。
- 带时间戳的翻译行可以和主歌词一起放入原始歌词数据中，模块会在 SystemUI 侧合并识别。

完整协议、字段说明和 Media3 示例见源码仓库文档：

https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge/blob/main/docs/PLAYER_INTEGRATION.zh-CN.md

### 安装

从 Releases 页面下载 APK，在 LSPosed 中按 `SCOPE` 启用模块，然后重启系统界面或直接重启设备。升级后建议同时重启目标播放器进程，确保播放器侧歌词 hook 与 SystemUI 侧锁屏歌词 hook 都重新加载。

## English

ColorOS Live Lyrics Bridge is an LSPosed/libxposed API 102 module that bridges timed lyrics from supported Android music players into the ColorOS/OPlus lock-screen lyric pipeline.

Source repository:

https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge

### Features

- Built-in Salt Player compatibility adapter.
- Built-in ConePlayer compatibility adapter, including the formal and Google Play package names.
- Player-provided `lyricInfo` integration protocol.
- Word-level lyric rendering when `rawLyric` is available.
- Lock-screen lyric layout, long-line wrapping, repeated-line matching, translation-line recognition, and track-switch stability improvements.
- Screen timeout keep-awake while the official lock-screen lyric UI is actually visible and the keyguard is still showing.
- Lock-screen lyric translation button support. When players such as Salt Player do not publish the legacy desktop-lyrics action, the module injects the public translation toggle action so ColorOS/SystemUI can build the translation button.

### Scope

The `SCOPE` file in this repository lists the static LSPosed scope:

```text
com.salt.music
ink.trantor.coneplayer
ink.trantor.coneplayer.gp
com.android.systemui
```

Salt Player and ConePlayer use compatibility adapters, so the module needs to run inside their player processes to capture lyrics. `com.android.systemui` is required for lock-screen lyric rendering, translation button visibility, and screen-timeout keep-awake.

### Player Integration Protocol

Players that already own timed lyrics should publish a `MediaMetadata["lyricInfo"]` JSON string in the active media session. A self-integrating player usually does not need to depend on the module APK or be added to the LSPosed player scope; the module detects the current media session dynamically from the SystemUI side.

Basic payload example:

```json
{
  "songName": "...",
  "artist": "...",
  "songId": "lockscreen-lyrics-...",
  "lyric": "[00:00.00]...",
  "rawLyric": "[00:00.000]word[00:00.120]..."
}
```

- `lyric`: timed line-level LRC for the native ColorOS/OPlus lock-screen lyric list.
- `rawLyric`: optional word-level timeline; when provided, the module enables word-level rendering.
- Timed translation lines may be included in the original lyric data and are merged on the SystemUI side.

Full protocol details, field definitions, and Media3 examples:

https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge/blob/main/docs/PLAYER_INTEGRATION.md

### Installation

Download the APK from Releases, enable the module in LSPosed for the packages listed in `SCOPE`, then restart System UI or reboot the device. After upgrading, also restart the target player process so both player-side lyric hooks and SystemUI-side lock-screen hooks are reloaded.
