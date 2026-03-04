---
name: MUI (Material UI & Joy UI)
description: Comprehensive MUI v7 reference for generating modern React UIs. Covers installation, theming (CSS variables, colorSchemes), advanced CSS (Gradients, 3D Transforms, Logical Properties), the sx prop, Grid (unified), component customization (standardized slots), CSS layers, and the MUI System deep dive.
---

# MUI v7 — LLM Reference Guide

> **Current Version**: Material UI 7.x (2025–2026)
> MUI v7 builds on v6 with improved ESM support, standardized slot pattern (`slots`/`slotProps`), CSS layers via `enableCssLayer`, Grid2→Grid rename, and removed v5 deprecated APIs. If your prior knowledge is based on v5 or v6, **read the "Migration Pitfalls" section** before generating any code.

---

## 1. Installation

### Material UI (Default — Emotion)

```bash
# npm
npm install @mui/material @emotion/react @emotion/styled

# pnpm
pnpm add @mui/material @emotion/react @emotion/styled

# yarn
yarn add @mui/material @emotion/react @emotion/styled
```

### With styled-components (Alternative)

```bash
npm install @mui/material @mui/styled-engine-sc styled-components
```

> **Note**: Emotion is strongly recommended for SSR projects. `styled-components` has compatibility issues with server-rendered MUI apps.

### Joy UI

```bash
# npm
npm install @mui/joy @emotion/react @emotion/styled

# pnpm
pnpm add @mui/joy @emotion/react @emotion/styled
```

> **Warning**: Joy UI is in **beta** and development is currently **on hold**. For new projects, Material UI is recommended.

### Peer Dependencies

```json
{
  "peerDependencies": {
    "react": "^17.0.0 || ^18.0.0 || ^19.0.0",
    "react-dom": "^17.0.0 || ^18.0.0 || ^19.0.0"
  }
}
```

> **Important (React 18 and below)**: MUI v7 uses `react-is@19`. If you're on React 18 or below, you must install a matching `react-is` version and set `overrides` (npm) or `resolutions` (yarn) in `package.json` to prevent runtime errors:
>
> ```bash
> npm install react-is@18.3.1
> ```
>
> ```json
> { "overrides": { "react-is": "^18.3.1" } }
> ```

### Roboto Font (Material UI default)

```bash
npm install @fontsource/roboto
```

```js
// Entry point (e.g., index.js or App.js)
import "@fontsource/roboto/300.css";
import "@fontsource/roboto/400.css";
import "@fontsource/roboto/500.css";
import "@fontsource/roboto/700.css";
```

Or via Google Fonts CDN:

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap"
/>
```

### Material Icons

```bash
npm install @mui/icons-material
```

Or via CDN:

```html
<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/icon?family=Material+Icons"
/>
```

### CssBaseline (Recommended)

Always include `CssBaseline` to normalize browser styles:

```jsx
import CssBaseline from "@mui/material/CssBaseline";

function App() {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      {/* Your app */}
    </ThemeProvider>
  );
}
```

---

## 2. Theming (`createTheme` & `ThemeProvider`)

Material UI uses a JavaScript theme object for all design tokens. Create a theme with `createTheme()` and inject it via `ThemeProvider`.

```jsx
import { createTheme, ThemeProvider } from "@mui/material/styles";

const theme = createTheme({
  palette: {
    primary: { main: "#1976d2" },
    secondary: { main: "#9c27b0" },
  },
  typography: {
    fontFamily: '"Inter", "Roboto", "Helvetica", sans-serif',
  },
  shape: {
    borderRadius: 8,
  },
});

function App() {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      {/* App content */}
    </ThemeProvider>
  );
}
```

### Theme Configuration Variables

| Key            | Purpose                                  |
| -------------- | ---------------------------------------- |
| `palette`      | Colors (primary, secondary, error, etc.) |
| `typography`   | Font family, sizes, weights, variants    |
| `spacing`      | Base spacing unit (default: 8px)         |
| `breakpoints`  | Responsive breakpoint values             |
| `shape`        | Border radius                            |
| `zIndex`       | Z-index values for layered components    |
| `transitions`  | Duration, easing for animations          |
| `components`   | Component-level default props & styles   |
| `colorSchemes` | Light/dark mode color definitions (v6+)  |

### Accessing the Theme

```jsx
import { useTheme } from "@mui/material/styles";

function MyComponent() {
  const theme = useTheme();
  return <span>{theme.spacing(2)}</span>; // "16px"
}
```

### Nesting Themes

```jsx
<ThemeProvider theme={outerTheme}>
  <Button>Outer theme</Button>
  <ThemeProvider theme={innerTheme}>
    <Button>Inner theme overrides outer</Button>
  </ThemeProvider>
</ThemeProvider>
```

You can also extend the outer theme with a function:

```jsx
<ThemeProvider theme={(outerTheme) => createTheme({ ...outerTheme, /* overrides */ })}>
```

### Custom Theme Variables

```js
const theme = createTheme({
  status: {
    danger: '#e53e3e',
  },
});

// TypeScript: Augment the theme
declare module '@mui/material/styles' {
  interface Theme {
    status: { danger: string };
  }
  interface ThemeOptions {
    status?: { danger?: string };
  }
}
```

---

## 3. Palette & Colors

### Color Tokens

Each palette color has four tokens:

| Token          | Description                          |
| -------------- | ------------------------------------ |
| `main`         | The main shade (required)            |
| `light`        | Lighter shade (auto-calculated)      |
| `dark`         | Darker shade (auto-calculated)       |
| `contrastText` | Text color contrasting `main` (auto) |

### Default Palette Colors

| Key         | Purpose                      | Default Main |
| ----------- | ---------------------------- | ------------ |
| `primary`   | Primary interface elements   | `#1976d2`    |
| `secondary` | Secondary interface elements | `#9c27b0`    |
| `error`     | Error states                 | `#d32f2f`    |
| `warning`   | Warning states               | `#ed6c02`    |
| `info`      | Informational elements       | `#0288d1`    |
| `success`   | Success states               | `#2e7d32`    |

### Custom Palette

```js
const theme = createTheme({
  palette: {
    primary: {
      main: "#FF5733",
      // light, dark, contrastText auto-calculated
    },
    secondary: {
      main: "#E0C2FF",
      light: "#F5EBFF",
      contrastText: "#47008F",
    },
  },
});
```

### Using MUI Color Objects

```js
import { createTheme } from "@mui/material/styles";
import { lime, purple } from "@mui/material/colors";

const theme = createTheme({
  palette: {
    primary: lime,
    secondary: purple,
  },
});
```

### Adding Custom Colors

```js
const theme = createTheme({
  palette: {
    ochre: {
      main: '#E3D026',
      light: '#E9DB5D',
      dark: '#A29415',
      contrastText: '#242105',
    },
  },
});

// TypeScript: Augment palette
declare module '@mui/material/styles' {
  interface Palette {
    ochre: Palette['primary'];
  }
  interface PaletteOptions {
    ochre?: PaletteOptions['primary'];
  }
}

// Augment component props
declare module '@mui/material/Button' {
  interface ButtonPropsColorOverrides {
    ochre: true;
  }
}
```

### Using `augmentColor`

```js
const theme = createTheme({
  palette: {
    ochre: theme.palette.augmentColor({
      color: { main: "#E3D026" },
      name: "ochre",
    }),
  },
});
```

---

## 4. Typography

### Default Variants

MUI provides 13 typography variants:

| Variant     | Default Element | Typical Use    |
| ----------- | --------------- | -------------- |
| `h1`        | `<h1>`          | Page title     |
| `h2`        | `<h2>`          | Section title  |
| `h3`        | `<h3>`          | Subsection     |
| `h4`        | `<h4>`          | Card title     |
| `h5`        | `<h5>`          | Sub-heading    |
| `h6`        | `<h6>`          | Small heading  |
| `subtitle1` | `<h6>`          | Subtitle       |
| `subtitle2` | `<h6>`          | Small subtitle |
| `body1`     | `<p>`           | Body text      |
| `body2`     | `<p>`           | Secondary text |
| `button`    | `<span>`        | Button label   |
| `caption`   | `<span>`        | Small caption  |
| `overline`  | `<span>`        | Overline text  |

### Usage

```jsx
import Typography from '@mui/material/Typography';

<Typography variant="h4" component="h1" gutterBottom>
  Page Title
</Typography>
<Typography variant="body1">
  Body content text here.
</Typography>
```

### Customizing Typography

```js
const theme = createTheme({
  typography: {
    fontFamily: '"Inter", "Roboto", sans-serif',
    h1: {
      fontSize: "2.5rem",
      fontWeight: 700,
    },
    body1: {
      fontSize: "1rem",
      lineHeight: 1.6,
    },
    button: {
      textTransform: "none", // Disable uppercase buttons
    },
  },
});
```

### Adding Custom Variants

```js
const theme = createTheme({
  typography: {
    poster: {
      fontSize: '4rem',
      fontWeight: 800,
      color: 'red',
    },
  },
  components: {
    MuiTypography: {
      defaultProps: {
        variantMapping: {
          poster: 'h1', // Maps <Typography variant="poster"> to <h1>
        },
      },
    },
  },
});

// TypeScript augmentation
declare module '@mui/material/styles' {
  interface TypographyVariants {
    poster: React.CSSProperties;
  }
  interface TypographyVariantsOptions {
    poster?: React.CSSProperties;
  }
}
declare module '@mui/material/Typography' {
  interface TypographyPropsVariantOverrides {
    poster: true;
  }
}
```

### Responsive Font Sizes

```js
import { createTheme, responsiveFontSizes } from "@mui/material/styles";

let theme = createTheme();
theme = responsiveFontSizes(theme);
// h1–h6 now scale down automatically on smaller screens
```

---

## 5. Responsive Design

### Default Breakpoints

| Key  | Min-width | Description |
| ---- | --------- | ----------- |
| `xs` | 0px       | Extra-small |
| `sm` | 600px     | Small       |
| `md` | 900px     | Medium      |
| `lg` | 1200px    | Large       |
| `xl` | 1536px    | Extra-large |

### Breakpoint Helpers (in `styled` / `sx`)

```js
// In styled or sx:
{
  [theme.breakpoints.up('md')]: {
    fontSize: '1.2rem',
  },
  [theme.breakpoints.down('sm')]: {
    padding: theme.spacing(1),
  },
  [theme.breakpoints.between('sm', 'lg')]: {
    maxWidth: 600,
  },
  [theme.breakpoints.only('md')]: {
    backgroundColor: '#f5f5f5',
  },
}
```

### Responsive `sx` Prop (Object Syntax)

```jsx
<Box
  sx={{
    width: {
      xs: "100%", // 0px+
      sm: "80%", // 600px+
      md: "60%", // 900px+
      lg: "50%", // 1200px+
    },
  }}
/>
```

### `useMediaQuery` Hook

```jsx
import useMediaQuery from "@mui/material/useMediaQuery";
import { useTheme } from "@mui/material/styles";

function MyComponent() {
  const theme = useTheme();
  const isMobile = useMediaQuery(theme.breakpoints.down("sm"));
  const isDesktop = useMediaQuery(theme.breakpoints.up("lg"));

  return isMobile ? <MobileView /> : <DesktopView />;
}
```

### Custom Breakpoints

```js
const theme = createTheme({
  breakpoints: {
    values: {
      xs: 0,
      sm: 600,
      md: 900,
      lg: 1200,
      xl: 1536,
      xxl: 1920, // Custom
    },
  },
});
```

---

## 6. The `sx` Prop

The `sx` prop is the primary way to add one-off custom styles to MUI components. It is theme-aware, supports responsive values, and supports pseudo-selectors.

### Basic Usage

```jsx
<Box
  sx={{
    bgcolor: "primary.main",
    color: "primary.contrastText",
    p: 2, // padding: theme.spacing(2) = 16px
    m: 1, // margin: 8px
    borderRadius: 1, // theme.shape.borderRadius * 1
    boxShadow: 3, // theme.shadows[3]
    fontSize: "h6.fontSize",
  }}
>
  Styled Box
</Box>
```

### Shorthand Properties

| `sx` Key                                  | CSS Property       | Theme Mapping      |
| ----------------------------------------- | ------------------ | ------------------ |
| `m`, `mx`, `my`, `mt`, `mr`, `mb`, `ml`   | `margin-*`         | `theme.spacing()`  |
| `p`, `px`, `py`, `pt`, `pr`, `pb`, `pl`   | `padding-*`        | `theme.spacing()`  |
| `bgcolor`                                 | `background-color` | `theme.palette`    |
| `color`                                   | `color`            | `theme.palette`    |
| `boxShadow`                               | `box-shadow`       | `theme.shadows`    |
| `borderRadius`                            | `border-radius`    | `theme.shape`      |
| `typography`                              | font styles        | `theme.typography` |
| `zIndex`                                  | `z-index`          | `theme.zIndex`     |
| `gap`                                     | `gap`              | `theme.spacing()`  |
| `display`                                 | `display`          | —                  |
| `width`, `height`, `minWidth`, `maxWidth` | sizing             | Fraction → `%`     |

### Responsive sx Values

```jsx
<Box
  sx={{
    flexDirection: { xs: "column", md: "row" },
    gap: { xs: 1, sm: 2, md: 3 },
    p: { xs: 1, md: 3 },
  }}
/>
```

### Callback Values

```jsx
<Box
  sx={(theme) => ({
    ...theme.typography.body1,
    color: theme.palette.primary.main,
    border: `1px solid ${theme.palette.divider}`,
  })}
/>
```

> **v6+ Change**: Callback as individual value is deprecated. Use callback at the top level.

### Array Values (Conditional Styles)

```jsx
<Box
  sx={[
    { color: "red", "&:hover": { backgroundColor: "white" } },
    isActive && { "&:hover": { backgroundColor: "grey" } },
    isHighlighted && { "&:hover": { backgroundColor: "yellow" } },
  ]}
/>
```

### Nested Selectors & Pseudo-classes

```jsx
<Box
  sx={{
    "&:hover": { opacity: 0.8 },
    "&:focus-within": { outline: "2px solid blue" },
    "& .MuiButton-root": { textTransform: "none" },
    "& > *:not(:last-child)": { mb: 2 },
  }}
/>
```

---

## 7. Dark Mode

### Simple Dark Theme

```jsx
import { createTheme, ThemeProvider } from "@mui/material/styles";
import CssBaseline from "@mui/material/CssBaseline";

const darkTheme = createTheme({
  palette: {
    mode: "dark",
  },
});

function App() {
  return (
    <ThemeProvider theme={darkTheme}>
      <CssBaseline />
      <main>Dark mode app</main>
    </ThemeProvider>
  );
}
```

### Built-in Light/Dark Support (CSS Variables — v6+ Recommended)

```jsx
const theme = createTheme({
  cssVariables: true,
  colorSchemes: {
    dark: true, // Enables both light and dark
  },
});

function App() {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      {/* Automatically supports light/dark via system preference */}
    </ThemeProvider>
  );
}
```

### Toggling Mode with `useColorScheme`

```jsx
import { useColorScheme } from "@mui/material/styles";

function ModeToggle() {
  const { mode, setMode } = useColorScheme();

  if (!mode) return null; // Prevent hydration mismatch

  return (
    <Button onClick={() => setMode(mode === "dark" ? "light" : "dark")}>
      {mode === "dark" ? "☀️ Light" : "🌙 Dark"}
    </Button>
  );
}
```

### Styling in Dark Mode with `theme.applyStyles()`

```jsx
// Preferred approach (v6/v7)
<Box
  sx={(theme) => ({
    bgcolor: "#fff",
    color: "#000",
    ...theme.applyStyles("dark", {
      bgcolor: "#121212",
      color: "#fff",
    }),
  })}
/>
```

### Preventing SSR Dark Mode Flicker

Use CSS theme variables (`cssVariables: true`) plus `InitColorSchemeScript`:

```jsx
import InitColorSchemeScript from "@mui/material/InitColorSchemeScript";

// In your HTML head (e.g., Next.js _document or layout)
<InitColorSchemeScript />;
```

---

## 8. CSS Theme Variables

Enable CSS custom properties from your MUI theme for better DevTools debugging, no dark-mode flicker, and easier third-party integration.

```js
const theme = createTheme({
  cssVariables: true,
});
```

This generates global CSS like:

```css
:root {
  --mui-palette-primary-main: #1976d2;
  --mui-palette-background-default: #fff;
  --mui-shape-borderRadius: 4px;
  /* ... */
}
```

Components automatically use `var(--mui-palette-primary-main)` instead of raw values.

### Advantages

- Prevents dark-mode SSR flickering
- Unlimited color schemes beyond light/dark
- Better DevTools debugging (see token names, not raw values)
- Automatic sync between browser tabs
- Easier third-party integration

---

## 9. Grid System

MUI v7 uses a unified `Grid` component (CSS Flexbox-based, 12-column system). In v7, the old `Grid` (v1) was renamed to `GridLegacy`, and `Grid2` was promoted to `Grid`.

### Basic Usage

```jsx
import Grid from "@mui/material/Grid";

<Grid container spacing={2}>
  <Grid size={{ xs: 12, md: 8 }}>
    <Item>xs=12 md=8</Item>
  </Grid>
  <Grid size={{ xs: 12, md: 4 }}>
    <Item>xs=12 md=4</Item>
  </Grid>
</Grid>;
```

### Key Props

| Prop        | Description                                             |
| ----------- | ------------------------------------------------------- |
| `container` | Makes the Grid a flex container                         |
| `size`      | Column span (number 1–12, `"auto"`, or `"grow"`)        |
| `spacing`   | Gap between items (number, uses `theme.spacing`)        |
| `direction` | Flex direction (`row`, `column`, etc.)                  |
| `offset`    | Column offset (`{ md: 2 }` offsets by 2 columns at md+) |
| `columns`   | Total number of columns (default: 12)                   |

### Responsive Grid

```jsx
<Grid container spacing={{ xs: 1, md: 3 }}>
  <Grid size={{ xs: 12, sm: 6, md: 4, lg: 3 }}>
    <Card>Responsive card</Card>
  </Grid>
  <Grid size={{ xs: 12, sm: 6, md: 4, lg: 3 }}>
    <Card>Responsive card</Card>
  </Grid>
</Grid>
```

### Auto-layout

```jsx
<Grid container spacing={2}>
  <Grid size="grow">Fills remaining space</Grid>
  <Grid size="auto">Fits content</Grid>
  <Grid size={6}>Fixed 6 columns</Grid>
</Grid>
```

### Nested Grids

```jsx
<Grid container spacing={2}>
  <Grid size={6}>
    <Grid container spacing={1}>
      <Grid size={6}>Nested</Grid>
      <Grid size={6}>Nested</Grid>
    </Grid>
  </Grid>
</Grid>
```

### Grid with Offset

```jsx
<Grid container spacing={2}>
  <Grid size={6} offset={{ md: 3 }}>
    Centered 6-column item
  </Grid>
</Grid>
```

---

## 10. Layout Components

### Box

The fundamental layout primitive. Renders a `<div>` by default, supports all `sx` props.

```jsx
import Box from "@mui/material/Box";

<Box
  sx={{
    display: "flex",
    alignItems: "center",
    gap: 2,
    p: 3,
    bgcolor: "background.paper",
    borderRadius: 2,
    boxShadow: 1,
  }}
>
  <Typography>Content</Typography>
</Box>;
```

### Stack

One-dimensional layout (vertical or horizontal).

```jsx
import Stack from "@mui/material/Stack";

<Stack direction="row" spacing={2} alignItems="center">
  <Button variant="contained">One</Button>
  <Button variant="outlined">Two</Button>
  <Button variant="text">Three</Button>
</Stack>;

{
  /* Responsive direction */
}
<Stack
  direction={{ xs: "column", sm: "row" }}
  spacing={{ xs: 1, sm: 2, md: 4 }}
>
  <Item>Item 1</Item>
  <Item>Item 2</Item>
</Stack>;
```

### Container

Centered, max-width container for page-level layout.

```jsx
import Container from "@mui/material/Container";

<Container maxWidth="lg">
  <Typography>Centered content (max 1200px)</Typography>
</Container>;

{
  /* Fixed width */
}
<Container fixed>
  <Typography>Fixed-width container</Typography>
</Container>;
```

---

## 11. Component Customization

### 1. One-off: `sx` prop

```jsx
<Button sx={{ borderRadius: 28, textTransform: "none" }}>Custom Button</Button>
```

### 2. Reusable: `styled()` API

```jsx
import { styled } from "@mui/material/styles";
import Button from "@mui/material/Button";

const GradientButton = styled(Button)(({ theme }) => ({
  background: "linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)",
  borderRadius: 8,
  color: "white",
  padding: "8px 24px",
  boxShadow: "0 3px 5px 2px rgba(255, 105, 135, .3)",
  "&:hover": {
    background: "linear-gradient(45deg, #FE6B8B 60%, #FF8E53 90%)",
  },
}));

<GradientButton>Gradient</GradientButton>;
```

### 3. Global: Theme `components` key

#### Default Props

```js
const theme = createTheme({
  components: {
    MuiButton: {
      defaultProps: {
        variant: "contained",
        disableElevation: true,
      },
    },
    MuiTextField: {
      defaultProps: {
        variant: "outlined",
        size: "small",
      },
    },
  },
});
```

#### Style Overrides

```js
const theme = createTheme({
  components: {
    MuiButton: {
      styleOverrides: {
        root: {
          borderRadius: 8,
          textTransform: "none",
          fontWeight: 600,
        },
        contained: {
          boxShadow: "none",
          "&:hover": {
            boxShadow: "none",
          },
        },
      },
    },
    MuiCard: {
      styleOverrides: {
        root: {
          borderRadius: 12,
          boxShadow: "0 2px 12px rgba(0,0,0,0.08)",
        },
      },
    },
  },
});
```

#### Custom Variants

```js
const theme = createTheme({
  components: {
    MuiButton: {
      variants: [
        {
          props: { variant: 'dashed' },
          style: {
            border: '2px dashed currentColor',
            borderRadius: 8,
          },
        },
        {
          props: { variant: 'dashed', color: 'secondary' },
          style: {
            border: '2px dashed #9c27b0',
          },
        },
      ],
    },
  },
});

// TypeScript
declare module '@mui/material/Button' {
  interface ButtonPropsVariantOverrides {
    dashed: true;
  }
}
```

---

## 12. Common Components Quick Reference

### Inputs

```jsx
import Button from '@mui/material/Button';
import IconButton from '@mui/material/IconButton';
import TextField from '@mui/material/TextField';
import Checkbox from '@mui/material/Checkbox';
import Radio from '@mui/material/Radio';
import Switch from '@mui/material/Switch';
import Select from '@mui/material/Select';
import Slider from '@mui/material/Slider';
import Autocomplete from '@mui/material/Autocomplete';

{/* Button variants */}
<Button variant="text">Text</Button>
<Button variant="contained">Contained</Button>
<Button variant="outlined">Outlined</Button>

{/* Button colors */}
<Button color="primary">Primary</Button>
<Button color="secondary">Secondary</Button>
<Button color="error">Error</Button>
<Button color="warning">Warning</Button>
<Button color="info">Info</Button>
<Button color="success">Success</Button>

{/* Button sizes */}
<Button size="small">Small</Button>
<Button size="medium">Medium</Button>
<Button size="large">Large</Button>

{/* Button with icon */}
<Button variant="contained" startIcon={<SaveIcon />}>Save</Button>
<Button variant="outlined" endIcon={<SendIcon />}>Send</Button>

{/* Loading button */}
<Button loading>Loading</Button>
<Button loading loadingPosition="start" startIcon={<SaveIcon />}>Save</Button>

{/* TextField */}
<TextField label="Name" variant="outlined" />
<TextField label="Name" variant="filled" />
<TextField label="Name" variant="standard" />
<TextField label="Email" type="email" required error helperText="Invalid email" />
<TextField label="Bio" multiline rows={4} />
```

### Data Display

```jsx
import Typography from '@mui/material/Typography';
import Avatar from '@mui/material/Avatar';
import Chip from '@mui/material/Chip';
import Divider from '@mui/material/Divider';
import List from '@mui/material/List';
import Table from '@mui/material/Table';
import Tooltip from '@mui/material/Tooltip';
import Badge from '@mui/material/Badge';

{/* Typography */}
<Typography variant="h4" gutterBottom>Heading</Typography>
<Typography variant="body1" color="text.secondary">Body text</Typography>

{/* Chip */}
<Chip label="React" color="primary" />
<Chip label="Deletable" onDelete={handleDelete} />
<Chip label="Clickable" onClick={handleClick} variant="outlined" />

{/* Avatar */}
<Avatar src="/photo.jpg" alt="Name" />
<Avatar sx={{ bgcolor: 'primary.main' }}>AB</Avatar>

{/* Badge */}
<Badge badgeContent={4} color="error">
  <MailIcon />
</Badge>
```

### Surfaces

```jsx
import Card from '@mui/material/Card';
import CardContent from '@mui/material/CardContent';
import CardActions from '@mui/material/CardActions';
import Paper from '@mui/material/Paper';
import Accordion from '@mui/material/Accordion';
import AppBar from '@mui/material/AppBar';

{/* Card */}
<Card elevation={3}>
  <CardContent>
    <Typography variant="h5">Title</Typography>
    <Typography variant="body2" color="text.secondary">
      Description text
    </Typography>
  </CardContent>
  <CardActions>
    <Button size="small">Learn More</Button>
  </CardActions>
</Card>

{/* Paper */}
<Paper elevation={2} sx={{ p: 2 }}>Content on paper</Paper>
<Paper variant="outlined" sx={{ p: 2 }}>Outlined paper</Paper>
```

### Navigation

```jsx
import AppBar from "@mui/material/AppBar";
import Toolbar from "@mui/material/Toolbar";
import Drawer from "@mui/material/Drawer";
import Tabs from "@mui/material/Tabs";
import Tab from "@mui/material/Tab";
import BottomNavigation from "@mui/material/BottomNavigation";
import Breadcrumbs from "@mui/material/Breadcrumbs";
import Link from "@mui/material/Link";

{
  /* AppBar */
}
<AppBar position="static">
  <Toolbar>
    <Typography variant="h6" sx={{ flexGrow: 1 }}>
      My App
    </Typography>
    <Button color="inherit">Login</Button>
  </Toolbar>
</AppBar>;

{
  /* Tabs */
}
<Tabs value={value} onChange={handleChange}>
  <Tab label="Tab 1" />
  <Tab label="Tab 2" />
  <Tab label="Tab 3" />
</Tabs>;
```

### Feedback

```jsx
import Dialog from '@mui/material/Dialog';
import Snackbar from '@mui/material/Snackbar';
import Alert from '@mui/material/Alert';
import CircularProgress from '@mui/material/CircularProgress';
import LinearProgress from '@mui/material/LinearProgress';
import Skeleton from '@mui/material/Skeleton';

{/* Alert */}
<Alert severity="error">This is an error alert</Alert>
<Alert severity="warning">This is a warning alert</Alert>
<Alert severity="info">This is an info alert</Alert>
<Alert severity="success">This is a success alert</Alert>
<Alert variant="filled" severity="success">Filled success</Alert>
<Alert variant="outlined" severity="info">Outlined info</Alert>

{/* Progress */}
<CircularProgress />
<CircularProgress variant="determinate" value={75} />
<LinearProgress />
<LinearProgress variant="determinate" value={50} />

{/* Skeleton */}
<Skeleton variant="text" width={210} />
<Skeleton variant="circular" width={40} height={40} />
<Skeleton variant="rectangular" width={210} height={118} />
<Skeleton variant="rounded" width={210} height={60} />
```

---

## 13. The `styled()` API

### Basic Usage

```jsx
import { styled } from "@mui/material/styles";

const StyledDiv = styled("div")(({ theme }) => ({
  backgroundColor: theme.palette.background.paper,
  padding: theme.spacing(2),
  borderRadius: theme.shape.borderRadius,
  color: theme.palette.text.primary,
}));
```

### Styling MUI Components

```jsx
import { styled } from "@mui/material/styles";
import Card from "@mui/material/Card";

const GlassCard = styled(Card)(({ theme }) => ({
  background: "rgba(255, 255, 255, 0.1)",
  backdropFilter: "blur(10px)",
  border: "1px solid rgba(255, 255, 255, 0.2)",
  borderRadius: 16,
  boxShadow: "0 8px 32px rgba(0, 0, 0, 0.1)",
}));
```

### Dynamic Styles with Props

```jsx
const FlexBox = styled(Box, {
  shouldForwardProp: (prop) => prop !== "gap" && prop !== "direction",
})(({ theme, gap = 2, direction = "row" }) => ({
  display: "flex",
  flexDirection: direction,
  gap: theme.spacing(gap),
  alignItems: "center",
}));

<FlexBox gap={3} direction="column">
  <Typography>Item</Typography>
</FlexBox>;
```

### Dark Mode in `styled`

```jsx
const StyledCard = styled(Card)(({ theme }) => ({
  backgroundColor: "#fff",
  ...theme.applyStyles("dark", {
    backgroundColor: "#1a1a2e",
  }),
}));
```

---

## 14. Spacing System

MUI uses a **factor-based** spacing system. The default factor is **8px**.

```jsx
theme.spacing(1)  // '8px'
theme.spacing(2)  // '16px'
theme.spacing(0.5) // '4px'
theme.spacing(3)  // '24px'

// In sx prop
<Box sx={{ p: 2, m: 1, gap: 3 }} />
// p: 16px, m: 8px, gap: 24px

// Custom spacing
const theme = createTheme({
  spacing: 4, // factor = 4px
});
// theme.spacing(2) = '8px'

// Custom function
const theme = createTheme({
  spacing: (factor) => `${0.25 * factor}rem`,
});
// theme.spacing(2) = '0.5rem'
```

---

## 15. Transitions & Animations

### Theme Transitions

```jsx
<Box
  sx={{
    transition: (theme) =>
      theme.transitions.create(["background-color", "transform"], {
        duration: theme.transitions.duration.standard, // 300ms
        easing: theme.transitions.easing.easeInOut,
      }),
    "&:hover": {
      backgroundColor: "primary.dark",
      transform: "scale(1.02)",
    },
  }}
/>
```

### Transition Components

```jsx
import Fade from '@mui/material/Fade';
import Grow from '@mui/material/Grow';
import Slide from '@mui/material/Slide';
import Collapse from '@mui/material/Collapse';
import Zoom from '@mui/material/Zoom';

<Fade in={show} timeout={500}>
  <Box>Fades in</Box>
</Fade>

<Grow in={show}>
  <Box>Grows in</Box>
</Grow>

<Slide direction="up" in={show}>
  <Box>Slides up</Box>
</Slide>

<Collapse in={show}>
  <Box>Collapses/Expands</Box>
</Collapse>

<Zoom in={show}>
  <Box>Zooms in</Box>
</Zoom>
```

---

## 16. Joy UI Quick Reference

> **Status**: Joy UI is in **beta** and development is **on hold**. Use Material UI for new projects.

Joy UI is MUI's alternative component library with its own design system ("Joy Design") — a cleaner, more modern aesthetic that doesn't follow Material Design.

### Installation

```bash
npm install @mui/joy @emotion/react @emotion/styled
```

### Key Differences from Material UI

| Feature        | Material UI           | Joy UI                   |
| -------------- | --------------------- | ------------------------ |
| Design System  | Material Design 2     | Joy Design (custom)      |
| Theme Provider | `ThemeProvider`       | `CssVarsProvider`        |
| Create Theme   | `createTheme()`       | `extendTheme()`          |
| CSS Variables  | Opt-in                | Built-in by default      |
| Color System   | `palette.mode`        | CSS variables w/ modes   |
| Import Path    | `@mui/material/*`     | `@mui/joy/*`             |
| Customization  | `sx`, `styled`, theme | `sx`, `styled`, CSS vars |

### Basic Usage

```jsx
import { CssVarsProvider, extendTheme } from "@mui/joy/styles";
import CssBaseline from "@mui/joy/CssBaseline";
import Button from "@mui/joy/Button";
import Sheet from "@mui/joy/Sheet";
import Typography from "@mui/joy/Typography";

const theme = extendTheme({
  colorSchemes: {
    light: {
      palette: {
        primary: {
          50: "#e3f2fd",
          500: "#2196f3",
          900: "#0d47a1",
        },
      },
    },
  },
});

function App() {
  return (
    <CssVarsProvider theme={theme}>
      <CssBaseline />
      <Sheet variant="outlined" sx={{ p: 3, borderRadius: "md" }}>
        <Typography level="h4">Hello Joy UI</Typography>
        <Button variant="solid" color="primary">
          Click me
        </Button>
      </Sheet>
    </CssVarsProvider>
  );
}
```

### Joy UI Component Variants

Joy UI components support four variants:

| Variant    | Description             |
| ---------- | ----------------------- |
| `solid`    | Filled background       |
| `soft`     | Light tinted background |
| `outlined` | Border only             |
| `plain`    | No background or border |

```jsx
<Button variant="solid">Solid</Button>
<Button variant="soft">Soft</Button>
<Button variant="outlined">Outlined</Button>
<Button variant="plain">Plain</Button>
```

### Joy UI CSS Variables

Joy UI exposes CSS variables on every component for easy customization:

```jsx
<List
  sx={{
    "--ListItem-radius": "8px",
    "--ListItem-minHeight": "40px",
    "--List-gap": "4px",
    "--List-padding": "8px",
  }}
>
  <ListItem>Item 1</ListItem>
  <ListItem>Item 2</ListItem>
</List>
```

---

## 17. Import Best Practices

### Named Imports (Tree-shakeable)

```jsx
// ✅ Recommended — tree-shakeable, one import per component
import Button from "@mui/material/Button";
import TextField from "@mui/material/TextField";
import { createTheme, ThemeProvider } from "@mui/material/styles";
```

> **v7 Breaking Change**: Deep imports with more than one level are no longer supported.
>
> ```jsx
> // ❌ No longer works in v7:
> import createTheme from "@mui/material/styles/createTheme";
> // ✅ Use named import instead:
> import { createTheme } from "@mui/material/styles";
> ```

### Barrel Imports (Convenient but larger bundles)

```jsx
// ⚠️ Works but may increase bundle size without tree-shaking
import { Button, TextField, Box } from "@mui/material";
```

### Icons

```jsx
// ✅ Individual imports (recommended)
import DeleteIcon from "@mui/icons-material/Delete";
import SearchIcon from "@mui/icons-material/Search";

// ⚠️ Barrel import (larger bundle)
import { Delete, Search } from "@mui/icons-material";
```

---

## 18. CSS Layers (v7)

MUI v7 supports opt-in CSS layers via the `enableCssLayer` prop. This wraps MUI styles in a `@layer mui` rule, improving integration with utility-first frameworks like Tailwind CSS v4 and giving you full control over style precedence.

### Client-side Apps (Vite, CRA)

```jsx
import { StyledEngineProvider } from "@mui/material/styles";

function App() {
  return (
    <StyledEngineProvider enableCssLayer>
      <ThemeProvider theme={theme}>
        <CssBaseline />
        {/* Your app */}
      </ThemeProvider>
    </StyledEngineProvider>
  );
}
```

### Next.js App Router

```jsx
import { AppRouterCacheProvider } from "@mui/material-nextjs/v15-appRouter";

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <AppRouterCacheProvider options={{ enableCssLayer: true }}>
          {children}
        </AppRouterCacheProvider>
      </body>
    </html>
  );
}
```

### Benefits

- Avoids specificity conflicts with Tailwind CSS v4 and other utility frameworks
- Clear style ordering in browser DevTools
- Your custom CSS can easily override MUI styles without `!important`

---

## 19. Standardized Slot Pattern (v7)

MUI v7 standardized the `slots` / `slotProps` API across **all** components. The older `components` / `componentsProps` props are removed.

### Usage

```jsx
// ✅ v7: Use slots and slotProps
<TextField
  slots={{ input: CustomInput }}
  slotProps={{
    input: { className: 'custom-input' },
    htmlInput: { 'aria-label': 'Name' },
  }}
/>

<Autocomplete
  slots={{ paper: CustomPaper, popper: CustomPopper }}
  slotProps={{
    paper: { elevation: 8 },
    listbox: { sx: { maxHeight: 300 } },
  }}
/>

// ❌ v5/v6 deprecated (removed in v7):
// <TextField InputProps={{ ... }} inputProps={{ ... }} />
// <Autocomplete PaperComponent={...} PopperComponent={...} />
```

### Key Points

- `slots` lets you replace internal component elements with custom components
- `slotProps` passes props to each slot's rendered element
- Consistent API across all MUI components

---

## 20. Migration Pitfalls (v5/v6 → v7)

> **CRITICAL**: LLMs trained on older data will suggest deprecated v5/v6 patterns. Always use the v7 patterns below.

| ❌ v5/v6 (DO NOT USE)                                   | ✅ v7 (USE THIS)                                                   |
| ------------------------------------------------------- | ------------------------------------------------------------------ |
| `<Grid item xs={6}>`                                    | `<Grid size={{ xs: 6 }}>` (unified Grid)                           |
| `<Grid item xs={12} sm={6}>`                            | `<Grid size={{ xs: 12, sm: 6 }}>`                                  |
| `<Grid container item>`                                 | `<Grid container>` (all Grids are items by default)                |
| `import Grid from '@mui/material/Grid2'`                | `import Grid from '@mui/material/Grid'` (Grid2 is now Grid)        |
| `palette.mode: 'dark'` only                             | `colorSchemes: { dark: true }` + `cssVariables: true`              |
| `sx={{ color: (theme) => theme.palette.primary.main }}` | `sx={(theme) => ({ color: theme.palette.primary.main })}`          |
| `theme.palette.mode === 'dark' ? ... : ...`             | `theme.applyStyles('dark', { ... })` or `theme.vars.*`             |
| Deep imports: `@mui/material/styles/createTheme`        | `import { createTheme } from '@mui/material/styles'`               |
| `InputProps`, `inputProps`, `PaperComponent`, etc.      | `slots` / `slotProps` (standardized in v7)                         |
| `<InputLabel size="normal">`                            | `<InputLabel size="medium">` (standardized in v7)                  |
| `@mui/styles` (JSS) / `makeStyles()` / `withStyles()`   | `styled()` or `sx` prop (Emotion)                                  |
| `<Hidden xsDown>` / `<Hidden mdUp>`                     | `useMediaQuery()` or responsive `sx={{ display: { xs: 'none' } }}` |
| `onBackdropClick` on Dialog/Modal                       | Use `onClose` callback with `reason === 'backdropClick'`           |
| `createMuiTheme()`                                      | `createTheme()`                                                    |
| `experimentalStyled`                                    | `styled`                                                           |
| Components from `@mui/lab` (graduated)                  | Import from `@mui/material` (Alert, Skeleton, Rating, etc.)        |
| No `shouldForwardProp` in styled                        | Use `shouldForwardProp` to prevent custom props on DOM             |

### What Still Works

- All component names (`Button`, `Card`, `TextField`, etc.) remain the same.
- The `sx` prop syntax is largely the same.
- `createTheme()` and `ThemeProvider` are the same.
- The `styled()` API from `@mui/material/styles` is the same.
- Color palette keys (`primary`, `secondary`, `error`, etc.) are unchanged.
- CSS variables, `colorSchemes`, `theme.applyStyles()` all carry over from v6.

### v7 Theme Behavior Change

With CSS variables enabled + light/dark color schemes, the theme object **no longer re-renders** when toggling dark mode. Use `theme.vars.*` to reference CSS variables directly in styles:

```jsx
const Custom = styled("div")(({ theme }) => ({
  color: theme.vars.palette.text.primary,
  background: theme.vars.palette.primary.main,
}));
```

For runtime calculations, use `color-mix()` instead of JS:

```jsx
color: `color-mix(in srgb, ${theme.vars.palette.text.primary}, transparent 50%)`;
```

If you need the old re-render behavior:

```jsx
<ThemeProvider forceThemeRerender />
```

---

## 21. Design Best Practices

1. **Always use `CssBaseline`** — Normalizes browser defaults and applies theme background/text colors.

2. **Prefer `sx` for one-off styles** — Use the `sx` prop instead of inline `style`. It's theme-aware and supports responsive values.

3. **Use `styled()` for reusable styled components** — Create shared styled components for consistency.

4. **Use theme `components` for global overrides** — Set default props and styles at the theme level to maintain consistency across your app.

5. **Responsive from the start** — Use responsive `sx` objects or the `Grid` component with breakpoint-based sizes.

6. **Enable CSS variables for dark mode** — Set `cssVariables: true` and `colorSchemes: { dark: true }` to avoid SSR flicker and enable seamless toggling.

7. **Use `theme.applyStyles('dark', {...})`** — Instead of checking `theme.palette.mode` for dark mode styles.

8. **Import components individually** — Use `import Button from '@mui/material/Button'` instead of barrel imports for smaller bundles.

9. **Leverage MUI `colors`** — Use pre-built color palettes from `@mui/material/colors` for quick prototyping.

10. **Use semantic Typography** — Map variants to proper HTML elements with the `component` prop for better accessibility.

11. **Don't override with `!important`** — Use the theme's `styleOverrides` or `sx` array syntax for specificity instead.

12. **Use `Stack` for 1D layout, `Grid` for 2D** — Stack for rows/columns of items, Grid for complex dashboard-like layouts.

13. **Use `slots`/`slotProps` for component customization** — The standardized v7 API replacing the old `InputProps`, `componentsProps`, etc.

14. **Use `theme.vars.*` for CSS variable references** — When CSS variables are enabled, prefer `theme.vars.palette.primary.main` over `theme.palette.primary.main` for better dark mode performance.

15. **Enable CSS layers for Tailwind integration** — Use `enableCssLayer` on `StyledEngineProvider` to avoid specificity conflicts.

---

## 22. Gradients (Linear, Radial, Conic)

MUI supports standard CSS gradients in both the `sx` prop and the `styled()` API.

### Linear Gradients

```jsx
<Box
  sx={{
    background: "linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)",
    height: 100,
    borderRadius: 1,
  }}
/>
```

### Radial & Conic Gradients

```jsx
<Box
  sx={{
    background: 'radial-gradient(circle, rgba(63,94,251,1) 0%, rgba(252,70,107,1) 100%)',
    height: 150,
  }}
/>

<Box
  sx={{
    background: 'conic-gradient(from 0deg, red, yellow, green, blue, red)',
    width: 200,
    height: 200,
    borderRadius: '50%',
  }}
/>
```

### Theme-Aware Gradients (using palette variables)

If `cssVariables: true` is enabled:

```jsx
<Box
  sx={{
    background: (theme) =>
      `linear-gradient(to right, ${theme.vars.palette.primary.main}, ${theme.vars.palette.secondary.main})`,
  }}
/>
```

---

## 23. 3D Transforms

Use 3D transform properties directly within the `sx` prop or `styled()` function.

```jsx
<Box
  sx={{
    perspective: "1000px",
    "&:hover .inner": {
      transform: "rotateY(180deg)",
    },
  }}
>
  <Box
    className="inner"
    sx={{
      width: 200,
      height: 200,
      transition: "transform 0.6s",
      transformStyle: "preserve-3d",
      position: "relative",
      "& > div": {
        position: "absolute",
        width: "100%",
        height: "100%",
        backfaceVisibility: "hidden",
      },
    }}
  >
    <Box sx={{ bgcolor: "primary.main" }}>Front</Box>
    <Box sx={{ bgcolor: "secondary.main", transform: "rotateY(180deg)" }}>
      Back
    </Box>
  </Box>
</Box>
```

| `sx` Property        | Description                                     |
| -------------------- | ----------------------------------------------- |
| `perspective`        | Distance from viewer to z=0 plane               |
| `transformStyle`     | Specify if children are 3D (`preserve-3d`)      |
| `backfaceVisibility` | Show/hide back of element (`hidden`, `visible`) |
| `rotateX`, `rotateY` | 3D rotations                                    |
| `translateZ`         | Z-axis translation                              |

---

## 24. Logical Properties (v6+ Support)

MUI v6 fully supports [CSS Logical Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_logical_properties_and_values) in the `sx` prop. These properties adapt to the writing mode (LTR vs. RTL) automatically.

| Logical Property | Physical Mapping (LTR) | MUI Spacing Support |
| ---------------- | ---------------------- | ------------------- |
| `paddingBlock`   | `padding-top/bottom`   | ✅ (theme.spacing)  |
| `paddingInline`  | `padding-left/right`   | ✅ (theme.spacing)  |
| `marginBlock`    | `margin-top/bottom`    | ✅ (theme.spacing)  |
| `marginInline`   | `margin-left/right`    | ✅ (theme.spacing)  |
| `blockSize`      | `height`               | ✅                  |
| `inlineSize`     | `width`                | ✅                  |
| `insetBlock`     | `top/bottom`           | ✅                  |
| `insetInline`    | `left/right`           | ✅                  |

### Usage Example

```jsx
<Box
  sx={{
    marginBlock: 2, // margin-top & margin-bottom: 16px
    paddingInline: 4, // padding-left & padding-right: 32px
    inlineSize: "100%", // width: 100%
    borderInlineStart: (theme) => `4px solid ${theme.palette.primary.main}`,
  }}
>
  Internationalization-ready container
</Box>
```

---

## 25. Text Shadows & Masks

### Text Shadows

```jsx
<Typography
  variant="h2"
  sx={{
    textShadow: "2px 2px 4px rgba(0,0,0,0.3)",
    fontWeight: "bold",
  }}
>
  Glow Effect
</Typography>
```

### Masks

Use `maskImage` (or `-webkit-mask-image`) to hide parts of an element.

```jsx
<Box
  component="img"
  src="photo.jpg"
  sx={{
    width: 300,
    maskImage: "linear-gradient(to bottom, black 50%, transparent 100%)",
    WebkitMaskImage: "linear-gradient(to bottom, black 50%, transparent 100%)",
  }}
/>
```

---

## 26. Advanced States & Variants (Pseudo-classes)

Beyond `hover` and `focus`, the `sx` prop supports all CSS pseudo-selectors.

```jsx
<Box
  sx={{
    // Select every second child
    "& > *:nth-of-type(2n)": {
      bgcolor: "action.hover",
    },
    // Target everything EXCEPT the first child
    "& > *:not(:first-of-type)": {
      mt: 1,
    },
    // Only child styles
    "&:only-of-type": {
      border: "2px dashed blue",
    },
    // Disabled state (for inputs/buttons)
    "&.Mui-disabled": {
      opacity: 0.5,
      pointerEvents: "none",
    },
  }}
>
  {/* Children */}
</Box>
```

---

## 27. MUI System Deep Dive

For building custom component libraries or low-level layout primitives, use `@mui/system` directly.

### `createBox` & `createStack`

Allows creating custom Box/Stack versions with restricted or expanded system props.

```js
import { createBox } from '@mui/system';

const MyBox = createBox({
  defaultStates: { ... },
  defaultTheme: customTheme,
});
```

### `styled` with custom components

When wrapping non-MUI components to give them system support:

```jsx
import { styled } from "@mui/material/styles";

const MyComponent = ({ className }) => <div className={className}>...</div>;

const StyledMyComponent = styled(MyComponent)({
  display: "flex",
  padding: 16,
});
```

### The `shouldForwardProp` Trick

Prevent custom internal props from reaching the DOM:

```jsx
const MyStyledButton = styled(Button, {
  shouldForwardProp: (prop) => prop !== "isFancy",
})(({ theme, isFancy }) => ({
  color: isFancy ? theme.palette.primary.main : theme.palette.grey[500],
}));
```
