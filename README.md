<div align="center">

# ✦ POLARIS ✦

### *Your universe, one hub.*

**A single, multi-game hub that launches packed, obfuscated builds — automatically matched to the game you're actually in.**

[![Version](https://img.shields.io/badge/version-1.1.0-84c6ff?style=flat-square)](./polaris.lua)
[![Games](https://img.shields.io/badge/supported%20games-5-2dd4bf?style=flat-square)](#supported-games)
[![Language](https://img.shields.io/badge/Luau-Roblox-ff4530?style=flat-square)](https://luau-lang.org/)
[![Builds](https://img.shields.io/badge/builds-packed%20%2F%20obfuscated-9b8cff?style=flat-square)](#how-it-works)

</div>

---

## ⚡ Quick Start

Run **this one line** in your executor while you're in any supported game:

```lua
loadstring(game:HttpGet("https://cdn.luaprotect.dev/u/a80b01/66uTTgVKTlnS7drv"))()
```

That's it. The hub detects the current game by **PlaceId** and shows you the right cards — join the correct game first, or use **Join Game** on a card to teleport there.

> **Loader chain:** the CDN asset above loads the packed hub from this repo:
> ```lua
> loadstring(game:HttpGet("https://raw.githubusercontent.com/fervidos/PolarisHub/main/build/polaris.lua", true))()
> ```
> So your executor needs HTTP access to **both** `cdn.luaprotect.dev` (entry point) **and** `raw.githubusercontent.com` (hub + every game build). Whitelist both if your executor blocks unknown domains.
>
> **Hub toggle:** `RightControl` — minimize / restore the hub.
> The hub auto-closes shortly after a successful launch (the game script takes the screen). Re-execute the loader to bring it back.

---

## 📑 Contents

- [How It Works](#how-it-works)
- [Supported Games](#supported-games)
- [The Hub](#the-hub-hub-features)
- [🎮 Game Modules](#-game-modules)
  - [Roblox Stock Exchange 2 — Quantum Trader](#1--roblox-stock-exchange-2--quantum-trader-v200)
  - [Sell a Million Sushi — Empire Commander](#2--sell-a-million-sushi--empire-commander)
  - [Steal a Forest — Anti-AFK](#3--steal-a-forest--anti-afk-v220)
  - [Cold War — Combat Suite](#4--cold-war--combat-suite)
  - [Scam With Your Friends — KolkataFarmer](#5--scam-with-your-friends--kolkatafarmer-v8)
- [Controls at a Glance](#controls-at-a-glance)
- [Cinematic Startup](#cinematic-startup)
- [Troubleshooting](#troubleshooting)
- [Developer Notes](#developer-notes)

---

## How It Works

The hub (`polaris.lua`) contains **zero game logic**. Every **Launch** fetches a **packed, obfuscated build** from this repo's `build/` directory over HTTP and runs it. Plain sources under `Games/` and `Assets/` are **never published** (see `.gitignore`).

| Layer | Role |
| --- | --- |
| **Hub** | Window, sidebar, game cards, detection, settings. Never runs game code itself. |
| **Packed builds** (`build/*.lua`) | The real scripts — obfuscated, integrity-stamped, fetched on demand. |
| **Marker check** | Each build begins with `POLARIS <name> build <digest> - packed build.` + the game's marker. The hub verifies both before compiling, so a GitHub 404 page can never be executed by mistake. |
| **Cinematic startup** | Every build embeds the **POLARIS cinematic intro** (starfield → Polaris star → orbits → loader). It plays as an opaque overlay on top of the game load — **click / tap to skip**. Progress is *real* (game loaded, player/camera, character), never padded with artificial waits. If the intro ever fails, the game underneath still starts. |

**Why one hub?** One loader, one UI to learn, one version to track. When a game updates, only its packed build is repacked — the hub just fetches the fresh file on next launch.

---

## Supported Games

| # | Game | PlaceId | Packed build | Module |
| --- | --- | --- | --- | --- |
| 01 | **Roblox Stock Exchange 2** | `110527353762049` | `build/roblox_stock_exchange_2.lua` | Quantum Trader v2.0.0 |
| 02 | **Sell a Million Sushi** | `125866884667103` | `build/sell_million_sushi.lua` | Empire Commander |
| 03 | **Steal a Forest** | `140149493280576` | `build/steal_a_forest.lua` | Anti-AFK v2.2.0 |
| 04 | **Cold War** | `13687899540` | `build/coldwar.lua` (+ `build/ui-template.lua`, fetched by itself) | Combat Suite |
| 05 | **Scam With Your Friends** | `133878278486520` | `build/scam_with_your_friends.lua` | KolkataFarmer v8 |

> Wrong game? The hub won't run the wrong build — it offers **Join Game** (teleports you to the exact PlaceId) instead.

---

## The Hub

*A polished, dark-glass interface with an animated Polaris star, drifting particle field, and four accent colors — built entirely from code, no image assets required.*

### Pages

- **Home** — game cards (thumbnail, index badge `01`–`05`, PlaceId) with a primary **Launch** button. Clicking a card opens its detail page.
- **Games** — full list with live status: `Detected: you are in this game` highlighted in your accent when you stand in that PlaceId.
- **Game detail** — big thumbnail, name, PlaceId, store URL, detection status, and a contextual **Launch** / **Join Game** button + the remote build path.
- **Settings** — UI scale, accent color, animations, reduced motion, minimize / reset / close.
- **Information** — version, developer, supported-game count, and description.

### Hub Settings

| Setting | Options | Notes |
| --- | --- | --- |
| **UI scale** | `70%` – `130%` in `10%` steps | Auto-fits to viewport; header drag stays accurate at any scale. |
| **Accent color** | **Ice** `84C6FF` · **Silver** `CCD4E2` · **Aurora** `7AE2CE` · **Violet** `AE9AFF` | Re-colors indicators, badges, toggles, and focused borders instantly. |
| **Animations** | On / Off | Off makes tweens instant. |
| **Reduced motion** | On / Off | Short fades, no particle drift. |
| **Minimize** | `RightControl`, `-` button, or Settings | Collapses to a floating star **dock** — click it to restore. |

Small details: draggable topbar, toast notifications, staggered card entrances, rounded cards with accent glow-stroke on hover, and `getgenv().POLARIS_HUB.Destroy()` singleton cleanup so re-executing never stacks two hubs.

---
<a id="game-modules"></a>

## 🎮 Game Modules

Every module below opens with the **POLARIS cinematic startup**. Click / tap the intro to skip it.

---

### 1 — Roblox Stock Exchange 2 · Quantum Trader v2.0.0

> *Predictive price engine + leveraged auto-trader + autofarm + empire automation.*
> PlaceId `110527353762049` · Menu `RightShift` · Panic `Delete`

**The edge:** the exchange generates every candle from its profile seed, and its own `Shared.PriceEngine` is readable. Quantum Trader computes **future prices exactly** instead of guessing at charts, scans **every listed stock, crypto, ETF, forex pair, future, and index**, and ranks the best predicted move in the next few seconds.

#### Auto Trade

- **Strategies** — `Peak Sniper` · `Balanced` · `Momentum`, with asset filter `All / Crypto / Stocks / ETF / Forex / Futures / Index`.
- **Leveraged entries** — leverage `1x`–`100x` (respecting server cap + level unlocks), margin-per-trade %, cash-reserve %, max positions `1`–`12`, max opens per scan.
- **Exact exits** — take profit at the **predicted peak**, re-planned **every tick** (Peak Exit Planner); hard stop loss; reversal exit; max hold timer.
- **Liquidation Guard** — every entry is vetted against a sampled worst-case path; trades whose worst path crosses the liquidation price are rejected.
- **Dynamic Leverage** — automatically lowers leverage when predicted drawdown is larger. **Turbo Mode** scans faster and allows two entries per cycle. Optional exchange-side stop/target on every order.
- **Live book** — Dashboard (level, max leverage, open positions, scan cycle), **Signals** tab (ranked predicted move), **Positions** tab (live positions), **Logs** tab.

#### Empire Automation (v2.0)

| System | What it does |
| --- | --- |
| **Launchpad** | Launches your **own memecoins**, dev-buys, manages and exits the bag. Take profit + stop loss, exit-at-curve-fill %, max launches/session, plus **Meme Sniper** momentum trades on other players' coins (beta, off by default). |
| **Company** | Incorporates a firm, answers company events with the best available choice, declares dividends, buys back stock below NAV, and optional **Founder Exit** (files notice + sells founder shares above target). Split via auto-capital % or fixed override. |
| **Estate** | Collects rent, repairs storm damage, builds/upgrades every owned tile, snaps up cheapest neighbouring land, accepts strong offers (multiple threshold), sells idle tiles, cashes in founder land. |
| **Finance** | **Vault sweep** (park excess cash, pull back when needed), **bond lending** into the best-yielding treasury tier, **Trader Pass** + **Retention** reward claims, and a sealed **Challenge Run** (DANGER — fresh-account path, off by default). |

#### Autofarm

Auto-claims daily desk objectives, career milestones, close-report payout, one-time favorite/group/like rewards; buys desk upgrades (commission, execution, margin, scanner tiers); backs predicted-pop IPOs; auto-rebirth when the book is flat; built-in **Anti-AFK**.

**Safety:** `Delete` = **panic close** — closes every position and stops the engine instantly. Settings persist to `RSE2_Quantum.json` (`writefile`/`readfile`). *All markets in the game are simulated — no real money involved.*

---

### 2 — Sell a Million Sushi · Empire Commander

> *All-in-one automation suite with a smooth, animated GUI. Free in-game mechanics only — no Robux purchases.*
> PlaceId `125866884667103` · GUI `RightShift` · Auto-Pilot `F8`

#### Dashboard

Live stats: **Status**, **Net Worth**, **Current Count**, **Fish Price** (`x` multiplier), counters for loot / deposits / buys / merges / upgrades — plus **Toggle Auto-Pilot** and **Teleport Home**.

#### Economy

- **Auto Collect Loot** & **Auto Collect Money** — sweeps fish loot tiers and customer-money buttons via proximity-touch (with a teleport-touch fallback).
- **Auto Deposit Fish** — deposits when the market price meets your threshold (+ **Smart Market Timing**).
- **Auto Handle Chests** — picks up chests and routes them: **Auto Keep Rare** (keeps tiers at/above your rarity threshold) · **Auto Discard** (free discard, no Robux) · **Auto Unlock** (⚠️ Robux risk, your call).
- **Auto Offline Income** & **Auto Group Reward** collectors.

#### Fishery (Production Scaling)

**Auto Buy Fishermen** (bulk via the `100 / 25 / 5 / 1` buttons) · **Auto Merge Fishermen** · **Auto Upgrade Sell Speed** · **Auto Instant Sell** (with its own price threshold) · **Smart Market Timing** (only act on good prices).

#### Movement Exploits

Add those to checkpoint buttons and you have complete plot control:

- **Fly** (`WASD` + `Space`/`Ctrl`, adjustable speed) · **Noclip** · **Infinite Jump** · **Speed Hack** (`16` → `100`)
- **Teleport to Sell Area** / **Teleport to Deposit** one-clicks.

#### Employees & Trader

**Auto Enable Employees** (toggles every owned employee ON), **Auto Trader Threshold** pinned to max `1.5x`, plus one-click **Enable All Employees Now** / **Set Trader to Max**.

#### Master Control

**Master Auto-Pilot** + **Master Feature Gate** (global on/off), **Anti-AFK** (jittered heartbeat + brief key-hold, blocks the 20-minute idle kick), toast notifications, draggable/minimizable window with a reopen pill, and config persistence (`EmpireCommanderConfig.json`). Destroys cleanly via **Destroy Script**.

---

### 3 — Steal a Forest · Anti-AFK v2.2.0

> *Human-like, multi-vector anti-AFK that never leaves your base zone.*
> PlaceId `140149493280576` · Show / hide `RightShift`

**Engine — independent pulses, all pcall-safe:**

- **VirtualUser** click (`ClickButton2`, modern `Vector2` form with legacy fallback)
- **VirtualInputManager** key taps (`W A S D`, `+ Space` only when Jump is on — a true no-jump mode)
- **Jump** (`Humanoid.Jump` + backup `Space` press; skipped in no-jump zones / when seated / airborne)
- **Camera** yaw/pitch nudge · **Spin** yaw turn · **Micro-move** — a `1.0–2.2` stud micro-orbit that returns you to your pulse start

**Why it doesn't disturb your farm:** pulses are anchored to your **idle spot** (spawn / base / guards zone). Drift beyond `10` studs triggers a soft return; **Reset Anchor** forces a new anchor anywhere. Groundedness is verified via `FloorMaterial` before every move.

**Modes & cadence:**

| Mode | Character |
| --- | --- |
| **Safe** | Sparse, minimal pulses |
| **Balanced** *(default)* | Even cadence with human jitter |
| **Aggressive** | Dense pulses for maximum safety |

Interval slider `15s`–`120s` (default `45s`) with `0.85x`–`1.15x` human-like jitter, **manual-activity reset** (your own input postpones the next pulse), and an **`Idled` emergency fallback**.

**GUI:** modern dark-glass window, draggable, animated show/hide, **live countdown**, **pulse log**, per-method chips (`Keys · Jump · Camera · Spin · VU · Move`), a mini-pill when hidden, and persistence (`Polaris_StealForest_AntiAFK.json`). No custom AFK RemoteEvent exists in this game (all 142 remotes scanned) — the genuine 20-minute idle detector is beaten, not bypassed.

---

### 4 — Cold War · Combat Suite

> *80-player NATO-vs-PACT shooter with real ballistics. Silent aim is a **bent shot**, not a claimed one — the direction is rewritten **before** the client encodes it, so the server's copy and the client's copy are the same genuine trajectory.*
> PlaceId `13687899540` · Panel `RightShift` · Panic `F1`–`F4` (configurable)

Ballistics are **solved exactly** using the game's own maths: `pos(t) = origin + dir·V/K·(1−e^(−Kt)) − up·g/2·t²`, with fixed-point flight-time iteration, lead prediction, and the arc **walked** with the bullet's own `RaycastParams` (team collision group included). A shot only bends when the first limb met is a limb of the target; a helmeted head (whose handle eats the claim) falls back to the torso. Bends of `0.1°`–`7.2°` were **measured registered** against real server limb health — the default `7°` FOV sits inside proven range. No `hookfunction`, no `hookmetamethod`: the volley lookup is swapped via a plain table-field swap. **Anticheat note:** the game never resizes a part, never adds a BodyMover, and the panel lives in `gethui()` — at identity 2 the `CoreGui` image scan resolves to `nil`, so it never reaches the panel. ESP is `Drawing` objects, not Instances.

#### ESP

Box · name · class (`Sniper` / `AT` / `Medic` …) · distance + weapon · health bar · head/torso numbers · status flags (`DOWNED` / `SPAWN` / `DISGUISED`) · **helmet marker + knocked flash** · skeleton · tracer · head dot · visible-only · dim-hidden · teammates (blue) · vehicles (+ empty) · max distance · text size · **bipod-mounted flag**.

#### Camera Aimbot

Hitkey / Always / While-firing trigger · aim key · `Head` / `Torso` / `Nearest` · bullet-drop + lead prediction · sticky target · visible-only · FOV circle · smooth H/V · **humanisation** (reaction min/max, wind-up ramp, speed ceiling `°/s`).

#### Silent Aim (Bent Shot)

`Head` / `Torso` / `Nearest` / `Random` · FOV in degrees · hit chance % · **Sniper deadly** (always head, `1400m`) · max distance `800m` · lead moving targets · FOV circle. **Mounted-gun silent aim** for turrets (`M2HB` / `M60T` / `PKT` …) up to `1200m` with its own chance/FOV/lead tuning.

#### RADAR / Telemetry

Rotating radar (range + size, position `Left / Right / Top left / Top right`) showing enemies, teammates, vehicles, turret direction, objectives, names, and sweep · threat list with nearest threat + distance · frontline scores · **launcher impact marker** (blast ring, crosshair, distance/arming label, flatten-to-ground).

#### INTEL

**Radio captions** — enemy callout subtitles + reloading flags · **HitCam armour x-ray** overlay · helmet-knocked sensor · **staff scan** (server staff list pre-warned).

#### Bipod ([BIPODS] update)

**Flag bipod-mounted enemies**, **auto-deploy when ready** (calls the game's own `toggle()` — gated by the game's own ground scan, so it can never plant where the game wouldn't), deploy-key hint, and a live **mount status** readout backed by a local BlockCast probe (`normal ≥ 0.9397`).

#### World

No fog · fullbright · no suppression blur · team-aware enemy checks (`Player.Team` — the exact check `HitReporter` uses), per-limb health (`Head 50 · Torso 150 · limbs 100`), ragdoll-aware (`Ragdolled` attribute), and skip-downed / skip-spawn rules.

---

### 5 — Scam With Your Friends · KolkataFarmer v8

> *Full-pledged farmer: persona calls, shop/gear/team max-out, virus crew, GUI.*
> PlaceId `133878278486520` · GUI `RightShift` to hide · Stop `_G.KF_STOP = true`

KolkataFarmer v8 is a complete office operator. It answers calls, works the full scam catalogue (merged live from `Catalog` + `DailyPlaybooks` + `PlaybookLadder`), and maxes the shop, gear, and team.

#### Persona Calls

A **persona bank** of `50+` playbooks (`fake_card`, `romance`, `police-impersonation`, `pet_rock`, `dragon_sitter`, `moon_camping`, `lost_socks`, `yodel_echo`, `duck_knight`, `pizza_…` and many more), each with objective-specific lines templated on the caller's `NAME`. A **drill mode** builds lines from the game's own objective keyword tables (the 9 ladder playbooks carry no keywords, so they fall back to persona lines). Full call loop: answer → select → haggle → chat → claim → hang up, with transcripts read per call, junk-reply detection (`junkGiveUp`), and auto-hang / auto-fire controls.

#### Shop / Gear / Team / Virus Crew

- **SHOP (ROI)** — buys the best-ROI unlockables when you can afford them.
- **TEAM** — hires and fills staff to max.
- **STYLE** — gear + style unlockables to max-out.
- **Virus crew** — dispatches the **Fire Extinguisher** crew to the nearest fire stands.
- **2× Boost** — auto-buys a spare commission charge and auto-uses it on valuable calls (aggressive mode, minimum-value threshold), with stock/reserve tuning.

Plus: **auto-rebirth** (delay-tuned), day-card dismissal, the desk computer/camera-app policy toggles, and per-call state pruning so multi-hour sessions stay lean.

#### Anti-AFK & Ops

The anti-AFK loop runs on its **own thread — deliberately not gated on `KF_STOP`** (parking at a desk is exactly when you want to stay connected), with jittered camera writes. Live stats (`answers / selects / chats / claims / haggles / fires / buys / boosts / gear / style / rebirths / afk`), a rolling log, and controller flags: `_G.KF_STOP`, `_G.KF_AFK_STOP`, `_G.KF.stats`, `_G.KF.log`, `_G.KF.afk`.

**GUI (`KFGUI`):** CONTROL / AUTOMATION / TUNING sections with `SHOP · TEAM · STYLE · 2X BOOST · PERSONA · HAGGLE · FIRE · BOOST BUY · REBIRTH · ANTI-AFK` toggles, AFK-interval + speed + max-sends steppers, persona-select mode, boost-aggressive switch, variant selector, and `PAUSE / RESUME / STOP`.

---

## Controls at a Glance

| Context | Control | Action |
| --- | --- | --- |
| **Hub** | `RightControl` | Minimize / restore hub |
| **RSE2** | `RightShift` · `Delete` | Menu · **Panic close everything** |
| **Sushi** | `RightShift` · `F8` | Toggle GUI · Toggle Auto-Pilot |
| **Steal a Forest** | `RightShift` | Show / hide Anti-AFK GUI |
| **Cold War** | `RightShift` · `F1`–`F4` | Panel · Panic key (changeable) |
| **KolkataFarmer** | `RightShift` | Hide / show GUI |
| **Any intro** | Click / tap | Skip cinematic startup |

---

## Cinematic Startup

Embedded in every packed build (source: `Assets/startup_animation.lua`):

- A `150`-star field, twinkling dust, and nebula glow with a slowly rotating sky, spiralling gather particles, and three orbiting light trails around the four-pointed **Polaris star**
- Per-letter typography with a light-sweep reveal, plus celestial-chart captions (`URSAE MINORIS` / `DEC +89° 15'`)
- A real progress loader driven by actual init tasks — **click / tap anywhere to skip** to the finale, and any failure is contained so the game below **always** loads

---

## Troubleshooting

- **"could not download build"** — the executor's HTTP is blocked or `raw.githubusercontent.com` isn't whitelisted. Allow **both** `cdn.luaprotect.dev` and `raw.githubusercontent.com` and relaunch — the CDN loader hands off to GitHub for the hub, and GitHub also serves every game build.
- **"not a packed build"** — the branch was pushed without `build/`, or a cache served a stale file. Wait ~5 minutes for raw CDN expiry or re-push `build/`.
- **Wrong game** — join the exact PlaceId from the table first. The hub will offer **Join Game** instead of running the wrong build.
- **Hub doesn't appear / double hub** — re-execute the loader. The hub destroys any previous instance (`POLARIS_HUB.Destroy`) on boot.
- **A game GUI doesn't open** — check the per-game key in [Controls at a Glance](#controls-at-a-glance) (most are `RightShift`).

---

## Developer Notes

Plain sources, the intro, and the build tooling live locally and are **not published** (see `.gitignore`). The repo only ever carries the packed outputs. To publish a new version, run your private builder locally, add only the public artifacts, and push:

```sh
git add .gitignore README.md build/
git commit -m "publish packed builds"
git push origin main
```

`Games/`, `Assets/` and `Obfuscator/` are git-ignored, so `git add .` is also safe — it will never stage them.

```
Polaris Hub/
├── polaris.lua        # hub source (local only — published as build/polaris.lua)
├── Games/             # private plain sources (git-ignored)
├── Assets/            # private cinematic startup source (git-ignored)
├── Obfuscator/        # private build tooling (git-ignored)
└── build/             # ← the ONLY published code: packed builds (hub + 5 games + ui-template)
```

### Branch note

Game builds are fetched from `https://raw.githubusercontent.com/fervidos/PolarisHub/main/...`. If you push to a different branch, update `Config.RepoBase` in `polaris.lua` (and `PANEL_URL` in `Games/coldwar.lua`) to match.

---

> *All tools are game-automation scripts for simulated in-game economies. Use at your own risk — if a game updates, its build is repacked and the hub serves the fresh version on next launch.*

<div align="center">

### ✦ PRECISION IN EVERY DETAIL ✦

*POLARIS — v1.1.0 · fಬೇಕridos / PolarisHub*

</div>
