# Glance — Default Theme

This is the design system Glance skins every artifact with. It is intentionally plain: a light canvas, a calm dark surface for emphasis, and a single green accent. Swap this file for your own look and everything inherits it (see the README).

The tokens below match the `:root` starter block in `components.md`. If you edit colors here, mirror them there.

## Colors

Accent (for primary actions and key highlights only, never body text or large fills):
- accent: `#10b981`
- accent deep: `#059669`
- accent soft (tints, badges): `#d1fae5`
- text on accent: `#04231b`

Dark surface (bands, sidebars, code blocks, emphasis):
- darkest: `#0c2926`
- dark: `#134e4a`
- dark mid: `#115e59`

Surfaces:
- canvas (page and cards): `#ffffff`
- surface: `#f8faf9`
- surface soft: `#f1f5f4`
- feature tint: `#ecfdf5`

Lines:
- hairline: `#e3e8e6`
- hairline strong: `#cbd5d2`

Text:
- ink (headings, body): `#0c2926`
- slate (secondary): `#3f5a55`
- steel (captions): `#5f7a74`
- muted (disabled): `#aab9b5`
- on dark: `#ffffff`

Status and category accents (tags and encoding only):
- purple `#7c3aed`, orange `#ea580c`, pink `#db2777`, blue `#2563eb`
- warning background `#fef3c7`, warning text `#92400e`

## Type
- UI and body: Inter, with system fallbacks
- Code: Source Code Pro, with mono fallbacks
- Headings sit slightly tight; body is comfortable at 1.55 line height.

## Shape and spacing
- Corners: 6px small, 8px inputs, 12px cards, pill (9999px) for buttons and status chips.
- Spacing steps: 4 / 8 / 12 / 16 / 20 / 24 / 32 / 40 / 64.
- Mostly flat. Use soft shadows only for things that float, like menus or modals.

## To re-skin
Replace the values above with your own, then update the matching `:root` tokens in `components.md` (and the `:root` block in `glance-board/viewer.html` for the dashboard). Keep the token names the same and everything picks up the new look.
