# Arabic Scrabble Integrated Version

A two-player, local Arabic Scrabble game that runs entirely in the browser.

This project evolved from the original repository, [DAXEI08/update-version](https://github.com/DAXEI08/update-version), and merges its two Arabic tile-set variants into one shared game engine, with a selectable visual theme system on top.

> **One game engine, one ruleset, two tile sets, multiple visual themes.**

## Features

- Two-player local Arabic Scrabble on the classic 15×15 board
- **Expanded** and **Basic** tile modes (chosen when a game starts)
- Selectable visual themes, applied instantly and remembered on the device
- Per-player timer (the clock starts after the first move)
- Undo of the last move
- Tile swap
- **Unlimited pass**: there is no pass or skip limit
- Human **tahkim** (judging): the challenger rules the last move **✔ صحيح** or **✖ خطأ**
- Move history with words, scores, bingos, passes, swaps and tahkim verdicts
- Blank tiles (always worth 0)
- Responsive layout for desktop, tablet and mobile, portrait and landscape
- Adjustable interface dimensions (board, racks, buttons, history)

There is no automatic dictionary: word validity is decided only by the players through tahkim.

## Modes

Both modes use the same engine, gameplay, board, scoring, timer, tahkim, UI, and features. **The only difference is the tile distribution and the blank-tile alphabet.**

| Mode | Arabic label | Tiles | Blank tile can become |
|---|---|---|---|
| Expanded | الحروف الموسعة | the 28 basic letters plus `أ إ آ ء ؤ ئ ة ى` (101 tiles incl. 2 blanks) | any of those 36 letters |
| Basic | الحروف الأساسية | the 28 basic letters only (84 tiles incl. 2 blanks) | only the 28 basic letters |

- The mode is picked at game setup: **PLAY → tile mode → duration → confirmation → game**. It cannot change while a game is running; **New game** goes back through mode selection.
- Tiles are never normalised (`أ` stays `أ`, `ة` stays `ة`, …). The letter `ه` is stored as plain `ه`; the `هـ` face on the tile is display-only (`FACE` in `config.js`).
- Both tile sets, their values and counts, and the blank alphabets live in `js/modes.js` as plain data. The engine just reads `TILE_MODES[app.mode]`.

## Themes

Themes only change presentation. They never affect the mode, tile distribution, scoring, timer, tahkim, board coordinates, placement, game state, history or dimensions. Choose one in **الإعدادات → المظهر**; it applies immediately without reloading and never resets the game.

| Theme | Arabic | Character |
|---|---|---|
| Classic | كلاسيكي | wood tones, cream tiles, dark green lettering (default) |
| Dark | داكن | charcoal panels, high-contrast light tiles, amber accents |
| Emerald | زمردي | emerald green, cream and gold |
| Desert | صحراوي | a light theme: sand, terracotta, warm brown, ivory, muted gold |

The selected theme is stored in `localStorage` under `scrabble.theme`. If the value is missing, unreadable, or not a known theme id, the game falls back to Classic. Interface dimensions are stored separately under `scrabble.dims`, so resetting one never affects the other.

### How themes work

`js/themes.js` only sets `<html data-theme="id">`. All colours are CSS custom properties defined once per theme in `css/themes.css`; there is no per-element styling from JavaScript and no duplicated stylesheet. To add a theme, add a block in `themes.css` that defines every token, and an entry in `THEMES` in `themes.js`.

## Technology

HTML, CSS, vanilla JavaScript (ES Modules). No framework, no build step, no dependencies, no audio subsystem. Fonts (Amiri for Arabic, Tinos for Latin and digits) are bundled locally.

## Running

ES modules need to be served over HTTP (opening `index.html` from `file://` is blocked by browsers). From the project folder:

```sh
python3 -m http.server 8000
```

then open <http://localhost:8000>. Any static host (GitHub Pages, Netlify, …) works as-is.

## Folder structure

```
index.html
README.md

css/
  style.css       layout and components (colours come from theme tokens)
  cover.css       cover screen
  themes.css      theme palettes (CSS custom properties)

js/
  config.js       global constants: board size, durations, premium layout, glyph/label maps, dimension limits
  modes.js        Expanded and Basic tile data, blank alphabets, mode metadata
  themes.js       theme metadata, load/save/apply, theme selector
  state.js        game state, tile bag, snapshots (undo / tahkim)
  game.js         turns, placement, submit, undo, swap, pass, game over
  scoring.js      word finding and scoring
  judge.js        tahkim
  ui.js           rendering and dialogs (including mode and duration selection, blank picker)
  input.js        click/tap, keyboard, drag and drop, buttons, dialog flow
  dims.js         interface dimension settings
  cover.js        animated cover screen
  main.js         start-up

assets/
  fonts/
    Amiri-Regular.woff2
    Amiri-Bold.woff2
    Tinos-Regular.woff2
    Tinos-Bold.woff2
```

## Credits

By esbepe. Evolved from [DAXEI08/update-version](https://github.com/DAXEI08/update-version).
