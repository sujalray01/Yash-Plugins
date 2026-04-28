# Token Dictionary

Production-ready design system foundation based on Tailwind conventions. Use this as a starting point for projects without an existing design system.

## Color Palette

### Primary (Brand Blue)
| Token | Tailwind Class | Hex | Usage |
|-------|---------------|-----|-------|
| primary-50 | bg-primary-50 | #eff6ff | Hover backgrounds |
| primary-100 | bg-primary-100 | #dbeafe | Light backgrounds |
| primary-500 | bg-primary-500 | #3b82f6 | Brand color |
| primary-600 | bg-primary-600 | #2563eb | Primary buttons |
| primary-700 | bg-primary-700 | #1d4ed8 | Hover states |

**CSS Custom Properties:**
```css
:root {
  --color-primary-50: #eff6ff;
  --color-primary-100: #dbeafe;
  --color-primary-500: #3b82f6;
  --color-primary-600: #2563eb;
  --color-primary-700: #1d4ed8;
}
```

### Neutrals
| Token | Tailwind Class | Hex | Usage |
|-------|---------------|-----|-------|
| gray-50 | bg-gray-50 | #f9fafb | Page background |
| gray-100 | bg-gray-100 | #f3f4f6 | Card background |
| gray-200 | bg-gray-200 | #e5e7eb | Border |
| gray-400 | bg-gray-400 | #9ca3af | Disabled text |
| gray-600 | bg-gray-600 | #4b5563 | Secondary text |
| gray-700 | bg-gray-700 | #374151 | Body text |
| gray-800 | bg-gray-800 | #1f2937 | Headings |
| gray-900 | bg-gray-900 | #111827 | Primary text |

**CSS Custom Properties:**
```css
:root {
  --color-gray-50: #f9fafb;
  --color-gray-100: #f3f4f6;
  --color-gray-200: #e5e7eb;
  --color-gray-400: #9ca3af;
  --color-gray-600: #4b5563;
  --color-gray-700: #374151;
  --color-gray-800: #1f2937;
  --color-gray-900: #111827;
}
```

### Semantic Colors
| Token | Tailwind Class | Hex | Usage |
|-------|---------------|-----|-------|
| red-50 | bg-red-50 | #fef2f2 | Error background |
| red-200 | border-red-200 | #fecaca | Error border |
| red-500 | border-red-500 | #ef4444 | Error input border |
| red-600 | text-red-600 | #dc2626 | Error text |
| red-700 | text-red-700 | #b91c1c | Error banner text |
| green-50 | bg-green-50 | #f0fdf4 | Success background |
| green-600 | text-green-600 | #16a34a | Success text |
| yellow-50 | bg-yellow-50 | #fefce8 | Warning background |
| yellow-600 | text-yellow-600 | #ca8a04 | Warning text |

**CSS Custom Properties:**
```css
:root {
  --color-error-bg: #fef2f2;
  --color-error-border: #fecaca;
  --color-error-text: #dc2626;
  --color-success-bg: #f0fdf4;
  --color-success-text: #16a34a;
  --color-warning-bg: #fefce8;
  --color-warning-text: #ca8a04;
}
```

### Dark Mode Overrides
| Light | Dark |
|-------|------|
| bg-white | dark:bg-gray-800 |
| bg-gray-50 | dark:bg-gray-900 |
| text-gray-900 | dark:text-gray-100 |
| text-gray-600 | dark:text-gray-400 |
| border-gray-200 | dark:border-gray-700 |

**CSS Custom Properties:**
```css
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg-primary: var(--color-gray-800);
    --color-bg-secondary: var(--color-gray-900);
    --color-text-primary: var(--color-gray-100);
    --color-text-secondary: var(--color-gray-400);
    --color-border: var(--color-gray-700);
  }
}
```

---

## Typography

### Font Families
- **Body**: `Inter` (Google Fonts) — `font-sans`
- **Heading**: `Inter` (same family, heavier weight) — `font-display`
- **Monospace**: `'JetBrains Mono', monospace` — `font-mono`
- **Fallback**: `system-ui, -apple-system, sans-serif`
- **Loading**: `font-display: swap` mandatory

**CSS Custom Properties:**
```css
:root {
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-display: 'Inter', system-ui, -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', 'Courier New', monospace;
}
```

### Type Scale
| Role | Tailwind | Size | Weight | Line Height | Usage |
|------|---------|------|--------|-------------|-------|
| Page title (h1) | text-2xl font-bold | 24px (1.5rem) | 700 | 1.2 | Main headings |
| Section heading (h2) | text-xl font-semibold | 20px (1.25rem) | 600 | 1.3 | Section titles |
| Subsection (h3) | text-lg font-semibold | 18px (1.125rem) | 600 | 1.4 | Subsections |
| Body | text-base | 16px (1rem) | 400 | 1.5 | Body text |
| Label | text-sm font-medium | 14px (0.875rem) | 500 | 1.4 | Form labels |
| Helper / error | text-sm | 14px (0.875rem) | 400 | 1.4 | Helper text |
| Caption | text-xs | 12px (0.75rem) | 400 | 1.3 | Captions |

**CSS Custom Properties:**
```css
:root {
  /* Font sizes */
  --text-xs: 0.75rem;      /* 12px */
  --text-sm: 0.875rem;     /* 14px */
  --text-base: 1rem;       /* 16px */
  --text-lg: 1.125rem;     /* 18px */
  --text-xl: 1.25rem;      /* 20px */
  --text-2xl: 1.5rem;      /* 24px */
  
  /* Font weights */
  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;
  
  /* Line heights */
  --leading-tight: 1.2;
  --leading-snug: 1.3;
  --leading-normal: 1.4;
  --leading-relaxed: 1.5;
}
```

---

## Spacing Scale

Strict Tailwind 4px base increments. No arbitrary values.

| Token | Value | Tailwind | Usage |
|-------|-------|---------|--------|
| space-0 | 0 | p-0, m-0 | Reset |
| space-1 | 4px (0.25rem) | p-1, m-1 | Tight padding |
| space-2 | 8px (0.5rem) | p-2, m-2 | Small gaps |
| space-3 | 12px (0.75rem) | p-3, m-3 | Medium-tight |
| space-4 | 16px (1rem) | p-4, m-4 | Base spacing |
| space-6 | 24px (1.5rem) | p-6, m-6 | Extra spacing |
| space-8 | 32px (2rem) | p-8, m-8 | Large spacing |
| space-12 | 48px (3rem) | p-12, m-12 | Section spacing |
| space-16 | 64px (4rem) | p-16, m-16 | Page margins |

**CSS Custom Properties:**
```css
:root {
  --space-0: 0;
  --space-1: 0.25rem;  /* 4px */
  --space-2: 0.5rem;   /* 8px */
  --space-3: 0.75rem;  /* 12px */
  --space-4: 1rem;     /* 16px */
  --space-6: 1.5rem;   /* 24px */
  --space-8: 2rem;     /* 32px */
  --space-12: 3rem;    /* 48px */
  --space-16: 4rem;    /* 64px */
}
```

---

## Border Radius

| Component | Class | Value | Usage |
|-----------|-------|-------|-------|
| None | rounded-none | 0 | Square elements |
| Small | rounded-sm | 2px | Subtle rounding |
| Base | rounded | 4px | Default |
| Medium | rounded-md | 6px | Cards |
| Large | rounded-lg | 8px | Inputs, buttons |
| Extra large | rounded-xl | 12px | Modals, panels |
| 2XL | rounded-2xl | 16px | Hero cards |
| Full | rounded-full | 9999px | Pills, badges, avatars |

**CSS Custom Properties:**
```css
:root {
  --radius-none: 0;
  --radius-sm: 0.125rem;   /* 2px */
  --radius-base: 0.25rem;  /* 4px */
  --radius-md: 0.375rem;   /* 6px */
  --radius-lg: 0.5rem;     /* 8px */
  --radius-xl: 0.75rem;    /* 12px */
  --radius-2xl: 1rem;      /* 16px */
  --radius-full: 9999px;
}
```

---

## Elevation (Shadows)

| Level | Class | Value | Usage |
|-------|-------|-------|-------|
| 0 | shadow-none | none | Flat elements |
| 1 | shadow-sm | 0 1px 2px rgba(0,0,0,0.05) | Subtle lift |
| 2 | shadow | 0 1px 3px rgba(0,0,0,0.1), 0 1px 2px rgba(0,0,0,0.06) | Base cards |
| 3 | shadow-md | 0 4px 6px rgba(0,0,0,0.07), 0 2px 4px rgba(0,0,0,0.06) | Dropdowns |
| 4 | shadow-lg | 0 10px 15px rgba(0,0,0,0.1), 0 4px 6px rgba(0,0,0,0.05) | Modals |
| 5 | shadow-xl | 0 20px 25px rgba(0,0,0,0.1), 0 10px 10px rgba(0,0,0,0.04) | Popovers |
| 6 | shadow-2xl | 0 25px 50px rgba(0,0,0,0.25) | Overlays |

**CSS Custom Properties:**
```css
:root {
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  --shadow-2xl: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
}
```

---

## Motion & Animation

| Property | Token | Value | Usage |
|----------|-------|-------|-------|
| Duration fast | duration-75 | 75ms | Instant feedback |
| Duration base | duration-150 | 150ms | Hover effects |
| Duration moderate | duration-200 | 200ms | Standard transitions |
| Duration slow | duration-300 | 300ms | Complex animations |
| Easing linear | ease-linear | linear | Constant speed |
| Easing in | ease-in | cubic-bezier(0.4, 0, 1, 1) | Accelerate |
| Easing out | ease-out | cubic-bezier(0, 0, 0.2, 1) | Decelerate |
| Easing in-out | ease-in-out | cubic-bezier(0.4, 0, 0.2, 1) | Smooth |

**CSS Custom Properties:**
```css
:root {
  --duration-75: 75ms;
  --duration-150: 150ms;
  --duration-200: 200ms;
  --duration-300: 300ms;
  
  --ease-linear: linear;
  --ease-in: cubic-bezier(0.4, 0, 1, 1);
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
}

/* Respect user preference */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

**Standard Transition:**
```css
.interactive-element {
  transition: all var(--duration-200) var(--ease-in-out);
}
```

---

## Z-Index Scale

| Level | Token | Value | Usage |
|-------|-------|-------|-------|
| Base | z-0 | 0 | Normal flow |
| Dropdown | z-10 | 10 | Dropdowns |
| Sticky | z-20 | 20 | Sticky headers |
| Fixed | z-30 | 30 | Fixed elements |
| Modal backdrop | z-40 | 40 | Modal overlays |
| Modal | z-50 | 50 | Modal dialogs |
| Popover | z-60 | 60 | Popovers, tooltips |
| Toast | z-70 | 70 | Notifications |

**CSS Custom Properties:**
```css
:root {
  --z-base: 0;
  --z-dropdown: 10;
  --z-sticky: 20;
  --z-fixed: 30;
  --z-modal-backdrop: 40;
  --z-modal: 50;
  --z-popover: 60;
  --z-toast: 70;
}
```

---

## Breakpoints

| Breakpoint | Tailwind | Min Width | Max Container | Usage |
|------------|---------|-----------|--------------|-------|
| xs | (none) | 0 | 100% | Mobile portrait |
| sm | sm: | 640px | 640px | Mobile landscape |
| md | md: | 768px | 768px | Tablet |
| lg | lg: | 1024px | 1024px | Desktop |
| xl | xl: | 1280px | 1280px | Large desktop |
| 2xl | 2xl: | 1536px | 1536px | Extra large |

**CSS Custom Properties:**
```css
:root {
  --breakpoint-sm: 640px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
  --breakpoint-xl: 1280px;
  --breakpoint-2xl: 1536px;
}
```

---

## Component Tokens

### Input Field
```css
.input {
  /* Base */
  width: 100%;
  padding: var(--space-2) var(--space-4);
  border: 1px solid var(--color-gray-300);
  border-radius: var(--radius-lg);
  font-size: var(--text-base);
  
  /* Focus */
  &:focus {
    outline: none;
    ring: 2px var(--color-primary-500);
    border-color: transparent;
  }
  
  /* Error */
  &[aria-invalid="true"] {
    border-color: var(--color-red-500);
    ring: 2px var(--color-red-500);
  }
  
  /* Dark mode */
  @media (prefers-color-scheme: dark) {
    background-color: var(--color-gray-700);
    border-color: var(--color-gray-600);
    color: var(--color-gray-100);
  }
}
```

**Tailwind:**
```html
<input class="w-full px-4 py-2 border border-gray-300 rounded-lg text-base
              focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent
              dark:bg-gray-700 dark:border-gray-600 dark:text-gray-100
              aria-[invalid=true]:border-red-500 aria-[invalid=true]:ring-red-500" />
```

### Primary Button
```css
.button-primary {
  width: 100%;
  padding: var(--space-2.5) var(--space-4);
  background-color: var(--color-primary-600);
  color: white;
  font-weight: var(--font-semibold);
  border-radius: var(--radius-lg);
  transition: background-color var(--duration-200) var(--ease-in-out);
  
  &:hover:not(:disabled) {
    background-color: var(--color-primary-700);
  }
  
  &:focus {
    outline: none;
    ring: 2px var(--color-primary-500);
    ring-offset: 2px;
  }
  
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}
```

**Tailwind:**
```html
<button class="w-full py-2.5 px-4 bg-primary-600 text-white font-semibold rounded-lg
               hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2
               transition-colors duration-200
               disabled:opacity-50 disabled:cursor-not-allowed">
  Submit
</button>
```

### Card
```css
.card {
  padding: var(--space-6);
  background-color: white;
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-md);
  
  @media (prefers-color-scheme: dark) {
    background-color: var(--color-gray-800);
  }
}
```

**Tailwind:**
```html
<div class="p-6 bg-white rounded-xl shadow-md dark:bg-gray-800">
  <!-- content -->
</div>
```

---

## Tailwind Configuration

```js
// tailwind.config.js
module.exports = {
  darkMode: 'class', // or 'media'
  theme: {
    extend: {
      colors: {
        primary: {
          50:  '#eff6ff',
          100: '#dbeafe',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
        }
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
        display: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
        mono: ['JetBrains Mono', 'Courier New', 'monospace'],
      },
      spacing: {
        // Tailwind includes 0-96 by default
      },
      borderRadius: {
        // Tailwind includes defaults
      }
    }
  },
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
  ]
}
```

---

## Accessibility Requirements

### Color Contrast
- **Normal text** (< 18px): 4.5:1 minimum
- **Large text** (≥ 18px or ≥ 14px bold): 3:1 minimum
- **UI components** (borders, icons): 3:1 minimum

**Verified Combinations:**
- `#2563eb` (primary-600) on `#ffffff` (white) = **5.9:1** ✅
- `#dc2626` (red-600) on `#ffffff` (white) = **5.74:1** ✅
- `#111827` (gray-900) on `#ffffff` (white) = **17.38:1** ✅

### Focus Indicators
```css
:focus-visible {
  outline: 2px solid var(--color-primary-500);
  outline-offset: 2px;
}

/* Or with ring utilities */
.focus-ring {
  @apply focus:ring-2 focus:ring-primary-500 focus:ring-offset-2;
}
```

### Motion Preferences
```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## Usage Examples

### Consuming Tokens in React/TypeScript

**❌ WRONG:**
```typescript
const Button = () => (
  <button style={{ 
    backgroundColor: '#2563eb',
    padding: '10px 16px',
    borderRadius: '8px'
  }}>
    Click me
  </button>
);
```

**✅ CORRECT (Tailwind):**
```typescript
const Button = () => (
  <button className="bg-primary-600 px-4 py-2.5 rounded-lg">
    Click me
  </button>
);
```

**✅ CORRECT (CSS Custom Properties):**
```typescript
// styles.css
.button {
  background-color: var(--color-primary-600);
  padding: var(--space-2-5) var(--space-4);
  border-radius: var(--radius-lg);
}

// Component
const Button = () => (
  <button className="button">Click me</button>
);
```

**✅ CORRECT (JS needs the value):**
```typescript
// tokens.ts
export const tokens = {
  colors: {
    primary: {
      600: 'var(--color-primary-600)'
    }
  },
  space: {
    4: 'var(--space-4)'
  }
} as const;

// Component
import { tokens } from './tokens';

const Button = () => (
  <button style={{ 
    backgroundColor: tokens.colors.primary[600],
    padding: `${tokens.space[2.5]} ${tokens.space[4]}`
  }}>
    Click me
  </button>
);
```

---

## Export Formats

### Style Dictionary Config
```json
{
  "source": ["tokens/**/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "buildPath": "dist/css/",
      "files": [{
        "destination": "variables.css",
        "format": "css/variables"
      }]
    },
    "js": {
      "transformGroup": "js",
      "buildPath": "dist/js/",
      "files": [{
        "destination": "tokens.js",
        "format": "javascript/es6"
      }]
    }
  }
}
```

### TypeScript Token Types
```typescript
export interface DesignTokens {
  colors: {
    primary: Record<50 | 100 | 500 | 600 | 700, string>;
    gray: Record<50 | 100 | 200 | 400 | 600 | 700 | 800 | 900, string>;
    semantic: {
      error: { bg: string; border: string; text: string };
      success: { bg: string; text: string };
      warning: { bg: string; text: string };
    };
  };
  spacing: Record<0 | 1 | 2 | 3 | 4 | 6 | 8 | 12 | 16, string>;
  typography: {
    fontFamily: { sans: string; display: string; mono: string };
    fontSize: Record<'xs' | 'sm' | 'base' | 'lg' | 'xl' | '2xl', string>;
    fontWeight: Record<'normal' | 'medium' | 'semibold' | 'bold', number>;
  };
  radius: Record<'none' | 'sm' | 'base' | 'md' | 'lg' | 'xl' | '2xl' | 'full', string>;
  shadows: Record<'none' | 'sm' | 'base' | 'md' | 'lg' | 'xl' | '2xl', string>;
  motion: {
    duration: Record<75 | 150 | 200 | 300, string>;
    easing: Record<'linear' | 'in' | 'out' | 'in-out', string>;
  };
}
```
