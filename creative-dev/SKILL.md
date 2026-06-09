---
name: creative-dev
version: 1.0
description: |
  Advanced creative development skill for exceptional, Awwwards-caliber UI/UX. Extends and overrides
  frontend-design when creative, immersive, or motion-forward intent is strong. Reference library of
  43 award-winning sites in 7 technique clusters. Fixed CDN stack — free, publicly accessible, no paid plugins.
  Validates all technique compatibility with the Claude artifact environment before generating.

  TRIGGER on: creative dev, creative-dev, make it stunning, immersive, Awwwards, awwwards-style,
  unique UI, advanced UI, cinematic, next-level, experimental, WebGL, webgl, canvas, three.js, threejs,
  scroll experience, interactive story, motion-forward, generative, 3D, particle, shader, custom cursor,
  split text, scroll reveal, make it beautiful, editorial, atmospheric, dark luxury, full bleed,
  typographic, agency-grade, art direction, creative portfolio, full-page experience, animate it,
  bring it to life, world-class, wow factor, make it move, interactive, GSAP, gsap, ScrollTrigger
---

# creative-dev

**Relationship to `frontend-design`**: This skill EXTENDS frontend-design and OVERRIDES it when creative
intent is strong. For this user, default to creative-dev on any UI request with visual ambition.
Load `frontend-design` only for purely functional UI with no aesthetic brief.

**Sub-files** — load as needed:
- `./references.md` — 43 sites in 7 clusters; use when matching brief to visual direction
- `./techniques.md` — 30 techniques with code stubs; use when selecting and implementing
- `./constraints.md` — CDN strings, what works/breaks; ALWAYS read before writing code
- `./artifacts/` — 6 validated working HTML files (scroll-reveal, cursor, three.js, clip-path, split-text, canvas-particles)

---

## STEP 0 — Mode Declaration
Output this as the first line of every response, before any explanation or code:

```
▸ Mode: [HTML / React] — [one sentence reason, max 12 words]
```

**Examples:**
```
▸ Mode: HTML — Three.js full-page scene, GSAP smooth scroll, no state needed
▸ Mode: React — recharts data integration, component state drives UI
▸ Mode: HTML — canvas particle system as primary, zero CDN dependency
```

Never ask for confirmation. Proceed directly. Only flag ambiguity if the brief is genuinely split between Two.js/GSAP AND complex component state.

**Decision rules (from constraints.md):**
- Three.js/WebGL primary → **HTML**
- GSAP + Three.js together → **HTML**
- Canvas animations central → **HTML**
- Full-page immersive scroll → **HTML**
- anime.js needed → **HTML**
- Component state drives UX → **React**
- recharts, d3, Tone.js → **React**
- Ambiguous → HTML if motion-forward, React if UI-forward

---

## STEP 1 — Brief Decoder (answer silently before writing a line of code)

1. **Purpose** — What is the ONE thing this communicates? (Product, portfolio, campaign, story, tool, generative art?)
2. **Cluster** — A: 3D / B: Scroll narrative / C: Typography / D: Playful / E: Dark editorial / F: Brand/product / G: Agency
3. **Highlight technique** — Which SINGLE ⭐ technique anchors the piece? Choose from techniques.md.
4. **Supporting techniques** — Max 2-3. Must serve the highlight, not compete with it.
5. **Stack** — Which CDN libs are actually needed? Default to minimum.

---

## STEP 2 — Cluster Matcher

Match brief mood/signals → cluster → primary technique. When uncertain, pick the reference site that
feels closest and steal its defining pattern.

| Brief signals | Cluster | Reach for |
|--------------|---------|-----------|
| 3D, space, cosmos, WebGL, immersive world | A | T27 (particle field) or T29 (GLSL shader) |
| Story, documentary, journey, sequential | B | T11 (horizontal scroll) or T10 (parallax layers) |
| Type-first, editorial, foundry, minimal | C | T04 (char split) or T06 (masked reveal) |
| Playful, game, colorful, generative, joy | D | T24 (canvas particles) or T25 (generative bg) |
| Dark, cinematic, film, investigative | E | T19 (grain) + T13 (clip-path) as combo |
| Product, brand, luxury, ecommerce | F | T13 (clip-path reveal) + T02 (custom cursor) |
| Agency, portfolio, case study | G | T09 (scroll entrance) + T02 (custom cursor) |

Load `./references.md` for deeper cluster guidance and site-specific patterns to steal.

---

## STEP 3 — Design Principles (ordered — earlier = higher priority)

1. **Concept first.** Name the ONE idea before touching code. Everything serves it or is cut.
2. **The highlight technique gets space.** One ⭐ per piece, uncluttered, given room to land.
3. **Motion serves meaning.** Every animation earns its place or is removed.
4. **Typography is structure.** Font choice + scale IS the layout. Choose distinctively — see NEVER list below.
5. **Color is committed.** Dominant hue + 1 accent max. Executed fully, not hedged with neutrals.
6. **Restraint over accumulation.** One effect executed beautifully > five competing effects.
7. **Interaction reveals character.** Custom cursor, hover states, transitions = personality layer.
8. **Performance is UX.** No effect worth dropping below 50fps on average hardware.
   Cap pixel ratio at 2. Dispose Three.js on unload.

---

## STEP 4 — Technique Selection

### ⭐ HIGHLIGHT (one per piece — give it space)
T04 · T11 · T13 · T24 · T25 · T27 · T28 · T29

### Supporting (up to 3 — invisible infrastructure)
T01 · T02 · T03 · T05 · T06 · T07 · T08 · T09 · T10 · T12 · T14 · T15 · T16 · T17 · T18 · T19 · T20 · T21 · T22 · T23 · T26 · T30

### Utility (always appropriate, nearly invisible)
**T01** (smooth scroll) · **T02** (custom cursor) · **T03** (load orchestration)

---

## Library Stack

### HTML Mode
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.1/anime.min.js"></script>
```
Always: `gsap.registerPlugin(ScrollTrigger);` before first ScrollTrigger use.

### React Mode
```javascript
import * as THREE from 'three';   // r128, same constraints as HTML
import * as d3 from 'd3';
import * as Tone from 'tone';
import _ from 'lodash';
```

Load `./constraints.md` for full CDN strings and what breaks before writing code.

---

## Design Anti-Patterns — NEVER default to these

**Fonts:** Inter, Roboto, Arial, system-ui
→ Use: display serifs, editorial grotesques, condensed display, geometric slab

**Visuals:** Purple-to-pink gradient on white · three.js spinning cube · particle system with no conceptual reason ·
box-shadow as primary depth tool · generic glassmorphism hero

**Motion:** Fade-in on everything · CSS `transition: all` · scroll reveal on every element ·
stacking two ⭐ HIGHLIGHT techniques · animation without purpose

**The test:** Would an Awwwards SOTD jury scroll past it in 3 seconds? If yes, redesign.

---

## Integration with frontend-design

When creative-dev triggers:
- Ignore frontend-design aesthetic defaults — use this skill's cluster references instead
- This CDN stack supersedes frontend-design's library suggestions
- Principle hierarchy in Step 3 takes precedence
- Load frontend-design only if this skill explicitly defers (e.g. "design tokens for a component system")

---

## Learning Protocol

When a conversation surfaces a technique or pattern that is:
- Novel — not documented in techniques.md
- Elegant — more concise or capable than existing approach
- Reusable — applicable across multiple brief types

→ Propose an addition at the END of the response with this format:
```
[PROPOSED T31: technique-name — one sentence description + cluster affinity]
```
No formal commands needed. Recognition is enough.

---

## Placeholder: data-viz-as-art

D3 force graphs, data-driven generative art, analytical visualization styled as design, and
data-as-aesthetic patterns are intentionally excluded from this skill.

They are reserved for a forthcoming `data-viz` skill that will handle the intersection of
data engineering and visual art — built to complement creative-dev for data-heavy creative briefs.

When a brief clearly needs both: use d3 in React mode for the data layer, apply creative-dev
techniques for the visual layer, and note `[data-viz skill needed for full treatment]`.
