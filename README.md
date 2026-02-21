# design-kit

CSS-only design system implementing the Swiss Rationalist visual language. One import, consistent styling across all internal apps.

## Quick Start

### 1. Add as submodule

```bash
git submodule add <repo-url> lib/design-kit
```

### 2. Serve via Express

```js
app.use('/design-kit', express.static('./lib/design-kit/src'));
```

### 3. Load fonts (index.html)

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Hanken+Grotesk:wght@300;400;500;600;700&display=swap" rel="stylesheet" />
```

### 4. Import in your CSS

```css
@import '/design-kit/index.css';

/* Override accent color per-app (optional) */
:root {
  --accent: #0d7377;
  --accent-dim: #e6f2f2;
}
```

## What's Included

| Layer | File | Purpose |
|---|---|---|
| Tokens | `tokens.css` | Colors, spacing, typography, geometry, motion + dark mode |
| Reset | `reset.css` | Box-sizing, margin reset, body defaults |
| Base | `base.css` | Typography hierarchy, scrollbar, links |
| Layout | `layout.css` | Topbar, shell, sidebar, content, page-header |
| Components | `components/*.css` | Buttons, forms, cards, tables, badges, pills, modals, toggles, panels, toolbar, tabs, spinner, empty-state |
| Headless | `headless/*.css` | Radix Dialog, Radix Select/Dropdown, Radix Tooltip, TanStack Table, dnd-kit |
| Icons | `icons.css` | Lucide React sizing classes |
| Utilities | `utilities.css` | Flex, gap, text, spacing, visibility helpers |

## Dark Mode

Add `data-theme="dark"` to the `<html>` element. All tokens auto-swap.

```js
document.documentElement.setAttribute('data-theme', 'dark');
```

## Token Customization

Override any token in your app's CSS:

```css
:root {
  --accent: #0d7377;       /* Teal accent instead of indigo */
  --accent-dim: #e6f2f2;
  --r1: 4px;               /* Rounder corners if desired */
}
```

## App-Specific Styles

The kit handles the design system. Your app handles domain-specific styling:

- Resource type colors
- Domain-specific badge variants
- Custom grid layouts for your data
- Page-specific component styling

```css
@import '/design-kit/index.css';

/* App domain styles below */
.resource-badge-human   { --badge-bg: #eef; }
.consensus-bar          { /* app-specific */ }
```

## AI Agents

See [COMPONENTS.md](./COMPONENTS.md) for which headless library to use for each UI pattern, icon usage rules, and code examples.


1. Init the project and add the submodules


mkdir my-new-app && cd my-new-app
git init
git submodule add <design-kit-repo-url> lib/design-kit
git submodule add <auth-sso-kit-repo-url> lib/auth-sso-kit
2. Add this to your CLAUDE.md


Design system: lib/design-kit/src/index.css
Follow lib/design-kit/COMPONENTS.md for all UI decisions.
Follow lib/design-kit/templates/ for page layouts.
Auth: lib/auth-sso-kit
3. That's it.

When you tell Claude "build me a user management page," it will:

Read COMPONENTS.md — know to use Radix, TanStack, Lucide
Read the relevant template — know the exact layout
Use the kit's CSS classes — .btn-accent, .table, .toolbar, etc.
Install the right npm packages — lucide-react, @radix-ui/react-dialog, etc.