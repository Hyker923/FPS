# NEON STRIKE

Krunker-style browser arena FPS. One file, no build step, no assets.

## Run

Double-click `index.html`, then press **DEPLOY** and click the game view to lock the mouse.

Needs internet on first load: three.js is pulled from a CDN (jsDelivr, unpkg fallback).
If the engine fails to load, the page shows an "ENGINE OFFLINE" panel. Fix: check network,
or serve the folder locally:

```
python -m http.server 8080
```

then open the printed `http://localhost:8080/` URL.

Chrome, Edge, Firefox with WebGL2. Desktop only (mouse + keyboard).

## Controls

| Input | Action |
| --- | --- |
| W A S D | run |
| SPACE | jump (hold to bunny hop) |
| SHIFT | slide — free speed boost, jump out of it to keep momentum |
| Left mouse | fire |
| Right mouse | aim down sight (sniper gets scope) |
| R | reload |
| 1 / 2 / 3 / 4 | assault rifle / auto pistol / sniper / combat dagger |
| Q or mouse wheel | cycle weapon |
| TAB (hold) | scoreboard |
| ESC | pause (releases mouse) |

## Rules

Free-for-all, 5 bots, 5:00 match, first to 25 kills.
Difficulty is picked on the start screen (Easy / Medium / Hard, default Medium):

| | accuracy | aim cap | damage | reaction | burst pause | speed | simultaneous shooters | spawn shield |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Easy | ×0.65 | 0.50 | ×0.65 | ×1.5 | ×1.35 | ×0.9 | 2 | 3.5 s |
| Medium | ×1.0 | 0.72 | ×1.0 | ×1.0 | ×1.0 | ×1.0 | 2 | 3.0 s |
| Hard | ×1.8 | 0.95 | ×1.4 | ×0.5 | ×0.55 | ×1.18 | 3 | 1.8 s |

Hard bots also convert 35% of their hits into headshots, and their misses land much closer to you.
Bots are hostile to everyone: they fight you **and** each other. If a bot reaches 25 first, you lose.
Headshots do 2x+ damage — AR kills in 2, sniper one-shots body and head.
Health regenerates after 5.5 s without damage. You respawn 3 s after dying.

You get a 2.5–3 s spawn shield (health label shows `SHIELD n s`) — use it to reach cover.
Bots need 0.4–0.8 s of continuous line of sight before firing, and at most 2 bots shoot at you at once.

## What's inside

- single HTML file, ~1.7k lines, inline CSS + ES module script
- custom AABB collision (axis-separated push-out, auto step-up on stairs, slide/jump bhop physics)
- hitscan weapons with ray-AABB against head/body/leg hitboxes, wall occlusion
- 4 weapons: AR (30/120), auto pistol (18/90, full-auto 660 rpm, compensator), sniper (5/25, scope), combat dagger (melee, 2.7 m fan, 45 dmg / 67 headshot, outside-in slash)
- ammo has a reserve pool now; reloads pull from it
- solo factions: every combatant has a tag and colour (`NEON` for you, `VOID`/`RUST`/`ACID`/`COBALT`/`PLASMA` for bots)
- live standings panel top-right: rank, faction dot, name, kills, your row highlighted
- minimap top-left: real-time positions — faction-coloured dots for every bot, HP pip, player arrow
- detailed view models: receiver, rail, handguard, barrel, muzzle brake, glass/mag, stock, red-dot optic on the AR, oversized scope with bell and turrets on the sniper, tan hands + blue sleeve
- karambit is a single extruded claw silhouette (solid at any swing angle) with a dark spine tube, neon edge tube and ring pommel
- the auto pistol is compact: short slide, exposed frame, angled grip, compensator and extended mag — no stock or handguard, so it never reads as a rifle
- bots use blocky Minecraft-style skins: hair cap/fringe/sideburns, sclera+iris eyes and brows, collared shirt with belt, shoulder pads and backpack, two-tone sleeve/forearm arms, striped cargo legs, glowing soles and chest strip
- 5 skin palettes: orange-hair green tee, blue-hair cyan-visor tech jacket, teal hoodie, magenta jacket, moss work wear; hurt flash tints every cloth material
- beach dressing: sand patches and palm trees around the arena edges
- bot AI: FFA targeting (player + other bots), LOS checks, strafing, burst fire, respawn away from the nearest enemy
- hard mode flips on an `advanced` bot profile: they lead moving targets, plant their feet mid-burst and reposition after, panic-strafe and hop when tagged, bunny-hop the fight, land headshots on 35% of hits — and they push forward while strafing, only breaking contact below 22 hp and then no more than once every 5–8 s
- bots behave like the player: they hop while moving and while rushing, carry an AR / sniper / pistol loadout, switch to the pistol when a sniper gets crowded, and pull out the dagger below 3.4 m for melee swings
- bots hold their weapon in a one-handed carry pose only while engaged; when they are just moving or hunting they lower the gun and the arms swing naturally with the stride (blend at 6/s both ways), so they no longer look permanently raised
- the support (left) arm never grips the gun: it always swings with the stride, even while the bot is firing
- each bot gun is fully detailed (receiver, stock, handguard, barrel + brake, grip, magazine, rail/red-dot for the AR, scope with rings and bipod for the sniper) and is parented to the right forearm, so it tracks the hands
- tracers and muzzle flashes spawn at the actual barrel tip of the held gun
- bot-vs-bot damage and kills are tracked in the standings and killfeed
- HUD: radar alone in the top-left, clock/mode/server/ping block top-centre (no overlap), standings top-right, segmented HP bottom-left, big `mag/reserve` ammo bottom-right
- weapon swaps take time: 0.3 s for the dagger, 0.35–0.65 s for guns; the view model dips and no shot can be fired until the swap finishes. Bots use the same rule, they just swap their held mesh when the timer expires
- killfeed shows a small weapon glyph between killer and victim (AR / auto pistol / pistol / sniper / dagger), red-tinted on your own death
- ammo drops: every kill you land spawns a glowing pack where the bot fell; walk over it (1.2 m) to add reserve ammo to your current weapon (+40% of a mag, capped at 2x reserve). Packs despawn after 22 s
- procedural WebAudio SFX (no audio files)
- enemy health bars: screen-projected, occlusion-checked, colour-coded, shows `name · hp`
- footsteps: distance-driven step sounds for you and nearby bots, landing thud + camera shake, dust puffs at bot feet, animated leg swing and body bob
- HUD: health, ammo, match timer, killfeed, dynamic crosshair, hitmarkers, minimap, scoreboard
- neon arena: central tower + stairs, crates, two roof buildings, side cover

## Push to GitHub

Target repo: <https://github.com/Hyker923/FPS> (already created, currently holds only a stub README).

1. Double-click `push-to-github.bat` — the URL is pre-filled.
2. If git says the push was rejected (the remote already has a commit), answer `y` to the force-push prompt. That overwrites the stub README with this project.
3. Authenticate in the credential window (use a PAT as the password if needed).

GitHub Pages: repo → Settings → Pages → Branch `main` / root. The game then runs at
<https://hyker923.github.io/FPS/> — no build step, the CDN import works over https.

## Tuning

All knobs live near the top of the script in the sandbox:

- `TUNE` — gravity, jump, speeds, slide time, match length, kill target
- `WEAPONS` — damage, headshot multiplier, RPM, mag size, reload, spread, ADS FOV, recoil
- `BOT_COUNT`, `BOT_NAMES`, `BOT_COLORS`
