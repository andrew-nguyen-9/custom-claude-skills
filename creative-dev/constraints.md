---
creative-dev / constraints.md
Environment constraints for Claude HTML and React artifacts.
Read this before writing ANY code. CDN strings are exact — do not guess versions.
---

# Environment Constraints

## HTML Artifact Mode

### Verified CDN Stack
All external scripts must load from `https://cdnjs.cloudflare.com`.

```html
<!-- GSAP Core (always load first) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>

<!-- GSAP Plugins (load after core, register before use) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/TextPlugin.min.js"></script>
<!-- Register: gsap.registerPlugin(ScrollTrigger, TextPlugin); -->

<!-- Three.js r128 — exposes global window.THREE -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<!-- anime.js 3.2.1 — exposes global window.anime -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.1/anime.min.js"></script>

<!-- D3 v7 -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.8.5/d3.min.js"></script>
```

---

### Lenis (smooth scroll) — NOT on cdnjs
Use GSAP ticker-based smooth scroll instead. Wraps all page content in a fixed div.

```javascript
// Setup: body { overflow: hidden; } + wrapper div wrapping all content
let curr = 0, tgt = 0;
window.addEventListener('wheel', e => {
  e.preventDefault();
  tgt = Math.max(0, Math.min(tgt + e.deltaY, document.body.scrollHeight - window.innerHeight));
}, { passive: false });
gsap.ticker.add(() => {
  curr += (tgt - curr) * 0.08; // ease factor: lower = smoother and slower
  gsap.set('.scroll-wrapper', { y: -curr });
});
// When using with ScrollTrigger, add: ScrollTrigger.normalizeScroll(true)
```

---

### Splitting.js — Uncertain cdnjs availability
Use manual split utilities instead (validated in artifacts, zero CDN dependency):

```javascript
// Characters
function splitChars(el) {
  el.innerHTML = el.textContent.split('').map(c =>
    c === ' ' ? '<span class="sp"> </span>' : `<span class="ch">${c}</span>`
  ).join('');
}

// Words
function splitWords(el) {
  el.innerHTML = el.textContent.trim().split(/\s+/).map(w =>
    `<span class="w-wrap"><span class="w-inner">${w}</span></span>`
  ).join(' ');
}

// Lines (approximate by sentence)
function splitSentences(el) {
  el.innerHTML = el.textContent.split('. ').map(s =>
    `<span class="line"><span class="line-inner">${s.trim()}.</span></span>`
  ).join(' ');
}
```

---

### Works ✓ in HTML artifacts

**Canvas & rendering**
- Canvas 2D API, requestAnimationFrame — fully supported
- THREE.js r128: Scene, PerspectiveCamera, WebGLRenderer, BufferGeometry, BufferAttribute,
  Points, PointsMaterial, Mesh, MeshStandardMaterial, MeshBasicMaterial, ShaderMaterial,
  RawShaderMaterial, SphereGeometry, BoxGeometry, PlaneGeometry, CylinderGeometry,
  AmbientLight, DirectionalLight, PointLight, SpotLight, Clock, Vector2, Vector3, Vector4,
  Color, Quaternion, FogExp2, Fog, TextureLoader (data URIs), AdditiveBlending,
  NormalBlending, MultiplyBlending

**CSS**
- Custom properties (CSS variables), @keyframes, all transitions
- clip-path: inset(), polygon(), circle(), ellipse() — all directions
- mix-blend-mode, backdrop-filter, filter (blur, contrast, hue-rotate, etc.)
- CSS scroll-driven animations (@scroll-timeline) — Chrome 115+, use as enhancement
- SVG SMIL animations
- CSS grid, flexbox — fully supported

**JS APIs**
- Pointer events, MouseEvent, TouchEvent, wheel events
- Intersection Observer API
- Web Audio API (AudioContext, OscillatorNode, etc.)
- requestAnimationFrame, performance.now(), setTimeout, setInterval
- ResizeObserver

---

### Broken / Not Available ✗

| Thing | Reason | Fix |
|-------|--------|-----|
| `localStorage` / `sessionStorage` | Hard blocked | Use in-memory JS variables |
| `<form>` tags | Avoid entirely | Use onClick/onChange event handlers |
| `THREE.OrbitControls` | Not in r128 CDN bundle (requires jsm/) | Manual mouse parallax on camera |
| `THREE.CapsuleGeometry` | Added in r142, not r128 | Use CylinderGeometry + SphereGeometry |
| `THREE.FontLoader` / `TextGeometry` | Requires external JSON font file | Use canvas 2D text or SVG overlay |
| `THREE.EffectComposer` / `UnrealBloomPass` | Not in base r128 bundle | Additive blending workaround (see below) |
| Lenis CDN | Not on cdnjs | GSAP ticker smooth scroll |
| GSAP SplitText | Club GSAP (paid) | Manual split functions |
| GSAP MorphSVG | Club GSAP (paid) | CSS clip-path polygon morphing |
| npm packages | No Node.js runtime | CDN only from cdnjs |

---

### Three.js Glow Without EffectComposer
Two-layer particle trick — additive blending fakes bloom:

```javascript
// Layer 1: large, faint outer glow
const matOuter = new THREE.PointsMaterial({
  size: 0.06, color: 0x4466ff, transparent: true, opacity: 0.12,
  blending: THREE.AdditiveBlending, depthWrite: false
});
// Layer 2: small, bright core
const matInner = new THREE.PointsMaterial({
  size: 0.015, color: 0xaabbff, transparent: true, opacity: 0.9,
  blending: THREE.AdditiveBlending, depthWrite: false
});
scene.add(new THREE.Points(geometry.clone(), matOuter));
scene.add(new THREE.Points(geometry, matInner));
renderer.setClearColor(0x000005, 1); // near-black, not pure black
```

---

## React Artifact Mode

### Available Libraries + Import Syntax
```javascript
import { useState, useEffect, useRef, useCallback, useMemo } from 'react';
import * as THREE from 'three';              // r128, same constraints as HTML
import * as d3 from 'd3';                    // full d3 v7
import * as Tone from 'tone';               // audio synthesis
import _ from 'lodash';
import * as math from 'mathjs';
import { LineChart, BarChart, AreaChart, PieChart, ScatterChart,
         XAxis, YAxis, Tooltip, CartesianGrid, ResponsiveContainer } from 'recharts';
import { Play, Pause, Volume2, ... } from 'lucide-react'; // lucide-react@0.383.0
// shadcn/ui: import { Button } from '@/components/ui/button'
// tensorflow: import * as tf from 'tensorflow'
// papaparse: import Papa from 'papaparse'
// SheetJS: import * as XLSX from 'xlsx'
```

### Not Available in React Mode
- anime.js (HTML CDN only)
- GSAP (HTML CDN only)
- p5.js
- Splitting.js
- Any CDN-only library

### Motion in React — Without GSAP
```javascript
// 1. CSS class toggle via state
const [active, setActive] = useState(false);
<div className={`el ${active ? 'active' : ''}`} style={{ transition: 'transform 0.6s cubic-bezier(.25,.46,.45,.94)' }} />

// 2. requestAnimationFrame loop in useEffect
useEffect(() => {
  let raf;
  const loop = (t) => { /* update logic */ raf = requestAnimationFrame(loop); };
  raf = requestAnimationFrame(loop);
  return () => cancelAnimationFrame(raf);
}, []);

// 3. d3 transitions for data-driven animation
d3.select(ref.current).transition().duration(600).ease(d3.easeCubicOut).attr('r', newR);
```

### React Gotchas
- No `<form>` tags — use `onClick` / `onChange`
- No `localStorage` / `sessionStorage`
- Tailwind: core utility classes only — no compiler, no custom config, no arbitrary values like `w-[342px]`
- Three.js in React: always init in `useEffect`, always clean up in the return function
- Cleanup pattern:
```javascript
useEffect(() => {
  const renderer = new THREE.WebGLRenderer();
  let rafId;
  const animate = () => { rafId = requestAnimationFrame(animate); renderer.render(scene, camera); };
  animate();
  return () => { cancelAnimationFrame(rafId); renderer.dispose(); };
}, []);
```
- Canvas access: always via `useRef` — `const canvasRef = useRef(null)`, check `canvasRef.current` before use
- Tone.js: AudioContext requires user gesture to start — always gate behind a click

---

## Mode Decision Table

| Signal in brief | HTML | React |
|----------------|------|-------|
| Three.js / WebGL as primary experience | ✓ | — |
| GSAP + Three.js needed together | ✓ | — |
| Canvas animations central | ✓ | — |
| Full-page immersive scroll | ✓ | — |
| anime.js needed | ✓ | — |
| Component state drives UX | — | ✓ |
| recharts / data visualization | — | ✓ |
| d3 generative / force graph | — | ✓ |
| Tone.js audio-reactive | — | ✓ |
| Complex multi-component UI | — | ✓ |
| Ambiguous | HTML if motion-forward | React if UI-forward |

---

## Universal Rules (apply to both modes)
- Never `localStorage` or `sessionStorage` — use in-memory state
- Never `<form>` tags — event handlers only
- Cap pixel ratio: `Math.min(window.devicePixelRatio, 2)` in any canvas/WebGL work
- Always handle `window.addEventListener('resize', ...)` in canvas/Three.js
- Always dispose Three.js resources: `renderer.dispose()`, `geometry.dispose()`, `material.dispose()`
- Prefer CSS-only when equivalent quality — more reliable, smaller payload
