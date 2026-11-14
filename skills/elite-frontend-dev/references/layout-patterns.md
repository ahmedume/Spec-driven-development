# Layout Patterns Reference

Read this file during Phase 1 (Design System) when planning layout.
These are alternatives to the standard hero-features-testimonials-CTA pattern.

---

## Hero Section Alternatives

### 1. Full-Bleed Typography Hero
No image. No illustration. Just type so large and well-set that it commands attention.
```
┌─────────────────────────────────────────┐
│ NAV                              CTA →  │
├─────────────────────────────────────────┤
│                                         │
│  THE ONLY TOOL                          │
│  DEVELOPERS                             │
│  ACTUALLY KEEP.         [pill badge]    │
│                                         │
│  [Short subtext, max 2 lines]           │
│  [CTA]          [Secondary link]        │
│                                         │
│ ─────────────────────────────────────── │
│  LOGO  LOGO  LOGO  LOGO  LOGO  (marquee)│
└─────────────────────────────────────────┘
```
**Best for:** Developer tools, SaaS, B2B

### 2. Asymmetric Split Hero
Left: text stack. Right: oversized visual/screenshot. Not equal halves — 40/60 or 35/65.
```
┌───────────────┬─────────────────────────┐
│ NAV           │                         │
├───────────────┤                         │
│               │    [APP SCREENSHOT]     │
│  HEADLINE     │    [rotated/floating]   │
│               │                         │
│  Subtext      │                         │
│               │                         │
│  [CTA]        │                         │
└───────────────┴─────────────────────────┘
```
**Best for:** SaaS products, apps, productivity tools

### 3. Magazine / Editorial Grid Hero
Multiple content blocks in a structured grid, like an editorial spread.
```
┌────────┬──────────────────┬─────────────┐
│  NAV                                    │
├────────┤                  │             │
│        │  HEADLINE        │  STAT BOX   │
│ IMAGE  │  HEADLINE        │             │
│        │                  ├─────────────┤
│        │  Subtext         │  STAT BOX   │
│        │  [CTA]           │             │
└────────┴──────────────────┴─────────────┘
```
**Best for:** Media brands, portfolios, agencies

### 4. Fullscreen Video/Canvas Hero
Immersive. The hero IS the visual. Text overlays with high contrast.
```
┌─────────────────────────────────────────┐
│ NAV (transparent, sticky)               │
│ ─────────────────────────────────────── │
│                                         │
│         [VIDEO / WEBGL / CANVAS]        │
│                                         │
│  HEADLINE                               │
│  Subtext                                │
│  [CTA]                                  │
└─────────────────────────────────────────┘
```
**Best for:** Creative agencies, product launches, luxury brands

### 5. Card Mosaic / Bento Grid Hero
The hero itself is a bento box showing the product's key value props.
```
┌─────────────────────────────────────────┐
│ NAV                                     │
├───────────────────┬─────────┬───────────┤
│                   │ CARD    │ CARD      │
│  HEADLINE +       │ (feat.) │ (feat.)   │
│  SUBTEXT +        ├─────────┴───────────┤
│  CTA              │ WIDE FEATURE CARD   │
│                   │                     │
└───────────────────┴─────────────────────┘
```
**Best for:** Feature-rich SaaS, productivity tools, dashboards

---

## Features Section Alternatives

### Anti-pattern: 3-column icon grid
That's the generic. Use one of these instead:

### 1. Alternating Split Rows
```
┌───────────────────────────────────────┐
│  [TEXT + EYEBROW]    │  [VISUAL/UI]  │
│  Headline            │               │
│  Description         │               │
├───────────────────────────────────────┤
│  [VISUAL/UI]  │  [TEXT + EYEBROW]    │
│               │  Headline            │
│               │  Description         │
└───────────────────────────────────────┘
```

### 2. Numbered Process List (if there's actual sequence)
Only use this if the features ARE steps or a flow. Not as decoration.
```
  01 → Feature Name        Long description that explains the actual value.
       ──────────────────────────────────────────────────────────────────────
  02 → Feature Name        Long description that explains the actual value.
       ──────────────────────────────────────────────────────────────────────
  03 → Feature Name        Long description that explains the actual value.
```

### 3. Large Feature Cards (2-up, not 3-up)
Two large cards feel more premium than three small ones.
```
┌──────────────────────┬──────────────────────┐
│                      │                      │
│  [VISUAL]            │  [VISUAL]            │
│                      │                      │
│  Feature Title       │  Feature Title       │
│  Description         │  Description         │
│  [link →]            │  [link →]            │
└──────────────────────┴──────────────────────┘
```

### 4. Sticky Scroll Feature Showcase
Left side: sticky text that updates. Right side: scrolls through screenshots.
```
┌─────────────────────────────────────────┐
│  [STICKY]              [SCROLLS]        │
│  Feature 1 text        Screenshot 1     │
│                                         │
│  (auto-changes as     (new screenshot   │
│   user scrolls)        per section)     │
│                                         │
│  Feature 2 text        Screenshot 2     │
└─────────────────────────────────────────┘
```

---

## Navigation Patterns

### 1. Minimal Float Nav
```
[LOGO]                          [Links]  [CTA]
────────────────────────────────────────────────
Floats above content, becomes opaque on scroll
```

### 2. Side Rail Nav (for single-page apps)
```
┌──────┬──────────────────────────────────┐
│ LOGO │                                  │
│      │                                  │
│ NAV  │  CONTENT                         │
│ ITEMS│                                  │
│      │                                  │
│ CTA  │                                  │
└──────┴──────────────────────────────────┘
```

### 3. Full-Screen Overlay Nav
Clean, minimal nav bar. On open: full-screen takeover with large-type links.
Closing animation should be as polished as opening.

### 4. Nav + Announcement Bar
```
┌─────────────────────────────────────────┐
│  🔔 New: Feature name  →  Read more     │  ← dismissable
├─────────────────────────────────────────┤
│  [LOGO]      [LINKS]          [CTA]     │
└─────────────────────────────────────────┘
```

---

## Social Proof Alternatives

### Anti-pattern: "⭐⭐⭐⭐⭐  10,000+ users trust us" with fake logos

### 1. One Long Quote, Full Width
A single, compelling testimonial in large type. More believable than a grid of 6 small ones.
```
┌─────────────────────────────────────────┐
│                                         │
│  "This made me rethink how we approach  │
│   [domain]. The impact was immediate."  │
│                                         │
│  [AVATAR]  Jane Doe · Head of X @ Co   │
│                                         │
└─────────────────────────────────────────┘
```

### 2. Live Logo Marquee
Real company logos, infinite scroll, no fake ones.
```
BRAND  ◦  BRAND  ◦  BRAND  ◦  BRAND  ◦  BRAND  →  (loops)
```

### 3. Stats Strip
Three big numbers, each with context.
```
  147%          3.4M           < 48h
  avg. revenue  queries/month  avg. setup time
  increase
```

---

## CTA Section Alternatives

### Anti-pattern: Blue box, "Ready to get started?", "Sign Up Free" button

### 1. Full-bleed Contrast Section
```
┌─────────────────────────────────────────┐
│  [HIGH-CONTRAST BACKGROUND]             │
│                                         │
│  The question isn't whether you need    │
│  [Product]. It's how soon.              │
│                                         │
│  [Primary CTA]    [Secondary: demo]     │
└─────────────────────────────────────────┘
```

### 2. Asymmetric CTA with Visual
```
┌───────────────────────┬─────────────────┐
│  [EYEBROW]            │   [APP UI OR    │
│                       │    ILLUSTRATION]│
│  COMPELLING           │                 │
│  HEADLINE             │                 │
│                       │                 │
│  [CTA]                │                 │
└───────────────────────┴─────────────────┘
```

### 3. Newsletter CTA (for content-driven sites)
```
┌─────────────────────────────────────────┐
│  Stay in the loop.                      │
│                                         │
│  [email input] ←─────────────[Subscribe]│
│                                         │
│  No spam. Unsubscribe anytime.          │
└─────────────────────────────────────────┘
```

---

## Footer Alternatives

### Anti-pattern: 4-column links to nowhere, copyright line

### 1. Minimal Statement Footer
```
┌─────────────────────────────────────────┐
│  [LOGO]                    [CTA]        │
│                                         │
│  Making [thing] better for everyone.   │
│                                         │
│  [Social icons]    [Legal links]  2025 │
└─────────────────────────────────────────┘
```

### 2. Editorial Footer
```
┌─────────────────────────────────────────┐
│                                         │
│  LET'S BUILD                            │
│  SOMETHING.                             │
│                                         │
│  [email]          [Twitter]  [GitHub]   │
│  [Location]       [LinkedIn] [Dribbble] │
│                                         │
│  ──────────────────────────── 2025     │
└─────────────────────────────────────────┘
```
