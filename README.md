# Serca Gear Planner

An interactive week-by-week planner for Lost Ark's Shadow Raid: Serca, helping players visualize Destined Tremor gear progression (crafts + breakthroughs) based on their weekly raid runs.

**Live demo:** https://thejunglewalrus.github.io/serca-gear-progression/

**Run locally:** open `index.html` in any modern browser. No build step, no server.

---

## What it does

Players pick a difficulty (Normal / Hard–Nightmare) for each gate per week, toggle whether they bought the extra-reward box for each gate, then commit craft or breakthrough actions against their running mat pool. Totals carry forward across weeks. If an earlier week is changed, any downstream actions that are no longer affordable get marked red with a reason so the player can see what broke and re-plan.

G1 is mandatory. G2 can be skipped by clicking its active difficulty button again.

## Tech

- Single-file HTML artifact (`index.html`)
- React 18 + Babel standalone via CDN — no build step
- All state in-memory (no localStorage, no backend)
- Dark-mode grey theme (Cinzel + Inter + JetBrains Mono)

## The CONFIG block

All tunable numbers live in a single `CONFIG` object at the top of the `<script>` block in `index.html`. If drop quantities or costs turn out to be wrong, edit there — one place, nothing else to change.

```js
const CONFIG = {
  drops: {
    1: { normal: {thorn:0, frag:4}, hard: {thorn:10, frag:0} },
    2: { normal: {thorn:0, frag:6}, hard: {thorn:15, frag:0} },
  },
  extraRewardMultiplier: 2,
  slotCost: { weapon:40, helm:15, chest:15, gloves:15, pants:15, shoulders:15 },
  slots: [...],
};
```

## Confirmed assumptions (April 2026 patch)

- **G1 drops:** Hard/Nightmare = 10 Agony Thorns. Normal = 4 Fragmented Agony Thorns (40% of Hard).
- **G2 drops:** Hard/Nightmare = 15 Agony Thorns. Normal = 6 Fragmented (40% of Hard).
- **Extra Reward box** (per gate, checked by default): doubles drops for that gate.
- **Weapon craft cost:** 40 mats. **Breakthrough cost = craft cost.**
- **Armor craft cost:** 15 mats per piece (helm, chest, gloves, pants, shoulders). **Breakthrough = craft.**
- **Fragmented Agony Thorns** spend 1:1 with Agony Thorns. The spend algorithm burns Fragmented first, then Thorns.

## Gear slots tracked

Weapon + 5 armor pieces (helm, chest, gloves, pants, shoulders). Not tracked: accessories, honing levels.

## Scope explicitly out

- **Honing** — gear acquisition (craft) and the post-+20 Breakthrough only.
- **Multi-character / roster view** — one character's weekly plan.
- **Persistence** — no save/load yet. If added, prefer URL-hash serialization over localStorage to keep plans shareable.

## Open questions to verify against live game data

1. Exact G1 / G2 drop counts on Hard vs. Nightmare — currently modeled as identical.
2. Exact Fragmented → Agony conversion rate, if not 1:1.
3. Exact Extra Reward box cost (gold? tokens?).
4. Whether Nightmare-only mats exist that should be tracked separately.

## License

Personal project. Not affiliated with Smilegate or Amazon Games.
