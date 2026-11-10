# config/event-banner.json

The app downloads this file when the main menu opens and shows it in the red event banner.
Copy `event-banner.example.json`, edit, and commit to `main`. GitHub Pages serves it within
a few minutes (browsers and the app may cache it for about 10 minutes).

| Field | Required | Meaning | Limit |
|---|---|---|---|
| `label` | no | The small yellow tag at the left of the banner | ≤ 10 characters |
| `marquee` | no | The scrolling lines, joined with " · " | ≤ 5 lines, ≤ 120 characters each |
| `tapNotice` | no | Toast shown when the player taps the banner. **Leave the field out and a tap does nothing.** | ≤ 60 characters |
| `hotCard` | no | Which main-menu mode card carries the red "🔥 HOT" tag: `practice` (Luyện tập), `cothe` (Cờ Thế), `coup` (Cờ Úp), `cotuong` (Cờ Tướng), or `none` to hide it. **Leave the field out and the tag stays on Luyện tập.** | one of those words |
| `hotLabel` | no | The text on that tag, e.g. `HOT`, `MỚI` | ≤ 6 characters |

Rules
- A missing or empty field is fine: `label` and `marquee` fall back to the wording built into the
  app, a missing `tapNotice` means no toast, a missing `hotCard` keeps the tag on Luyện tập and a
  missing `hotLabel` reads `HOT`. A misspelt `hotCard` also keeps it on Luyện tập: to remove the tag
  write `"hotCard": "none"`.
- The HOT tag needs an app build from 2026-11-02 or later; older builds ignore both fields.
- While the player is offline the app hides the tag (the menu points at Cờ Thế and Luyện tập another way).
- Plain text only. The `<` character is removed.
- Longer text is cut to the limits above.
- The file must be valid JSON (double quotes, no trailing comma, no comments). If it is not, the app
  keeps showing the last good version.
- Check the result by opening
  `https://playmetisstudio.github.io/metischess-pub/config/event-banner.json` in a browser.

## Maintenance window (`maintenance`)

Announces a server maintenance (main-menu.md Rule 8b). Add the object to `event-banner.json`, keeping the other
fields; **remove it to cancel**.

```json
"maintenance": {
  "start": "2026-11-11T00:30:00+07:00",
  "end": "2026-11-11T08:00:00+07:00",
  "reason": "Nâng cấp máy chủ"
}
```

| Field | Required | Meaning | Limit |
|---|---|---|---|
| `start` | yes | When online play closes. `YYYY-MM-DDTHH:MM:SS+07:00`; without `+07:00` it is read as Vietnam time | — |
| `end` | yes | When it opens again; must be after `start` | at most 3 days after `start` |
| `reason` | no | The small line under the times. **Leave it out and it reads "Nâng cấp máy chủ".** | ≤ 40 characters |

What the players see
- **Before `start`**: once per app launch, a "Máy chủ sắp bảo trì" card with the times ("00:30 – 08:00, Thứ Tư 11/11");
  online play stays open.
- **From `start` to `end`**: Cờ Úp, Cờ Tướng, Shop, Xếp hạng, Hồ sơ and xem cờ are locked even if the server is still on;
  a "Máy chủ đang bảo trì" card (once per launch, and again on every tap on something locked) with buttons to Cờ Thế
  and Luyện tập, which keep working.
- **At `end`**: the menu opens again by itself (checked every minute) with "Đã hết bảo trì, chúc bạn chơi vui!".

Rules
- Times are compared with the phone's clock. Publish the window early (Pages and the app cache up to ~10 minutes).
- A window whose `end` is not after `start`, or that is longer than 3 days, is ignored (a typo cannot lock the game).
- Stopping the server is still done separately (phone Shortcut / scheduler); this file only tells the players.
- Needs an app build from 2026-11-10 or later; older builds ignore the object and say "mất kết nối" while the server is off.

# config/menu-scene.json

The painted courtyard behind the top half of the main menu (main-menu.md Rule 10d). Changing the
picture needs no app build.

| Field | Required | Meaning | Limit |
|---|---|---|---|
| `version` | yes | Raise it every time the picture changes; the app downloads the picture again only then | whole number ≥ 1 |
| `image` | no | HTTPS address of the picture (PNG or JPG, e.g. a file in this repo's `config/`). **Empty = the Văn Miếu picture built into the app.** | HTTPS only |
| `scale` | no | Points of picture per pixel (the built-in 1600 × 1280 picture uses 0.42) | 0.1 – 2, default 0.42 |
| `offsetY` | no | Top of the picture relative to the top of the screen, in points (negative = above). The same on every screen | −600 – 200, default −49 |

Rules
- A new picture keeps the template: a pale area behind the header (top ~120 points), a quiet courtyard
  behind the players and the small board (about 230–430 points down on an iPhone 14), wide enough for an iPad.
- The app keeps the last good picture on the device; any failure (bad JSON, `http://`, a file that is not a
  picture) keeps what is showing. Needs an app build from 2026-11-04 or later; older builds ignore the file.
