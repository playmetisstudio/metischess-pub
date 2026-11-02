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
