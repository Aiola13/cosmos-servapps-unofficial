# Cosmos ServApps — Unofficial

An unofficial third-party app store for [Cosmos Cloud](https://cosmos-cloud.io/). It provides ready-to-install ServApps that are not (yet) in the [official marketplace](https://github.com/azukaar/cosmos-servapps-official).

## How to use

1. Open your Cosmos dashboard.
2. Go to **Market** → **Sources**.
3. Add this URL to the app store URL box:

```
https://aiola13.github.io/cosmos-servapps-unofficial/servapps.json
```

The apps then show up in the Market, next to the official ones.

## Available apps

| App | Description | Image | Arch |
|---|---|---|---|
| [Beszel](https://github.com/henrygd/beszel) | Lightweight server monitoring hub with Docker stats and alerts | `henrygd/beszel` | amd64, arm64, arm |
| [Docmost](https://github.com/docmost/docmost) | Collaborative wiki and docs (Notion/Confluence alternative). PostgreSQL and Redis bundled | `docmost/docmost` | amd64, arm64 |
| [Forgejo](https://codeberg.org/forgejo/forgejo) | Git forge with pull requests, issues and CI. Newer release line than the official market (v11). SQLite built in | `codeberg.org/forgejo/forgejo:17` | amd64, arm64, arm |
| [Gameyfin](https://github.com/gameyfin/gameyfin) | Video game library manager with metadata and downloads from the browser | `ghcr.io/gameyfin/gameyfin:2` | amd64, arm64 |
| [Glance](https://github.com/glanceapp/glance) | Lightweight dashboard for your feeds (RSS, weather, markets…) | `glanceapp/glance` | amd64, arm64, arm |
| [Kutt](https://github.com/thedevs-network/kutt) | URL shortener with custom domains, statistics and a REST API. SQLite built in | `kutt/kutt` | amd64, arm64 |
| [Linkding](https://github.com/sissbruecker/linkding) | Minimal and fast bookmark manager. SQLite built in | `sissbruecker/linkding` | amd64, arm64, arm |
| [MediaManager](https://github.com/maxdorninger/MediaManager) | Movie and TV library manager. PostgreSQL bundled | `quay.io/maxdorninger/mediamanager` | amd64, arm64 |
| [MkDocs Material](https://github.com/squidfunk/mkdocs-material) | Markdown documentation site with the Material theme, live reload | `squidfunk/mkdocs-material` | amd64, arm64, arm |
| [Neko](https://github.com/m1k1o/neko) | Virtual browser shared over WebRTC | `ghcr.io/m1k1o/neko/firefox` | amd64, arm64 |
| [NextGBA](https://github.com/nicotsx/nextgba) | GBA / GBC / GB emulator in the browser | `ghcr.io/nicotsx/nextgba` | amd64, arm64 |
| [Open WebUI](https://github.com/open-webui/open-webui) | ChatGPT-like interface for Ollama and OpenAI-compatible APIs | `ghcr.io/open-webui/open-webui:main` | amd64, arm64 |
| [PairDrop](https://github.com/schlagmichdoch/PairDrop) | AirDrop-like file sharing between devices, in the browser | `lscr.io/linuxserver/pairdrop` | amd64, arm64 |
| [Pocket ID](https://github.com/pocket-id/pocket-id) | Simple OIDC provider with passkey sign-in. SQLite built in | `ghcr.io/pocket-id/pocket-id:v2` | amd64, arm64 |
| [Readeck](https://codeberg.org/readeck/readeck) | Read-it-later and bookmark manager | `codeberg.org/readeck/readeck` | amd64, arm64 |
| [RomM](https://github.com/rommapp/romm) | ROM library manager with metadata and in-browser play. MariaDB bundled | `rommapp/romm` | amd64, arm64 |
| [ruTorrent](https://github.com/crazy-max/docker-rtorrent-rutorrent) | rTorrent and ruTorrent web UI | `crazymax/rtorrent-rutorrent` | amd64, arm64 |
| [Vikunja](https://github.com/go-vikunja/vikunja) | To-do app with lists, Kanban and Gantt views. SQLite built in | `vikunja/vikunja` | amd64, arm64, arm |
| [Wiki.js 3](https://github.com/requarks/wiki) | Next-generation Wiki.js. PostgreSQL 18 bundled | `ghcr.io/requarks/wiki:3` | amd64, arm64 |

### App notes

- **Beszel**: this installs the hub only. You create the admin account on first visit, then add each machine with **Add system**, which gives the agent command to run on it.
- **Forgejo**: finish the setup page on first visit; the first account registered becomes the administrator. Git over SSH uses the host port chosen at install (`222` by default). The image follows the `17` tag, so auto-update stays on 17.x. Do not point it at the data volume of a Forgejo 11 install without reading the upstream upgrade notes.
- **Gameyfin**: you create the admin account on first visit, so open the app right after install. The library chosen at install is mounted at `/opt/gameyfin-library` in the container. `APP_KEY` must be a base64 string that decodes to a valid AES key, so it is built from one Cosmos password plus a fixed suffix. Do not change it after install.
- **Glance**: it needs a `glance.yml` to start. The entrypoint is overridden to download the sample from `servapps/Glance/artefacts/glance.yml` into the config volume when the file is missing. Glance has no built-in login by default, so its route turns on **Cosmos authentication**.
- **Kutt**: you create the admin account on first visit, so open the app right after install. Registration and anonymous links are off by default (`DISALLOW_REGISTRATION`, `DISALLOW_ANONYMOUS_LINKS`). `DEFAULT_DOMAIN` is set from the hostname chosen at install. Change it if you change the route's hostname.
- **Linkding**: an `admin` account is created on first start with a generated password, shown at the end of the install and stored in `LD_SUPERUSER_PASSWORD`.
- **MkDocs Material**: it runs `mkdocs serve` (the live preview server) on the folder chosen at install. The entrypoint is overridden to create a starter project with the Material theme when the folder has no `mkdocs.yml`. The server has no login, so its route turns on **Cosmos authentication**. Files created by the container belong to root.
- **Open WebUI**: the first account created becomes the administrator, so open the app right after install. Ollama is not bundled: the Ollama URL asked at install defaults to an Ollama running on the host (`host.docker.internal:11434`).
- **PairDrop**: it has no login and its route is public, so that phones and guests can use it. Turn on Cosmos authentication on the route if you want to restrict it. Set `WS_FALLBACK=true` if devices cannot connect peer-to-peer.
- **Pocket ID**: open `/setup` right after install to create the admin account. Passkeys need HTTPS. `ENCRYPTION_KEY` is generated and must not change afterwards.
- **RomM**: the first visit runs a setup wizard that creates the admin account, so open the app right after install. The library folder must contain a `roms` folder with one sub-folder per platform. The ScreenScraper, RetroAchievements and SteamGridDB keys asked at install are optional.
- **Vikunja**: registration is open by default. Create your account, then set `VIKUNJA_SERVICE_ENABLEREGISTRATION=false`. The container runs as root (`user: 0:0`) because the image runs as UID 1000 and cannot write to fresh Docker volumes otherwise.
- **NextGBA** and **ruTorrent** have no built-in login, so their route turns on **Cosmos authentication** (`AuthEnabled`).
- **Neko** uses WebRTC and publishes the UDP range `52000-52100` on the host. To stream outside your LAN, forward that range on your router and set `NEKO_WEBRTC_NAT1TO1` to your public IP. The generated user and admin passwords are shown at the end of the install.
- **Wiki.js 3**: the admin email and password are asked at install. They only apply on first start, so change the password after your first login.
- Database passwords come from Cosmos (`{Passwords.N}`). They are kept across updates with the `cosmos-persistent-env` label.

## Repository structure

```
servapps/
└── <AppName>/
    ├── cosmos-compose.json   # Cosmos compose (or docker-compose.yml)
    ├── description.json      # Market metadata
    ├── icon.png              # App icon
    ├── screenshots/          # At least one screenshot (1.png, 2.png…)
    └── artefacts/            # Optional extra files
index.js                      # Builds servapps.json / index.json
.github/workflows/static.yml  # Builds and deploys to GitHub Pages on push to master
```

### `description.json`

```json
{
  "name": "MyApp",
  "description": "Short one-line description",
  "longDescription": "<p>Longer HTML description</p>",
  "tags": ["tag1", "tag2"],
  "repository": "https://github.com/owner/myapp",
  "image": "https://hub.docker.com/r/owner/myapp",
  "supported_architectures": ["amd64", "arm64"]
}
```

### `cosmos-compose.json` conventions

- Name the main service `{ServiceName}`. Prefix the others: `{ServiceName}-db`, `{ServiceName}-redis`…
- Group every service with the labels `"cosmos-stack": "{ServiceName}"` and `"cosmos-stack-main": "{ServiceName}"`.
- Use `{Passwords.0}` to `{Passwords.3}` for generated secrets. Cosmos makes 4 random strings of 24 characters each. Concatenate two if you need a longer secret.
- List generated env vars in `cosmos-persistent-env` so they are not regenerated on update.
- Use `{DefaultDataPath}` for default bind paths, rather than a hard-coded `/mnt/...`.
- Point `cosmos-icon` at `https://aiola13.github.io/cosmos-servapps-unofficial/servapps/<AppName>/icon.png`. The path is **case-sensitive**.
- Placeholders you can use: `{ServiceName}`, `{StaticServiceName}`, `{Context.<formField>}`, `{Hostnames.<service>.<route>.host}`, `{DefaultDataPath}`, `{RootHostname}`, `{RootProtocol}`.

## Adding an app

1. Create `servapps/<AppName>/` with the files above.
2. Build locally and check that the build gives no errors:

   ```bash
   node index.js
   ```

   This writes `servapps.json`, `index.json` and `servapps_test.json`. They are git-ignored.
3. Optional: test in Cosmos with a local server. `servapps_test.json` points to `http://localhost:3000`:

   ```bash
   npx serve -p 3000
   ```

   Then add `http://localhost:3000/servapps_test.json` as a source in Cosmos.
4. Commit and push to `master`. GitHub Actions publishes the store to GitHub Pages.

## Disclaimer

This repository is not affiliated with Cosmos Cloud or with the upstream projects. Each app keeps its own licence. Review a compose file before you install it.

## Licence

See [LICENCE](LICENCE).
