# Steam Widget

> **Not recommended.** This widget is held together by a hacky workaround and I personally dislike it. Use at your own risk.

## Why it exists and why it sucks

Unlike Discord (local IPC socket) or OBS (WebSocket server), Steam exposes no TCP or socket interface for third-party applications to talk to. There is no official way to query friends, presence or chat from outside the Steam process.

The workaround: the widget bridges through the **Steamworks SDK** by loading `libsteam_api.so` directly via Python `ctypes`. This fakes being a game (using AppID 480 — Spacewar, Valve's SDK test app) to trick Steam into handing over an API context. From there it can read friends lists, rich presence and receive chat messages.

This approach has several problems:

- Steam must be running and logged in
- Steam may show a "Spacewar" game session in your activity while the widget is open
- `libsteam_api.so` must be present on the system (comes with the Steam Linux runtime)
- The ctypes bindings are fragile — any Steamworks SDK update can silently break them
- No official support, no guarantees, could stop working at any time

## Requirements

- Steam installed and running
- `libsteam_api.so` available (usually at `~/.steam/sdk64/libsteam_api.so` or inside the Steam Linux runtime)

## Notes

If Steam exposes a proper local API in the future this widget will be rewritten. Until then, it is what it is.
