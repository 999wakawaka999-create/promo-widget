# promo-widget

Remote content for the in-game "check out this game too" card. One folder per game that SHOWS
the card; each folder holds that game's `promo.json` plus the logo/capsule of the game it promotes.
Editing a JSON here changes the shipped games without a build (raw URLs are CDN-cached ~5 min).

| Folder     | Read by (shows the card) | Raw URL the game fetches                                                                  |
|------------|--------------------------|-------------------------------------------------------------------------------------------|
| `konbini/` | Konbini Cleanup (4951190)| https://raw.githubusercontent.com/999wakawaka999-create/promo-widget/main/konbini/promo.json |
| `spacerockbreaker/` | ⚠ **NOBODY — inert, see below** | https://raw.githubusercontent.com/999wakawaka999-create/promo-widget/main/spacerockbreaker/promo.json |

(`srb-promo` is a separate, older repo holding a Space Rock Breaker Supporter Pack promo — same
DLC 5063290, different copy. Not read by anything in this repo. See
`spacerockbreaker/presets/README.md` before reusing it.)

## ⚠ `spacerockbreaker/` is INERT — do not edit it expecting results

**Space Rock Breaker has a working promo card, but it reads a DIFFERENT repo:**

```
https://raw.githubusercontent.com/999wakawaka999-create/srb-promo/main/promo.json
```

`srb-promo` serves one game, so its `promo.json` sits at the repo **root**, with art foldered by
promoted app id (`4949230/`, `5063290/`). **To change what Space Rock Breaker shows, edit that
file.** Nothing fetches anything under `spacerockbreaker/` here.

This folder was created on 2026-09-08 on the mistaken assumption that the game had no card and
would be wired to this repo. Its content and presets are kept as staging/reference only. Either
migrate them into `srb-promo` or delete the folder — but do not treat it as live.

## Updating a game's promo

Two ways in, then the same three steps out.

**Either** edit `<game>/promo.json` directly — art goes in the repo alongside it, and `imageUrl`
points at its raw URL. **Or**, if the game keeps a `presets/` library, copy a ready-made one:

```
cd <game>
cp presets/<name>/promo.json promo.json      # nothing else to edit
```

Then, always:

1. **Commit and push.** This is the live change; nothing else deploys it.
2. **Mirror it into the game's shipped fallback.** The fallback is a copy of this JSON compiled
   into the game (in Unreal, the `Fallback=(...)` line in `Config/DefaultGame.ini`). Offline
   players, failed fetches and timeouts all get it. Nothing keeps the two in sync automatically —
   skip this and offline players keep seeing the old promo forever.
3. **Verify.** Raw URLs are CDN-cached ~5 minutes, so wait before concluding anything, then
   `curl` the raw URL. In-game the proof is one log line: `Promo/Config source=remote` with the
   new app id. `source=fallback` means the fetch failed and you are looking at the shipped copy.

Rules that bite:

- **A remote config replaces the fallback wholesale**, field by field it is not merged. A missing
  key falls back to the code's struct default, not to your shipped values. Publish complete files.
- **`<game>/promo.json` is a permanent URL.** Shipped builds fetch that exact path forever, so it
  can never move. Anything else (art, presets) can be reorganised freely.
- **Locale codes must match the reading game's culture names exactly.** A wrong code is not an
  error — that language silently renders English. See the per-game lists below.
- **Never copy content between games**; their locale sets differ.
- **Kill switch:** `"enabled": false` (or `"appId": 0`) hides the card for everyone on next launch.

## promo.json schema (keys case-insensitive)

```json
{
  "enabled": true,                 // false = remote kill-switch, card hidden everywhere
  "appId": 1234567,                // Steam AppID of the PROMOTED game; 0 = hidden
  "imageUrl": "https://raw.githubusercontent.com/999wakawaka999-create/promo-widget/main/konbini/logo.png",
  "tagline": "English line above the box",
  "taglines": [ { "locale": "ja", "text": "..." } ],
  "cta": "WISHLIST NOW!",
  "ctas":     [ { "locale": "ja", "text": "..." } ]
}
```

Locale codes must match the reading game's culture names exactly, else that language falls back
to the English field. Konbini Cleanup: `cs de es es-419 fr hu id it ja ko pl pt-BR ru th tr uk vi
zh-Hans zh-Hant`.
