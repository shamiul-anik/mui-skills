# MUI v7 Skills for LLMs

A comprehensive, AI-optimized reference guide for **MUI v7** (Material UI & Joy UI). This repository contains a `SKILL.md` file designed to help LLMs (Claude, Gemini, ChatGPT, GitHub Copilot) understand and apply the latest MUI features, best practices, and migration patterns.

## 🚀 Why This Exists

LLMs are often trained on older data. When working with **MUI v7**, they frequently suggest deprecated v5 or v6 patterns (like legacy Grid, `InputProps` instead of `slotProps`, or barrel/deep imports). This skill file provides the necessary context for AI models to:

- Use the **unified Grid** system (v2 promoted to default).
- Implement the **Standardized Slot Pattern** (`slots` / `slotProps`) across all components.
- Leverage **CSS Theme Variables** and the `colorSchemes` API for seamless dark mode.
- Enable **CSS Layers** via `enableCssLayer` for perfect Tailwind CSS v4 integration.
- Apply the **`sx` prop** and **`styled()` API** correctly with modern v7 patterns.
- Avoid common **v5/v6 → v7 migration pitfalls**.
- Follow **Import Best Practices** (restricting deep imports).

## 📂 What's Inside `SKILL.md`

The file is organized into **27 sections** covering everything an LLM needs:

| Section | Topic                                                                                                     |
| ------- | --------------------------------------------------------------------------------------------------------- |
| 1       | **Installation** — Material UI, Joy UI, Peer Deps (React 18 `react-is` req), Fonts, Icons, CssBaseline    |
| 2       | **Theming** — `createTheme`, `ThemeProvider`, custom variables, nesting                                   |
| 3       | **Palette & Colors** — Tokens (main/light/dark), default colors, custom palettes, `augmentColor`          |
| 4       | **Typography** — Default variants, customization, custom variants, responsive font sizes                  |
| 5       | **Responsive Design** — Breakpoints, Helpers, Responsive `sx`, `useMediaQuery`                            |
| 6       | **The `sx` Prop** — Shorthands, responsive values, callbacks, array values, nesting                       |
| 7       | **Dark Mode** — `colorSchemes`, `useColorScheme`, `applyStyles`, SSR flicker prevention                   |
| 8       | **CSS Theme Variables** — Advantages, activation (`cssVariables: true`), usage                            |
| 9       | **Grid System** — Unified Grid (replacing Grid v1/Grid2), size prop, responsive, auto-layout              |
| 10      | **Layout Components** — `Box`, `Stack`, `Container`                                                       |
| 11      | **Component Customization** — `defaultProps`, `styleOverrides`, custom variants                           |
| 12      | **Common Components Reference** — Quick reference for Inputs, Data Display, Surfaces, Feedback components |
| 13      | **The `styled()` API** — Basic usage, prop forwarding, dark mode in styled                                |
| 14      | **Spacing System** — Factor-based spacing, custom spacing functions                                       |
| 15      | **Transitions & Animations** — Theme transitions, Transition components (Fade, Grow, etc.)                |
| 16      | **Joy UI Quick Reference** — Differences from Material UI, CSS variables, variants                        |
| 17      | **Import Best Practices** — Named vs. Barrel imports, Deep Import restrictions                            |
| 18      | **CSS Layers (v7)** — `enableCssLayer` for Tailwind CSS v4 and style precedence control                   |
| 19      | **Standardized Slot Pattern (v7)** — `slots` / `slotProps` reference tables and mapping                   |
| 20      | **Migration Pitfalls (v5/v6 → v7)** — Critical migration table, Codemods, and Theme behavior changes      |
| 21      | **Design Best Practices** — Mobile-first, semantic HTML, v7-specific customization tips                   |
| 22      | **Gradients** — Linear, Radial, and Conic gradients in `sx` and `styled`                                  |
| 23      | **3D Transforms** — `perspective`, `rotateX/Y`, `transformStyle` in MUI                                   |
| 24      | **Logical Properties** — v7 support for `paddingBlock`, `marginInline`, etc.                              |
| 25      | **Text Shadows & Masks** — Advanced typography and image masking patterns                                 |
| 26      | **Advanced States & Variants** — Pseudo-selectors (`:nth-of-type`, `:not`, etc.) in `sx`                  |
| 27      | **MUI System Deep Dive** — `createBox`, `createStack`, and custom system components                       |

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

## 🆕 v7 Highlights (2025–2026)

- **Standardized Slot Pattern**: Consistent `slots` and `slotProps` API across all components.
- **Improved ESM Support**: Updated package layout for better integration with Vite, webpack, and Node.js.
- **CSS Layers**: Built-in support for CSS Layers (`@layer mui`) to manage style precedence.
- **Unified Grid**: `Grid2` promoted to the primary `Grid` component.
- **Removal of v5 Deprecations**: Cleaned up API surface by removing long-deprecated functions like `createMuiTheme`.
- **CSS Variable Performance**: Themes with CSS variables no longer trigger unnecessary re-renders on mode toggle.
- **React 19 Readiness**: Optimized for the latest React features and best practices.

## 📄 License

MIT
