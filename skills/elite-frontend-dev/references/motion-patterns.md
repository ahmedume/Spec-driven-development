# Motion Patterns Reference

Read this file during Phase 4 (Motion) when building animations.

---

## GSAP ScrollTrigger Patterns

### Hero Text Reveal (Split by lines)
```javascript
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import SplitType from 'split-type';

gsap.registerPlugin(ScrollTrigger);

const heroText = new SplitType('.hero-title', { types: 'lines' });

gsap.from(heroText.lines, {
  y: '100%',
  opacity: 0,
  duration: 1,
  stagger: 0.1,
  ease: 'power4.out',
  delay: 0.2
});
```

### Pinned Horizontal Scroll Section
```javascript
const sections = gsap.utils.toArray('.h-section');

gsap.to(sections, {
  xPercent: -100 * (sections.length - 1),
  ease: 'none',
  scrollTrigger: {
    trigger: '.h-scroll-container',
    pin: true,
    scrub: 1,
    snap: 1 / (sections.length - 1),
    end: () => '+=' + document.querySelector('.h-scroll-container').offsetWidth
  }
});
```

### Clip-path Reveal
```javascript
gsap.from('.clip-reveal', {
  clipPath: 'inset(0 100% 0 0)',
  duration: 1.2,
  ease: 'power4.inOut',
  stagger: 0.15,
  scrollTrigger: {
    trigger: '.clip-section',
    start: 'top 70%'
  }
});
```

### Counter Animation
```javascript
function animateCounter(el) {
  const target = parseInt(el.dataset.target);
  gsap.from({ value: 0 }, {
    value: target,
    duration: 2,
    ease: 'power2.out',
    onUpdate: function() {
      el.textContent = Math.round(this.targets()[0].value).toLocaleString();
    },
    scrollTrigger: {
      trigger: el,
      start: 'top 80%',
      once: true
    }
  });
}
document.querySelectorAll('[data-target]').forEach(animateCounter);
```

### Parallax Image
```javascript
document.querySelectorAll('[data-parallax]').forEach(el => {
  const speed = parseFloat(el.dataset.parallax) || 0.3;
  gsap.to(el, {
    yPercent: -30 * speed,
    ease: 'none',
    scrollTrigger: {
      trigger: el.parentElement,
      start: 'top bottom',
      end: 'bottom top',
      scrub: true
    }
  });
});
```

---

## Framer Motion Variants (React)

### Page Transition
```typescript
// lib/motion.ts
export const pageVariants = {
  initial: { opacity: 0, y: 20 },
  animate: { opacity: 1, y: 0, transition: { duration: 0.5, ease: [0.22, 1, 0.36, 1] } },
  exit: { opacity: 0, y: -10, transition: { duration: 0.3 } }
};

export const stagger = {
  animate: { transition: { staggerChildren: 0.1 } }
};

export const fadeUp = {
  initial: { opacity: 0, y: 24 },
  animate: { opacity: 1, y: 0, transition: { duration: 0.6, ease: [0.22, 1, 0.36, 1] } }
};
```

### Magnetic Button (React + Framer Motion)
```typescript
'use client';
import { useRef, useState } from 'react';
import { motion, useSpring } from 'framer-motion';

export function MagneticButton({ children }: { children: React.ReactNode }) {
  const ref = useRef<HTMLButtonElement>(null);
  const x = useSpring(0, { stiffness: 300, damping: 20 });
  const y = useSpring(0, { stiffness: 300, damping: 20 });

  const handleMouseMove = (e: React.MouseEvent) => {
    if (!ref.current) return;
    const rect = ref.current.getBoundingClientRect();
    x.set((e.clientX - rect.left - rect.width / 2) * 0.35);
    y.set((e.clientY - rect.top - rect.height / 2) * 0.35);
  };

  return (
    <motion.button
      ref={ref}
      style={{ x, y }}
      onMouseMove={handleMouseMove}
      onMouseLeave={() => { x.set(0); y.set(0); }}
    >
      {children}
    </motion.button>
  );
}
```

### List Stagger
```typescript
const listVariants = {
  hidden: {},
  visible: {
    transition: { staggerChildren: 0.08, delayChildren: 0.2 }
  }
};

const itemVariants = {
  hidden: { opacity: 0, x: -16 },
  visible: { opacity: 1, x: 0, transition: { duration: 0.5, ease: 'easeOut' } }
};

// Usage:
<motion.ul variants={listVariants} initial="hidden" whileInView="visible">
  {items.map(item => (
    <motion.li key={item.id} variants={itemVariants}>{item.label}</motion.li>
  ))}
</motion.ul>
```

---

## Intersection Observer (Vanilla — no library needed for basic reveals)

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('is-visible');
      observer.unobserve(entry.target); // only animate once
    }
  });
}, { threshold: 0.15 });

document.querySelectorAll('.animate-on-scroll').forEach(el => observer.observe(el));
```

```css
.animate-on-scroll {
  opacity: 0;
  transform: translateY(32px);
  transition: opacity 0.7s cubic-bezier(0.22, 1, 0.36, 1),
              transform 0.7s cubic-bezier(0.22, 1, 0.36, 1);
}

.animate-on-scroll.is-visible {
  opacity: 1;
  transform: translateY(0);
}

@media (prefers-reduced-motion: reduce) {
  .animate-on-scroll {
    opacity: 1;
    transform: none;
    transition: none;
  }
}
```

---

## Cursor Effects

### Custom Cursor (Vanilla)
```javascript
const cursor = document.querySelector('.cursor');
const cursorDot = document.querySelector('.cursor-dot');

let mouseX = 0, mouseY = 0;
let curX = 0, curY = 0;

document.addEventListener('mousemove', e => {
  mouseX = e.clientX;
  mouseY = e.clientY;
  cursorDot.style.transform = `translate(${mouseX}px, ${mouseY}px)`;
});

function animateCursor() {
  curX += (mouseX - curX) * 0.12;
  curY += (mouseY - curY) * 0.12;
  cursor.style.transform = `translate(${curX}px, ${curY}px)`;
  requestAnimationFrame(animateCursor);
}
animateCursor();

// Hover states
document.querySelectorAll('a, button').forEach(el => {
  el.addEventListener('mouseenter', () => cursor.classList.add('is-hovering'));
  el.addEventListener('mouseleave', () => cursor.classList.remove('is-hovering'));
});
```

```css
.cursor {
  width: 40px; height: 40px;
  border: 1px solid var(--accent);
  border-radius: 50%;
  position: fixed;
  top: -20px; left: -20px;
  pointer-events: none;
  z-index: 9999;
  transition: width 0.3s, height 0.3s, background-color 0.3s;
}

.cursor.is-hovering {
  width: 60px; height: 60px;
  background-color: color-mix(in srgb, var(--accent) 15%, transparent);
}

.cursor-dot {
  width: 6px; height: 6px;
  background: var(--accent);
  border-radius: 50%;
  position: fixed;
  top: -3px; left: -3px;
  pointer-events: none;
  z-index: 10000;
}
```

---

## CSS-Only Micro-interactions

```css
/* Button with underline animation */
.btn-underline {
  position: relative;
  display: inline-block;
}

.btn-underline::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 100%;
  height: 1px;
  background: currentColor;
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s cubic-bezier(0.22, 1, 0.36, 1);
}

.btn-underline:hover::after {
  transform: scaleX(1);
  transform-origin: left;
}

/* Card tilt (CSS only — for GSAP-free contexts) */
/* Note: For full 3D tilt, use vanilla-tilt.js or GSAP */

/* Shimmer loading state */
@keyframes shimmer {
  to { background-position: 200% center; }
}

.skeleton {
  background: linear-gradient(
    90deg,
    var(--surface-alt) 25%,
    color-mix(in srgb, var(--surface-alt) 60%, var(--ink)) 50%,
    var(--surface-alt) 75%
  );
  background-size: 200% auto;
  animation: shimmer 1.5s linear infinite;
  border-radius: var(--radius-sm);
}
```
