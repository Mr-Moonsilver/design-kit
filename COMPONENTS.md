# COMPONENTS.md — AI Agent Guide

> Which library to use for each UI pattern. Which template to follow for each page. Follow this exactly.

---

## Page Templates

Before building any page, check if a template exists. Templates define **where things go** — the arrangement of buttons, search, filters, tables, and panels. Follow the template's structure exactly.

| Page type | Template | Use for |
|---|---|---|
| User management | `templates/admin-users.html` | User lists, team management, access control |
| Settings / config | `templates/admin-settings.html` | App configuration, feature toggles, preferences |
| Data grid (CRUD) | `templates/data-grid.html` | Resource lists, inventory, any filterable table |
| Linking grid | `templates/linking-grid.html` | Matrix views, resource-capability linking, grid with sticky context card |
| List + detail | `templates/list-detail.html` | Qualification views, inspection, select-to-view |
| Dashboard / home | `templates/dashboard.html` | Overview pages, stat summaries, quick actions |
| Login / auth | `templates/login.html` | SSO entry, user selection, authentication |

### How to use templates

1. Read the template HTML for the matching page type
2. Follow the **exact arrangement**: toolbar position, button placement, column order
3. Replace placeholder content with your app's real data and labels
4. Keep the same class names — the kit CSS handles all styling
5. Only add app-specific markup *within* the template's sections, not around them

### Extracting new templates

When you build a page that works well and doesn't match an existing template:

1. Strip the JSX down to its HTML skeleton (remove state, handlers, React syntax)
2. Replace real data with generic placeholders
3. Add a comment block at the top describing the layout pattern
4. Save to `templates/<pattern-name>.html`
5. Add it to this table

---

## Library Map

| Pattern | Library | Kit CSS | Install |
|---|---|---|---|
| Data grids / tables | TanStack Table | `headless/tanstack-table.css` | `@tanstack/react-table` |
| Modals / dialogs | Radix Dialog | `headless/radix-dialog.css` | `@radix-ui/react-dialog` |
| Alert dialogs | Radix AlertDialog | `headless/radix-dialog.css` | `@radix-ui/react-alert-dialog` |
| Dropdowns / select | Radix Select | `headless/radix-dropdown.css` | `@radix-ui/react-select` |
| Context menus | Radix DropdownMenu | `headless/radix-dropdown.css` | `@radix-ui/react-dropdown-menu` |
| Tooltips | Radix Tooltip | `headless/radix-tooltip.css` | `@radix-ui/react-tooltip` |
| Drag and drop | dnd-kit | `headless/dnd-kit.css` | `@dnd-kit/core @dnd-kit/sortable` |
| Icons | Lucide React | `icons.css` | `lucide-react` |
| Simple forms | Plain HTML + kit classes | `components/forms.css` | None |
| Bare inputs | Plain HTML + kit classes | `components/forms.css` | None |
| Selectable buttons | Plain HTML + kit classes | `components/forms.css` | None |
| Matrix grids | Plain HTML + kit classes | `components/matrix-grid.css` | None |
| Cards / lists | Plain HTML + kit classes | `components/cards.css` | None |
| Buttons | Plain HTML + kit classes | `components/buttons.css` | None |
| Badges / chips | Plain HTML + kit classes | `components/badges.css`, `components/chips.css` | None |
| Tabs | Plain HTML + kit classes | `components/tabs.css` | None |
| Toggles | Plain HTML + kit classes | `components/toggles.css` | None |

### Decision rule

Use plain HTML + kit CSS classes for everything unless the pattern requires:
- Focus management or ARIA → use Radix
- Virtualized / sortable data → use TanStack Table
- Drag reordering → use dnd-kit

Do NOT reach for a library when a `<button class="btn">` will do.

---

## Matrix Grid

A CSS Grid with sticky row + column headers and a corner cell. Open-frame border model (no outer border, no radius). Used for linking matrices, qualification tables, and any two-axis data view.

### Structure

```html
<div class="matrix-grid-container">
  <div class="matrix-grid">
    <!-- Row 1: corner + column headers -->
    <div class="matrix-grid-corner"></div>
    <div class="matrix-grid-col">Col A</div>
    <div class="matrix-grid-col">Col B</div>

    <!-- Row 2+: row header + data cells -->
    <div class="matrix-grid-row">Row Label</div>
    <div class="matrix-grid-cell">Content</div>
    <div class="matrix-grid-cell">Content</div>
  </div>
</div>
```

### Classes

| Class | Purpose |
|---|---|
| `.matrix-grid-container` | Scrollable wrapper (`overflow: auto`) |
| `.matrix-grid` | CSS Grid. Sets border-top + border-left; cells close right + bottom |
| `.matrix-grid-corner` | Top-left sticky cell — sticky both axes, z-index 3 |
| `.matrix-grid-col` | Column header — sticky top, z-index 2, `--surface` bg, `--t-13` |
| `.matrix-grid-row` | Row header — sticky left, z-index 1, vertical text, uppercase, tracked |
| `.matrix-grid-cell` | Content cell — flex-column stack by default |
| `.matrix-grid-cell--wrap` | Modifier — flex-row wrap for chips, small cards, tags |

### Column count

Column count defaults to `48px repeat(6, minmax(190px, 1fr))` (corner + 6 columns). Override with the `--matrix-cols` custom property:

```html
<!-- 4-column grid -->
<div class="matrix-grid"
     style="--matrix-cols: 48px repeat(4, minmax(190px, 1fr))">
```

```tsx
// In React — type the style object
<div className="matrix-grid"
     style={{ '--matrix-cols': '48px repeat(4, minmax(190px, 1fr))' } as React.CSSProperties}>
```

### Cell variants

| Variant | Use for |
|---|---|
| `.matrix-grid-cell` (default) | Stacked content — capability cards, ratings, tall items |
| `.matrix-grid-cell--wrap` | Wrapped chips — resource badges, small tags, compact items |

### Sticky behavior

- **Corner** (z-index 3): Stays fixed at top-left during both horizontal and vertical scroll
- **Column headers** (z-index 2): Stick to top on vertical scroll
- **Row headers** (z-index 1): Stick to left on horizontal scroll

### Template

See `templates/linking-grid.html` for a full page layout using the matrix grid with a detail card above it.

---

## Icons — Lucide React

**The only icon library.** Never use Font Awesome, Heroicons, Material Icons, or any other set.

### Install

```bash
npm install lucide-react
```

### Import pattern

```tsx
import { Search, Plus, Trash2, ChevronDown } from 'lucide-react';
```

Tree-shakeable — only imported icons are bundled.

### Sizing classes

| Class | Size | Use for |
|---|---|---|
| `.icon-sm` | 16px | Buttons, nav items, table cells |
| `.icon` / `.icon-md` | 20px | Standalone, toolbar actions |
| `.icon-lg` | 24px | Empty states, page headers |

### Usage

```tsx
// In a button
<button className="btn btn-accent">
  <Plus className="icon-sm" /> Add Item
</button>

// Standalone in toolbar
<Search className="icon" />

// In empty state
<FolderOpen className="icon-lg icon-muted" />

// Inline with text
<Info className="icon-sm icon-inline" /> Note
```

### Rules

1. **Always use a sizing class** — never rely on default SVG dimensions
2. **Stroke width is 1px** — set in `icons.css`, never override
3. **Color inherits** — Lucide icons use `currentColor`, so they follow the parent's `color` property. Use `--ink-1`, `--ink-2`, `--ink-3` via parent class
4. **No wrapper spans** — apply className directly to the Lucide component
5. **Consistent sizes within context** — all icons in a button row should be the same size

### Helpers

| Class | Effect |
|---|---|
| `.icon-inline` | Vertically aligns icon with text baseline |
| `.icon-muted` | 40% opacity (secondary/decorative icons) |
| `.icon-spin` | Rotating animation (loading states) |

---

## Headless Library Patterns

### Radix Dialog

```tsx
import * as Dialog from '@radix-ui/react-dialog';

<Dialog.Root>
  <Dialog.Trigger asChild>
    <button className="btn">Open</button>
  </Dialog.Trigger>
  <Dialog.Portal>
    <Dialog.Overlay />
    <Dialog.Content>
      <div className="modal-header">
        <Dialog.Title>Title</Dialog.Title>
        <Dialog.Close />
      </div>
      <div className="modal-body">Content</div>
      <div className="modal-footer">
        <Dialog.Close asChild>
          <button className="btn">Cancel</button>
        </Dialog.Close>
        <button className="btn btn-accent">Confirm</button>
      </div>
    </Dialog.Content>
  </Dialog.Portal>
</Dialog.Root>
```

### Radix Select

```tsx
import * as Select from '@radix-ui/react-select';

<Select.Root>
  <Select.Trigger>
    <Select.Value placeholder="Choose..." />
    <Select.Icon>
      <ChevronDown className="icon-sm" />
    </Select.Icon>
  </Select.Trigger>
  <Select.Portal>
    <Select.Content>
      <Select.Viewport>
        <Select.Item value="a">
          <Select.ItemIndicator>
            <Check className="icon-sm" />
          </Select.ItemIndicator>
          <Select.ItemText>Option A</Select.ItemText>
        </Select.Item>
      </Select.Viewport>
    </Select.Content>
  </Select.Portal>
</Select.Root>
```

### Radix Tooltip

```tsx
import * as Tooltip from '@radix-ui/react-tooltip';

<Tooltip.Provider delayDuration={300}>
  <Tooltip.Root>
    <Tooltip.Trigger asChild>
      <button className="btn btn-icon">
        <Info className="icon-sm" />
      </button>
    </Tooltip.Trigger>
    <Tooltip.Portal>
      <Tooltip.Content sideOffset={4}>
        Helpful text
        <Tooltip.Arrow />
      </Tooltip.Content>
    </Tooltip.Portal>
  </Tooltip.Root>
</Tooltip.Provider>
```

### TanStack Table

```tsx
import { useReactTable, getCoreRowModel, flexRender } from '@tanstack/react-table';

const table = useReactTable({ data, columns, getCoreRowModel: getCoreRowModel() });

<table className="table">
  <thead>
    {table.getHeaderGroups().map(hg => (
      <tr key={hg.id}>
        {hg.headers.map(h => (
          <th key={h.id} aria-sort={h.column.getIsSorted() || undefined}>
            {flexRender(h.column.columnDef.header, h.getContext())}
          </th>
        ))}
      </tr>
    ))}
  </thead>
  <tbody>
    {table.getRowModel().rows.map(row => (
      <tr key={row.id}>
        {row.getVisibleCells().map(cell => (
          <td key={cell.id}>
            {flexRender(cell.column.columnDef.cell, cell.getContext())}
          </td>
        ))}
      </tr>
    ))}
  </tbody>
</table>
```

### dnd-kit Sortable

```tsx
import { DndContext, closestCenter } from '@dnd-kit/core';
import { SortableContext, useSortable, verticalListSortingStrategy } from '@dnd-kit/sortable';
import { CSS } from '@dnd-kit/utilities';

function SortableItem({ id }) {
  const { attributes, listeners, setNodeRef, transform, transition, isDragging } = useSortable({ id });
  const style = { transform: CSS.Transform.toString(transform), transition };

  return (
    <div ref={setNodeRef} style={style} data-dnd-sortable data-dnd-dragging={isDragging} {...attributes}>
      <span className="dnd-handle" {...listeners}>
        <GripVertical className="icon-sm" />
      </span>
      {/* content */}
    </div>
  );
}
```

---

## Accent Color Configuration

Each app can have its own accent color, configurable from the admin settings page. This lets you distinguish apps visually while keeping the same design system.

### How it works

1. Server stores the accent color (database or settings file)
2. Server injects it as an inline style on `<html>`:
   ```html
   <html data-theme="light" style="--accent: #0d7377; --accent-dim: #e6f2f2;">
   ```
3. The inline `--accent` override cascades through the entire kit — every button, link, badge, and active state updates automatically

### Preset palette

| Name | Accent | Accent Dim | Suggested for |
|---|---|---|---|
| Indigo | `#1a1682` | `#eeedf8` | Default / primary app |
| Teal | `#0d7377` | `#e6f2f2` | Secondary app |
| Violet | `#7c5cbf` | `#f0ecf8` | Creative / design tools |
| Rose | `#c05580` | `#f8ecf0` | HR / people tools |
| Slate | `#4a5568` | `#edf0f4` | Ops / infrastructure |

### Admin UI

See `templates/admin-settings.html` — the "Appearance" card section shows the pattern: color swatches using `.color-swatch` + `.color-swatch.active`, with an optional `<input type="color">` for custom values.

### Generating accent-dim from accent

`accent-dim` is the accent at ~8% opacity on white. Quick formula:

```js
function accentDim(hex) {
  const r = parseInt(hex.slice(1, 3), 16);
  const g = parseInt(hex.slice(3, 5), 16);
  const b = parseInt(hex.slice(5, 7), 16);
  const mix = (c) => Math.round(c * 0.08 + 255 * 0.92).toString(16).padStart(2, '0');
  return `#${mix(r)}${mix(g)}${mix(b)}`;
}
```
