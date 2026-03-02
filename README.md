# MUI v6 Skills for LLMs

A comprehensive, AI-optimized reference guide for **MUI v6** (Material UI & Joy UI). This repository contains a `SKILL.md` file designed to help LLMs (Claude, Gemini, ChatGPT, GitHub Copilot) understand and apply the latest MUI features, best practices, and migration patterns.

## 🚀 Why This Exists

LLMs are often trained on older data. When working with **MUI v6**, they frequently suggest deprecated v5 patterns (like legacy Grid, `theme.palette.mode` checks, or barrel imports). This skill file provides the necessary context for AI models to:

- Use the new **unified Grid v2** system.
- Leverage **CSS Theme Variables** and the `colorSchemes` API for seamless dark mode.
- Apply the **`sx` prop** and **`styled()` API** correctly with modern v6 patterns.
- Implement **Dark Mode** without SSR flicker using `InitColorSchemeScript`.
- Avoid common **v5 → v6 migration pitfalls**.
- Follow **Import Best Practices** for optimal bundle size.

## 📂 What's Inside `SKILL.md`

The file is organized into **19 sections** covering everything an LLM needs:

| Section | Topic                                                                                                     |
| ------- | --------------------------------------------------------------------------------------------------------- |
| 1       | **Installation** — Material UI, Joy UI, Peer Deps, Fonts, Icons, CssBaseline                              |
| 2       | **Theming** — `createTheme`, `ThemeProvider`, custom variables, nesting                                   |
| 3       | **Palette & Colors** — Tokens (main/light/dark), default colors, custom palettes, `augmentColor`          |
| 4       | **Typography** — Default variants, customization, custom variants, responsive font sizes                  |
| 5       | **Responsive Design** — Breakpoints, Helpers, Responsive `sx`, `useMediaQuery`                            |
| 6       | **The `sx` Prop** — Shorthands, responsive values, callbacks, array values, nesting                       |
| 7       | **Dark Mode** — `colorSchemes`, `useColorScheme`, `applyStyles`, SSR flicker prevention                   |
| 8       | **CSS Theme Variables** — Advantages, activation (`cssVariables: true`), usage                            |
| 9       | **Grid System (Grid v2)** — Unified Grid, size prop, responsive, auto-layout, offsets                     |
| 10      | **Layout Components** — `Box`, `Stack`, `Container`                                                       |
| 11      | **Component Customization** — `defaultProps`, `styleOverrides`, custom variants                           |
| 12      | **Common Components Reference** — Quick reference for Inputs, Data Display, Surfaces, Feedback components |
| 13      | **The `styled()` API** — Basic usage, prop forwarding, dark mode in styled                                |
| 14      | **Spacing System** — Factor-based spacing, custom spacing functions                                       |
| 15      | **Transitions & Animations** — Theme transitions, Transition components (Fade, Grow, etc.)                |
| 16      | **Joy UI Quick Reference** — Differences from Material UI, CSS variables, variants                        |
| 17      | **Import Best Practices** — Named vs. Barrel imports, Tree-shaking                                        |
| 18      | **Migration Pitfalls (v5 → v6)** — Critical "DO NOT / USE THIS" table                                     |
| 19      | **Design Best Practices** — Mobile-first, semantic HTML, specific customization tips                      |

## 🛠 How to Use

### Antigravity (Google)

Place the `SKILL.md` inside a skills directory in your project:

```
your-project/
├── .agents/
│   └── skills/
│       └── mui/
│           └── SKILL.md   ← Place here
```

Antigravity automatically discovers skills from `.agents/skills/` directories.

### Cursor

Add the file as a project rule or doc:

```
your-project/
├── .cursor/
│   └── rules/
│       └── mui.mdc   ← Paste SKILL.md content here
```

### VS Code + GitHub Copilot

Copy `SKILL.md` content into your project's Copilot instructions file:

```
your-project/
├── .github/
│   └── copilot-instructions.md   ← Paste SKILL.md content here
```

### Claude Code

Place the file as a project-level instruction:

```
your-project/
├── CLAUDE.md   ← Paste SKILL.md content here
```

## 🆕 v6 Highlights (2025–2026)

- **Unified Grid v2**: Replaces the old `Grid` API with a simpler, more robust layout system.
- **CSS Theme Variables**: Native CSS variables for all theme tokens.
- **Improved Color Schemes**: Simplified light/dark mode configuration via `colorSchemes`.
- **Zero-Flicker Dark Mode**: Using `InitColorSchemeScript` for perfect SSR support.
- **Loading Button Graduation**: Moved from Lab to Core.
- **React 19 Support**: Full compatibility with the latest React version.

## 📄 License

MIT
