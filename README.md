# Serca Gear Planner

An interactive week-by-week planner for Lost Ark's Shadow Raid: Serca, helping players visualize Destined Tremor gear progression (crafts + breakthroughs) based on their weekly raid runs.

**Live artifact:** open `serca-planner.html` in any modern browser. No build step, no server.

---

## What it does

Players pick a difficulty for Gate 1 and Gate 2 each week (Skip / Normal / Hard / Nightmare), toggle whether they bought the extra-reward box for each gate, then commit craft or breakthrough actions against their running mat pool. Totals carry forward across weeks. If an earlier week is changed (e.g., Hard → Normal), any downstream actions that are no longer affordable get marked red with a reason so the player can see what broke and re-plan.

## Tech

- Single-file HTML artifact (`serca-planner.html`)
- React 18 + Babel standalone via CDN — no build step
- All state in-memory (no localStorage, no backend)
- Aesthetic: dark occult/witchy theme (Cinzel + EB Garamond + JetBrains Mono)

## The CONFIG block

All tunable numbers live in a single `CONFIG` object at the top of the `<script>` block in `serca-planner.html`. If drop quantities or costs turn out to be wrong, edit there — one place, nothing else to change.

```js
const CONFIG = {
  drops: {
    1: { skip:{thorn:0,frag:0}, normal:{thorn:0,frag:4}, hard:{thorn:10,frag:0}, nightmare:{thorn:10,frag:0} },
    2: { skip:{thorn:0,frag:0}, normal:{thorn:0,frag:6}, hard:{thorn:15,frag:0}, nightmare:{thorn:15,frag:0} },
  },
  extraRewardMultiplier: 2,
  slotCost: { weapon:40, helm:15, chest:15, gloves:15, pants:15, shoulders:15 },
  slots: [...],
};
```

## Confirmed assumptions (April 2026 patch, confirmed by project owner)

- **Gate 1 drops:** Hard/Nightmare = 10 Agony Thorns. Normal = 4 Fragmented Agony Thorns (40% of Hard).
- **Gate 2 drops:** Hard/Nightmare = 15 Agony Thorns. Normal = 6 Fragmented (40% of Hard).
- **Extra Reward box** (per gate, checked by default): doubles drops for that gate (applies to both Thorns and Fragmented).
- **Weapon craft cost:** 40 mats. **Breakthrough cost = craft cost.**
- **Armor craft cost:** 15 mats per piece (helm, chest, gloves, pants, shoulders). **Breakthrough = craft.**
- **Fragmented Agony Thorns** spend 1:1 with Agony Thorns. The spend algorithm burns Fragmented first, then Thorns. If the real exchange rate turns out different (e.g., 5 Fragmented = 1 Thorn), update the `spend()` function in `serca-planner.html`.

## Gear slots tracked

Weapon + 5 armor pieces (helm, chest, gloves, pants, shoulders). Not tracked: accessories, honing levels.

## Scope explicitly out

- **Honing** — the project owner decided honing is not part of this tool. Only gear acquisition (craft) and the post-+20 Breakthrough.
- **Multi-character / roster view** — current scope is one character's weekly plan.
- **Persistence** — no save/load; the planner is a working surface. If persistence is added later, prefer URL-hash serialization over localStorage to keep it shareable.

## Open questions to verify against live game data

These were not publicly available at build time (patch was 2 days old). Confirm and adjust `CONFIG` if wrong:

1. Exact Gate 1 / Gate 2 drop counts on Hard vs. Nightmare. Does Nightmare drop more than Hard, or the same? (Currently modeled as the same; community data may reveal a bump.)
2. Exact Fragmented → Agony conversion rate, if not 1:1.
3. Exact Extra Reward box cost (gold? tokens?) — the tool doesn't model the cost of buying the box, just the ×2 benefit.
4. Whether Nightmare-only mats exist (e.g., Ark Grid cores that don't convert to Thorns) that should be tracked separately.

## Design decisions worth preserving

- **Buttons, not dropdowns.** Player wanted gates/difficulties as toggle buttons at the top of each week card.
- **Red-marking, not auto-deletion.** When a prior-week change invalidates a downstream action, the action stays visible, struck-through, with a reason. The player removes it manually. This was an explicit request.
- **Committed actions visible below craft/breakthrough buttons.** Each has an × button to remove.
- **Extra Reward defaults to checked** for both gates.
- **Add-week button inherits the previous week's gate settings** (convenience, not required).
- **Sticky ledger bar** at top showing total Thorns, total Fragmented, and total spendable pool.

## Files in this repo

- `serca-planner.html` — the actual app. Double-click to run.
- `README.md` — this file.
- `.gitignore` — standard ignore file.

## Running locally

```
# Just open the file. There is no build.
open serca-planner.html      # macOS
xdg-open serca-planner.html  # Linux
start serca-planner.html     # Windows
```

## License

Personal project. Not affiliated with Smilegate or Amazon Games.
