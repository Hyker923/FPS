# NEON STRIKE

Krunker-style browser arena FPS — warm dusk shopping plaza, four weapons, five bots, live scoreboard screen.
Single file, no build step, no external assets.

---

## Quick start

1. Double-click `index.html`, or open it in Chrome / Edge / Firefox.
2. Press **DEPLOY**, then click the view to lock the mouse.

Needs internet on first load — three.js is pulled from a CDN (jsDelivr, unpkg fallback).
If it fails you get an "ENGINE OFFLINE" panel; serve the folder instead:

```
python -m http.server 8080
```

then open `http://localhost:8080/`.

Desktop only (mouse + keyboard), WebGL2 required.

## Controls

| Input | Action |
| --- | --- |
| W A S D | run |
| SPACE | jump (hold to bunny hop) |
| SHIFT | slide — speed boost, jump out of it to keep momentum |
| Left mouse | fire |
| Right mouse | aim down sight (sniper gets the scope) |
| R | reload |
| 1 / 2 / 3 / 4 | assault rifle / auto pistol / sniper / combat dagger |
| Q or mouse wheel | cycle weapon |
| TAB (hold) | scoreboard |
| ESC | pause — RESUME / RESTART MATCH / QUIT TO MENU |

QUIT TO MENU also exists on the end screen; it returns to the title screen and discards the match score.

## Match rules

**Skydive intro.** Every match starts in first person, facing into the plaza, 70 m up. A four-engine
transport crosses the sky from left to right while you freefall, then the canopy opens and you glide
to your spawn. The five bots drop alongside on their own coloured canopies, staggered across the sky.
Bots stay frozen until everyone touches down, and the match clock only starts on landing.
Click or press any key to skip and land the whole lobby instantly.

**Mode.** Free-for-all, 5 bots, 5:00, first to 25 kills. Bots are hostile to everyone — they fight you
and each other. If a bot reaches 25 first you lose. You respawn 3 s after dying.

**Damage.** Headshots do 2×+. AR kills in two headshots, the sniper one-shots body and head.
Health regenerates after 5.5 s without damage. Landing a kill drops an ammo pack.

**Difficulties** (start screen, default Medium):

| | accuracy | aim cap | damage | reaction | burst pause | speed | simultaneous shooters | spawn shield |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Easy | ×0.65 | 0.50 | ×0.65 | ×1.5 | ×1.35 | ×0.90 | 2 | 3.5 s |
| Medium | ×1.0 | 0.72 | ×1.0 | ×1.0 | ×1.0 | ×1.0 | 2 | 3.0 s |
| Hard | ×1.8 | 0.95 | ×1.4 | ×0.5 | ×0.55 | ×1.18 | 3 | 1.8 s |

Hard bots also turn 35% of their hits into headshots, miss much tighter, lead moving targets, plant
their feet mid-burst, panic-strafe and hop when tagged, and push forward while strafing.

## Features

**Weapons**
- AR (30/120) — red-dot sight, open ring with a centred glowing dot, no housing block
- Auto pistol (18/90, full-auto 660 rpm) — compensator, extended mag, compact handgun silhouette
- Sniper (5/25) — oversized scope with bell, turrets, transparent lens, centre red dot overlay
- Combat dagger — single extruded karambit silhouette, outside-in horizontal slash, 45 dmg / 67 headshot
- Hitscan ray-AABB against head / body / leg boxes with wall occlusion
- Reserve ammo pool; reloads pull from it; weapon swaps take 0.3–0.65 s with a view-model dip

**Bots**
- FFA targeting across player and bots, LOS checks, strafing, burst fire, respawn away from enemies
- Two-handed carry pose only while engaged; otherwise arms swing with the stride
- Weapon switching by range (AR / sniper / pistol primary, dagger under 3.4 m) with the same swap delay
- Bots bunny-hop, dodge-jump when hit, fire from the real barrel tip with muzzle flashes
- Blocky skins: layered hair, ears / nose / cheeks, jacket with zip, pocket, badge, buckle, hip pouch,
  backpack, elbow pads, gloves, knee pads, boots with soles, thigh holster, cap or glowing headphones
- Five palettes: orange-hair green tee, blue-hair tech jacket, teal hoodie, magenta jacket, moss work wear

**HUD**
- Circular radar top-left with live faction-coloured dots and HP arcs
- Clock / mode / server / ping block top-centre, live standings top-right
- Segmented HP bar bottom-left, big `mag/reserve` ammo bottom-right, `kills vs deaths` line
- Killfeed with weapon glyphs, hitmarkers, floating enemy health bars, damage vignette
- Big wall screen on the north wall: 1024×512 canvas redrawn twice a second with the live MVP
  (blocky avatar from that player's palette, name, kills) and a live top-six ranking

**Map — warm dusk shopping plaza**
- Open central pavilion: two half-height wooden walls and two steel columns under a red/white striped canopy
- Small crates, striped market barriers and parked cars as cover; lowered mall wings
- Ten awning storefronts around the perimeter (cafe, shoes, tech, books, bakery, arcade, flowers, music,
  food court, mall), plus department-store and cinema wings
- Raised pavement ring, benches, dumpsters, vending machines, road barriers, crosswalks, rooftop AC units
- Big top-centre billboards and neon signage, sodium street lamps, wall pipes
- Far scenery: 700 m city ground plane, 28 lit mid-distance blocks with rooftop beacons, 36 street lamps,
  30-tower skyline in fog
- Warm palette: sandstone facades, warm asphalt and paving, amber neon, peach-to-orange dusk sky
- One 1024² sun shadow map; walls, props and bots cast, floor and props receive

**Audio** — all procedural WebAudio: gunshots per weapon, hit and headshot blips, reload clicks,
footsteps with distance falloff, landing thud, knife swing, bot gunfire. No audio files.

## How it is built

- One `index.html` (~4k lines): inline CSS, inline ES module, no dependencies except three.js from CDN
- Custom AABB physics: axis-separated push-out, auto step-up, slide, bunny-hop air control
- Procedural canvas textures (asphalt, sandstone facade with lit windows, pavement, concrete, metal,
  wood, signage) — no image files
- Everything else is box/cylinder geometry built at load: map, guns, characters, plane, chutes

## Deploy to GitHub

Target repo: <https://github.com/Hyker923/FPS>

**Option A — helper script**

1. Double-click `push-to-github.bat` (URL pre-filled).
2. If git rejects the push because the remote already has a commit, answer `y` to force-push.
3. Authenticate in the credential window (PAT as password if needed).

**Option B — web upload**

Open the repo → **Add file → Upload files** → drag `index.html` and `README.md` in → **Commit changes**.

**Play online:** repo → Settings → Pages → Branch `main` / root, then open
<https://hyker923.github.io/FPS/>.

## Tuning

All knobs are near the top of the script:

- `TUNE` — gravity, jump, speeds, slide time, match length, kill target
- `DIFFICULTIES` — per-difficulty accuracy, damage, reaction, speed, shooter cap, spawn shield
- `WEAPONS` — damage, headshot multiplier, RPM, mag / reserve, reload, spread, ADS FOV, recoil, swap time
- `BOT_WEAPONS` / `BOT_PRIMARIES` / `BOT_COUNT` — bot loadouts and roster
- `SKINS` — character palettes and accessories (`acc: 'cap' | 'phones' | 'none'`)
- `INTRO` — skydive duration
