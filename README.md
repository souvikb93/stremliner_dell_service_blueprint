> **Moved.** These assets now live in one repo, split by project:
> https://github.com/souvikb93/framer-assets
>
> This repo is kept so the old Pages URLs keep resolving and so the
> `pre-type-scale` / `type-scale-v1` rollback tags stay reachable.
> **Edit the monorepo, not this one.**

# Streamliner — to-be service blueprint

The interactive service blueprint for the Dell Streamliner case study. One
self-contained HTML file, no build step, no dependencies beyond the Uncut Sans
webfont from jsDelivr.

**Live:** https://souvikb93.github.io/stremliner_dell_service_blueprint/

**Framer node:** `iVhhBwFBm` on `/projects/streamliner`

## Using it in Framer

On the Embed node, switch to **URL** mode and paste:

```
https://souvikb93.github.io/stremliner_dell_service_blueprint/
```

Then set the node's **Height to Fixed, 678** — URL embeds cannot auto-measure,
and Framer will show "URL embeds do not support auto height" until you do.

678 is the document's own rendered height at desktop. There is no `min-height`
lock here, because the to-be shares its stylesheet with the as-is (see below) and
the two boards hold different amounts of content — as-is renders at 820. If you
add or remove a row, re-measure and update the node.

## The two blueprints

| Path | Board | Rendered height |
|---|---|---|
| `as-is/index.html` | The process before Streamliner | 820 |
| `to-be/index.html` | The process after Streamliner | 678 |
| `index.html` | Copy of `to-be/` — this is what the Framer node loads | 678 |

Both use the **same design**: swimlane badges, coloured header strips, dashed
blueprint dividers, yellow callouts. Only the content differs. Their stylesheets
are byte-identical, and they should stay that way — if you restyle one, restyle
the other, then `cp to-be/index.html index.html` so the served file follows.

## Changing it

```bash
vim index.html
open index.html          # check it renders
git commit -am "blueprint: <what changed>"
git push
```

GitHub Pages redeploys in under a minute. Hard-refresh the Framer preview to
clear the CDN cache. The embed URL never changes, so the Framer node is only
ever touched once.

## What it contains

43 cards across four swimlanes — user journey, frontstage, backstage, support
processes — with the three standard blueprint dividers (line of interaction,
line of visibility, line of internal interaction).

The animation walks a credit request through all 43 steps in order, auto-panning
horizontally to keep the active card in view. Cards are dim until visited, lit
while active, and held at partial opacity once passed. Yellow callouts appear
only on the step they belong to. The dashed pink group box marks the business
rule engine, and appears while steps `v5`–`v8` run.

Controls: **Play**, **Pause**, **Fit all** (scales the whole board to the frame
width). Clicking any card jumps to that step and pauses.

## Constraints

- **Fixed desktop height.** Framer measures the embed document and writes the
  value back to the node, so a document whose height changes makes the page jump.
- **`setTimeout`, not `requestAnimationFrame`.** rAF does not advance under
  headless virtual time, which makes the asset impossible to verify in CI or
  screenshot. This one is timer-driven for that reason.
- **Responsive inside the file.** Framer's M (810) and S (390) breakpoints are
  zero-override replicas of L (1200) and share one node height, so all
  responsive behaviour lives in the HTML's own media queries.
- **Reduced motion respected** — scroll snapping and card transitions collapse,
  content stays visible.

Shared design rules for every asset in this case study live in the main repo:
https://github.com/souvikb93/streamliner-final
