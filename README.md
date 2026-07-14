# Handoff: nn27.com — Personal Site

## Overview
A single-page personal/portfolio site for Nobu Nakaguchi (Chief Design Officer & co-founder of Zola). Left column: name, bio, and social links, fixed in place. Right column: a scrollable index of ~20 projects (1999–present) with year, hover states, and per-project hover previews (image, image gallery, or muted looping video). Clicking a linked project title opens its URL in a new tab.

## About the Design Files
The files in this bundle are **design references built in HTML/CSS/vanilla JS** — they show the intended look, layout, and interaction behavior. They are not production code to copy verbatim. The task is to **recreate this design in the target codebase's existing environment** (React, Vue, Swift, etc., following its established patterns and component library) — or, if no environment exists yet, choose the most appropriate framework and implement it there.

## Fidelity
**High-fidelity.** Colors, typography, spacing, and interaction behavior are final. Recreate pixel-perfectly using the target codebase's own styling system/tokens where possible, substituting equivalent utilities for the raw CSS custom properties used here.

## Screens / Views
Single view, single page — no routing.

### Layout
- Two-column layout on desktop (>860px viewport width): a **fixed-position** left column (`position: fixed`, does not scroll) and a right column offset with `margin-left` to sit beside it. Both columns are equal width, computed as `calc(50vw - gap/2 - page-padding)` — i.e. the two columns plus the gap plus the outer padding exactly fill the viewport width.
- Below 860px: left column becomes a normal static, top-aligned block; right column follows below it; the whole page scrolls together (nothing stays fixed).
- Outer padding and the gap between columns both use `clamp(24px, 4vw, 40px)` / `clamp(48px, 7vw, 96px)` respectively — fluid with viewport width.
- Left column content is vertically centered (`justify-content: center`) within a `100vh`-tall fixed box. **Every left-column font-size and margin uses a `vh`-based `clamp()`** (e.g. name: `clamp(2rem, 1rem + 4vh, 5.5rem)`) so total content height is mathematically bounded well under 100vh at any viewport height — this guarantees the name and social links are never clipped or scrolled out of view, with no internal scrollbar.

### Left column
- **Name**: "nn27", serif, weight 300, size `clamp(2rem, 1rem + 4vh, 5.5rem)`, line-height 1.05.
- **Bio paragraph**: serif, weight 300, size `clamp(1.1rem, 0.6rem + 2.2vh, 2rem)`, line-height 1.35, full width of the column (no max-width cap). Exact copy: "I'm Nobu Nakaguchi, Chief Design Officer and co-founder of Zola. Based in NYC, for over two decades, I've led UX and product design teams to turn early-stage ideas into award-winning digital products, collaborating closely with engineering, marketing, and business partners."
- **Social links**: stacked vertically, left-aligned (only the text itself is tappable — not the full row width), serif weight 300, size `clamp(1rem, 0.5rem + 1.8vh, 1.5rem)`. Links: Email (`mailto:nnakaguchi@gmail.com`), LinkedIn (`https://linkedin.com/in/nnakaguchi`), Instagram (`https://instagram.com/nnakaguchi`).
- All left-column text/links are set in `--page-fg` (a soft ivory, see Design Tokens).

### Right column: project index
- One row per project: title (link) left, year right, `border-bottom: 1px solid var(--page-fg)` divider, `padding: 22px 0`.
- Title: serif weight 300, `clamp(1.25rem, 1.1rem + 0.5vw, 1.6rem)`, underlines on hover (`text-decoration-thickness: 1px`, `text-underline-offset: 6px`).
- Year: serif weight 300, `0.95rem`, oldstyle numerals (`font-variant-numeric: oldstyle-nums`).
- Linked projects show a small "↗" arrow (`font-size: 0.55em`, `margin-left: 10px`) after the title and open `target="_blank" rel="noopener"`.
- Projects list, in order (title — year — has link — hover preview): see `index.html`'s `projects` array for the full authoritative list (title, year, url, preview/gallery/caption) — do not retype it by hand, read it from source.

## Interactions & Behavior
- **Hover preview**: hovering any project row with a `preview` (single image or `.mp4` video) or `gallery` (array of images) swaps the ENTIRE left column content (name, bio, socials all hidden) for that media, positioned in the exact same box. On mouse-leave, it reverts to the default name/bio/socials view.
  - Single image → `<img>`.
  - Single `.mp4` → `<video muted loop playsinline>`, `.play()` on hover, `.pause()` on leave.
  - `gallery: [...]` → renders each image stacked vertically (`flex-direction: column`) by default, or side-by-side (`flex-direction: row`, each image `flex:1`) when the project also sets `galleryLayout: "row"`.
  - `caption` (optional, string) → renders as a paragraph below the media while the preview is showing.
- **Click**: rows with a `url` set navigate to that URL in a new tab on click (via a plain `<a href target="_blank" rel="noopener">`).
- No animations/transitions beyond simple `text-decoration` on hover — no easing curves, fades, or motion design in this build.

## State Management
No framework state. Plain DOM: the `projects` array (a JS literal in an inline `<script>`) is rendered to HTML via template strings on page load; hover swapping toggles a `showing-preview` class and directly sets `img.src` / `video.src` / gallery `innerHTML` on `mouseenter`/`mouseleave` listeners attached to each `.proj-row[data-preview], .proj-row[data-gallery]`.

## Design Tokens
Defined as CSS custom properties at the top of `index.html`:
- `--page-bg`: `oklch(23% 0.095 152)` — deep forest green, page background.
- `--page-fg`: `oklch(90% 0.010 85)` — soft ivory, all text/links/dividers.
- `--page-pad`: `clamp(24px, 4vw, 40px)` — outer page padding (all sides).
- `--col-gap`: `clamp(48px, 7vw, 96px)` — gap between the two columns.
- `--col-width`: `calc(50vw - var(--col-gap) / 2 - var(--page-pad))` — width of each column.

Broader design-system tokens (full color scale, type scale, spacing scale) are in `tokens/colors.css`, `tokens/typography.css`, `tokens/spacing.css`, imported via `styles.css`. Fonts: **Source Serif 4** (weights 300–700, loaded from Google Fonts — see note below) for all display/body/UI text on this page; Inter is defined as the system's sans but is not used on this particular page (the whole page is set in the serif).

### Font substitution note
No brand font files were supplied for this project. Source Serif 4 and Inter were chosen as free Google Fonts substitutes for a "minimal but classic" serif/sans pairing. If exact brand font files exist, swap them in and remove the Google Fonts `@import`.

## Assets
All in `assets/`, referenced by the `preview`/`gallery` fields in the `projects` array in `index.html`:
- Images (`.png`/`.jpg`) and three `.mp4` video clips (muted hover previews for ITP Kinetic Memory Triggers, ITP Make it rain, ITP Tug-of-war).
- `forest-green-walnut.jpg` — original color reference photo (not shown on the page itself, used only during design/token creation).

## Files
- `index.html` — the entire page (markup, styles, and the `projects` data + interaction JS, all inline). This is the primary reference.
- `styles.css`, `tokens/*.css` — the broader token system this page's `<link>` pulls in (colors/typography/spacing scales beyond what's used directly on this page).
- `assets/` — all images and videos referenced above.
