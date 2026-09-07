# promo-widget

Remote content for the in-game "check out this game too" card. One folder per game that SHOWS
the card; each folder holds that game's `promo.json` plus the logo/capsule of the game it promotes.
Editing a JSON here changes the shipped games without a build (raw URLs are CDN-cached ~5 min).

| Folder     | Read by (shows the card) | Raw URL the game fetches                                                                  |
|------------|--------------------------|-------------------------------------------------------------------------------------------|
| `konbini/` | Konbini Cleanup (4951190)| https://raw.githubusercontent.com/999wakawaka999-create/promo-widget/main/konbini/promo.json |
| `spacerockbreaker/` | Space Rock Breaker — **not live yet** | https://raw.githubusercontent.com/999wakawaka999-create/promo-widget/main/spacerockbreaker/promo.json |

(Super Retail Boss still reads the older `srb-promo` repo.)

`spacerockbreaker/` is STAGED, not wired: art only, no `promo.json`, and the game has no promo
card yet — it is Unity, while the existing card is Unreal/C++. It will promote SmashCore
(4945040). Note the folder is named for the game that SHOWS the card, per the rule above — not
for SmashCore, which is the game being advertised.

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
