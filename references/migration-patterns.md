# Migration Patterns

Common refactoring patterns for consolidating design tokens and establishing a single source of truth.

## Pattern 1: Eliminate Duplication Between CSS and JS

### Problem
Same value defined in both SCSS variables and JS constants:

```scss
// styles/_variables.scss
$modal-width: 42rem;
$modal-padding: 2rem;
```

```typescript
// constants/layout.ts
export const MODAL_WIDTH = '42rem';
export const MODAL_PADDING = '2rem';
```

### Solution: CSS Custom Properties as Source of Truth

**Phase 1: Expand** (add CSS custom properties)
```css
/* styles/tokens.css */
:root {
  --modal-width: 42rem;
  --modal-padding: 2rem;
}
```

**Phase 2: Migrate** (update consumers)
```scss
// Update SCSS to reference CSS custom properties
$modal-width: var(--modal-width);
$modal-padding: var(--modal-padding);

.modal {
  width: var(--modal-width);
  padding: var(--modal-padding);
}
```

```typescript
// Update JS to read from CSS custom properties
export function getModalWidth(): string {
  return getComputedStyle(document.documentElement)
    .getPropertyValue('--modal-width')
    .trim();
}

// Or use the CSS variable directly in styled-components/emotion
const Modal = styled.div`
  width: var(--modal-width);
  padding: var(--modal-padding);
`;
```

**Phase 3: Contract** (remove old constants)
```typescript
// ❌ Delete old constants file
// export const MODAL_WIDTH = '42rem'; // DELETED
```

---

## Pattern 2: Extract Hardcoded Values to Tokens

### Problem
Magic numbers scattered across components:

```typescript
// Button.tsx
<button style={{ 
  padding: '8px 16px',
  borderRadius: '8px',
  fontSize: '14px'
}}>
  Click me
</button>

// Card.tsx
<div style={{ 
  padding: '24px',
  borderRadius: '12px'
}}>
  Content
</div>
```

### Solution: Create Token Scale

**Phase 1: Expand** (create token system)
```css
/* tokens.css */
:root {
  /* Spacing scale */
  --space-2: 0.5rem;   /* 8px */
  --space-4: 1rem;     /* 16px */
  --space-6: 1.5rem;   /* 24px */
  
  /* Radius scale */
  --radius-lg: 0.5rem;  /* 8px */
  --radius-xl: 0.75rem; /* 12px */
  
  /* Typography */
  --text-sm: 0.875rem; /* 14px */
}
```

**Phase 2: Migrate** (update components)
```typescript
// Button.tsx
<button className="px-4 py-2 rounded-lg text-sm">
  Click me
</button>

// Or with CSS modules
<button className={styles.button}>
  Click me
</button>

// styles/Button.module.css
.button {
  padding: var(--space-2) var(--space-4);
  border-radius: var(--radius-lg);
  font-size: var(--text-sm);
}
```

**Phase 3: Contract** (enforce via linting)
```javascript
// .eslintrc.js
rules: {
  'no-magic-numbers': ['error', { 
    ignore: [0, 1, -1],
    ignoreArrayIndexes: true
  }]
}
```

---

## Pattern 3: Consolidate Color Palette

### Problem
Inconsistent color values across codebase:

```typescript
// Component A
<div style={{ color: '#1a73e8' }} />

// Component B  
<div style={{ color: '#2563eb' }} />

// Component C
<div style={{ color: 'rgb(37, 99, 235)' }} />
```

### Solution: Centralized Color System

**Phase 1: Expand** (audit and define palette)
```bash
# Find all color values
grep -r '#[0-9a-fA-F]\{6\}' src/
grep -r 'rgb(' src/
```

**Create unified palette:**
```css
/* tokens/colors.css */
:root {
  /* Primary brand colors */
  --color-primary-500: #3b82f6;
  --color-primary-600: #2563eb;
  --color-primary-700: #1d4ed8;
  
  /* Semantic aliases */
  --color-link: var(--color-primary-600);
  --color-button-primary: var(--color-primary-600);
  --color-button-primary-hover: var(--color-primary-700);
}
```

**Phase 2: Migrate** (systematic replacement)
```typescript
// Before
<a style={{ color: '#1a73e8' }}>Link</a>
<button style={{ backgroundColor: '#2563eb' }}>Button</button>

// After
<a className="text-primary-600">Link</a>
<button className="bg-primary-600 hover:bg-primary-700">Button</button>

// Or with CSS
<a className="link">Link</a>
<button className="button-primary">Button</button>

// styles.css
.link {
  color: var(--color-link);
}

.button-primary {
  background-color: var(--color-button-primary);
}

.button-primary:hover {
  background-color: var(--color-button-primary-hover);
}
```

**Phase 3: Contract** (prevent new hardcoded colors)
```javascript
// stylelint.config.js
module.exports = {
  rules: {
    'color-named': 'never',
    'color-no-hex': true, // Only allow via CSS custom properties
    'function-disallowed-list': ['rgb', 'rgba', 'hsl', 'hsla']
  }
};
```

---

## Pattern 4: Unify Component Library Tokens

### Problem
Bypassing component library theme:

```typescript
// Using Material-UI but overriding at component level
<Button sx={{ 
  backgroundColor: '#2563eb',
  padding: '8px 16px',
  borderRadius: '8px'
}}>
  Submit
</Button>
```

### Solution: Extend Component Library Theme

**Phase 1: Expand** (configure theme)
```typescript
// theme.ts
import { createTheme } from '@mui/material/styles';

export const theme = createTheme({
  palette: {
    primary: {
      main: '#2563eb',
      light: '#3b82f6',
      dark: '#1d4ed8',
    },
  },
  spacing: 8, // 1 unit = 8px
  shape: {
    borderRadius: 8,
  },
  typography: {
    button: {
      textTransform: 'none',
      fontWeight: 600,
    },
  },
});
```

**Phase 2: Migrate** (use theme tokens)
```typescript
// Before
<Button sx={{ 
  backgroundColor: '#2563eb',
  padding: '8px 16px',
  borderRadius: '8px'
}}>
  Submit
</Button>

// After
<Button 
  variant="contained" 
  color="primary"
  sx={{ px: 2, py: 1 }} // Uses theme.spacing
>
  Submit
</Button>
```

**Phase 3: Contract** (enforce theme usage)
```typescript
// eslint-plugin-custom-rules.js
module.exports = {
  rules: {
    'no-sx-literals': {
      create(context) {
        return {
          JSXAttribute(node) {
            if (node.name.name === 'sx') {
              // Check for literal values in sx prop
              // Flag violations
            }
          }
        };
      }
    }
  }
};
```

---

## Pattern 5: Responsive Design Tokens

### Problem
Inconsistent breakpoints and responsive values:

```typescript
// Component A
@media (min-width: 768px) { ... }

// Component B  
@media (min-width: 769px) { ... }

// Component C
<Box sx={{ display: { xs: 'none', md: 'block' } }} />
```

### Solution: Centralized Breakpoint System

**Phase 1: Expand** (define breakpoints)
```css
/* tokens/breakpoints.css */
:root {
  --breakpoint-sm: 640px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
  --breakpoint-xl: 1280px;
}

/* Media query mixins */
@custom-media --sm (min-width: 640px);
@custom-media --md (min-width: 768px);
@custom-media --lg (min-width: 1024px);
@custom-media --xl (min-width: 1280px);
```

```typescript
// tokens/breakpoints.ts
export const breakpoints = {
  sm: 640,
  md: 768,
  lg: 1024,
  xl: 1280,
} as const;

export type Breakpoint = keyof typeof breakpoints;
```

**Phase 2: Migrate** (use consistent breakpoints)
```typescript
// Before
@media (min-width: 768px) {
  .sidebar { display: block; }
}

// After (CSS)
@media (--md) {
  .sidebar { display: block; }
}

// Or Tailwind
<div className="hidden md:block">Sidebar</div>

// Or Material-UI with theme
<Box sx={{ display: { xs: 'none', md: 'block' } }}>
  Sidebar
</Box>
```

**Phase 3: Contract** (lint for hardcoded breakpoints)
```javascript
// stylelint.config.js
module.exports = {
  rules: {
    'media-feature-name-disallowed-list': ['width', 'min-width', 'max-width'],
    // Force use of custom media queries
  }
};
```

---

## Pattern 6: Animation Duration Standardization

### Problem
Inconsistent transition/animation durations:

```css
.button { transition: all 0.2s ease; }
.modal { transition: opacity 200ms ease-in-out; }
.dropdown { animation: slideDown 0.15s cubic-bezier(0.4, 0, 0.2, 1); }
```

### Solution: Motion Token System

**Phase 1: Expand** (create motion tokens)
```css
/* tokens/motion.css */
:root {
  /* Durations */
  --duration-fast: 150ms;
  --duration-base: 200ms;
  --duration-slow: 300ms;
  
  /* Easings */
  --ease-in: cubic-bezier(0.4, 0, 1, 1);
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
  
  /* Standard transition */
  --transition: var(--duration-base) var(--ease-in-out);
}

/* Accessibility */
@media (prefers-reduced-motion: reduce) {
  :root {
    --duration-fast: 0.01ms;
    --duration-base: 0.01ms;
    --duration-slow: 0.01ms;
  }
}
```

**Phase 2: Migrate** (apply motion tokens)
```css
/* Before */
.button {
  transition: all 0.2s ease;
}

/* After */
.button {
  transition: all var(--transition);
}

/* Or be specific */
.button {
  transition-property: background-color, transform;
  transition-duration: var(--duration-base);
  transition-timing-function: var(--ease-in-out);
}
```

**Phase 3: Contract** (enforce via tooling)
```javascript
// stylelint.config.js
module.exports = {
  rules: {
    'declaration-property-value-disallowed-list': {
      '/^transition/': ['/ms/', '/s/'], // Disallow literal time values
      '/^animation/': ['/ms/', '/s/'],
    }
  }
};
```

---

## Pattern 7: Z-Index Management

### Problem
Random z-index values causing stacking issues:

```css
.dropdown { z-index: 999; }
.modal { z-index: 9999; }
.tooltip { z-index: 99999; }
.header { z-index: 100; }
```

### Solution: Z-Index Scale

**Phase 1: Expand** (define z-index scale)
```css
/* tokens/z-index.css */
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

```typescript
// tokens/z-index.ts
export const zIndex = {
  base: 0,
  dropdown: 10,
  sticky: 20,
  fixed: 30,
  modalBackdrop: 40,
  modal: 50,
  popover: 60,
  toast: 70,
} as const;
```

**Phase 2: Migrate** (replace arbitrary values)
```css
/* Before */
.dropdown { z-index: 999; }
.modal { z-index: 9999; }

/* After */
.dropdown { z-index: var(--z-dropdown); }
.modal { z-index: var(--z-modal); }
```

```typescript
// React component
import { zIndex } from './tokens/z-index';

<Modal style={{ zIndex: zIndex.modal }} />
```

**Phase 3: Contract** (enforce scale usage)
```javascript
// stylelint.config.js
module.exports = {
  rules: {
    'declaration-property-value-disallowed-list': {
      'z-index': ['/^[0-9]+$/'], // Only allow CSS variables
    }
  }
};
```

---

## Verification Checklist

After migration, verify:

- [ ] All hardcoded values replaced with tokens
- [ ] CSS and JS consume from same source (CSS custom properties)
- [ ] Dark mode works (if applicable)
- [ ] Responsive breakpoints consistent
- [ ] No visual regressions (screenshot tests)
- [ ] Accessibility preserved (contrast, focus, motion)
- [ ] Build succeeds, no TypeScript errors
- [ ] Linting rules enforce token usage
- [ ] Documentation updated
- [ ] Team onboarded to new system

---

## Tooling Recommendations

### For Auditing
- **grep/ripgrep**: Find hardcoded values
- **Chrome DevTools**: Inspect computed styles
- **axe DevTools**: Check color contrast

### For Migration
- **jscodeshift**: Automated code transformations
- **postcss-custom-properties**: Fallbacks for old browsers
- **Style Dictionary**: Multi-platform token compilation

### For Enforcement
- **stylelint**: CSS linting
- **eslint-plugin-styled-components**: JS-in-CSS linting
- **Danger.js**: PR checks for hardcoded values
- **chromatic/percy**: Visual regression testing
