---
name: cinematic-3d-web
description: >
  Use this skill whenever the user wants a cinematic, scroll-driven 3D website —
  luxury product pages (watches, cars, jewelry, perfume), "Abyssal"-style WebGL
  experiences, immersive 3D portfolios, or any site where the camera moves through
  a 3D scene as the user scrolls. Trigger on phrases like "3D website", "scroll
  animation website", "cinematic landing page", "luxury product site", "like
  Awwwards sites", "WebGL portfolio", "make my scroll feel like a movie", or
  references to Three.js/R3F/GSAP ScrollTrigger visual experiences — even if the
  user doesn't name the tech. This is a specialization of elite-frontend-dev for
  the specific case of camera-driven WebGL storytelling; use it INSTEAD of a plain
  HTML/CSS animated site whenever depth, a moving camera, or a "walk through a
  scene" feeling is implied. Do NOT use this for standard flat SaaS/dashboard UIs
  with no 3D or camera motion — that's elite-frontend-dev's territory alone.
---

# Cinematic 3D Web Skill

You are building the kind of site that wins Awwwards Site of the Day: the browser
window becomes a camera, and scrolling is the timeline. The person isn't reading a
page — they're walking through a space you built. If the result looks like a
static page with a 3D hero bolted on top, you have failed at this skill.

**The standard:** the camera should feel *directed*, not just "an object rotates
while you scroll." Every shot should look intentional, like a DP planned it.

---

## Core mental model

Stop thinking in "sections." Think in **shots**:

- The page scroll position (0 → 1) is a **timeline**, not a list of divs.
- The **camera** moves along an authored path (a spline or a sequence of GSAP
  tweens) — position, rotation, and FOV are all animatable.
- **HTML content is a subtitle track** layered on top of the WebGL canvas, pinned
  and revealed in sync with camera position, not scrolling independently of it.
- **Lighting and post-processing set mood per shot** — a "night" zone and a
  "reveal" zone should not share the same bloom/exposure settings.

If the user's brief is a luxury watch site, a portfolio, or an "Abyssal-style"
site, the shape is the same: hero shot → descent/orbit through 2-4 key moments →
a resolving final shot (product/CTA/contact). Plan the shots before writing code.

---

## Phase 0: Plan the shot list (always do this first)

Before touching code, write out 4-8 "shots" in plain language, e.g.:

1. **Hero** — camera far back, object rotating slowly, title fades in.
2. **Push-in** — camera dollies toward the object as the first detail callout
   appears beside it.
3. **Orbit** — camera arcs around the object while a second callout pins.
4. **Descent** — environment changes (light shifts, fog thickens), transitional.
5. **Reveal** — object turns to face camera dead-on, hero shot #2, main CTA.

For each shot, note: camera position/target, what's lit, what HTML overlays, and
what triggers the transition to the next shot (scroll progress range). This shot
list becomes your `ScrollTrigger` timeline plan — skipping it is why AI-generated
3D sites feel like a slideshow instead of a journey.

---

## Tech stack (default choices — matches modern Next.js projects)

- **React Three Fiber (`@react-three/fiber`) + drei** — for scene/object
  management inside Next.js/TypeScript. Use raw Three.js only for a static HTML
  demo with no framework.
- **GSAP + ScrollTrigger** — the industry-standard way to bind scroll progress to
  camera/material/DOM state. `scrub: true` for camera work, not raw scroll
  listeners.
- **Lenis** (`@studio-freight/lenis` or `lenis`) — smooth/inertial scroll. Without
  it, native scroll deltas are too jittery for cinematic camera motion. Sync
  Lenis's `scroll` event into GSAP's ticker.
- **`@react-three/postprocessing`** — bloom, depth of field, chromatic aberration,
  vignette. This is what separates "flat 3D" from "cinematic."
- Keep the canvas `position: fixed` behind the HTML, or use R3F's `<View>` /
  drei's `ScrollControls` if the whole page is one continuous 3D scene.

```bash
npm install three @react-three/fiber @react-three/drei @react-three/postprocessing gsap lenis
```

---

## Camera-to-scroll binding pattern

This is the single most important piece of code in this skill. Get this right and
everything else (lighting, models, HTML overlays) just plugs into it.

```tsx
// hooks/useScrollCamera.ts
import { useRef } from "react";
import { useFrame } from "@react-three/fiber";
import * as THREE from "three";
import gsap from "gsap";

export function useScrollCamera() {
  const progress = useRef(0); // 0 -> 1, driven by Lenis + ScrollTrigger
  const target = useRef(new THREE.Vector3());

  useFrame(({ camera }) => {
    // Never set camera position directly from scroll — lerp toward it.
    // A hard sync feels robotic; a lerp of 0.06-0.1 feels directed.
    const shot = getShotForProgress(progress.current); // your authored keyframes
    camera.position.lerp(shot.position, 0.08);
    target.current.lerp(shot.lookAt, 0.08);
    camera.lookAt(target.current);
    camera.fov = THREE.MathUtils.lerp(camera.fov, shot.fov, 0.08);
    camera.updateProjectionMatrix();
  });

  return progress;
}
```

Author `getShotForProgress` as a small array of keyframes (progress 0→1, each with
position/lookAt/fov) and interpolate between the two nearest keyframes with
`smoothstep` easing — don't linearly lerp between more than 2 at once, or corners
in the camera path go mushy.

Drive `progress.current` from a single `ScrollTrigger` on the page's total
height, updated in its `onUpdate`, not from multiple independent triggers — one
source of truth for "where are we in the story."

---

## HTML overlay sync

Pin overlay text to the *same* progress ranges as the shots that motivate them,
using GSAP `ScrollTrigger` with `pin: true` on wrapper sections, and fade
opacity/translateY based on `self.progress` inside the trigger's range — not a
generic AOS-style "fade in when it enters viewport," which fights the camera
timing and desyncs quickly.

```tsx
gsap.timeline({
  scrollTrigger: {
    trigger: "#shot-2",
    start: "top top",
    end: "+=100%",
    scrub: 1,
    pin: true,
  },
})
  .fromTo(".shot-2-copy", { autoAlpha: 0, y: 40 }, { autoAlpha: 1, y: 0 })
  .to(".shot-2-copy", { autoAlpha: 0, y: -40 }, "+=0.3");
```

---

## Lighting & post-processing (what makes it feel expensive)

- **One key light per shot**, not one global light for the whole scene. Swap
  intensity/color via GSAP tweens synced to progress, matching your shot list's
  mood notes.
- **Bloom** (`UnrealBloomPass` / `@react-three/postprocessing`'s `<Bloom>`) on
  emissive/highlight materials only — strength ~0.4-1.0, threshold ~0.7-0.9.
  Global heavy bloom looks cheap; selective bloom looks premium.
- **Environment/IBL** (`<Environment preset="studio">` from drei, or a custom
  HDRI) for anything reflective (watches, cars, glass, metal). Flat materials
  under a single directional light are the #1 tell of an AI-generated 3D scene.
- **Depth of field** sparingly — only on hero/reveal shots, disabled on mobile
  (see performance section).
- **Fog** (`scene.fog`) is a cheap, high-impact way to sell depth and mood shifts
  between zones — cheaper than particles, use it before reaching for particle
  systems.

---

## Performance (non-negotiable, not optional polish)

3D scroll sites die on mobile and low-end laptops if you skip this:

- **Detect device tier once on mount** (rough heuristic: `navigator.hardwareConcurrency`,
  `matchMedia('(pointer: coarse)')` for touch, or a quick WebGL renderer-info
  check) and branch: disable DOF/bloom-heavy passes, cap `pixelRatio` at 1.5, and
  reduce geometry/particle counts on the low tier.
- **Deferred initialization**: don't build every shot's geometry/textures at
  mount. Initialize a zone's heavy assets just before its progress range is
  reached, dispose when far behind — spreads GPU upload cost across the scroll
  instead of front-loading a multi-second freeze.
- **Dispose on unmount**: call `.dispose()` on geometries/materials/render
  targets and `renderer.forceContextLoss()` in cleanup — Next.js hot reload and
  route changes leak GPU memory fast otherwise.
- **`prefers-reduced-motion`**: provide a non-animated fallback (static hero image
  + normal scroll) — this is an accessibility requirement, not a nice-to-have.
- **SEO**: WebGL canvas content is invisible to crawlers. Every fact/CTA that
  lives in the 3D scene must have an HTML equivalent (can be visually hidden,
  not `display: none`) so the page has real content.
- Target 60fps on the primary device you're demoing on; profile with Chrome's
  Performance panel before calling a shot "done," not just eyeballing smoothness.

---

## Anti-patterns (the generic-AI 3D site tells)

- A single spinning `<mesh>` with `OrbitControls` left enabled — this looks like
  a Three.js tutorial, not a product site. Camera should be scroll-authored;
  don't leave free orbit controls on in the final build.
- Bloom/vignette turned up uniformly across the whole page regardless of shot.
- Text that fades in/out identically for every section (that's the 2D AOS.js
  tell, ported badly into 3D).
- No fallback for `prefers-reduced-motion` or low-end devices.
- Stock primitive geometries (`SphereGeometry`, `TorusKnotGeometry`) standing in
  for the actual product with no real model — fine for a placeholder, but flag it
  to the user rather than shipping it as final.

---

## Reference points to pull from (search before designing)

When the brief doesn't specify an aesthetic, search for concrete references
rather than defaulting to a generic dark-mode-plus-particles look:

- `site:awwwards.com "3d" scroll site of the day`
- `codrops scroll-driven 3D` (Codrops tutorials double as strong visual refs)
- Luxury brand microsites (watches, cars, perfume) for lighting/material cues
- `three.js discourse showcase` for recent community demos and technique names

Extract the *camera choreography and mood*, never copy geometry/shader code
verbatim from a specific commercial site.

---

## Deliverable checklist before calling it done

- [ ] Shot list written and camera path matches it (not ad-hoc per-scroll-pixel tweaks)
- [ ] Single progress source of truth driving both camera and HTML overlays
- [ ] Per-shot lighting/mood, not one global light
- [ ] Bloom/DOF selective, not global
- [ ] Device-tier branch + `prefers-reduced-motion` fallback implemented
- [ ] Geometries/materials disposed on unmount
- [ ] Real HTML content backing anything critical inside the canvas (SEO)
- [ ] Profiled for 60fps on the demo device
