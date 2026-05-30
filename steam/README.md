# Steam Widget

> **Not recommended.** This widget is held together by a hacky workaround and I personally dislike it. Use at your own risk.

## Why it exists and why it sucks

Unlike Discord (local IPC socket) or OBS (WebSocket server), Steam exposes no TCP or socket interface for third-party applications to talk to. There is no official way to query friends, presence or chat from outside the Steam process.

The workaround: the widget bridges through the **Steamworks SDK** by loading `libsteam_api.so` directly via Python `ctypes`. This fakes being a game (using AppID 480 — Spacewar, Valve's SDK test app) to trick Steam into handing over an API context. From there it can read friends lists, rich presence and receive chat messages.

## Known issues

- **You will appear to be playing Spacewar** while the widget is open. This is visible to all your Steam friends.
- **`steam_appid.txt` is written to the scripts folder** on every launch and stays on disk. This can confuse other Steam integrations that scan for it.
- **`libsteam_api.so` path is hardcoded** to `~/.local/share/Steam/steamrt64/libsteam_api.so`. Breaks on Flatpak installs, non-default Steam paths, or if Valve changes the runtime layout.
- **Chat history is stored inside the plugin folder.** It gets wiped on plugin reinstall or update.
- **Events are polled, not pushed.** `SteamAPI_RunCallbacks` is called every 10 seconds. Messages, persona state changes and game invites can be missed or delayed between polls.
- **`SteamAPI_InitFlat` may not exist** on older Steam installs. It was added in a specific SDK version — earlier versions will fail silently or crash.
- **Any Steamworks SDK update can break the ctypes bindings** with no warning. There is no versioning or compatibility guarantee.

## Requirements

- Steam installed and running
- `libsteam_api.so` present at `~/.local/share/Steam/steamrt64/libsteam_api.so`

## Notes

If Steam ever exposes a proper local API this widget will be rewritten properly. Until then, it is what it is.
