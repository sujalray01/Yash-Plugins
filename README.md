# Design System Audit Plugin

A VS Code agent skill for auditing and refactoring design tokens to establish a single source of truth.

## Installation

This skill is installed at:
```
C:\Users\sujal.ray\.claude\skills\design-system-audit\
```

It's automatically available in VS Code Copilot after installation.

## Usage

### Invoke as Slash Command

Type `/design-system-audit` in the VS Code chat to activate the skill.

### Automatic Invocation

The skill is automatically loaded when you mention any of these keywords:
- "design system audit"
- "token consolidation"
- "CSS architecture"
- "eliminate hardcoded values"
- "single source of truth"
- "design tokens"
- "Tailwind config"
- "CSS custom properties"
- "dark mode tokens"
- "component token library"

## What It Does

1. **Inventory**: Scans your codebase for all design-related values (colors, spacing, typography, etc.)
2. **Analysis**: Identifies duplications, inconsistencies, and accessibility issues
3. **Recommendation**: Proposes a single source of truth architecture
4. **Migration Plan**: Provides phased refactoring steps (Expand → Migrate → Contract)
5. **Validation**: Checks accessibility, responsiveness, and browser support

## Features

- ✅ Consolidate CSS and JS design tokens
- ✅ Eliminate hardcoded values
- ✅ Establish token scales (colors, spacing, typography, shadows, motion)
- ✅ Dark mode token setup
- ✅ Accessibility validation (contrast, focus, motion)
- ✅ Tailwind config optimization
- ✅ Component library theme configuration
- ✅ Migration patterns with before/after examples

## File Structure

```
design-system-audit/
├── SKILL.md                      # Main skill instructions
├── references/
│   ├── token-dictionary.md       # Production-ready token reference
│   ├── migration-patterns.md     # Common refactoring patterns
│   └── accessibility.md          # A11y validation guidelines
└── README.md                     # This file
```

## Example Prompts

**Basic audit:**
> "Audit my codebase for design token issues"

**Specific problem:**
> "I have hardcoded colors scattered everywhere. Help me consolidate them."

**Component focus:**
> "Review my button components for token consistency"

**Architecture question:**
> "Should I use CSS custom properties or Tailwind for my design tokens?"

**Migration help:**
> "Create a step-by-step plan to migrate from SCSS variables to CSS custom properties"

## Constraints

This skill:
- ❌ Does NOT propose new visual designs
- ❌ Does NOT touch business logic or backend code
- ❌ Does NOT recommend component libraries unless necessary
- ✅ ONLY focuses on the design/styling layer

## Optimization

The skill is designed for progressive loading:
1. **Discovery** (~100 tokens): Skill description
2. **Instructions** (~2000 tokens): Main SKILL.md
3. **References** (loaded on demand): Additional reference docs

## Troubleshooting

**Skill not loading?**
- Verify the folder name matches: `design-system-audit`
- Check YAML frontmatter in SKILL.md
- Restart VS Code

**Not appearing in slash commands?**
- Type `/` in chat and look for "design-system-audit"
- Ensure `user-invocable: true` in frontmatter

**Automatically loaded when you don't want it?**
- Mention "use default agent" to bypass
- Set `disable-model-invocation: true` in frontmatter

## Learn More

- [VS Code Agent Skills Documentation](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- [WCAG 2.1 Level AA Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [Tailwind Design System Guide](https://tailwindcss.com/docs/theme)

## License

This skill is part of your personal VS Code customizations.
