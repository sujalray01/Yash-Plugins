# Accessibility & Design Token Validation

Accessibility guidelines and validation rules for design tokens to ensure WCAG 2.1 Level AA compliance.

## Color Contrast Requirements

### WCAG 2.1 Level AA Standards

| Content Type | Minimum Ratio | Examples |
|--------------|---------------|----------|
| Normal text (< 18px) | **4.5:1** | Body text, labels, descriptions |
| Large text (≥ 18px or ≥ 14px bold) | **3:1** | Headings, callouts |
| UI components | **3:1** | Borders, icons, focus indicators |
| Inactive/disabled | **No requirement** | Disabled buttons, greyed-out text |

### Contrast Checking Tools

**Online:**
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [Coolors Contrast Checker](https://coolors.co/contrast-checker)

**Browser DevTools:**
- Chrome: Inspect element → Contrast ratio in color picker
- Firefox: Accessibility inspector → Check for issues

**Command Line:**
```bash
npm install -g a11y
a11y https://your-site.com --contrast
```

### Validated Token Combinations

| Foreground | Background | Ratio | Pass/Fail |
|------------|-----------|-------|-----------|
| gray-900 (#111827) | white (#ffffff) | 17.38:1 | ✅ AAA |
| gray-800 (#1f2937) | white | 14.07:1 | ✅ AAA |
| gray-700 (#374151) | white | 10.05:1 | ✅ AAA |
| gray-600 (#4b5563) | white | 7.27:1 | ✅ AAA |
| gray-500 (#6b7280) | white | 5.20:1 | ✅ AA |
| gray-400 (#9ca3af) | white | 3.41:1 | ❌ Fail |
| primary-600 (#2563eb) | white | 5.90:1 | ✅ AAA |
| primary-500 (#3b82f6) | white | 4.53:1 | ✅ AA |
| red-600 (#dc2626) | white | 5.74:1 | ✅ AAA |
| green-600 (#16a34a) | white | 4.58:1 | ✅ AAA |

**CSS for Dynamic Contrast Checking:**
```css
/* Use CSS relative color syntax (future) */
:root {
  --text-primary: #111827;
  --bg-primary: #ffffff;
  
  /* Ensure minimum contrast */
  @supports (color: color-contrast(white vs black, red)) {
    --text-primary: color-contrast(var(--bg-primary) vs #111827, #1f2937, #374151);
  }
}
```

---

## Focus Indicators

### Requirements
- **Minimum size**: 2px thick or 1px with offset
- **Minimum contrast**: 3:1 against adjacent colors
- **Visibility**: Must be visible on all backgrounds
- **No removal**: Never use `outline: none` without replacement

### Token Implementation

```css
/* tokens/focus.css */
:root {
  --focus-ring-width: 2px;
  --focus-ring-offset: 2px;
  --focus-ring-color: var(--color-primary-500);
  --focus-ring-opacity: 1;
}

/* Standard focus style */
*:focus-visible {
  outline: var(--focus-ring-width) solid var(--focus-ring-color);
  outline-offset: var(--focus-ring-offset);
}

/* Alternative: ring-based (Tailwind style) */
.focus-ring:focus-visible {
  box-shadow: 
    0 0 0 var(--focus-ring-offset) var(--bg-primary),
    0 0 0 calc(var(--focus-ring-width) + var(--focus-ring-offset)) var(--focus-ring-color);
}
```

### Tailwind Implementation
```html
<!-- Standard focus ring -->
<button class="focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2">
  Button
</button>

<!-- High contrast mode -->
<button class="focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2
               forced-colors:outline forced-colors:outline-2">
  Button
</button>
```

---

## Typography Accessibility

### Font Size
- **Minimum body text**: 16px (1rem) — never smaller
- **Labels/helper text**: 14px (0.875rem) minimum
- **Avoid**: Font sizes below 12px

### Font Weight
- **Minimum for body**: 400 (normal)
- **Minimum for emphasis**: 600 (semibold)
- **Avoid**: 100-300 weights for small text

### Line Height
- **Body text**: 1.5 minimum (WCAG requirement)
- **Headings**: 1.2-1.3
- **Tight text**: Use sparingly, never below 1.2

```css
/* tokens/typography.css */
:root {
  --text-base: 1rem;           /* 16px */
  --text-sm: 0.875rem;         /* 14px */
  --text-xs: 0.75rem;          /* 12px - use sparingly */
  
  --leading-normal: 1.5;        /* Body text */
  --leading-relaxed: 1.6;       /* Long-form content */
  --leading-snug: 1.3;          /* Headings */
}

body {
  font-size: var(--text-base);
  line-height: var(--leading-normal);
}
```

### Font Display
```css
/* Prevent invisible text during web font loading */
@font-face {
  font-family: 'Inter';
  src: url('/fonts/inter.woff2') format('woff2');
  font-display: swap; /* Required for accessibility */
}
```

---

## Motion & Animation

### Requirements
- **Respect user preference**: Honor `prefers-reduced-motion`
- **Maximum duration**: 300ms for UI transitions (longer = frustrating)
- **Essential motion only**: Don't animate for decoration

### Token Implementation

```css
/* tokens/motion.css */
:root {
  --duration-instant: 75ms;
  --duration-fast: 150ms;
  --duration-base: 200ms;
  --duration-slow: 300ms;
}

/* Respect user preferences */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
  
  /* Override token values */
  :root {
    --duration-instant: 0.01ms;
    --duration-fast: 0.01ms;
    --duration-base: 0.01ms;
    --duration-slow: 0.01ms;
  }
}
```

### Safe Animation Patterns

**✅ Safe (position/layout changes):**
```css
.dropdown {
  transform: translateY(-8px);
  opacity: 0;
  transition: transform var(--duration-base), opacity var(--duration-base);
}

.dropdown.open {
  transform: translateY(0);
  opacity: 1;
}
```

**❌ Avoid (causes motion sickness):**
```css
/* Continuous spinning */
.loader {
  animation: spin 1s linear infinite;
}

/* Parallax scrolling */
.hero {
  transform: translateY(calc(var(--scroll) * 0.5));
}
```

---

## Touch Target Size

### Requirements (WCAG 2.1 Level AA)
- **Minimum**: 44×44px for touch targets
- **Spacing**: 8px minimum between adjacent targets

### Token Implementation

```css
/* tokens/touch-targets.css */
:root {
  --touch-target-min: 44px;
  --touch-target-spacing: 8px;
}

.button,
.link,
.checkbox,
.radio {
  min-height: var(--touch-target-min);
  min-width: var(--touch-target-min);
  
  /* Or use padding to achieve size */
  padding: var(--space-3) var(--space-4);
}

/* Spacing between targets */
.button + .button {
  margin-left: var(--touch-target-spacing);
}
```

---

## Dark Mode

### Color Adjustments for Dark Mode

| Light | Dark | Reason |
|-------|------|--------|
| black text | gray-100 text | Pure white is too harsh |
| white bg | gray-900 bg | Pure black causes smearing (OLED) |
| gray-200 border | gray-700 border | Maintain 3:1 contrast |
| Bright colors | Desaturated colors | Reduce eye strain |

### Token Implementation

```css
/* tokens/colors.css */
:root {
  /* Light mode (default) */
  --color-text-primary: #111827;
  --color-text-secondary: #4b5563;
  --color-bg-primary: #ffffff;
  --color-bg-secondary: #f9fafb;
  --color-border: #e5e7eb;
}

/* Dark mode overrides */
@media (prefers-color-scheme: dark) {
  :root {
    --color-text-primary: #f3f4f6;
    --color-text-secondary: #9ca3af;
    --color-bg-primary: #1f2937;
    --color-bg-secondary: #111827;
    --color-border: #374151;
  }
}

/* Or class-based (Tailwind) */
.dark {
  --color-text-primary: #f3f4f6;
  --color-text-secondary: #9ca3af;
  --color-bg-primary: #1f2937;
  --color-bg-secondary: #111827;
  --color-border: #374151;
}
```

### Validation Checklist
- [ ] All text meets 4.5:1 contrast in dark mode
- [ ] Focus indicators visible on dark backgrounds
- [ ] Disabled states distinguishable
- [ ] Color blindness tested (use NoCoffee extension)
- [ ] Images/icons adjusted for dark backgrounds

---

## Forced Colors Mode (Windows High Contrast)

### Requirements
- **Honor system colors**: Use semantic color keywords
- **Preserve focus**: Don't remove outlines
- **Test**: Windows High Contrast Mode or `forced-colors: active` media query

### Token Implementation

```css
/* tokens/forced-colors.css */
@media (forced-colors: active) {
  :root {
    /* Map to system colors */
    --color-text-primary: CanvasText;
    --color-bg-primary: Canvas;
    --color-link: LinkText;
    --color-border: ButtonBorder;
  }
  
  /* Ensure borders visible */
  .card,
  .button,
  .input {
    border: 1px solid ButtonBorder;
  }
  
  /* Preserve focus */
  *:focus-visible {
    outline: 2px solid Highlight;
    outline-offset: 2px;
  }
}
```

---

## Screen Reader Considerations

### Hidden but Accessible Text

```css
/* Visually hidden but screen reader accessible */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

/* Show on focus (skip links) */
.sr-only:focus {
  position: static;
  width: auto;
  height: auto;
  padding: initial;
  margin: initial;
  overflow: visible;
  clip: auto;
  white-space: normal;
}
```

---

## Validation Workflow

### Automated Testing

```json
// package.json
{
  "scripts": {
    "test:a11y": "pa11y-ci --config .pa11yci.json",
    "test:contrast": "node scripts/check-contrast.js",
    "test:wcag": "axe src/**/*.html"
  }
}
```

### Contrast Checking Script

```javascript
// scripts/check-contrast.js
import { readFileSync } from 'fs';
import { contrast } from 'wcag-contrast';

const tokens = JSON.parse(readFileSync('./tokens.json'));

const combinations = [
  { fg: tokens.colors.gray[900], bg: tokens.colors.white, name: 'body-text' },
  { fg: tokens.colors.primary[600], bg: tokens.colors.white, name: 'primary-button' },
  // ... more combinations
];

combinations.forEach(({ fg, bg, name }) => {
  const ratio = contrast.rgb(fg, bg);
  const pass = ratio >= 4.5;
  console.log(`${name}: ${ratio.toFixed(2)}:1 ${pass ? '✅' : '❌'}`);
});
```

### Manual Testing Checklist

- [ ] Keyboard navigation works (Tab, Shift+Tab, Enter, Space, Esc)
- [ ] Focus indicators visible at all times
- [ ] Screen reader announces content correctly (use NVDA/JAWS/VoiceOver)
- [ ] Color blind simulation (use DevTools or NoCoffee)
- [ ] Zoom to 200% without layout breaking
- [ ] Windows High Contrast Mode tested
- [ ] Dark mode toggle works
- [ ] Motion reduced when user preference set

---

## Common Accessibility Violations with Design Tokens

### ❌ Violation 1: Low Contrast Text
```css
/* BAD: 2.5:1 ratio - fails WCAG */
.secondary-text {
  color: var(--color-gray-400); /* #9ca3af */
  background: var(--color-white); /* #ffffff */
}
```

**Fix:**
```css
/* GOOD: 7.27:1 ratio - passes AAA */
.secondary-text {
  color: var(--color-gray-600); /* #4b5563 */
  background: var(--color-white);
}
```

### ❌ Violation 2: Invisible Focus
```css
/* BAD: Removes focus without replacement */
.button:focus {
  outline: none;
}
```

**Fix:**
```css
/* GOOD: Custom focus style */
.button:focus-visible {
  outline: 2px solid var(--color-primary-500);
  outline-offset: 2px;
}
```

### ❌ Violation 3: Motion Without Preference Check
```css
/* BAD: Ignores user preference */
.modal {
  animation: slideIn 300ms ease-out;
}
```

**Fix:**
```css
/* GOOD: Respects prefers-reduced-motion */
.modal {
  animation: slideIn var(--duration-base) ease-out;
}

@media (prefers-reduced-motion: reduce) {
  .modal {
    animation: none;
  }
}
```

### ❌ Violation 4: Small Touch Targets
```html
<!-- BAD: 24×24px button - too small --  >
<button style="width: 24px; height: 24px;">×</button>
```

**Fix:**
```html
<!-- GOOD: 44×44px touch target -->
<button class="min-w-[44px] min-h-[44px] flex items-center justify-center">
  ×
</button>
```

---

## Resources

**WCAG Guidelines:**
- [WCAG 2.1 Level AA Quick Reference](https://www.w3.org/WAI/WCAG21/quickref/?levels=aa)

**Testing Tools:**
- [axe DevTools](https://www.deque.com/axe/devtools/)
- [WAVE Browser Extension](https://wave.webaim.org/extension/)
- [Chrome Lighthouse](https://developers.google.com/web/tools/lighthouse)

**Color Contrast:**
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [Accessible Color Palette Builder](https://toolness.github.io/accessible-color-matrix/)

**Screen Readers:**
- [NVDA (free, Windows)](https://www.nvaccess.org/)
- [JAWS (paid, Windows)](https://www.freedomscientific.com/products/software/jaws/)
- [VoiceOver (built-in, macOS/iOS)](https://www.apple.com/accessibility/voiceover/)
