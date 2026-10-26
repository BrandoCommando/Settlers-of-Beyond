# Settlers of Beyond — Design Document

A survival strategy / post-apocalyptic shooter that draws strong cues from Settlers of Catan at the **World** layer. Touch-first (mobile), with a web client as well.

**Document status:** Design only. No implementation, art pipeline, or networking in this pass. Numbers below are placeholders to make loops concrete.

---

## Vision & pillars

### Elevator pitch

You are a settler at the edge of a ruined world. You **run vertical missions** to survive and loot, **build up your HQ** so the next run is stronger, and eventually **claim hex territory** and look to the stars. The opening fantasy is a mobile vertical running shooter with light base building—not a card battler, not an MMO on day one.

### Pillars

1. **Survive the run** — Short vertical missions where loadout, noise, and positioning decide whether you extract with loot or limp home empty-handed.
2. **Grow the HQ** — Convert loot into structures that passively gather, craft, and unlock better gear for the next run.
3. **Expand Beyond (later)** — Hex World territory, alliances, then multi-world / galactic scale once the run ↔ HQ loop is proven.

### Genre stack (layered, not equal)

```
Vertical runner mission  →  Loot & resources  →  HQ build & craft  →  Better loadout  →  (repeat)
                                                              ↓ later
                                                    Hex World & alliances
                                                              ↓ much later
                                                       Beyond (galactic)
```

| Layer | Role | When |
| --- | --- | --- |
| **Run** | Primary minute-to-minute play | First 10 minutes and ongoing |
| **HQ** | Secondary fantasy; spends loot, gates progression | Same first session |
| **World** | Catan-like hexes, hotspots, shops, land claims | Mid-game (designed, not first playable) |
| **Beyond** | Multiple worlds, space travel | Late |

**Catan DNA** lives on the World layer: hexagonal geography, competing for resource-rich tiles, territory. It is **not** the opening loop.

### v0 non-goals

- No always-online MMO requirement
- No PvP
- No alliances required to progress
- No card-battler as the primary combat mode (heroes are **run loadouts**, represented as cards for identity/collection)
- No galactic / multi-world play

---

## First-session script (≈10 minutes)

**Goal:** Teach run → loot → build → re-run with a clear upgrade payoff. Player ends knowing what the game *is*.

| Time | Beat | What happens |
| --- | --- | --- |
| 0:00–0:45 | Wake | Intro: ruined outpost. Meet **Jack** (common Hero). Axe + handgun with **20 bullets**. Prompt: survive. |
| 0:45–1:30 | Controls | Short safe lane: swipe/tap lanes to dodge, tap to fire, hold/melee for close targets. Noise meter teased once (“loud shots wake the dead”). |
| 1:30–4:00 | Tutorial run | Vertical run: few animals (food/cloth), scrap wood, 2–3 zombies. Teach axe for quiet kills, gun for emergencies. Extract or reach end zone. |
| 4:00–5:00 | Extract | Return to **HQ** view. Loot summary: e.g. Food ×N, Wood ×N, Dirty Water ×1, Cloth scrap. |
| 5:00–7:00 | First build | Guided place of **Farm** *or* **Lumber mill** (player choice if both affordable; else scripted Farm). HQ grid shown as square sections. |
| 7:00–8:00 | Craft / stash | Optional: craft a scrap bandage or refill a few bullets if Gunsmith is locked—keep it HQ-side, not a second tutorial systems dump. Warehouse tip: stored resources are safe here. |
| 8:00–10:00 | Second run | Same or slightly harder lane. Payoff is explicit: Farm L1 → **+food on return** or Lumber → **bonus wood from tree nodes**; or handgun starts with **+5 bullets** from scavenged brass. Die or extract; either way the loop is clear. |

**Out of first session:** World map, alliances, troops, robots, rare heroes, electricity as a gate, heavy weapons.

**Story beat after tutorial (later sessions):** Leaving HQ into **World** and meeting alliances—called out in Future systems, not required for the first playable fantasy.

---

## Core loop

### Loop summary

1. Pick a **Challenge** (mission type) from HQ.
2. Enter a **vertical runner** lane with current Hero loadout.
3. Fight / stealth / gather; manage **noise** and ammo.
4. **Win** (extract or clear objective) → loot deposited; **Fail** (downed) → lose a portion of carried loot (warehouse stores are safe).
5. Spend resources on **HQ** upgrades and crafting.
6. Improved structures / gear → longer or richer runs.

### Mission structure

- **Length:** ~60–180 seconds for early challenges; scale up later.
- **Scroll:** Vertical (player advances “up” the lane). Touch-friendly lane movement + fire.
- **Objectives:** Depend on challenge type (see Challenges).
- **Extract:** Reach the end marker or call extract after objective; carried loot locks in on success.

### Win / fail (placeholders)

| Outcome | Result |
| --- | --- |
| Objective complete + extract | Full carried loot; challenge XP / progress |
| Objective complete, downed before extract | Partial loot (e.g. 50%) |
| Downed before objective | Minimal loot (e.g. 10–25%); challenge incomplete |
| Abandon | No loot; no progress |

### Loot & resources (tied to economy)

**Early-run focus (v0):** Food, Wood, Dirty/Clean Water, basic Cloth (wool/cotton scraps), light Structural (straw/plastic/wood). Currency and high tiers gated.

| Resource | Role in loop | Early sources |
| --- | --- | --- |
| **Food** | Sustain / craft; Farm output | Animals, Farm |
| **Water (Dirty)** | Tasks, crafting, watering | Pumps (dirty), scavenged barrels |
| **Water (Clean)** | Drinking / healing gate | Filter craft or upgraded pump |
| **Structural** | Straw → Plastic → Wood → Metal | Gather nodes, Lumber mill, later salvage |
| **Cloth** | Wool → Cotton → Leather → Kevlar | Animals, Farm wool, later scavenges |
| **Electricity** | Mid unlocks (vehicles, advanced craft) | Deferred as early gate |
| **Currency** | Coins → Rubies → Diamonds → Gold Bars | Deferred / shops / later rewards |

Placeholder example: **Farm L1** → +3 Food granted when returning from a successful run (in addition to carried loot).

---

## Combat model

### Feel

Mobile **vertical running shooter**: auto-advance or light forward momentum, **tap/swipe to change lane or strafe**, tap to shoot, dedicated melee control for quiet kills. Designed for thumbs; web client mirrors the same scheme with pointer/keys.

### Weapon categories (loadout)

| Category | Examples | Noise | Role |
| --- | --- | --- | --- |
| **Melee** | Stick, club, pipe, axe | Silent / very low | Quiet clears, ammo saving |
| **Silent projectile** | Bow, crossbow | Low | Stealthy ranged |
| **Power projectile** | Handgun, SMG, HMG, rockets; loud tools (e.g. chainsaw) | High | Panic button; attracts attention |
| **Static** | Traps, explosives | Varies (explosives loud) | Chokepoints, delayed kills |

**v0 loadout slots (Jack):** Melee (axe) + Power (handgun, 20 bullets). Bow and traps unlock via HQ / challenges soon after.

### Noise rules

- Each loud action adds to a **Noise** meter (or aggro radius).
- Crossing thresholds **spawns or pulls** extra zombies (later: robots).
- Silent kills and bows keep the lane manageable; spraying the handgun clears fast but risks a swarm.
- Tutorial teaches this once with a visible feedback beat.

### Enemies (run NPCs)

| Enemy | Tier | Notes |
| --- | --- | --- |
| **Animals** | Early | Food / cloth; low threat if not cornered |
| **Zombies** | Early–mid | Noise-responsive; core pressure |
| **Robots** | Mid+ | Deferred for first hour; armored, electricity loot |
| **Shops** | World layer | Not run combatants; specialty vendors on hex map |

Bosses that occupy hexes belong to **World**, not the first-session runner.

---

## Heroes

Heroes are the player’s **run identity and power**, shown as collectible **cards**, but combat is the vertical shooter—not a card-battle mode.

### Stats

- **Attack**, **Defense**, **Intelligence** → combined **Power**
- Rarity tiers: **Common (green)** → Uncommon (blue) → Rare (purple) → Ultra-rare (gold) → Maximum (diamond)

### Starter

- **Jack** — Common. Axe + handgun (20 bullets). Tutorial and default loadout.
- Rarity chase and multi-hero roster deepen **after** the run ↔ HQ loop is fun; ultra-rare / diamond economy is deferred in depth.

### Design stance

- Heroes = **who you take on the run** (stats, weapon affinities, maybe passive).
- Cards = **presentation / collection / progression UI**, not the primary verb in the first 10 minutes.

---

## HQ

HQ is a grid of **square sections** for structures. First-session UI focus: HQ view only (World / Beyond tabs locked or grayed).

### Tier 1 (first hour) — in focus

| Structure | Purpose |
| --- | --- |
| **Headquarters** | Hub level; unlocks other plots and challenge tiers |
| **Farms** | Food / wool over time or on-run bonuses |
| **Lumber mills** | Wood from runs / passive ticks |
| **Water pump** | Dirty water; upgrade path toward Clean |
| **Warehouses** | Protect stored resources from run failure / raids (raids later) |

### Tier 2+ — deferred

| Structure | Purpose |
| --- | --- |
| **Mechanic** | Make / fix vehicles |
| **Gunsmith** | Bullets and firearm mods |
| **Houses** | Troops; passive EXP |
| **Training camps** | Troop experience |
| **Hospitals** | Heal wounded troops |

Troops are a **base / World defense and army** layer, not the star of the opening runner fantasy. Exact split vs Heroes: see Open questions.

---

## Challenges

Challenges are the **mission picker**—primary early advancement. Each maps to a runner (or HQ) pattern:

| Challenge | Player verb | Typical rewards |
| --- | --- | --- |
| **Hunt** | Kill / harvest animals on the lane | Food, cloth |
| **Clear** | Wipe zombies / secure a stretch | “Land” progress (later World claim), scrap |
| **Gather** | Chop trees / loot nodes; optional combat | Wood, structural, dirty water |
| **Scavenge** | Runner with a **puzzle beat** (lock, wiring, pattern) | Technology scraps, later electricity |
| **Craft** | HQ-side (not a run): turn raw cloth → clothing | Gear for loadout |

Clearing zombies to “gain land” is aspirational toward World hex unlocks; in v0 it can mean unlocking the next challenge lane or an HQ plot.

---

## UI views

Outside of active challenges (runs), three main views:

| View | Role | Scope |
| --- | --- | --- |
| **HQ** | Base building, craft, challenge select, hero loadout | **Now** |
| **World** | Isometric hex map: player HQs (1 hex), bosses (1+ hexes), specialty shops, resource/tech hotspots | **Soon / mid** |
| **Beyond** | Galactic zoom; multiple balanced worlds; space travel much later | **Later** |

---

## Future systems (deferred stubs)

Ideas preserved so the long vision isn’t lost. Not required for first playable.

### World

- Large geographically diverse land, isometric **hex** tiles.
- Player HQ occupies one hex; bosses can span multiple.
- Specialty shops; **hotspots** rich in resources / technology.
- Catan-like pressure: valuable hexes, competition (design PvE-first until PvP is revisited).

### Alliances

- Main social layer after the player can leave HQ into World.
- Chat / coordination; shared stat benefits; claim **territories** (groups of hexes).
- Explicitly **not** required for v0 progression.

### Troops & technology

- Troops: housed, trained, hospitalized; likely World defense / mass fights vs Hero-led runs.
- Vehicles & advanced structures via Mechanic and Electricity.
- Robots as mid-tier run / World threats.

### Beyond

- Cap / balance players per World → multiple Worlds.
- Space travel as a late endgame fantasy.

### Full economy & arsenal depth

- Cloth through Kevlar; Metal; Electricity as a real gate.
- Chainsaws, SMGs, HMGs, rocket launchers—loud endgame toys.
- Deep rarity / diamond hero economy.

---

## Scope cheat sheet

| Now (v0 design focus) | Soon | Later |
| --- | --- | --- |
| Jack, vertical runs, noise/stealth | Bow, traps, Gunsmith bullets | Ultra-rare / diamond hero depth |
| Food, wood, water, basic cloth | Leather, metal, light electricity | Full currency stack, Kevlar |
| Melee + handgun | More power weapons carefully gated | Rockets, HMG spectacle |
| Animals + zombies | Robots, World shops | Alliance wars, hex claims |
| HQ + Tier-1 gather buildings | Mechanic, troops, hospital, training | Vehicles, multi-HQ strategies |
| Tutorial → second-run payoff | World tab, first hotspot | Beyond, space travel |

---

## Open questions backlog

- **Controls:** Full auto-run vs tap-to-advance; one-thumb vs two-thumb layout; web keybind parity.
- **Noise:** Continuous meter vs discrete aggro radius; how punishing are swarms for casual players?
- **Dirty vs Clean water:** Exact gameplay split (craft vs heal vs building recipes)—needs a single clear rule players remember.
- **Heroes vs Troops:** Who fights on runs vs who defends HQ / World? Avoid two redundant armies.
- **Fail penalty:** How much carried loot is lost on down—fun tension vs mobile-session frustration.
- **Land from Clear challenges:** Cosmetic unlock vs real hex claim—when does World become necessary?
- **Monetization:** Cosmetics, battle pass, convenience, gacha-on-heroes?—undecided; must not break v0 non-goals.
- **Multiplayer timing:** Solo-first online account vs shared World from first mid-game milestone.
- **Card UX:** How much “playing card” presentation in HQ without implying a TCG combat mode?

---

## Success criteria (this doc)

- A stranger can explain the **first 10 minutes** after reading the First-session script.
- Every major system from the original brainstorm has a home: **Now**, **Soon**, or **Later**.
- Genre ambiguity resolved: **shooter + base** is primary; **heroes/cards** support loadout identity; **Catan/hex** is World-layer DNA.
