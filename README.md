# Microsoft To-Do — iCUE Widget for Xeneon Edge

[![Release](https://img.shields.io/github/v/release/joshuawowk/icue-widget-mstodo)](https://github.com/joshuawowk/icue-widget-mstodo/releases/latest)
[![License: MIT](https://img.shields.io/github/license/joshuawowk/icue-widget-mstodo)](LICENSE)

A **My Day**-style Microsoft To-Do list for the CORSAIR Xeneon Edge, visually modeled on the To-Do Android widget: dark theme, circular checkboxes, list labels, red *Today* chips, and importance stars.

![preview](resources/preview.png)

> **Requires a companion service.** This widget does not talk to Microsoft directly. It reads from **[xeneon-graph-proxy](https://github.com/joshuawowk/xeneon-graph-proxy)**, a small service you run yourself that signs in to Microsoft Graph and serves read-only JSON for your To-Do lists. Set that up first:
>
> ```bash
> docker run -d --name xeneon-graph-proxy --restart unless-stopped -p 8585:8585 -v xeneon-graph-proxy-data:/data ghcr.io/joshuawowk/xeneon-graph-proxy:latest
> ```
>
> Run the proxy from its published image, `ghcr.io/joshuawowk/xeneon-graph-proxy` (linux/amd64, linux/arm64); the [proxy README](https://github.com/joshuawowk/xeneon-graph-proxy#quick-start-docker) has the full `docker run` and compose setup. Then start sign-in once with `curl -X POST http://localhost:8585/auth/start` and follow the printed instructions.

Microsoft Graph has no static API keys, so this widget pairs with **[xeneon-graph-proxy](https://github.com/joshuawowk/xeneon-graph-proxy)** — a tiny LAN service that signs in once via device code and serves read-only JSON. The widget even displays the device-code on screen when sign-in is needed.

## Features

- Default list (or any list by ID) with open tasks, ordered by due date then importance
- Due chips: red *Overdue*, pink *Today*, amber *Tomorrow / this week*
- Importance stars (filled for `high`)
- **On-screen device-code sign-in** — when the proxy has no token, the widget shows the code to enter at microsoft.com/devicelogin
- Offline resilience: keeps last good data with an `offline` tag
- Responsive across all Xeneon Edge slots (multi-column on L/XL horizontal)
- Personalization: colors, background image, glass blur, transparency

## Install

**Prerequisites**

- Windows PC with CORSAIR iCUE 5.52 or newer and a Xeneon Edge
- [xeneon-graph-proxy](https://github.com/joshuawowk/xeneon-graph-proxy) running and reachable from the PC (see the note above)

**Steps**

1. Download `microsoft-todo.icuewidget` from the [latest release](https://github.com/joshuawowk/icue-widget-mstodo/releases/latest).
2. Double-click the file, or in iCUE open **Widgets**, click **+**, and select the file.
3. Add the widget to a Xeneon Edge slot.
4. Set **Proxy URL** if your proxy is not at `http://localhost:8585`.
5. If the proxy is not signed in yet, the widget shows a device code; enter it at the URL displayed.

**Network permission.** The manifest declares `{ "type": "url", "domain": "localhost", "port": 8585 }` for the default proxy address, plus the spec's `{ "type": "url", "domain": "any" }` wildcard because the **Proxy URL** setting can point at any host on your network. iCUE shows these at install and you can revoke them in the widget settings. If the proxy runs on a different `localhost` port, iCUE will ask for permission at runtime; click Allow.

## Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| Proxy URL | text | `http://localhost:8585` | Where xeneon-graph-proxy runs |
| List ID | text | empty | Blank = your default To-Do list (`/api/todo/lists` to discover IDs) |
| Title | text | `My Day` | Header label |
| Max Tasks | slider | 12 | 3–30 |
| Refresh Interval | slider | 5 min | 1–30 min |
| Text / Accent / Background Color | color | white / `#e8617c` / `#15161c` | |
| Background Image + Glass Blur | media | — | Optional backdrop |
| Background Transparency | slider | 100% | |

## States

Loading · Sign-in (device code on screen) · Empty ("All done") · Error (proxy unreachable) · Content

## Privacy

The widget talks only to your own proxy on the LAN. The proxy holds read-only Graph scopes (`Tasks.Read`). No third parties.

## Development

Requires Node.js and the official CLI (`npm install -g icuewidget-cli`, tested with 0.5.0).

```bash
npm run validate   # icuewidget validate .
npm run package    # icuewidget package . -o dist/microsoft-todo.icuewidget
```

Always build the package with `icuewidget package` (it validates first); do not zip the folder by hand. `dist/` is gitignored.

## License

MIT
