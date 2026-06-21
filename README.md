# ColorOS Live Lyrics Bridge

ColorOS Live Lyrics Bridge is an LSPosed/libxposed API 102 module that bridges timed lyrics from supported Android music players into the ColorOS/OPlus lock-screen lyric pipeline.

It supports:

- Salt Player compatibility adapter.
- ConePlayer compatibility adapter.
- Player-provided `lyricInfo` integration.
- Word-level lyrics when `rawLyric` is available.
- Lock-screen lyric layout and visual optimizations.
- Screen timeout keep-awake while the official lock-screen lyric UI is actively visible.

Source repository:

https://github.com/Andrea-lyz/ColorOS-Live-Lyrics-Bridge

Install the APK from the Releases page, enable the module for the packages listed in `SCOPE`, then restart System UI or reboot the device.
