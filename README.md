<div align="center">

# ✦ POLARIS ✦

### *Your universe, one hub.*

One loader. Five games. Each one picks up the right script the moment you join.

[![Version](https://img.shields.io/badge/version-1.1.0-84c6ff?style=flat-square)](./polaris.lua)
[![Games](https://img.shields.io/badge/supported%20games-5-2dd4bf?style=flat-square)](#-supported-games)
[![Language](https://img.shields.io/badge/Luau-Roblox-ff4530?style=flat-square)](https://luau-lang.org/)
[![Builds](https://img.shields.io/badge/builds-packed%20%2F%20obfuscated-9b8cff?style=flat-square)](#-how-it-works)

</div>

---

## ⚡ Quick start

Join any supported game, paste this into your executor, and run it:

```lua
loadstring(game:HttpGet("https://cdn.luaprotect.dev/u/a80b01/66uTTgVKTlnS7drv"))()
```

That's the whole setup. Polaris reads your **PlaceId**, works out which game you're in, and shows the matching card. Not in the right game yet? Hit **Join Game** on a card and it'll teleport you there.

| Key | What it does |
| --- | --- |
| `RightControl` | Minimize / restore the hub |
| Click / tap on an intro | Skip the cinematic startup |

> **Heads up:** the hub closes itself shortly after a successful launch, since the game script takes over the screen. Run the loader again whenever you want it back.

---

## 🧭 Jump to

[How it works](#-how-it-works) · [Supported games](#-supported-games) · [The hub](#-the-hub) · [Game modules](#-game-modules) · [Controls](#-controls-at-a-glance) · [Startup animation](#-cinematic-startup) · [Troubleshooting](#-troubleshooting) · [For developers](#-for-developers)

---

## 🔭 How it works

The hub (`polaris.lua`) holds **no game logic at all**. When you press **Launch**, it downloads a packed, obfuscated build from this repo's `build/` folder and runs it. The readable sources never leave my machine.

| Piece | What it does |
| --- | --- |
| **Hub** | The window, sidebar, game cards, detection and settings. It never runs game code itself. |
| **Packed builds** (`build/*.lua`) | The real scripts. Obfuscated, integrity-stamped, and fetched only when you ask for them. |
| **Marker check** | Every build starts with a header (`POLARIS <name> build <digest> - packed build.`) plus a game marker. The hub checks both before compiling, so a GitHub 404 page can never be run by accident. |
| **Cinematic startup** | Each build plays the POLARIS intro as an overlay while the game loads. Progress is real (game loaded, player, camera, character), never padded with fake waits. If the intro fails, the game underneath still starts. |

**Why one hub?** One loader, one interface to learn, one version to keep track of. When a game updates, only its build gets repacked, and the hub grabs the fresh file the next time you launch.

---

## 🎯 Supported games

| # | Game | PlaceId | Module |
| :-: | --- | --- | --- |
| 01 | **Roblox Stock Exchange 2** | `110527353762049` | Quantum Trader v2.0.0 |
| 02 | **Sell a Million Sushi** | `125866884667103` | Empire Commander |
| 03 | **Steal a Forest** | `140149493280576` | Anti-AFK v2.3.0 |
| 04 | **Cold War** | `13687899540` | Combat Suite |
| 05 | **Scam With Your Friends** | `133878278486520` | KolkataFarmer v8 |

> **Wrong game?** Polaris won't run the wrong build. It offers **Join Game** instead and sends you to the exact PlaceId.

<details>
<summary>Build file paths</summary>

| Game | Packed build |
| --- | --- |
| Roblox Stock Exchange 2 | `build/roblox_stock_exchange_2.lua` |
| Sell a Million Sushi | `build/sell_million_sushi.lua` |
| Steal a Forest | `build/steal_a_forest.lua` |
| Cold War | `build/coldwar.lua` (it fetches `build/ui-template.lua` on its own) |
| Scam With Your Friends | `build/scam_with_your_friends.lua` |

</details>

---

## 🖥️ The hub

A dark-glass interface with an animated Polaris star, a drifting particle field and four accent colors. It's built entirely in code, so there are no image assets to load.

### Pages

- **Home:** game cards with a thumbnail, an index badge (`01`–`05`), the PlaceId and a **Launch** button. Click a card to open its detail page.
- **Games:** the full list with live status. The game you're standing in lights up with *"Detected: you are in this game"*.
- **Game detail:** a large thumbnail, name, PlaceId, store link, detection status, the remote build path, and a **Launch** or **Join Game** button depending on where you are.
- **Settings:** UI scale, accent color, animations, reduced motion, and minimize / reset / close.
- **Information:** version, developer, supported-game count and a short description.

### Settings

| Setting | Options | Notes |
| --- | --- | --- |
| **UI scale** | `70%` to `130%` in `10%` steps | Fits itself to your viewport, and dragging the header stays accurate at any scale. |
| **Accent color** | **Ice** `84C6FF` · **Silver** `CCD4E2` · **Aurora** `7AE2CE` · **Violet** `AE9AFF` | Recolors indicators, badges, toggles and focused borders instantly. |
| **Animations** | On / Off | Off makes every tween instant. |
| **Reduced motion** | On / Off | Short fades and no particle drift. |
| **Minimize** | `RightControl`, the `-` button, or Settings | Collapses to a small floating star. Click it to restore. |

A few small touches: a draggable top bar, toast notifications, staggered card entrances, and an accent glow on card hover. Re-running the loader never stacks a second hub, because `getgenv().POLARIS_HUB.Destroy()` cleans up the old one first.

---

## 🎮 Game modules

Every module opens with the POLARIS cinematic startup, and you can click or tap to skip it. Open a game below for the details.

<details>
<summary><b>01 · Roblox Stock Exchange 2 — Quantum Trader v2.0.0</b></summary>

<br>

> *A predictive price engine, a leveraged auto-trader, autofarm and empire automation, all in one.*
> PlaceId `110527353762049` · Menu `RightShift` · Panic `Delete`

**The idea:** the exchange builds every candle from a profile seed, and its `Shared.PriceEngine` can be read. So Quantum Trader doesn't guess at charts. It calculates future prices exactly, scans every listed stock, crypto, ETF, forex pair, future and index, and ranks whichever has the best predicted move over the next few seconds.

#### Auto Trade

- **Strategies:** `Peak Sniper`, `Balanced` or `Momentum`, with an asset filter (`All / Crypto / Stocks / ETF / Forex / Futures / Index`).
- **Leveraged entries:** `1x`–`100x` (within the server cap and your level unlocks), margin per trade %, cash reserve %, max positions `1`–`12`, and max opens per scan.
- **Exact exits:** take profit at the predicted peak, re-planned every tick by the Peak Exit Planner. There's also a hard stop loss, a reversal exit and a max hold timer.
- **Liquidation Guard:** every entry is tested against a sampled worst-case path, and any trade that would cross its liquidation price is rejected.
- **Dynamic Leverage** lowers leverage automatically when the predicted drawdown is larger. **Turbo Mode** scans faster and allows two entries per cycle. Exchange-side stop and target orders are optional.
- **Live book:** a Dashboard (level, max leverage, open positions, scan cycle), plus **Signals** (ranked predictions), **Positions** and **Logs** tabs.

#### Empire automation (v2.0)

| System | What it does |
| --- | --- |
| **Launchpad** | Launches your own memecoins, dev-buys, and manages and exits the bag. Take profit, stop loss, exit-at-curve-fill %, and a max launches per session. Also includes **Meme Sniper** momentum trades on other players' coins (beta, off by default). |
| **Company** | Incorporates a firm, answers company events with the best available choice, declares dividends, and buys back stock below NAV. Optional **Founder Exit** files notice and sells founder shares above a target. Capital split is automatic or a fixed override. |
| **Estate** | Collects rent, repairs storm damage, builds and upgrades every owned tile, buys the cheapest neighboring land, accepts strong offers, sells idle tiles, and cashes in founder land. |
| **Finance** | **Vault sweep** (park spare cash, pull it back when needed), **bond lending** into the best-yielding treasury tier, **Trader Pass** and **Retention** reward claims, and a sealed **Challenge Run** (DANGER: fresh-account path, off by default). |

#### Autofarm

Claims daily desk objectives, career milestones, the close-report payout and the one-time favorite / group / like rewards. It buys desk upgrades (commission, execution, margin, scanner tiers), backs IPOs predicted to pop, rebirths automatically when your book is flat, and has a built-in **Anti-AFK**.

#### Safety

`Delete` is the **panic button**: it closes every position and stops the engine instantly. Settings are saved to `RSE2_Quantum.json` via `writefile` / `readfile`. All markets in this game are simulated, so no real money is involved.

</details>

<details>
<summary><b>02 · Sell a Million Sushi — Empire Commander</b></summary>

<br>

> *An all-in-one automation suite with a smooth animated GUI. Free in-game mechanics only, no Robux purchases.*
> PlaceId `125866884667103` · GUI `RightShift` · Auto-Pilot `F8`

#### Dashboard

Live stats for **Status**, **Net Worth**, **Current Count** and **Fish Price** (with its `x` multiplier), plus counters for loot, deposits, buys, merges and upgrades. You also get **Toggle Auto-Pilot** and **Teleport Home**.

#### Economy

- **Auto Collect Loot** and **Auto Collect Money** sweep fish loot tiers and customer-money buttons using proximity-touch, with a teleport-touch fallback.
- **Auto Deposit Fish** deposits when the market price meets your threshold, and works with **Smart Market Timing**.
- **Auto Handle Chests** picks up chests and routes them: **Auto Keep Rare** (keeps tiers at or above your rarity threshold), **Auto Discard** (free, no Robux) and **Auto Unlock** (⚠️ costs Robux, so it's your call).
- **Auto Offline Income** and **Auto Group Reward** collect those for you.

#### Fishery

**Auto Buy Fishermen** (bulk, through the `100 / 25 / 5 / 1` buttons), **Auto Merge Fishermen**, **Auto Upgrade Sell Speed**, and **Auto Instant Sell** with its own price threshold. **Smart Market Timing** makes sure it only acts on good prices.

#### Movement

**Fly** (`WASD` + `Space` / `Ctrl`, adjustable speed), **Noclip**, **Infinite Jump**, and **Speed Hack** (`16` up to `100`). There are one-click teleports to the **Sell Area** and the **Deposit** spot, so you can combine these with the checkpoint buttons for full control of your plot.

#### Employees & Trader

**Auto Enable Employees** switches every owned employee on, and **Auto Trader Threshold** stays pinned at the max `1.5x`. You can also use the one-click **Enable All Employees Now** and **Set Trader to Max**.

#### Master control

**Master Auto-Pilot** and a **Master Feature Gate** (a global on/off), plus **Anti-AFK** (a jittered heartbeat and a brief key-hold that blocks the 20-minute idle kick). The window is draggable and minimizable with a reopen pill, shows toast notifications, saves its config to `EmpireCommanderConfig.json`, and shuts down cleanly with **Destroy Script**.

</details>

<details>
<summary><b>03 · Steal a Forest — Anti-AFK v2.3.0</b></summary>

<br>

> *A human-like, multi-vector anti-AFK that never pulls you out of your base zone — plus one-click server hopping to the emptiest, lowest-ping server.*
> PlaceId `140149493280576` · Show / hide `RightShift`

#### The engine

Each of these fires as its own independent pulse, and all of them are `pcall`-safe:

- **VirtualUser** click (`ClickButton2`, using the modern `Vector2` form with a legacy fallback)
- **VirtualInputManager** key taps (`W A S D`, plus `Space` only when Jump is on, so there's a true no-jump mode)
- **Jump** (`Humanoid.Jump` with a backup `Space` press, skipped in no-jump zones, while seated or while airborne)
- **Camera** yaw / pitch nudges
- **Spin** yaw turns
- **Micro-move:** a `1.0`–`2.2` stud micro-orbit that returns you to where the pulse started

#### Why it won't disturb your farm

Pulses are anchored to your idle spot (spawn, base or guards zone). If you drift more than `10` studs away, it gently walks you back, and **Reset Anchor** lets you set a new anchor anywhere. Before every move it checks you're actually on the ground using `FloorMaterial`.

#### Change server (lowest players + lowest ping)

The **SERVER** row shows where you are (`This: 5 players | 48ms`) and the best candidate found. Press **CHANGE SERVER** and it:

1. Fetches the public server list from `games.roblox.com` (ascending player count, two pages of `100`).
2. Keeps only servers with **fewer players than the one you're in**.
3. Takes the **emptiest** of those and picks the **lowest ping** among them.
4. Teleports with `TeleportService:TeleportToPlaceInstance`, and if that server rejects the teleport (`TeleportInitFailed`: full, reserved, shut down) it automatically tries the next-best one.
5. Queues an auto-reload with `queue_on_teleport`, so this build re-runs and the GUI comes back by itself after the hop.

| Control | What it does |
| --- | --- |
| **Left click** `CHANGE SERVER` | Scan, then teleport to the best server |
| **Right click** `CHANGE SERVER` | Scan only — shows the top 3 candidates in the log, no teleport |
| **MAX `-` / `+`** | Caps how busy a target may be (`1`–`20`, default `5`) |

The scan is cached for `15s` so repeated clicks don't hammer the API, and the chosen server is always less populated than the current one — if you're already in the emptiest server it says so instead of hopping you somewhere busier.

#### Modes & cadence

| Mode | Feel |
| --- | --- |
| **Safe** | Sparse, minimal pulses |
| **Balanced** *(default)* | Even cadence with human jitter |
| **Aggressive** | Dense pulses for maximum safety |

The interval slider runs from `15s` to `120s` (default `45s`) with `0.85x`–`1.15x` human-style jitter. If you move or type yourself, the next pulse gets postponed, and there's an `Idled` emergency fallback as a last line of defense.

#### GUI

A dark-glass, draggable window with animated show / hide, a **live countdown**, a **pulse log**, per-method chips (`Keys · Jump · Camera · Spin · VU · Move`), the **server-hop row**, a mini-pill when hidden, and saved settings (`Polaris_StealForest_AntiAFK.json`).

The game has no custom AFK RemoteEvent (all 142 remotes were scanned), so what this beats is the real 20-minute idle detector.

</details>

<details>
<summary><b>04 · Cold War — Combat Suite</b></summary>

<br>

> *An 80-player NATO vs PACT shooter with real ballistics. The silent aim here is a genuine bent shot: the direction is rewritten before the client encodes it, so the server and the client both see the same real trajectory.*
> PlaceId `13687899540` · Panel `RightShift` · Panic `F1`–`F4` (configurable)

#### How the aim works

Ballistics are solved exactly with the game's own maths:

`pos(t) = origin + dir·V/K·(1−e^(−Kt)) − up·g/2·t²`

That includes fixed-point flight-time iteration and lead prediction. The arc is walked with the bullet's own `RaycastParams`, team collision group included. A shot only bends when the first limb it meets belongs to the target, and a helmeted head (whose handle eats the claim) falls back to the torso. Bends from `0.1°` to `7.2°` were measured as registered against real server limb health, so the default `7°` FOV sits inside the proven range.

It uses no `hookfunction` and no `hookmetamethod`. The volley lookup is swapped with a plain table-field swap.

#### Under the hood

The game never resizes a part and never adds a BodyMover. The panel lives in `gethui()`, so at identity 2 the `CoreGui` image scan resolves to `nil` and never reaches it. ESP is made of `Drawing` objects rather than Instances.

#### ESP

Box, name, class (`Sniper` / `AT` / `Medic` …), distance and weapon, health bar, head / torso numbers, status flags (`DOWNED` / `SPAWN` / `DISGUISED`), helmet marker and knocked flash, skeleton, tracer, head dot, visible-only, dim-hidden, teammates (blue), vehicles (and empty ones), max distance, text size, and a **bipod-mounted** flag.

#### Camera aimbot

Hitkey, Always or While-firing triggers, plus an aim key. Targets `Head`, `Torso` or `Nearest`, with bullet-drop and lead prediction, sticky target, visible-only, a FOV circle, and smooth H/V. **Humanisation** covers reaction min / max, a wind-up ramp and a speed ceiling (`°/s`).

#### Silent aim (bent shot)

Targets `Head`, `Torso`, `Nearest` or `Random`. Settings for FOV in degrees, hit chance %, **Sniper deadly** (always head, `1400m`), max distance `800m`, lead for moving targets and a FOV circle. **Mounted-gun silent aim** covers turrets (`M2HB` / `M60T` / `PKT` …) up to `1200m`, with its own chance, FOV and lead tuning.

#### Radar & telemetry

A rotating radar (range, size, and position at `Left / Right / Top left / Top right`) showing enemies, teammates, vehicles, turret direction, objectives, names and the sweep. Also a threat list with the nearest threat and its distance, frontline scores, and a **launcher impact marker** (blast ring, crosshair, distance / arming label, flatten-to-ground).

#### Intel

**Radio captions** (enemy callout subtitles and reloading flags), a **HitCam armour x-ray** overlay, a helmet-knocked sensor, and a **staff scan** that pre-warns you about the server's staff list.

#### Bipod (the `[BIPODS]` update)

Flags bipod-mounted enemies and can **auto-deploy when ready**. It calls the game's own `toggle()`, which is gated by the game's own ground scan, so it can never plant somewhere the game wouldn't allow. It also shows a deploy-key hint and a live **mount status** readout backed by a local BlockCast probe (`normal ≥ 0.9397`).

#### World

No fog, fullbright and no suppression blur. Enemy checks are team-aware (`Player.Team`, the exact check `HitReporter` uses). It knows per-limb health (`Head 50 · Torso 150 · limbs 100`), is ragdoll-aware (`Ragdolled` attribute), and has skip-downed and skip-spawn rules.

</details>

<details>
<summary><b>05 · Scam With Your Friends — KolkataFarmer v8</b></summary>

<br>

> *A full-blown farmer: persona calls, shop / gear / team max-out, a virus crew and a GUI.*
> PlaceId `133878278486520` · GUI `RightShift` to hide · Stop with `_G.KF_STOP = true`

KolkataFarmer v8 runs the whole office. It answers calls, works the full scam catalogue (merged live from `Catalog`, `DailyPlaybooks` and `PlaybookLadder`), and maxes out the shop, gear and team.

#### Persona calls

A **persona bank** of `50+` playbooks (`fake_card`, `romance`, `police-impersonation`, `pet_rock`, `dragon_sitter`, `moon_camping`, `lost_socks`, `yodel_echo`, `duck_knight`, `pizza_…` and plenty more), each with objective-specific lines templated on the caller's `NAME`.

**Drill mode** builds lines from the game's own objective keyword tables. The 9 ladder playbooks carry no keywords, so those fall back to persona lines.

The full call loop is answer → select → haggle → chat → claim → hang up. It reads the transcript on each call, detects junk replies (`junkGiveUp`), and has auto-hang and auto-fire controls.

#### Shop, gear, team & virus crew

- **SHOP (ROI):** buys the best-ROI unlockables as soon as you can afford them.
- **TEAM:** hires and fills staff to the max.
- **STYLE:** maxes out gear and style unlockables.
- **Virus crew:** sends the **Fire Extinguisher** crew to the nearest fire stands.
- **2× Boost:** buys a spare commission charge and uses it on valuable calls (aggressive mode, minimum-value threshold), with stock and reserve tuning.

On top of that: delay-tuned **auto-rebirth**, day-card dismissal, the desk computer and camera-app policy toggles, and per-call state pruning so multi-hour sessions stay lean.

#### Anti-AFK & ops

The anti-AFK loop runs on its own thread and is deliberately **not** gated on `KF_STOP`, because parking at a desk is exactly when you want to stay connected. It uses jittered camera writes.

You get live stats (`answers / selects / chats / claims / haggles / fires / buys / boosts / gear / style / rebirths / afk`), a rolling log, and these controller flags: `_G.KF_STOP`, `_G.KF_AFK_STOP`, `_G.KF.stats`, `_G.KF.log`, `_G.KF.afk`.

#### GUI (`KFGUI`)

CONTROL, AUTOMATION and TUNING sections with toggles for `SHOP · TEAM · STYLE · 2X BOOST · PERSONA · HAGGLE · FIRE · BOOST BUY · REBIRTH · ANTI-AFK`. It also has steppers for AFK interval, speed and max sends, a persona-select mode, a boost-aggressive switch, a variant selector, and `PAUSE / RESUME / STOP`.

</details>

---

## ⌨️ Controls at a glance

| Context | Control | Action |
| --- | --- | --- |
| **Hub** | `RightControl` | Minimize / restore |
| **RSE2** | `RightShift` · `Delete` | Menu · **Panic close everything** |
| **Sushi** | `RightShift` · `F8` | Toggle GUI · Toggle Auto-Pilot |
| **Steal a Forest** | `RightShift` | Show / hide the Anti-AFK GUI |
| **Steal a Forest** | `CHANGE SERVER` button | Hop to the emptiest, lowest-ping server (right-click scans only) |
| **Cold War** | `RightShift` · `F1`–`F4` | Panel · Panic key (changeable) |
| **KolkataFarmer** | `RightShift` | Hide / show GUI |
| **Any intro** | Click / tap | Skip the cinematic startup |

---

## 🌌 Cinematic startup

Every packed build opens with it (source: `Assets/startup_animation.lua`):

- A `150`-star field, twinkling dust and a nebula glow, with a slowly rotating sky, spiralling gather particles and three orbiting light trails around the four-pointed **Polaris star**.
- Per-letter typography with a light-sweep reveal, plus celestial-chart captions (`URSAE MINORIS` / `DEC +89° 15'`).
- A real progress loader driven by actual init tasks. Click or tap anywhere to jump to the finale. Any failure is contained, so the game underneath **always** loads.

---

## 🛠️ Troubleshooting

| Problem | Fix |
| --- | --- |
| **"could not download build"** | Your executor's HTTP is blocked, or `raw.githubusercontent.com` isn't whitelisted. Allow **both** `cdn.luaprotect.dev` and `raw.githubusercontent.com`, then relaunch. The CDN loader hands off to GitHub for the hub, and GitHub also serves every game build. |
| **"not a packed build"** | The branch was pushed without `build/`, or a cache served a stale file. Wait about 5 minutes for the raw CDN to expire, or re-push `build/`. |
| **Wrong game** | Join the exact PlaceId from the table first. The hub will offer **Join Game** rather than run the wrong build. |
| **Hub missing or doubled** | Re-run the loader. The hub destroys any previous instance (`POLARIS_HUB.Destroy`) on boot. |
| **A game GUI won't open** | Check the key in [Controls at a glance](#-controls-at-a-glance). Most are `RightShift`. |
| **"Server scan failed: HTTP 429"** | Roblox rate-limited the public servers API. Wait a few seconds and press **CHANGE SERVER** again (right-click forces a fresh scan). |
| **"no candidates (already lowest?)"** | Every other public server has as many or more players than yours. Raise the **MAX** value if you want to allow busier targets. |

---

## 👩‍💻 For developers

The plain sources, the intro and the build tooling all live locally and are **not published** (see `.gitignore`). The repo only ever carries packed output. To ship a new version, run your private builder, add only the public files, and push:

```sh
git add .gitignore README.md build/
git commit -m "publish packed builds"
git push origin main
```

`Games/`, `Assets/` and `Obfuscator/` are git-ignored, so `git add .` is safe too. It will never stage them.

```
Polaris Hub/
├── polaris.lua        # hub source (local only, published as build/polaris.lua)
├── Games/             # private plain sources (git-ignored)
├── Assets/            # private cinematic startup source (git-ignored)
├── Obfuscator/        # private build tooling (git-ignored)
└── build/             # the ONLY published code: hub + 5 games + ui-template
```

### Branch note

Game builds are fetched from `https://raw.githubusercontent.com/fervidos/PolarisHub/main/...`. If you push to a different branch, update `Config.RepoBase` in `polaris.lua` (and `PANEL_URL` in `Games/coldwar.lua`) to match.

---

<div align="center">

> *These are automation scripts. Use them at your own risk. If a game updates, its build gets repacked and the hub serves the fresh version on your next launch.*

### ✦ PRECISION IN EVERY DETAIL ✦

*POLARIS · v1.1.0 · fervidos / PolarisHub*

</div>