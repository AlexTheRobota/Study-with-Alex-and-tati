# Учене с Алекс и тати

Every lesson is its own folder with an `index.html` (for example `subirane/`, `razshiryavane/`).

## Making a website or page

**Every time you make a new website, page or lesson, use the `web-animation` skill** (`.claude/skills/web-animation/`). Load it first, before writing any HTML, and follow it: build with GSAP + ScrollTrigger, Lenis smooth scrolling and React Bits–style effects, at the pinned versions it gives.

- Pick a different look from the lessons already in the repo, so they don't all look the same.
- Lessons are for a 5th grader, in Bulgarian. Make them interactive and fun, and make sure they work on a phone and an iPad.

## The standard for every lesson

The user loved `mesopotamia/` and wants every lesson made to that level. Use it as the reference for *quality*, not for the look: each new lesson gets its own fresh look.

- **Read the whole skill and any reference files it points to** before writing code.
- **Pick a concept from the subject's own world and let the whole lesson play it** (Mesopotamia = "you are the archaeologist", every activity is a "find"). The look, the wording and the progress counter all follow that idea.
- **A signature opening the child does with a finger**: something to uncover, build or set going in the first screen (the sand you brush away to find the mosaic).
- **Cover the whole textbook page(s)**: every fact, picture, question and the summary box, with answers the child can open.
- **Many kinds of activity**: a map or diagram you tap, a sorting game, 3D models with hotspots (three.js r128 + OrbitControls from cdnjs/jsdelivr), a story game ("ти си съдията"), a quiz, a creative task. Every activity gives feedback and counts towards a progress tally saved in `localStorage`.
- **Bold, committed design**: big display type, one accent colour, alternating light/dark sections, sharp details, purposeful motion with `prefers-reduced-motion` respected.
- **Test before delivering**: Playwright screenshots at iPhone, iPad and laptop sizes; no sideways scroll (check with fallback fonts too, since huge titles overflow); click through every game; no console errors. Fix what you see, then review it honestly: which part is the most generic, and does the signature opening really work?
- Add a `README.md` in the lesson folder, commit and push. If asked for an artifact, publish a copy without the `../` home link.
