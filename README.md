# POLARIS Hub

Multi-game hub. The hub (`polaris.lua`) contains no game logic — every
**Launch** fetches a **packed (obfuscated) build** from this repo's
`build/` directory over HTTP and runs it. Plain sources under `Games/`
and `Assets/` are **never published** (see `.gitignore`).

## Load the hub

In any supported game, run this one line in your executor:

```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/fervidos/PolarisHub/main/build/polaris.lua", true))()
```

Hub controls: **RightControl** minimizes / restores. The hub detects the
current game by `PlaceId` and only launches the build for that game —
join the right game first, or use **Join Game** on its card.

## Supported games (PlaceId -> build)

| Game | PlaceId | Remote build |
| --- | --- | --- |
| Roblox Stock Exchange 2 | 110527353762049 | `build/roblox_stock_exchange_2.lua` |
| Sell a Million Sushi | 125866884667103 | `build/sell_million_sushi.lua` |
| Steal a Forest | 140149493280576 | `build/steal_a_forest.lua` |
| Cold War | 13687899540 | `build/coldwar.lua` (+ `build/ui-template.lua`, fetched by itself) |
| Scam With Your Friends | 133878278486520 | `build/scam_with_your_friends.lua` |

Each build begins with the **POLARIS cinematic startup**
(embedded at build time from private source). It plays as an
opaque overlay on top of the game load — **click / tap to skip**. If the
intro ever fails, the game underneath still starts.

## Developer workflow (private builder, not in this repo)

Plain sources, the intro, and the build tooling live locally and are
**not published** (see `.gitignore`). The repo only ever carries the
packed outputs. To publish a new version, run your private builder
locally, add only the public artifacts, and push:

```sh
git add .gitignore README.md build/
git commit -m "publish packed builds"
git push origin main
```

`Games/`, `Assets/` and `Obfuscator/` are git-ignored, so `git add .`
is also safe — it will never stage them. Each build's first line carries
`POLARIS <name> build <digest> - packed build.`, which the hub checks
before compiling — a GitHub 404 page can never be run by mistake.

### Branch note

The hub fetches from `https://raw.githubusercontent.com/fervidos/PolarisHub/main/...`.
If you push to a different branch, update `Config.RepoBase` in
`polaris.lua` (and `PANEL_URL` in `Games/coldwar.lua`) to match.

## Troubleshooting

- **"could not download build"** — the executor's HTTP is blocked or
  `raw.githubusercontent.com` is not whitelisted; allow it and relaunch.
- **"not a packed build"** — the branch was pushed without `build/`, or a
  cache served an old file. Wait ~5 minutes for raw CDN expiry or
  re-push `build/`.
- **Wrong game** — join the exact PlaceId above first; the hub will
  offer **Join Game** instead of running the wrong build.
