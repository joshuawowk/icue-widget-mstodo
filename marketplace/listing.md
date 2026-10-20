# Maker Console listing: Task List for Microsoft To Do

Copy-paste kit for the Elgato Maker Console (https://maker.elgato.com). Values below were
drafted against the Elgato product guidelines (https://docs.elgato.com/guidelines/products)
as researched in October 2026; re-check the guidelines before submitting, because limits can change.

## Product name

Task List for Microsoft To Do

(29 characters; the limit is 30. "for Microsoft To Do" wording marks this as a third-party widget that works with the service; it does not use the word "Official" or any Microsoft logo. The manifest `name` is "Microsoft To-Do" (leads with the Microsoft mark and uses a hyphen); the listing name deliberately differs. The widget icon is a neutral circle-and-check mark; it is not the Microsoft To Do logo, but a reviewer may find it similar in spirit, so check it against Microsoft's brand guidance if in doubt.)

## Tagline

A My Day style Microsoft To Do list for the Xeneon Edge, read through a companion service you run.

(98 characters.)

## Description

Show your Microsoft To Do list on the Xeneon Edge. The widget is a My Day style list with circular checkboxes, list labels, due-date chips (red Overdue, pink Today, amber Tomorrow or this week) and importance stars, ordered by due date and then importance. It shows your default list, or any list by ID. It does not talk to Microsoft directly: it reads read-only JSON from xeneon-graph-proxy, a small companion service you run yourself that signs in to Microsoft Graph. When the proxy needs sign-in, the widget shows the device code on the panel so you can finish at microsoft.com/devicelogin. The last good data stays on screen with an offline tag if the proxy is unreachable, the layout adapts to every Xeneon Edge slot size, and you can set colors, a background image, glass blur and transparency. Requires iCUE 5.52 or later on Windows, a Xeneon Edge, and a running xeneon-graph-proxy signed in to your own Microsoft account; its setup is in the GitHub README. This is an independent, unofficial widget, not affiliated with Microsoft. It is free software under the MIT license.

(1081 characters; Maker Console asks for 250 to 1500. Counted with Python `len()` on the paragraph above.)

## Category

Productivity. (The widget is a task list; the closest observed category is Productivity.) The exact Maker Console category list for iCUE widgets was not verified when this kit was
written. The categories observed live on the Elgato Marketplace iCUE widgets page in October 2026 were Audio, Productivity,
Gaming, Artistic, Monitoring, Engagement and Social; pick the closest one the console offers.

## Release notes

Version 1.0.1

- Declares url network permissions in the manifest (`localhost:8585` and `any`).
- Sets `min_app_version` to 5.52.
- Bundles the media viewer inside the widget folder and self-closes the head elements, so the package passes the iCUE widget validator.
- Replaces personal host defaults with generic ones.
- Adds a SECURITY policy and a README Install section that points at the GitHub release.

(Condensed from the GitHub release notes for v1.0.1 (`gh release view v1.0.1 -R joshuawowk/icue-widget-mstodo`).)

## Requirements

- CORSAIR iCUE 5.52 or later (the manifest sets `min_app_version` to 5.52).
- Windows PC (the manifest targets `os` windows).
- A CORSAIR Xeneon Edge (`dashboard_lcd` device).
- xeneon-graph-proxy (https://github.com/joshuawowk/xeneon-graph-proxy) running and reachable from the PC, by default at `http://localhost:8585`. It is a companion service the user runs; the widget does not talk to Microsoft directly.
- A Microsoft account with To Do lists; the proxy signs in once by device code and holds read-only Graph scopes (`Tasks.Read`).
- Packaging status of the proxy is a snapshot: the v1.0.1 README says the `ghcr.io` container image was not yet published and the proxy must be built from source with `docker compose up -d --build`. Re-check with `gh release list -R joshuawowk/xeneon-graph-proxy` and the proxy README before submitting, and update this listing if the image is now published.

## Network permissions

The manifest declares two permissions: `{ "type": "url", "domain": "localhost", "port": 8585 }` for the default proxy address, and `{ "type": "url", "domain": "any" }`, the iCUE spec's wildcard for all external hosts, because the Proxy URL setting can point at any host on the user's network. The widget talks only to the user's own proxy; it contacts no third party. If the proxy runs on a different localhost port, iCUE asks for permission at runtime and the user clicks Allow. iCUE shows the permissions at install, and they can be revoked in the widget settings.

## License

MIT, see `LICENSE` in the repository root. Source: https://github.com/joshuawowk/icue-widget-mstodo

## Submission notes

- Asset specification unverified. The iCUE-specific listing asset requirements (icon size, gallery image count and
  dimensions, description limits) were not found in Elgato's documentation when this kit was written. This kit reuses the
  sizes of the Stream Deck kit: a 288x288 listing icon and 1920x960 gallery images, with a description of 250 to 1500
  characters. Re-check the Maker Console form and resize or re-render (`marketplace/build.sh`) if it asks for something else.
- Submission route unverified. Elgato's become-a-maker page lists Stream Deck, Wave Link, OBS and 3D as self-serve Maker
  Console categories but does not list iCUE widgets. If the console has no iCUE widget option, the fallback is the
  maker@elgato.com "New Product Submission" email route.
- File to upload: `microsoft-todo.icuewidget`, the `.icuewidget` attached to GitHub release v1.0.1:
  https://github.com/joshuawowk/icue-widget-mstodo/releases/download/v1.0.1/microsoft-todo.icuewidget
  Download it from the release; do not zip the folder by hand. To rebuild it, use `npm run package` (needs
  `npm install -g icuewidget-cli`).
- Manifest version is 1.0.1 and the file above matches release v1.0.1. If you submit a later release, update the
  version, release notes and file name in this kit first.
- Listing icon: `resources/icon.png` is 256x256 and was upscaled to 288x288 with ImageMagick (`convert resources/icon.png -resize 288x288!`), so it is slightly soft; re-export `resources/icon.svg` at 288 for a sharper icon if needed.
- Packaging caveat: `npm run package` runs `icuewidget package .` on the repository root, which zips every non-hidden file. A `marketplace/` folder would therefore be swept into future `.icuewidget` builds (about 0.8 MB of PNGs and HTML). Nothing in this kit changes the package script; stage the widget files or exclude `marketplace/` before the next release build.
- Validator note: `icuewidget validate` (CLI 0.5.0, run in October 2026 on the unzipped local `dist/` build of that version) passes with one warning that the `any` url permission grants access to every external host. The permission is intentional because the host is user-entered; expect reviewers to ask about it and point them at the Network permissions section.
- Not tested on hardware: the kit was written without Windows, iCUE or a Xeneon Edge. Gallery slides show the shipped preview and text only; no screenshot was taken from a running panel.

## Assets in this folder

| File | Size | Use |
| --- | --- | --- |
| `listing-icon-288.png` | 288x288 | Product icon |
| `gallery-01.png` | 1920x960 | Overview: product name and a crop of the widget header from `resources/preview.png` |
| `gallery-02.png` | 1920x960 | Feature cards: lists, due chips, importance stars, sign-in, offline, sizing |
| `gallery-03.png` | 1920x960 | Widget, xeneon-graph-proxy and Microsoft Graph, setup steps and network permission |

`resources/preview.png` is NOT used in full. It shows what look like real work task titles and list names, and the gallery images must carry no real task content, so slide 1 uses only a crop of its header strip (the "My Tasks" title and count) and the other slides are built from the feature list. `resources/preview.png` is also shipped inside the released `.icuewidget`; consider regenerating it from sample data.
`marketplace/src/` holds the HTML sources and `marketplace/build.sh` re-renders the gallery PNGs with
headless Chromium.

## Trademark note

"Microsoft", "Microsoft To Do" and "Microsoft Graph" are trademarks of Microsoft Corporation. This widget is independent and unofficial, is not affiliated with or endorsed by Microsoft, and uses no Microsoft logo.
