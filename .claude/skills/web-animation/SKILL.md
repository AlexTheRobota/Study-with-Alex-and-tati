---
name: web-animation
description: Build every new website, page or lesson in this repo with GSAP (animation + ScrollTrigger), Lenis (smooth scrolling) and effects in the style of React Bits. Use whenever making or redesigning a web page here, even if the user does not name these libraries.
---

# Web animation: GSAP + Lenis + React Bits

Every page in this repo is a single static `index.html` in its own folder (no build step, no npm, no React).
Build pages with these three tools:

| Tool | What it is for | Repo |
|---|---|---|
| **GSAP** + ScrollTrigger | all animation: entrances, timelines, scroll-linked effects, button feedback | https://github.com/greensock/GSAP |
| **Lenis** | smooth, buttery page scrolling | https://github.com/darkroomengineering/lenis |
| **React Bits** | a catalogue of animated text, backgrounds, cards and cursor effects to copy ideas from | https://github.com/DavidHDev/react-bits |

## 1. Load the libraries (exact versions, from jsDelivr)

Put these before your own `<script>`, at the end of `<body>`:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/lenis@1.3.27/dist/lenis.css">
<script src="https://cdn.jsdelivr.net/npm/gsap@3.15.1/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.15.1/dist/ScrollTrigger.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.15.1/dist/SplitText.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lenis@1.3.27/dist/lenis.min.js"></script>
```

Always pin these exact versions; never write `gsap@3` or `lenis@latest`.
Only add `SplitText` when the page splits text into letters or words.
For 3D models keep using three.js r128 like the other lessons
(`https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js` plus
`https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js`).

## 2. Wire Lenis to GSAP (copy this once per page)

```js
gsap.registerPlugin(ScrollTrigger);
const REDUCE = matchMedia('(prefers-reduced-motion: reduce)').matches;
let lenis = null;
if (!REDUCE) {
  lenis = new Lenis({ autoRaf: false, lerp: 0.1 });
  lenis.on('scroll', ScrollTrigger.update);
  gsap.ticker.add(t => lenis.raf(t * 1000));
  gsap.ticker.lagSmoothing(0);
}
// Anchor links: lenis ? lenis.scrollTo('#s2') : el.scrollIntoView()
```

Rules for Lenis:
- Add `data-lenis-prevent` to anything that scrolls by itself (a scrollable list or modal) and to every
  three.js / canvas stage that the user drags with a finger, so dragging a model does not scroll the page.
- Do not also set `scroll-behavior: smooth` in CSS; Lenis does that job.

## 3. GSAP patterns to use

```js
// Entrance on scroll: fade/slide each section in once
gsap.utils.toArray('section').forEach(sec => {
  gsap.from(sec.children, { y: 40, opacity: 0, duration: .8, ease: 'power3.out', stagger: .08,
    scrollTrigger: { trigger: sec, start: 'top 80%', once: true } });
});

// Headline split into letters that pop in (needs SplitText)
const split = SplitText.create('h1', { type: 'chars,words' });
gsap.from(split.chars, { yPercent: 120, rotate: 8, opacity: 0, stagger: .025, duration: .7, ease: 'back.out(1.7)' });

// Pinned story: a section stays on screen while steps play as you scroll
const tl = gsap.timeline({ scrollTrigger: { trigger: '#story', start: 'top top', end: '+=1600', pin: true, scrub: 1 } });
tl.to('.step1', { opacity: 1 }).to('.step2', { opacity: 1 }).to('.step3', { opacity: 1 });

// Reward feedback for a correct answer
gsap.fromTo(el, { scale: .8 }, { scale: 1, duration: .5, ease: 'elastic.out(1, .4)' });
// Wrong answer shake
gsap.to(el, { keyframes: { x: [0, -8, 8, -5, 5, 0] }, duration: .4, ease: 'none' });
```

Rules for GSAP:
- Animate `transform` and `opacity` (x, y, scale, rotate, opacity), not width, height, top or left.
- Use `gsap.from` for entrances so the page still reads correctly if JavaScript fails.
- Use `once: true` for entrances. Use `scrub` only for story sections where scrolling *is* the interaction.
- Call `ScrollTrigger.refresh()` after anything changes page height (images loading, opening a solution).
- If `REDUCE` is true, skip entrances and pinned scrubbing; show the final state straight away.

## 4. React Bits: borrow the effects, rebuild them in plain JS

React Bits components are React (installed with `npx shadcn@latest add @react-bits/<Name>-JS-CSS`).
This repo has no React or build step, so **do not install React Bits or load React**. Instead, pick an
effect from the React Bits catalogue (https://reactbits.dev) and rebuild it with GSAP, CSS or a small canvas.
Good fits for lessons:

- **Text animations**: split text, typewriter, rotating words, scroll-reveal text, count-up numbers for scores.
- **Animations**: magnet / tilt on hover and touch, click sparks, glowing or animated borders on cards.
- **Components**: stacked or flip cards for flashcards, a dock-style bottom menu, carousels for steps.
- **Backgrounds**: soft particles, gradient blobs, dot grids. Keep backgrounds calm behind text a child reads.

React Bits is "MIT + Commons Clause": copying the idea and rebuilding it is fine; when you copy code from a
component, keep it inside this repo and do not sell it.

## 5. Every page must still

- Be in Bulgarian, for a 5th grader, and work on a phone and an iPad (16px side padding, no sideways scroll).
- Look different from the lessons already in the repo (other colours, fonts and models).
- Keep animation purposeful: it explains a step, rewards an answer or guides the eye. Never animate everything.
- Respect `prefers-reduced-motion`, keep text readable (good contrast), and have a real `<title>`.
- Be tested in a browser at desktop and phone sizes before you say it is done.
