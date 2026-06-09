---
creative-dev / techniques.md
30 techniques with code stubs.
⭐ = HIGHLIGHT technique — use max 1-2 per piece; give it space to breathe.
✓ ARTIFACT = full working example in ./artifacts/ directory.
---

# Technique Library

## HIERARCHY — How to Compose a Piece

1. Choose ONE ⭐ HIGHLIGHT technique — it is the concept
2. Add 1-2 supporting techniques that serve, not compete
3. Layer utility techniques invisibly (smooth scroll, cursor, load orchestration)
4. Stop. Adding more degrades all of them.

**⭐ HIGHLIGHT techniques** (choose one):
T04 (split text with dramatic stagger), T11 (horizontal scroll), T13 (clip-path reveal), T24 (canvas particles), T25 (generative background), T27 (Three.js particle field), T28 (Three.js glow), T29 (GLSL shader)

**Supporting techniques** (pair up to 3):
T01, T02, T03, T05, T06, T09, T10, T12, T14, T15, T16, T17, T18, T19, T20, T21, T22, T23, T26, T30

**Utility** (always appropriate):
T01 (smooth scroll), T02 (custom cursor), T03 (load orchestration)

---

## Core / Utility

### T01 — Smooth Scroll (Lenis alternative)
**Difficulty**: ⭐ | **Mode**: HTML | **Lib**: GSAP ticker (no Lenis CDN needed)

```javascript
// Requires: body { overflow: hidden; } + .scroll-wrapper div wrapping all content
let curr = 0, tgt = 0;
window.addEventListener('wheel', e => {
  e.preventDefault();
  tgt = Math.max(0, Math.min(tgt + e.deltaY, document.body.scrollHeight - window.innerHeight));
}, { passive: false });
gsap.ticker.add(() => {
  curr += (tgt - curr) * 0.08; // 0.08 = snappy | 0.05 = dreamy
  gsap.set('.scroll-wrapper', { y: -curr });
});
// If using ScrollTrigger alongside: ScrollTrigger.normalizeScroll(true)
```

---

### T02 — Custom Cursor System ✓ ARTIFACT
**Difficulty**: ⭐ | **Mode**: HTML | **Lib**: Vanilla JS
**Full example**: `./artifacts/ref_custom-cursor.html`

```javascript
// body { cursor: none; } — hide default
const outer = document.getElementById('cursor-outer'); // ~36px circle, border-only
const dot   = document.getElementById('cursor-dot');   // ~5px filled dot
let mx = 0, my = 0, ox = 0, oy = 0;
document.addEventListener('mousemove', e => {
  mx = e.clientX; my = e.clientY;
  dot.style.left = mx + 'px'; dot.style.top = my + 'px';
});
(function lerp() {
  ox += (mx - ox) * 0.12; oy += (my - oy) * 0.12;
  outer.style.left = ox + 'px'; outer.style.top = oy + 'px';
  requestAnimationFrame(lerp);
})();
// Hover state: add data-hover to interactive elements,
// toggle body class 'is-hovering' to expand outer cursor via CSS
```

---

### T03 — Page Load Orchestration
**Difficulty**: ⭐ | **Mode**: HTML | **Lib**: GSAP

```javascript
const tl = gsap.timeline({ delay: 0.1 });
tl.from('.logo',       { opacity: 0, y: -20, duration: 0.5 })
  .from('.nav-item',   { opacity: 0, y: -10, stagger: 0.07, duration: 0.4 }, '-=0.2')
  .from('.hero .ch',   { y: '115%', rotation: 8, stagger: 0.035, duration: 0.8, ease: 'power4.out' }, '-=0.1')
  .from('.hero-sub',   { opacity: 0, y: 20, duration: 0.5 }, '-=0.4')
  .from('.hero-cta',   { opacity: 0, scale: 0.95, duration: 0.4 }, '-=0.2');
// Always: overflow: hidden on parent of split chars
// Use autoAlpha instead of opacity for elements with display:none
```

---

## Typography Techniques

### T04 — Split Text: Characters ✓ ARTIFACT ⭐ HIGHLIGHT
**Difficulty**: ⭐ | **Mode**: HTML | **Lib**: GSAP + manual split
**Full example**: `./artifacts/ref_split-text.html`
**Highlight when**: Character stagger IS the entrance — hero moments, large display type

```javascript
function splitChars(el) {
  el.innerHTML = el.textContent.split('').map(c =>
    c === ' ' ? '<span class="sp"> </span>' : `<span class="ch">${c}</span>`
  ).join('');
}
splitChars(document.querySelector('.hero-title')); // parent needs overflow:hidden
gsap.from('.ch', {
  y: '115%', rotation: 10,
  stagger: 0.04, duration: 0.9, ease: 'power4.out', delay: 0.2
});
```

---

### T05 — Split Text: Words & Lines (scroll-triggered)
**Difficulty**: ⭐ | **Mode**: HTML | **Lib**: GSAP + ScrollTrigger
**Full example**: `./artifacts/ref_split-text.html`

```javascript
function splitWords(el) {
  el.innerHTML = el.textContent.trim().split(/\s+/).map(w =>
    `<span class="w-wrap"><span class="w-inner">${w}</span></span>`
  ).join(' ');
  // CSS: .w-wrap { display: inline-block; overflow: hidden; }
  //      .w-inner { display: inline-block; transform: translateY(105%); }
}
splitWords(document.querySelector('.scroll-heading'));
gsap.to('.w-inner', {
  scrollTrigger: { trigger: '.scroll-heading', start: 'top 78%' },
  y: '0%', duration: 0.8, stagger: 0.06, ease: 'power3.out'
});
```

---

### T06 — Masked Text Reveal (scroll progress)
**Difficulty**: ⭐⭐ | **Mode**: HTML | **Lib**: GSAP ScrollTrigger scrub

```javascript
// Words fade in sequentially as reader scrolls — feels like the text appears for you
document.querySelector('.reveal-text').innerHTML = text.split(' ').map(w =>
  `<span class="rw" style="opacity:0.12;color:rgba(255,255,255,0.15)">${w} </span>`
).join('');
gsap.to('.rw', {
  scrollTrigger: { trigger: '.reveal-section', start: 'top center', end: 'bottom center', scrub: true },
  opacity: 1, color: '#f0ede6', stagger: 0.5, ease: 'none'
});
```

---

### T07 — Text Scramble / Glitch
**Difficulty**: ⭐⭐ | **Mode**: Both | **Lib**: Vanilla JS

```javascript
function scramble(el, target, ms = 1200) {
  const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%&';
  const start = performance.now();
  const tick = () => {
    const p = Math.min((performance.now() - start) / ms, 1);
    el.textContent = target.split('').map((c, i) =>
      i < Math.floor(p * target.length) ? c : chars[Math.floor(Math.random() * chars.length)]
    ).join('');
    if (p < 1) requestAnimationFrame(tick);
  };
  requestAnimationFrame(tick);
}
// Trigger: scramble(el, 'HELLO WORLD', 900);
// Pair with: el.addEventListener('mouseenter', () => scramble(el, el.dataset.text));
```

---

### T08 — Infinite Marquee Loop
**Difficulty**: ⭐ | **Mode**: Both | **Lib**: CSS only

```css
.marquee { overflow: hidden; white-space: nowrap; }
.marquee-track {
  display: inline-block;
  animation: marquee 18s linear infinite;
}
.marquee-track span { display: inline-block; padding: 0 3rem; }
@keyframes marquee { from { transform: translateX(0); } to { transform: translateX(-50%); } }
/* Duplicate content exactly once inside .marquee-track for seamless loop */
/* Reverse: animation-direction: reverse; | Pause on hover: animation-play-state: paused */
```

---

## Scroll & Motion

### T09 — Scroll-Triggered Entrance (Stagger) ✓ ARTIFACT
**Difficulty**: ⭐ | **Mode**: HTML | **Lib**: GSAP + ScrollTrigger
**Full example**: `./artifacts/ref_scroll-reveal.html`

```javascript
gsap.registerPlugin(ScrollTrigger);
gsap.from('.card', {
  scrollTrigger: { trigger: '.grid', start: 'top 78%' },
  y: 80, opacity: 0, duration: 0.9,
  stagger: { amount: 0.5, from: 'start' }, // 'random' for organic feel
  ease: 'power3.out'
});
// Best practice: set initial opacity:0 in CSS to avoid FOUC if JS is slow
```

---

### T10 — Parallax Layers (multi-depth)
**Difficulty**: ⭐⭐ | **Mode**: HTML | **Lib**: GSAP ScrollTrigger scrub

```javascript
// 3 layers: bg slowest, mid medium, fg natural speed
gsap.to('.layer-bg',  { scrollTrigger: { scrub: 1.5 }, y: '-30%' });
gsap.to('.layer-mid', { scrollTrigger: { scrub: 1 },   y: '-15%' });
// .layer-fg follows natural scroll
// Container: position: relative; overflow: hidden;
// Layers: position: absolute; will-change: transform;
// Each layer needs ~30% extra height for travel room
```

---

### T11 — Horizontal Scroll Section (pinned) ⭐ HIGHLIGHT
**Difficulty**: ⭐⭐ | **Mode**: HTML | **Lib**: GSAP ScrollTrigger
**Highlight when**: Sequential chapters, gallery, product lineup — spatial storytelling

```javascript
const panels = gsap.utils.toArray('.h-panel');
gsap.to('.h-track', {
  scrollTrigger: {
    trigger: '.h-section',
    pin: true, scrub: 1,
    end: () => '+=' + (panels.length - 1) * window.innerWidth
  },
  x: () => -((panels.length - 1) * window.innerWidth),
  ease: 'none'
});
// .h-section: height: 100vh; position: relative;
// .h-track: display: flex; width: calc(N * 100vw); height: 100%;
// .h-panel: width: 100vw; height: 100%; flex-shrink: 0;
```

---

### T12 — Scroll Progress Indicator
**Difficulty**: ⭐ | **Mode**: Both | **Lib**: Vanilla JS or CSS scroll-driven

```javascript
// JS version (universal)
const bar = document.querySelector('.progress');
window.addEventListener('scroll', () => {
  const pct = window.scrollY / (document.body.scrollHeight - window.innerHeight) * 100;
  bar.style.width = pct + '%';
}, { passive: true });
// .progress: position:fixed; top:0; left:0; height:2px; background:var(--accent); z-index:100;
```

---

### T13 — Clip-Path Reveal ✓ ARTIFACT ⭐ HIGHLIGHT
**Difficulty**: ⭐⭐ | **Mode**: Both | **Lib**: GSAP (preferred) or CSS transition
**Full example**: `./artifacts/ref_clip-path.html`
**Highlight when**: Image reveals, section wipes, hover reveals — directional and dramatic

```javascript
// Direction map (set initial state, animate to open)
const clips = {
  left:   { closed: 'inset(0 100% 0 0)',  open: 'inset(0 0% 0 0)' },
  right:  { closed: 'inset(0 0 0 100%)',  open: 'inset(0 0 0 0%)' },
  top:    { closed: 'inset(100% 0 0 0)',  open: 'inset(0% 0 0 0)' },
  bottom: { closed: 'inset(0 0 100% 0)',  open: 'inset(0 0 0% 0)' },
};
const { closed, open } = clips['left'];
gsap.set('.cover', { clipPath: closed });
el.addEventListener('mouseenter', () => gsap.to('.cover', { clipPath: open, duration: 0.6, ease: 'power3.inOut' }));
el.addEventListener('mouseleave', () => gsap.to('.cover', { clipPath: closed, duration: 0.5, ease: 'power3.inOut' }));
// Scroll wipe: gsap.to('.wipe-bg', { scrollTrigger: { scrub: 1 }, clipPath: 'inset(0 0 0% 0)' });
```

---

### T14 — Page Transition (overlay wipe)
**Difficulty**: ⭐⭐ | **Mode**: HTML | **Lib**: GSAP

```javascript
// .page-overlay: position:fixed; inset:0; z-index:100; background:var(--accent); clip-path:inset(0 100% 0 0)
function transitionTo(url) {
  gsap.to('.page-overlay', {
    clipPath: 'inset(0 0% 0 0)', duration: 0.55, ease: 'power3.inOut',
    onComplete: () => { window.location.href = url; }
  });
}
// On DOMContentLoaded: animate overlay away
gsap.to('.page-overlay', { clipPath: 'inset(0 0 0 100%)', duration: 0.55, ease: 'power3.inOut', delay: 0.1 });
```

---

## Hover & Interaction

### T15 — Magnetic Element
**Difficulty**: ⭐⭐ | **Mode**: Both | **Lib**: Vanilla JS

```javascript
document.querySelectorAll('[data-magnetic]').forEach(el => {
  el.addEventListener('mousemove', e => {
    const r = el.getBoundingClientRect();
    const x = (e.clientX - r.left - r.width  / 2) * 0.4; // 0.4 = pull strength
    const y = (e.clientY - r.top  - r.height / 2) * 0.4;
    el.style.transform = `translate(${x}px, ${y}px)`;
  });
  el.addEventListener('mouseleave', () => {
    el.style.transition = 'transform 0.5s cubic-bezier(.25,.46,.45,.94)';
    el.style.transform = 'translate(0, 0)';
    setTimeout(() => el.style.transition = '', 500);
  });
});
```

---

### T16 — Image Distortion on Hover (CSS)
**Difficulty**: ⭐ | **Mode**: Both | **Lib**: CSS only

```css
.img-wrap { overflow: hidden; }
.img-wrap img {
  transition: transform 0.7s cubic-bezier(.25,.46,.45,.94), filter 0.4s;
  will-change: transform;
}
.img-wrap:hover img {
  transform: scale(1.06) translateY(-2%);
  filter: contrast(1.08) brightness(1.04);
}
/* For WebGL displacement distortion: requires GLSL — see T29 and Active Theory reference */
```

---

### T17 — Perspective Tilt (3D card)
**Difficulty**: ⭐⭐ | **Mode**: Both | **Lib**: Vanilla JS

```javascript
document.querySelectorAll('[data-tilt]').forEach(el => {
  el.style.transition = 'transform 0.1s ease';
  el.addEventListener('mousemove', e => {
    const r  = el.getBoundingClientRect();
    const x  = (e.clientX - r.left) / r.width  - 0.5; // -0.5 to 0.5
    const y  = (e.clientY - r.top)  / r.height - 0.5;
    el.style.transform = `perspective(600px) rotateX(${-y*14}deg) rotateY(${x*14}deg) scale(1.02)`;
  });
  el.addEventListener('mouseleave', () => {
    el.style.transition = 'transform 0.5s ease';
    el.style.transform  = 'perspective(600px) rotateX(0) rotateY(0) scale(1)';
  });
});
// el: transform-style: preserve-3d;
```

---

### T18 — Cursor Trail / Follower
**Difficulty**: ⭐⭐ | **Mode**: HTML | **Lib**: Vanilla JS

```javascript
const N = 10;
let mx = 0, my = 0;
const dots = Array.from({ length: N }, (_, i) => {
  const d = document.createElement('div');
  const s = 6 - i * 0.4;
  d.style.cssText = `position:fixed;width:${s}px;height:${s}px;border-radius:50%;
    background:rgba(255,255,255,${0.75-i*0.07});pointer-events:none;z-index:9000;
    transform:translate(-50%,-50%)`;
  document.body.appendChild(d);
  return { el: d, x: 0, y: 0 };
});
document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; });
(function tick() {
  dots.forEach((d, i) => {
    const prev = i === 0 ? { x: mx, y: my } : dots[i - 1];
    d.x += (prev.x - d.x) * 0.3;
    d.y += (prev.y - d.y) * 0.3;
    d.el.style.left = d.x + 'px'; d.el.style.top = d.y + 'px';
  });
  requestAnimationFrame(tick);
})();
```

---

## Visual / Atmospheric

### T19 — Noise / Grain Texture Overlay
**Difficulty**: ⭐ | **Mode**: Both | **Lib**: CSS only (SVG data URI)

```css
/* Animated grain — no image file needed */
body::after {
  content: ''; position: fixed; inset: 0; pointer-events: none; z-index: 999;
  opacity: 0.3;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  animation: grain 0.4s steps(1) infinite;
}
@keyframes grain {
  0%,100% { transform: translate(0,0) }  20% { transform: translate(-3%,-2%) }
  40% { transform: translate(2%,3%) }    60% { transform: translate(-1%,2%) }
  80% { transform: translate(3%,-1%) }
}
/* Static grain: remove animation. Reduce opacity on light backgrounds. */
```

---

### T20 — CSS Blend Mode Layering
**Difficulty**: ⭐ | **Mode**: Both | **Lib**: CSS only

```css
.blend-container { position: relative; isolation: isolate; }
.blend-image { display: block; width: 100%; }
.blend-color {
  position: absolute; inset: 0;
  background: linear-gradient(135deg, #1a1a2e, #16213e);
  mix-blend-mode: multiply;  /* multiply: darkens, keeps texture */
}
/* Other useful modes:
   screen   — lightens, good for overlaying on dark
   overlay  — boosts contrast, dramatic
   color    — applies hue from layer, keeps luminance of base
   hue      — changes hue only
   soft-light — subtle contrast boost                          */
```

---

### T21 — SVG Path Draw-On
**Difficulty**: ⭐⭐ | **Mode**: Both | **Lib**: GSAP ScrollTrigger or CSS

```javascript
const path = document.querySelector('.draw-path');
const len  = path.getTotalLength();
// Set initial state
Object.assign(path.style, { strokeDasharray: len, strokeDashoffset: len, fill: 'none' });
// Animate on scroll
gsap.to(path, {
  strokeDashoffset: 0, ease: 'none',
  scrollTrigger: { trigger: path, start: 'top 80%', end: 'bottom 20%', scrub: 1 }
});
// CSS version: @keyframes draw { to { stroke-dashoffset: 0; } }
// path { stroke-dasharray: [len]; stroke-dashoffset: [len]; animation: draw 2s ease forwards; }
```

---

### T22 — Staggered Grid Reveal
**Difficulty**: ⭐ | **Mode**: Both | **Lib**: GSAP ScrollTrigger

```javascript
gsap.from('.grid > *', {
  scrollTrigger: { trigger: '.grid', start: 'top 72%' },
  scale: 0.88, opacity: 0, duration: 0.65,
  stagger: { amount: 0.5, from: 'random' }, // 'random' feels organic
  ease: 'power2.out'
});
// 'from: random' vs 'from: start' vs 'from: center' — each has different energy
```

---

### T23 — Color Theme Swap (CSS variables)
**Difficulty**: ⭐ | **Mode**: Both | **Lib**: CSS + Vanilla JS

```javascript
const themes = {
  dark:  { '--bg': '#080808', '--text': '#f0ede6', '--accent': '#c8ff00' },
  light: { '--bg': '#f0ede6', '--text': '#080808', '--accent': '#1a1aff' },
  warm:  { '--bg': '#120800', '--text': '#f5e6d3', '--accent': '#ff6b35' },
};
function setTheme(name) {
  Object.entries(themes[name]).forEach(([k, v]) =>
    document.documentElement.style.setProperty(k, v)
  );
}
// html { transition: background-color 0.4s, color 0.4s; } for smooth swap
```

---

## Canvas & Generative

### T24 — Canvas Particle System ✓ ARTIFACT ⭐ HIGHLIGHT
**Difficulty**: ⭐⭐ | **Mode**: HTML | **Lib**: Vanilla JS (zero CDN)
**Full example**: `./artifacts/ref_canvas-particles.html`
**Highlight when**: Ambient interactive field as hero or background

```javascript
// Core pattern — see artifact for mouse attraction + connection lines
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth; canvas.height = window.innerHeight;
const P = Array.from({ length: 80 }, () => ({
  x: Math.random() * canvas.width,  y: Math.random() * canvas.height,
  vx: (Math.random() - .5) * .6,   vy: (Math.random() - .5) * .6,
  hue: 210 + Math.random() * 50,    r: 1.5 + Math.random() * 1.5
}));
function draw() {
  ctx.fillStyle = 'rgba(6,6,18,.15)'; ctx.fillRect(0, 0, canvas.width, canvas.height);
  P.forEach(p => {
    p.x = (p.x + p.vx + canvas.width)  % canvas.width;
    p.y = (p.y + p.vy + canvas.height) % canvas.height;
    ctx.beginPath(); ctx.arc(p.x, p.y, p.r, 0, Math.PI*2);
    ctx.fillStyle = `hsla(${p.hue},70%,65%,.85)`; ctx.fill();
  });
  requestAnimationFrame(draw);
}
draw();
```

---

### T25 — Generative Background (animated art) ⭐ HIGHLIGHT
**Difficulty**: ⭐⭐⭐ | **Mode**: HTML | **Lib**: Vanilla canvas
**Highlight when**: The site IS the art — no content to distract from; ambient brand world

```javascript
// Wave field — layered sine waves on canvas
let t = 0;
function drawWaves() {
  ctx.clearRect(0, 0, W, H);
  for (let i = 0; i < 10; i++) {
    const hue = (t * 0.15 + i * 35) % 360;
    ctx.beginPath();
    for (let x = 0; x <= W; x += 4) {
      const y = H/2
        + Math.sin(x * 0.006 + t * 0.018 + i * 0.6) * (50 + i * 9)
        + Math.cos(x * 0.014 + t * 0.012 + i * 0.3) * 18;
      x === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
    }
    ctx.strokeStyle = `hsla(${hue},65%,55%,${0.18 - i * 0.015})`;
    ctx.lineWidth = 1.5; ctx.stroke();
  }
  t++; requestAnimationFrame(drawWaves);
}
```

---

### T26 — Image Sequence Scrubbing
**Difficulty**: ⭐⭐⭐ | **Mode**: HTML | **Lib**: GSAP ScrollTrigger + Canvas

```javascript
// Preload sequence, draw frame via scroll progress
const frames = [], TOTAL = 60;
for (let i = 1; i <= TOTAL; i++) {
  const img = new Image();
  img.src = `seq/frame_${String(i).padStart(4,'0')}.jpg`;
  frames.push(img);
}
const canvas = document.querySelector('canvas');
const ctx = canvas.getContext('2d');
const drawFrame = n => { if (frames[n]?.complete) ctx.drawImage(frames[n], 0, 0, canvas.width, canvas.height); };
gsap.to({ f: 0 }, {
  f: TOTAL - 1, snap: { f: 1 },
  scrollTrigger: { trigger: '.seq-section', pin: true, scrub: 0.5, end: '+=3000' },
  onUpdate() { drawFrame(Math.round(this.targets()[0].f)); }
});
// Note: requires pre-exported JPEG sequence. 30-80 frames at ~50-80% quality optimal.
```

---

## Three.js (Tier 3)

### T27 — Three.js Particle Field / Sphere ✓ ARTIFACT ⭐ HIGHLIGHT
**Difficulty**: ⭐⭐⭐ | **Mode**: HTML | **Lib**: Three.js r128
**Full example**: `./artifacts/ref_threejs-scene.html`
**Highlight when**: Space, cosmos, data cloud, organic 3D world — immersive and atmospheric

```javascript
// Fibonacci sphere — mathematically even point distribution
const count = 4000, pos = new Float32Array(count * 3);
const goldenAngle = Math.PI * (3 - Math.sqrt(5)); // ≈ 2.399 rad
for (let i = 0; i < count; i++) {
  const phi   = Math.acos(1 - 2 * i / count);   // polar angle
  const theta = goldenAngle * i;                  // azimuthal spiral
  const r     = 2 + (Math.random() - .5) * 0.8;  // slight scatter
  pos[i*3]   = r * Math.sin(phi) * Math.cos(theta);
  pos[i*3+1] = r * Math.sin(phi) * Math.sin(theta);
  pos[i*3+2] = r * Math.cos(phi);
}
const geo = new THREE.BufferGeometry();
geo.setAttribute('position', new THREE.BufferAttribute(pos, 3));
const mat = new THREE.PointsMaterial({
  size: 0.015, color: 0x88aaff, transparent: true,
  blending: THREE.AdditiveBlending, depthWrite: false
});
scene.add(new THREE.Points(geo, mat));
// Mouse parallax (no OrbitControls in r128 CDN):
// camera.position.x += (targetX - camera.position.x) * 0.04;
```

---

### T28 — Three.js Additive Glow (no EffectComposer) ⭐ HIGHLIGHT
**Difficulty**: ⭐⭐⭐ | **Mode**: HTML | **Lib**: Three.js r128
**Highlight when**: Neon/sci-fi, glowing particles, star fields — bloom without post-processing

```javascript
// Two-layer trick fakes UnrealBloomPass
const geoBase = new THREE.BufferGeometry();
geoBase.setAttribute('position', new THREE.BufferAttribute(pos, 3));

const matGlow = new THREE.PointsMaterial({ // large, faint outer layer
  size: 0.07, color: 0x4466ff, transparent: true, opacity: 0.12,
  blending: THREE.AdditiveBlending, depthWrite: false
});
const matCore = new THREE.PointsMaterial({ // small, bright inner layer
  size: 0.016, color: 0xaaccff, transparent: true, opacity: 0.95,
  blending: THREE.AdditiveBlending, depthWrite: false
});
scene.add(new THREE.Points(geoBase.clone(), matGlow));
scene.add(new THREE.Points(geoBase, matCore));
renderer.setClearColor(0x000008, 1); // near-black — pure black kills the glow illusion
```

---

### T29 — Three.js GLSL Shader Plane (advanced / optional)
**Difficulty**: ⭐⭐⭐⭐ | **Mode**: HTML | **Lib**: Three.js r128 ShaderMaterial
**Flag for users**: This requires GLSL knowledge. Skip if unsure — T25/T24 achieve similar visual impact.

```javascript
const mat = new THREE.ShaderMaterial({
  uniforms: {
    uTime:  { value: 0.0 },
    uMouse: { value: new THREE.Vector2(0.5, 0.5) }
  },
  vertexShader: `
    varying vec2 vUv;
    void main() {
      vUv = uv;
      gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
    }
  `,
  fragmentShader: `
    uniform float uTime;
    uniform vec2  uMouse;
    varying vec2  vUv;
    void main() {
      vec2 uv = vUv - 0.5;
      float d = length(uv - (uMouse - 0.5) * 0.3);
      float wave = sin(d * 18.0 - uTime * 2.5) * 0.5 + 0.5;
      gl_FragColor = vec4(0.2 + wave * 0.5, 0.4 + wave * 0.3, 1.0, 1.0);
    }
  `
});
const mesh = new THREE.Mesh(new THREE.PlaneGeometry(3, 3, 32, 32), mat);
scene.add(mesh);
// In loop: mat.uniforms.uTime.value = clock.getElapsedTime();
//          mat.uniforms.uMouse.value.set(normMouseX, normMouseY);
```

---

### T30 — CSS Scroll-Driven Animation (new spec, progressive enhancement)
**Difficulty**: ⭐⭐ | **Mode**: Both | **Lib**: CSS only (Chrome 115+, Safari 18+)
**Use as**: Enhancement alongside GSAP fallback — not as sole animation method yet

```css
/* Element animates based on its own scroll position into viewport */
@keyframes revealUp {
  from { opacity: 0; transform: translateY(40px); }
  to   { opacity: 1; transform: translateY(0); }
}
.scroll-anim {
  animation: revealUp linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 60%;
}
/* Page-level progress: animation-timeline: scroll(root) */
/* Check support: @supports (animation-timeline: scroll()) { ... } */
```

---

## Option B — Niche Alternatives
*Use when fixed stack falls short. Document new patterns per learning protocol.*

| Library | When to reach for it | CDN |
|---------|---------------------|-----|
| **anime.js** | Complex multi-property SVG animation, path morphing — better than GSAP free for SVG | cdnjs |
| **p5.js** | Rapid generative art prototyping, accessible to non-coders | cdnjs |
| **pixi.js** | Sprite-heavy 2D WebGL — 10x faster than canvas for many objects | cdnjs |
| **Motion One** | React/WAAPI-based animation, minimal bundle, no CDN dependency | React only |
| **Velocity.js** | Simple animation needs, lighter than GSAP | cdnjs |
| **Babylon.js** | Feature-complete 3D engine — when Three.js r128 limitations are blocking | cdnjs |
