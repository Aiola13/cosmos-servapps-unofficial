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
| [Docmost](https://github.com/docmost/docmost) | Collaborative wiki and docs (Notion/Confluence alternative). PostgreSQL and Redis bundled | `docmost/docmost` | amd64, arm64 |
| [Filebrowser Quantum](https://github.com/gtsteffaniak/filebrowser) | Web file manager with real-time search and OIDC/2FA | `gtstef/filebrowser` | amd64, arm64, arm |
| [MediaManager](https://github.com/maxdorninger/MediaManager) | Movie and TV library manager. PostgreSQL bundled | `ghcr.io/maxdorninger/mediamanager/mediamanager` | amd64, arm64 |
| [Neko](https://github.com/m1k1o/neko) | Virtual browser shared over WebRTC | `ghcr.io/m1k1o/neko/firefox` | amd64, arm64 |
| [NextGBA](https://github.com/nicotsx/nextgba) | GBA / GBC / GB emulator in the browser | `ghcr.io/nicotsx/nextgba` | amd64, arm64 |
| [Readeck](https://codeberg.org/readeck/readeck) | Read-it-later and bookmark manager | `codeberg.org/readeck/readeck` | amd64, arm64 |
| [ruTorrent](https://github.com/crazy-max/docker-rtorrent-rutorrent) | rTorrent and ruTorrent web UI | `crazymax/rtorrent-rutorrent` | amd64, arm64 |
| [Wiki.js 3](https://github.com/requarks/wiki) | Next-generation Wiki.js. PostgreSQL 18 bundled | `ghcr.io/requarks/wiki:3` | amd64, arm64 |

### App notes

- **NextGBA** and **ruTorrent** have no built-in login, so their route turns on **Cosmos authentication** (`AuthEnabled`).
- **Neko** uses WebRTC. To stream outside your LAN, publish the UDP range `52000-52100` (or set `NEKO_WEBRTC_UDPMUX`) and set `NEKO_WEBRTC_NAT1TO1` to your public IP.
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
