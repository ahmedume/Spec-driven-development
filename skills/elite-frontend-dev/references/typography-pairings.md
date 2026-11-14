# Typography Pairings Reference

Read this file during Phase 1 (Design System) when choosing fonts.
Every pairing below has been chosen for distinctiveness. None are the generic defaults.

---

## How to Choose a Pairing

Ask:
1. What is the *emotional register* of this brand? (editorial / technical / warm / authoritative / playful)
2. What's the *primary information hierarchy*? (long-form text / short headlines / data / mixed)
3. Is this *screen-first* or *print-influenced*?

Then match to a pairing below. Never default to Inter + any serif.

---

## Tier 1: High-Impact Editorial

### 1. Clash Display + Cabinet Grotesk
- **Personality:** Bold, modern editorial. Confident, slightly unconventional.
- **Use for:** Agency sites, creative studios, product launches, personal brands
- **Display:** Clash Display (700–800 weight, tight tracking at large sizes)
- **Body:** Cabinet Grotesk (400/500, -0.01em tracking)
- **CDN:** Fontshare (free, privacy-friendly)
```css
@import url('https://api.fontshare.com/v2/css?f[]=clash-display@400,500,700&f[]=cabinet-grotesk@400,500&display=swap');
```

### 2. Sentient + General Sans
- **Personality:** Humanist, editorial, slightly literary
- **Use for:** Portfolios, publishing, cultural institutions, luxury brands
- **Display:** Sentient (600–700, works brilliantly in italic)
- **Body:** General Sans (400, clean and readable)
- **CDN:** Fontshare

### 3. Satoshi + Zodiak
- **Personality:** Contemporary, slightly fashion-forward
- **Use for:** Fashion, lifestyle, modern SaaS, consumer apps
- **Display:** Zodiak (serif display, excellent at large sizes)
- **Body:** Satoshi (geometric grotesque, very readable)
- **CDN:** Fontshare

---

## Tier 2: Technical / Developer-Facing

### 4. Space Grotesk + Fira Code
- **Personality:** Technical, modern, slightly futuristic
- **Use for:** Developer tools, CLI apps, tech startups, AI products
- **Display:** Space Grotesk (700, good geometric feel)
- **Body:** Space Grotesk (400)
- **Mono:** Fira Code (for code, labels, data)
- **CDN:** Google Fonts

### 5. IBM Plex Sans + IBM Plex Mono
- **Personality:** Clean, systematic, precise. Corporate but not cold.
- **Use for:** Fintech, enterprise software, data dashboards, documentation
- **Display:** IBM Plex Sans (700)
- **Body:** IBM Plex Sans (400)
- **Mono:** IBM Plex Mono
- **CDN:** Google Fonts / IBM CDN

### 6. Geist + Geist Mono
- **Personality:** Minimal, system-like, modern. Vercel's own font.
- **Use for:** Dev tools, CLIs, minimal dashboards, open source projects
- **CDN:** @vercel/font or fontsource

---

## Tier 3: Warm & Accessible

### 7. Lora + Source Sans 3
- **Personality:** Warm, readable, trustworthy
- **Use for:** Healthcare, education, NGOs, long-form content
- **Display:** Lora (500–700 italic for pull quotes, 700 for headers)
- **Body:** Source Sans 3 (400)
- **CDN:** Google Fonts

### 8. Playfair Display + Raleway
- **Personality:** Classic elegance with a modern twist
- **Use for:** Hospitality, food & beverage, events, boutique services
- **Display:** Playfair Display (700, italic variant for accent text)
- **Body:** Raleway (400, 300 for subheadings)
- **CDN:** Google Fonts

### 9. Cormorant Garamond + Jost
- **Personality:** Refined, luxurious, fashion-editorial
- **Use for:** High-end fashion, fine dining, architecture, luxury real estate
- **Display:** Cormorant Garamond (700, very large sizes — 5rem+)
- **Body:** Jost (300–400)
- **CDN:** Google Fonts
- **Note:** Cormorant works best at 60px+. Avoid at body sizes.

---

## Tier 4: Distinctive / Experimental

### 10. Neue Machina + Rethink Sans
- **Personality:** Futuristic, tech-forward, bold geometry
- **Use for:** Crypto, AI/ML, gaming, cybersecurity, cutting-edge tech
- **Display:** Neue Machina (700–800)
- **Body:** Rethink Sans (400)
- **CDN:** Fontshare

### 11. Chillax + Switzer
- **Personality:** Young, clean, friendly-professional
- **Use for:** Startups, consumer apps, social platforms, wellness
- **Display:** Chillax (600–700)
- **Body:** Switzer (400)
- **CDN:** Fontshare

### 12. Boska + Synonym
- **Personality:** Bold, editorial, slightly retro-modern
- **Use for:** Media brands, editorial sites, magazines, bold consumer brands
- **Display:** Boska (900 — ultra-heavy display)
- **Body:** Synonym (400)
- **CDN:** Fontshare

---

## Variable Font Recommendations

For performance when using a single font across the type scale:

### 13. Fraunces (Variable)
- **Personality:** Quirky, literary, warm, has an "optical" axis
- **Axes:** Weight, optical size, wonk (unusual S-curve forms)
- **Best for:** Hospitality, culture, food, publications
- **CDN:** Google Fonts

### 14. Recursive (Variable)
- **Personality:** Technical, playful, versatile — mono OR sans
- **Axes:** Weight, slant, casualness (literally an axis called CASL)
- **Best for:** Developer tools, creative coding, personal sites
- **CDN:** Google Fonts

---

## The Forbidden List (Don't Default To These)

Unless the brief explicitly calls for it, don't default to:
- Inter (ubiquitous — used by 90% of SaaS)
- Plus Jakarta Sans (very 2022–2023 SaaS look)
- Nunito (too rounded for professional contexts)
- Poppins (overused in student/starter projects)
- DM Sans (fine, but safe to the point of invisible)
- Outfit (lovely, but the "2023 AI startup" font)
- Lato (dates the design to 2015)

These are not *bad* fonts — they're *invisible* choices. 
A visible typography choice is a design statement.

---

## CSS Implementation Template

```css
/* After importing your chosen fonts: */
:root {
  --font-display:  'Clash Display', sans-serif;
  --font-body:     'Cabinet Grotesk', sans-serif;
  --font-mono:     'Fira Code', monospace;

  /* Fluid type scale */
  --text-xs:    clamp(0.75rem,  0.7rem  + 0.25vw, 0.875rem);
  --text-sm:    clamp(0.875rem, 0.82rem + 0.25vw, 1rem);
  --text-base:  clamp(1rem,     0.95rem + 0.25vw, 1.125rem);
  --text-lg:    clamp(1.125rem, 1rem    + 0.5vw,  1.375rem);
  --text-xl:    clamp(1.375rem, 1.1rem  + 1vw,    1.875rem);
  --text-2xl:   clamp(1.75rem,  1.2rem  + 2vw,    2.75rem);
  --text-3xl:   clamp(2.5rem,   1.5rem  + 4vw,    4.5rem);
  --text-hero:  clamp(3.5rem,   2rem    + 6vw,    7rem);
}

body {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: 1.65;
  color: var(--ink);
}

h1, h2, h3 {
  font-family: var(--font-display);
  font-weight: 700;
  line-height: 1.1;
  letter-spacing: -0.025em;
}

.label, .eyebrow {
  font-family: var(--font-body);
  font-size: var(--text-xs);
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--ink-muted);
}

code, pre, .mono {
  font-family: var(--font-mono);
}
```
