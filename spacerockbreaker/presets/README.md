# Space Rock Breaker — promo presets

> ⚠ **INERT.** Space Rock Breaker reads `999wakawaka999-create/srb-promo` (root `promo.json`), not
> this repo. Copying a preset over `../promo.json` changes nothing that players see. These are
> staging/reference content; to go live they must be moved into `srb-promo`.

Each folder is one ready-to-go promo: its art and its `promo.json`, complete and self-contained.
Pick one by name ("show the smashcore deal", "switch to the supporter pack") and swap it in.

| Preset | Promotes | App id | Art |
|---|---|---|---|
| `smashcore-deal/` | SmashCore, -25% sale | 4945040 | `capsule.jpg` (462×174, deal badge) |
| `srb-supporter-pack/` | Space Rock Breaker - Supporter Pack (own DLC) | 5063290 | `capsule.png` (462×174) |

## Swapping

One file copy, from inside `spacerockbreaker/`:

```
cp presets/<name>/promo.json promo.json
git add promo.json && git commit -m "spacerockbreaker: switch to <name>" && git push
```

Nothing else to edit. Each preset's `imageUrl` already points at its own capsule, which keeps its
URL forever — so the art never moves and never needs rewriting. The live `promo.json` carries a
`"_preset"` key naming where it came from; the key is not part of the schema and is ignored by the
game's parser, so it is safe and purely for humans.

To see what is live: `grep _preset promo.json`.

## Rules

- **Presets are per-game.** Locale codes here are Space Rock Breaker's 17
  (`cs de es fr hu it ja ko pl pt ro ru th tr uk zh-Hans zh-TW`). Konbini uses a different set —
  `pt-BR` and `zh-Hant` where this game uses `pt` and `zh-TW`, plus `es-419`/`id`/`vi`, and no
  `ro`. Never copy a preset between games; a wrong code silently renders English.
- **Add a language to the game, add it to every preset here** — not just the live one.
- `spacerockbreaker/promo.json` is the only permanent URL. Shipped builds fetch that path forever,
  so it must never move. Everything under `presets/` can be reorganised freely.
- **No shipped fallback exists yet.** Space Rock Breaker has no promo card (it is Unity; the
  existing card is Unreal/C++), so none of this is fetched by anything today. When the card is
  built, its fallback must be seeded from whichever preset is live, and kept in sync from then on.

## Provenance

`smashcore-deal` is the content published 2026-09-08 (`e11fcde`), moved here unchanged apart from
the `imageUrl` now pointing into the preset folder.

`srb-supporter-pack` **diverges from an existing version in another repo.** A supporter-pack
promo.json already lives at `999wakawaka999-create/srb-promo` (`main/promo.json`) — same DLC
(5063290) and the same 17 locales, but different copy:

| | `srb-promo` (existing) | this preset |
|---|---|---|
| tagline | "Check out the supporter pack!" | "Enjoying the game? Support us!" |
| cta | "WISHLIST NOW!" | "GET THE SUPPORTER PACK!" |
| ctas[] | **empty** | 17 locales |
| imageUrl | `srb-promo/.../5063290/small_capsule.jpg` | this preset's `capsule.png` |

The copy here was written on 2026-09-08 without knowledge of the `srb-promo` version; the capsule
came from the game's marketing art (`LTP/SRB/small.png`). **Left as-is deliberately** — reconciling
the two is a decision, not a merge. Before this preset ever goes live, decide which wording wins,
and note that "WISHLIST NOW!" is wrong for an already-released DLC.
