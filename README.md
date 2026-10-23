# 🕌 Prayer Tracker (Google Sheets + Apps Script)

A yearly prayer tracker with a live Dashboard, automatic daily prayer times, and a Kaza rule.

---

## Features

- **Add any year** – creates a sheet named after the year (e.g. `2027`) with every date, headers, dropdowns and colours.
- **Colour-coded headers** – Sunnah, Farz, Nafl and Witr each keep their own colour.
- **Prayer-group separators** – thick dividers and alternating shading make Fajr / Zuhr / Asr / Maghrib / Isha / Witr easy to tell apart.
- **Status dropdowns** – `✔ Done`, `↺ Kaza`, `➜ Skipped` with colours.
- **Kaza rule** – if a Farz prayer is set to `↺ Kaza`, its linked Sunnah and Nafl cells become `➜ Skipped` automatically.
- **Dashboard** – today's prayer times, forbidden times, next prayer, today's progress, yearly summary, per-prayer and per-month tables, and a chart.
- **Year dropdown** – choose any year on the Dashboard to see that year's report. It defaults to the latest year.
- **Update button** – refresh the Dashboard with one tick.
- **Automatic daily refresh** – prayer times update every midnight and when the sheet is opened.

---

## Setup (one time)

1. Open your Google Sheet.
2. Go to **Extensions → Apps Script**, delete any default code, paste the full script, and save.
3. In Apps Script, open **Project Settings** and set the time zone to `Asia/Dhaka` (or your own).
4. Reload the sheet. A new menu **🕌 Prayer Tracker** appears.
5. Open your existing tracker tab and run **Setup Current Sheet for a Year…** → enter the year (e.g. `2026`). This formats the sheet, renames it to the year, and keeps your existing marks.
6. Run **Apply Kaza Rule to This Sheet** once to fix existing rows.
7. Run **Install Auto-Update & Button** once and approve the permissions. This creates the Dashboard and enables the daily refresh and the Update button.

---

## Menu guide

| Menu item | What it does |
|---|---|
| Add New Year… | Creates a new sheet named after the year and sets the Dashboard to the latest year |
| Setup Current Sheet for a Year… | Formats the open tab for a year and renames it to that year (marks are kept) |
| Apply Kaza Rule to This Sheet | Applies the Kaza rule to every row of the open year sheet |
| 🔄 Update Dashboard | Refreshes prayer times and Dashboard stats |
| Rebuild Dashboard | Redraws the Dashboard layout (keeps the selected year) |
| Install Auto-Update & Button | Installs the daily refresh, on-open refresh, and the button trigger |

---

## Using the Dashboard

- **Report Year (C3):** pick any year tab to view its report. Only year-named sheets (4 digits) appear in the list.
- **Update button (F3):** tick the checkbox next to "🔄 Update Dashboard". It refreshes and un-ticks itself.
  You can also draw a button (Insert → Drawing), then use ⋮ → Assign script → `updateDashboard`.
- **Today's Progress:** shows values only when the current year is selected, otherwise `–`.
- **Prayer times:** always today's times, whatever year is selected.

---

## Sheet layout (year sheets)

- Row 1: headers. Column A: dates. Columns B–Q: prayer marks.
- Prayer groups: Fajr (B:C), Zuhr (D:G), Asr (H:I), Maghrib (J:L), Isha (M:P), Witr (Q).
- Sheet names must be four digits (e.g. `2026`) so the Dashboard and the Kaza rule recognise them.

---

## Kaza rule details

When a Farz column is set to `↺ Kaza`, these columns in the same row become `➜ Skipped`:

| Farz column | Linked columns |
|---|---|
| Fajr Farz (C) | Fajr Sunnah (B) |
| Zuhr Farz (E) | Zuhr Sunnah (D, F), Zuhr Nafl (G) |
| Asr Farz (I) | Asr Sunnah (H) |
| Maghrib Farz (J) | Maghrib Sunnah (K), Maghrib Nafl (L) |
| Isha Farz (N) | Isha Sunnah (M), second Isha entry (O), Isha Nafl (P) |

Notes:
- Witr is not linked.
- The rule overwrites linked cells, even if they were `✔ Done`.
- Changing a Farz back to Done does not restore the linked cells.
- Pastes or fills of more than 200 cells are ignored by the automatic rule. Use **Apply Kaza Rule to This Sheet** instead.
- To change the links, edit `KAZA_LINKS` at the top of the script.

---

## Settings (top of the script)

| Setting | Meaning |
|---|---|
| `LAT`, `LNG`, `TZ` | Your location and time zone (default: Dhaka) |
| `METHOD` | Calculation method (1 = Karachi) |
| `SCHOOL` | Asr calculation (1 = Hanafi, 0 = Shafi'i) |
| `SUNRISE_BLOCK` | Forbidden minutes after sunrise (default 20) |
| `NOON_BLOCK` | Forbidden minutes before Zuhr (default 10) |
| `MAGHRIB_BLOCK` | Forbidden minutes before Maghrib (default 15) |
| `FARZ_COLS` | Columns counted in "Farz Kaza pending" |
| `KAZA_LINKS` | Which cells the Kaza rule skips |

Prayer times come from the free Aladhan API (api.aladhan.com).

---

## Troubleshooting

- **Menu doesn't appear:** reload the sheet; if it still doesn't, run `onOpen` once from Apps Script.
- **"Could not fetch prayer times":** check your internet connection and try **Update Dashboard** again.
- **`#NAME?` or `#REF!` on the Dashboard:** run **Rebuild Dashboard**. Make sure at least one year sheet (e.g. `2026`) exists.
- **Dropdown has no years:** rename your tracker tab to the year, or run **Setup Current Sheet for a Year…**.
- **Update checkbox does nothing:** run **Install Auto-Update & Button** once and approve the permissions.
- **Wrong "today" or times:** set the Apps Script project time zone and the spreadsheet time zone to your own.
- **Kaza rule not firing:** the sheet name must be four digits, and the edit must be made by hand (large pastes are skipped).
