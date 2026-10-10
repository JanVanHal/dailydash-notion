# Build "Dagelijks Overzicht" v2: dailydash restyled in the Notion design, with every button working

## Goal
Make a **new copy** of Jan's personal daily dashboard and rebuild it in the Notion "warm paper" design system used by NetPulse, UnitPulse, HealthPulse and Family Budget. Keep every feature, and **inspect and fix the code so every button and setting actually works**.

- Source (read-only, do NOT modify): `https://github.com/JanVanHal/dailydash`, one file `index.html` (~75 KB). Ignore the variants `dailydash_uwhd` and `dailydashX`.
- Design reference (read-only): `https://github.com/JanVanHal/unitpulse` and `https://github.com/JanVanHal/healthpulse`. Reuse their `styles.css` tokens (`--nt-*`, Inter + Source Serif 4).
- This repo: `JanVanHal/dailydash-notion`. Build here, push to `main` and enable GitHub Pages from `main` / root.

## What the current app has (keep all of it)
- **Header**: title "Dagelijks Overzicht" plus buttons `btn-refresh` (refresh data), `btn-theme` (cycles Light / Medium / Dark), `btn-toggle-layout` (2-column ↔ 3-column with the widgets on the right), `btn-text-size` (article text size) and `btn-open-settings`.
- **Widgets**: EUR/THB exchange rate (`api.frankfurter.app`, fallback `open.er-api.com`), weather for **Chiang Rai** and **Noordwijk aan Zee** (`api.open-meteo.com`) with links to the rain radar (RainViewer Chiang Rai, Buienradar Noordwijk), and clocks / time zones (Chiang Rai and the Netherlands).
- **News**: "Nederlands Nieuws" and "Sportnieuws" from NOS RSS feeds (algemeen, economie, politiek, wereld; sport algemeen, voetbal, wielrennen, schaatsen) via `api.rss2json.com`, grouped by category.
- **Settings modal**: Weergave & Kolommen, Klokken & Tijdzones, Weerlocatie 1 and 2, Wisselkoers EUR/THB, Zichtbaarheid Onderdelen, plus buttons `btn-reset-defaults`, `btn-cancel-settings`, `btn-save-settings` and `btn-close-settings`.
- Settings are saved in `localStorage` under **`dashboard_config_v3`**. Keep this key and its shape so Jan's current settings carry over (same origin `janvanhal.github.io`).
- The UI language is **Dutch**. Keep all labels in Dutch.

## Design
- Remove the **Tailwind CDN** and all Tailwind classes. Remove **DM Sans**. Use the UnitPulse `styles.css` as the base and add dashboard components at the end, using existing `--nt-*` tokens only.
- Cards are `tool-panel` style (white surface, hairline `--nt-border`, card radius) on the warm `--nt-canvas`. Widget headings are small caps captions; news is a clean list with source time, title and a short summary.
- **Three themes**, all from tokens: Light (`--nt-canvas` / `--nt-surface`), Medium (slightly darker paper, for example a mix of `--nt-peach` / `--nt-canvas` tones), and Dark (`--nt-midnight` / `--nt-surface-dark` with `--nt-on-dark` text, and the accent `#00dcfa` like the Pulse logos). Switch them with a `data-theme` attribute on `<html>` and save the choice in the config.
- Icons: keep **Lucide**, but pin a fixed version instead of `@latest` (for example `https://unpkg.com/lucide@0.469.0`), and call `lucide.createIcons()` after every render that adds icons.
- Make an SVG icon `img/dailydash-icon.svg` (favicon plus header) in the same flat style as the Pulse icons: a dark rounded square with a cyan sun/clock line icon.
- Mobile-first: one column on the phone, 2 or 3 columns on desktop following the layout toggle.

## Code: inspect and make every control work
Go through the code systematically, then fix and test:
1. Every button: refresh, theme, layout, text size, open, close, cancel, save, reset. Each needs exactly one listener, a visible effect, the correct `aria-pressed` / `aria-label` state and keyboard support. Remove duplicate or dead listeners.
2. Every settings field: on **Save**, apply it immediately and store it; **Cancel** and **Close** discard changes; **Reset** asks for confirmation, then restores the defaults. Escape and a click on the backdrop close the modal. Trap focus while it's open.
3. "Zichtbaarheid Onderdelen" toggles really hide or show each section, and the layout reflows without gaps.
4. Clocks tick every second without drifting, use `Intl.DateTimeFormat` with IANA zones (`Asia/Bangkok`, `Europe/Amsterdam`) so daylight saving is correct, and the zones can be changed in settings.
5. Weather: changing a location in settings (name plus lat/lon) re-fetches the forecast. Show the current temperature, an icon, wind, rain chance and the day's min/max. Radar links follow the location where possible.
6. Exchange rate: try Frankfurter first, then er-api. Cache the last good rate with its date in `localStorage` and show "laatst bijgewerkt …". Do the same for weather and news.
7. News: rss2json has rate limits, so fetch the categories one by one, cache each for 15 minutes, and show a friendly Dutch error with a "Opnieuw proberen" button per section instead of an empty box. Links open in a new tab with `rel="noopener"`.
8. Refresh re-fetches everything, shows a spinner state on the button and can't be double-clicked into parallel runs.
9. Text size cycles through 3 steps and is saved.
10. No console errors, no unhandled promise rejections, and no `innerHTML` with unescaped feed text (escape titles and summaries).

Structure: still a single `index.html` plus `styles.css` (and the icon). Vanilla JS in clear sections (config/storage, theme, layout, clocks, weather, rates, news, settings modal, events). No build step.

## Done when
- The look matches the Pulse suite (fonts, colours, cards) in all three themes.
- Every item in the checklist above has been tested by hand in the browser, including on a narrow mobile width, and works.
- Old settings saved under `dashboard_config_v3` load correctly.
- `README.md` lists the features, data sources and settings. Push to `main` with GitHub Pages enabled.
- Report back: the repo link, the Pages URL, and a short list of the bugs you found and fixed in the old code.

## Workflow
- This file is the source of truth. Build from it, commit with clear messages and push to `main`.
- Fix requests will be added under "Fix list". When asked to "apply the fix list", do every unchecked item, tick it (`- [x]`) and push.

## Fix list
_(empty)_
