# Levanta Dark Theme for Standard Notes

[![License: MIT](https://img.shields.io/badge/License-MIT-a79ef4.svg)](LICENSE)
[![Standard Notes](https://img.shields.io/badge/Standard%20Notes-Theme-6366f1.svg)](https://standardnotes.com)

A soft, dark theme for [Standard Notes](https://standardnotes.com) with a lavender accent, where **the note you write is set in [Karla](https://fonts.google.com/specimen/Karla) for English and [IBM Plex Sans Arabic](https://fonts.google.com/specimen/IBM+Plex+Sans+Arabic) for Arabic**, and typed arrows like `->` and `=>` become real arrows in both languages. The rest of the app — note titles, sidebar, note list, menus — stays in [Inter](https://rsms.me/inter/).

Levanta Dark is a fork of [Lavender](https://github.com/beshoysabri/sn-lavender-theme): the same charcoal palette, stylesheet structure, GitHub Pages hosting and pre-publish checks, with a new [writing area](#the-writing-area) and [arrows](#arrows). Its daytime twin is [Levanta Light](https://github.com/beshoysabri/sn-levanta-light-theme). It keeps the family's version line, so the history is shared — v2.8.0 is the first Levanta release.

## Installation

1. Open the Standard Notes **desktop** app
2. Go to **Preferences** > **Plugins** > **Install Custom Plugin** (at the bottom of the page — older versions had it under **General** > **Advanced Settings**)
3. Paste the following URL:

```
https://beshoysabri.github.io/sn-levanta-dark-theme/ext.json
```

4. Click **Install**
5. Select **Levanta Dark** from the Appearance menu

There's a preview and a copy button at **[beshoysabri.github.io/sn-levanta-dark-theme](https://beshoysabri.github.io/sn-levanta-dark-theme)**.

Levanta Dark has its own identifier, so it installs alongside Lavender and Dark Blue rather than replacing either.

**Install from the desktop app, not the web app.** The web app's Content-Security-Policy only lets it fetch plugin links from Standard Notes' own servers and `raw.githubusercontent.com`, and even that one fails, because the request is sent with credentials and GitHub answers `Access-Control-Allow-Origin: *`. Pasting the link there gives *"Error downloading package details"* — for every self-hosted theme. Installed themes sync with your account, though, and the web app loads the stylesheet and fonts from Pages without complaint, so once it's installed on the desktop it shows up on the web too.

### Pair it with Levanta Light

Install [Levanta Light](https://beshoysabri.github.io/sn-levanta-light-theme) as well, then go to **Preferences** > **Appearance**, turn on **Use system color scheme**, and choose:

- **Automatic Light Theme:** Levanta Light
- **Automatic Dark Theme:** Levanta Dark

Standard Notes then switches between the two with your OS. The choice is stored per device, so set it once on each.

| Client | Colours | Inter (the app) | Karla, Plex Arabic, arrows (the editor) |
|---|---|---|---|
| Web | yes | downloaded automatically | downloaded automatically |
| Desktop | yes | install on the machine — see below | install on the machine — see below |
| Mobile | yes | install on the device — works on iOS and Android | install on the device |

### Everywhere except the web app, install the fonts on the device

The desktop app sends `Content-Security-Policy: default-src 'self' blob:` with no `font-src`, so it refuses to download **any** webfont — from this repo, from a CDN, or inlined as a `data:` URI. No theme can ship a font file into it.

What does work is a font already installed on the device. There's no download, so CSP has nothing to block — it's just a family-name lookup, and the stacks are written so installed copies are found automatically. The same trick works on mobile.

**Desktop** — install these, then fully quit Standard Notes and reopen it; the system font list is read at launch.

| Font | Get it from | Install |
|---|---|---|
| Inter | [rsms.me/inter](https://rsms.me/inter/) | `InterVariable.ttf` and `InterVariable-Italic.ttf`, inside `Desktop/` in the zip |
| Karla | [Google Fonts](https://fonts.google.com/specimen/Karla) → Get font → Download all | the two variable files, roman and italic |
| IBM Plex Sans Arabic | [Google Fonts](https://fonts.google.com/specimen/IBM+Plex+Sans+Arabic) → Get font → Download all | at least Regular and Bold |
| Levanta Arrows | this repo | [`LevantaArrows-Regular.ttf`](https://beshoysabri.github.io/sn-levanta-dark-theme/dist/fonts/LevantaArrows-Regular.ttf) and [`LevantaArrows-Bold.ttf`](https://beshoysabri.github.io/sn-levanta-dark-theme/dist/fonts/LevantaArrows-Bold.ttf) |

Font Book on macOS, right-click → Install on Windows. Install bold faces too: the theme turns off synthetic bold, so a family with only a Regular shows bold text unbolded.

**Mobile** — install the same fonts as system fonts (iOS needs a font-installer app that adds a configuration profile; Android varies by manufacturer), then restart Standard Notes. Inter this way is confirmed working on both iOS and Android; the editor fonts use the same family-name lookup, but haven't been tried on a phone yet.

Each font is optional. Without Karla the editor falls back to Inter, without Plex Arabic uses the system's Arabic face, and without Levanta Arrows `->` stays as typed. Skip them all and the theme still works — you get the colours on the system font. That's a correct fallback, not a failure.

<details>
<summary>Alternative: jsDelivr</summary>

The theme is also reachable over jsDelivr:

```
https://cdn.jsdelivr.net/gh/beshoysabri/sn-levanta-dark-theme@master/ext.json
```

Both origins serve the same files. GitHub Pages is the default because branch-pinned jsDelivr URLs are cached for roughly 12 hours, so a push doesn't show up until the cache expires or you purge it. Pages republishes on every push to `master`.

</details>

## The writing area

What you type in a note — the body of a Super note or a plain-text note — is set in Karla for English and IBM Plex Sans Arabic for Arabic. Note titles, the sidebar, the note list and menus stay in Inter. Section 4 of `dist/dist.css` does it.

**Arabic finds its font on its own.** Nothing has to detect it. A browser picks a font per character, walking the family stack until one has the glyph: Karla has no Arabic, so Arabic letters fall through to Plex, and a sentence that mixes the two languages gets each script in its own face. The Plex faces are also limited to the Arabic Unicode blocks with `unicode-range`, so Plex's own Latin letters never stand in for Karla's, and a note without Arabic never downloads Plex. Checked in app.standardnotes.com with Chrome's per-element font report: in a mixed Super paragraph the Arabic glyphs came from IBM Plex Sans Arabic, the Latin ones from Karla and the arrows from Levanta Arrows.

**Direction is the editor's job, and it already does it.** Super gives every paragraph `dir="auto"`, and the plain-text editor is a `dir="auto"` textarea, which browsers lay out line by line. A paragraph that starts in Arabic runs right to left and aligns right, and the next one, in English, runs left to right. No theme rule is involved.

**Why IBM Plex Sans Arabic rather than Noto Sans Arabic.** Both were set beside Karla at the editor's size, and Plex was the better match:

- Karla's curves are squarish and its terminals crisp. Plex Arabic was drawn to sit beside IBM Plex's similarly engineered Latin, and it reads as the same kind of letter. Noto Sans Arabic is rounder and softer, closer to a humanist sans.
- Size: Plex's `ه` stands 0.41 em, just under Karla's x-height of 0.48 em, so in a mixed line the two scripts look the same size. Noto's stands 0.50 em, about 20% taller, and its teeth (`س`) a third taller than Plex's, so it reads a size larger.
- Plex's Regular and Bold sit at Karla's weight, in two files of about 72 KB — all that Super uses.

**Weights and styles.** Karla is the variable font, roman and italic, 200 to 800. Plex ships Regular (400) and Bold (700), Super's body and its bold text and headings. Arabic has no italic; italic Arabic is slanted by the browser.

**Code and the Monospace option.** Code blocks and inline code keep a monospace face, with no arrows. Standard Notes' **Monospace font** option switches the editor to monospace as well — under Lavender's app-wide Inter it had no effect.

## Arrows

Type an arrow the way you would in plain text and it's drawn as a real one, in English and in Arabic:

| Type | Get |
|---|---|
| `->` `<-` `<->` | → ← ↔ |
| `=>` `<=>` | ⇒ ⇔ |
| `-->` `<--` `<-->` | ⟶ ⟵ ⟷ |
| `==>` `<==` `<==>` | ⟹ ⟸ ⟺ |

`<=` and `>=` are left alone; they mean "less/greater than or equal". Code never gets arrows. Only the drawing changes: the note still holds the characters you typed, so search, copy and export all see `->`.

**In an Arabic paragraph, `->` points the way the text reads: ←.** It's the same mirroring that flips `(` into `)` in right-to-left text — without the theme you'd see `<-` there, two separate characters. English inside an Arabic paragraph keeps its own direction, so `start -> end` still gives →.

**Why it needs a font of its own.** Karla has no arrow ligatures, so on its own `->` would stay two characters. Inter has them, but only for left-to-right text: in a right-to-left run the browser mirrors `>` into `<` *before* it looks up ligatures, so Inter sees `-<` and its `->` rule never matches. That's why arrows never formed in Arabic under Lavender. **Levanta Arrows** is a 2 KB font that holds Karla's own `-` `<` `=` `>`, Inter's arrows redrawn at Karla's stroke weight and centred on Karla's `<` and `>`, and rules for both directions. It comes first in the editor's stack with a `unicode-range` of just those characters, so it touches nothing else, and a hyphen in an ordinary word is still Karla's. `tools/build-arrows.py` rebuilds it (`pip install fonttools brotli`), byte for byte.

To keep `->` as typed in the editor, set `--levanta-text-ligatures` in section 4 to `common-ligatures no-contextual`; section 7 has a block that turns arrows off everywhere.

One rare mix gets it wrong. In an *English* paragraph, `<=` right after an Arabic word is drawn ⇐, and `=>` there stays as typed. The font can't tell that spot from `=>` typed in an Arabic paragraph, which is the commoner case, so the Arabic reading wins.

## Hosting

Pushing to `master` triggers `.github/workflows/pages.yml`, which verifies the theme before publishing — that `ext.json` and `package.json` agree on a version, that every font file exists, that every `@font-face` source is an absolute Pages URL, that all **90** colour declarations still hash to the lavender palette's digest, that some rule applies `--sn-inter-font` to Standard Notes' own page and no rule reaches into editor plugins' frames, and another applies `--levanta-text-font` to the editor, that code inside the editor stays monospace, that the section 7 opt-ins are still commented out, and that braces balance in the comment-stripped `dist.css`. A failed check blocks the deploy rather than shipping a broken stylesheet.

The digest covers every custom property except the three `--sn-inter-*` and two `--levanta-*` typography knobs, so Levanta's colours are proven identical to Lavender's.

Enable it once under **Settings → Pages → Source → GitHub Actions**.

## Color Palette

| Role | Hex | Preview |
|------|-----|---------|
| Background | `#1e1e1e` | ![1e1e1e](https://img.shields.io/badge/-1e1e1e-1e1e1e?style=flat-square) |
| Sidebar & surfaces | `#252525` | ![252525](https://img.shields.io/badge/-252525-252525?style=flat-square) |
| Border | `#2a2a2a` | ![2a2a2a](https://img.shields.io/badge/-2a2a2a-2a2a2a?style=flat-square) |
| Separator | `#333333` | ![333333](https://img.shields.io/badge/-333333-333333?style=flat-square) |
| Foreground | `#eeeeee` | ![eeeeee](https://img.shields.io/badge/-eeeeee-eeeeee?style=flat-square) |
| Accent (Lavender) | `#a79ef4` | ![a79ef4](https://img.shields.io/badge/-a79ef4-a79ef4?style=flat-square) |
| Accent, darkened | `#8a7ff0` | ![8a7ff0](https://img.shields.io/badge/-8a7ff0-8a7ff0?style=flat-square) |
| Selection wash | `#a79ef4` at 18% | ![373545](https://img.shields.io/badge/-373545-373545?style=flat-square) |
| Text selection | `#a79ef4` at 35% | ![4e4b69](https://img.shields.io/badge/-4e4b69-4e4b69?style=flat-square) |
| Text on accent | `#1e1e1e` | ![1e1e1e](https://img.shields.io/badge/-1e1e1e-1e1e1e?style=flat-square) |
| Success | `#2b9612` | ![2b9612](https://img.shields.io/badge/-2b9612-2b9612?style=flat-square) |
| Warning | `#cc8800` | ![cc8800](https://img.shields.io/badge/-cc8800-cc8800?style=flat-square) |
| Danger | `#f80324` | ![f80324](https://img.shields.io/badge/-f80324-f80324?style=flat-square) |

The two translucent rows are previewed as they render on the background.

## Typography

Inter v4.1 sets the app: sidebar, note list, menus and note titles. The note body is Karla and IBM Plex Sans Arabic — see [The writing area](#the-writing-area). All the fonts are bundled in `dist/fonts/` under the SIL Open Font License.

Two details make the webfonts work at all:

- **The `@font-face` sources are absolute, not relative.** Standard Notes inlines theme CSS instead of linking it, so a relative `url('fonts/…')` resolves against the app's own origin and 404s. v2.2.0 shipped that bug; the fonts never loaded in any client.
- **The family is set on elements, not through a variable.** Standard Notes doesn't read `--sn-stylekit-sans-serif-font` for its own chrome, so setting that variable changes nothing. A rule on every element is what reaches the UI, and the editor rule in section 4 is more specific, so it wins inside the editor only. Since v2.8.1 both are scoped to Standard Notes' own page, `:where(body:has(> #app-group-root))`: SN also loads the theme into every editor plugin's frame, and an unscoped `body *` rule replaced the plugins' own fonts with Inter.

Code keeps a monospace face everywhere, and contextual alternates are off there so `->` stays two characters where that matters.

Five variables in `dist/dist.css` control it:

| Variable | Purpose |
|---|---|
| `--sn-inter-font` | The app's family stack. Inter has no Arabic or CJK glyphs, so those scripts fall through to the next family rather than rendering as boxes. |
| `--sn-inter-mono` | Code face. Defaults to whatever Standard Notes already uses. |
| `--sn-inter-features` | Inter's OpenType features. `cv05` gives lowercase `l` a tail so it stops looking like a capital `I`. |
| `--levanta-text-font` | The editor's stack: Levanta Arrows, Karla, Plex Arabic, then installed copies and system fallbacks. |
| `--levanta-text-ligatures` | Arrows in the editor: `common-ligatures contextual` (on) or `common-ligatures no-contextual` (off). |

Section 7 has opt-ins: tighter heading tracking, roomier leading in Super, tabular figures in Super tables, arrows off everywhere, the note title in the writing fonts, and the system font outside the editor.

## Daily View

Standard Notes' Daily notebook borrows the app's `danger` token for today's date, the selected day, the entry tiles and both create-note buttons — so on a lavender theme they came out red. Section 5 of the stylesheet repaints those specific elements with the accent instead.

It's scoped deliberately rather than recolouring `--sn-stylekit-danger-color` wholesale, for two reasons: destructive controls should stay red, and the rules only *reference* the accent variables rather than declaring new ones — so the colour digest is unaffected.

Because they reference `--sn-stylekit-info-color` and friends, retuning the accent moves the Daily view with it automatically — including the dark text on today's date and the entry tiles.

## Selection

Standard Notes uses one colour for both hover and selection, in the two places that matter: `--navigation-item-selected-background-color` in the sidebar, and `--item-cell-selected-background-color` in the note list, which the app defines as the contrast background. Under native Dark both are near-black (`#0f1011` and `#000000`), so the selected tag and the selected note each became a black bar.

Section 6 keeps hover a neutral grey and gives the selected sidebar item and the selected note an 18% lavender wash, with the sidebar label in lavender (4.6:1 on the wash). Like the Daily view rules, they only *reference* variables, so the colour digest is unaffected — and they match Standard Notes' own class names, so they may need revisiting if the app restyles its sidebar or note list.

The label rule sets `-webkit-text-fill-color` as well as `color`. Standard Notes paints tag titles with `-webkit-text-fill-color: var(--navigation-item-text-color)`, which wins over `color`, so in Lavender v2.6.0, which sets only `color`, the label stays `#eeeeee` on screen. Levanta Dark draws it in lavender.

## What Changed vs Native Dark

The accent, the base it sits on, and the fonts. Through v2.5.1 only the accent family moved; v2.6.0 softened the base too:

- **Charcoal, not near-black.** Background `#0f1011` → `#1e1e1e`; sidebar and surfaces `#1c1d1e` → `#252525`; borders `#181a1b` → `#2a2a2a`; separators `#28292b` → `#333333`; menus are solid charcoal instead of 65%-transparent black.
- **Soft white.** Sidebar labels, section titles and note text `#ffffff` → `#eeeeee`, matching the rest of the UI — 14.4:1 on the base instead of 19.1:1.
- **In-between lavenders.** The accent backdrop is 18% instead of 6%, which was invisible on dark; text selection is a 35% lavender wash under light text (7.1:1) instead of a solid block; the scrollbar thumb is lavender at 40%.
- **Selection is a lavender wash, not a black bar.** See [Selection](#selection).

The purple-to-lavender swap applies to:

- Selected items and navigation highlights
- Links, buttons, and focus rings
- Text selection background
- Scrollbar thumb
- Note preview progress bar
- Component highlight color
- Titlebar hover state
- Super, plain-text and rich-text note icons, the selected note's left edge, and the note's editing-status pill (accessory tint 1)

That last one isn't the accent in name, but Standard Notes designs it to match: it's `#086dd6` natively, the same blue as the default accent, and the app pairs it with accent-tinted fills. Setting tint 1 to `#a79ef4` means no blue is left over. The other five accessory tints stay native.

The darkened shade used for hover states (`#8a7ff0`) keeps lavender's hue and drops its lightness by the same amount Dark Blue's `#086DD6` → `#065cb5` did.

**Text on the accent is dark.** Lavender is much lighter than the native purple or Dark Blue's blue: the near-white `#eeeeee` that native Dark puts on accent fills manages only 2.0:1 against `#a79ef4`, under even the 3:1 floor for large text. So `--sn-stylekit-info-contrast-color` is the background colour `#1e1e1e` instead, at 7.0:1 — primary buttons, the new-note button, badges, and the Daily view's date tiles and create-note buttons.

## Customization

Want a different lavender? Replace `a79ef4` in `dist/dist.css` with your preferred shade — without the `#`, so the translucent variants (`#a79ef42e`, `#a79ef459`, `#a79ef466`) follow it:

| Name | Hex |
|------|-----|
| **Lavender** | `#a79ef4` (default) |
| Catppuccin Lavender | `#b4befe` |
| Dracula Purple | `#bd93f9` |
| Material Deep Purple 200 | `#b39ddb` |
| Tailwind Violet 400 | `#a78bfa` |

All of these are light enough for the dark text-on-accent (6:1 or better). A mid-tone accent such as Dark Blue's `#086DD6` reads better with light text — set `--sn-stylekit-info-contrast-color` back to `#eeeeee` — and `--sn-stylekit-info-color-darkened` should move with whatever accent you pick.

If you publish your own copy, the Pages workflow refuses any colour change until `EXPECTED` in `.github/workflows/pages.yml` is updated to match; the failed run prints the new digest.

## License

Theme: MIT — see `LICENSE`.
Fonts: Inter, Karla, IBM Plex Sans Arabic and Levanta Arrows are all under the SIL Open Font License 1.1 — see `LICENSE-3RD-PARTY`. The Plex files are IBM's own, unmodified; Levanta Arrows is built from Karla and Inter.
