# design-kit

This is a reusable CSS design system. It provides all generic UI components. Apps import it as a submodule.

## Rules for AI agents

1. **Read `COMPONENTS.md` before building any UI.** It specifies which libraries, patterns, and CSS classes to use.
2. **Read `templates/` before building any page.** Match the template structure exactly.
3. **Never write inline styles or custom CSS for something the kit already provides.** Check the `src/components/` directory first.
4. **Do not modify this submodule** unless the change is genuinely reusable across multiple apps.

## Key components (check before reinventing)

- `matrix-grid.css` — Grid with sticky headers, vertical row labels, column headers
- `cards.css` — Detail cards, stat cards, card layouts
- `modals.css` — Modal overlays, full-screen modals
- `panels.css` — Slide-in panels, panel headers
- `buttons.css` — btn, btn-accent, btn-ghost, btn-icon
- `badges.css` — Badges, meta-tags with category colors
- `forms.css` — Form inputs, selects, textareas
- `tables.css` — Data tables with sorting
- `layout.css` — Shell, topbar, sidebar, content area
- `empty-state.css` — Empty state cards
