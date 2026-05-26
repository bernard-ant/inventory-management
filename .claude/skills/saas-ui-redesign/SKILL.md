---
name: saas-ui-redesign
description: Redesign a Vue 3 app's UI into a modern SaaS interface — replace a top nav bar with a left vertical sidebar, introduce a CSS design-token layer, and apply consistent spacing and a polished professional look. Use this skill when asked to modernize/redesign/restyle a Vue 3 UI, convert a top nav to a sidebar, improve visual consistency, or tidy spacing across views.
---

# SaaS UI Redesign for Vue 3

A repeatable playbook for transforming a Vue 3 (Composition API) single-page app from a
top-navigation layout into a modern SaaS interface: a **left vertical sidebar**, a **CSS
design-token layer**, **consistent spacing**, and a **polished, professional** look. It is
written to work on any Vue 3 app, with concrete, copy-adaptable CSS patterns.

The redesign is **token-first and shell-first**: introduce variables, restructure the app
shell, and restyle the small set of shared/global classes. Because most Vue apps centralize
look-and-feel in a handful of shared classes, this delivers a broad visual upgrade with minimal
per-view churn.

## Mandatory: delegate `.vue` edits to vue-expert

In this repo, **any time you create or significantly modify a `.vue` file you MUST delegate to
the `vue-expert` subagent** (see root `CLAUDE.md`). This skill defines *what* and *why*; let
vue-expert handle the *how* (templates, reactivity, scoped styles). Drive it with a precise brief:
the target structure, the token names, and the exact class/selector changes. Non-`.vue` files
(e.g. `index.html`, a global CSS file) can be edited directly.

## Design principles (the "polished SaaS" target)

- **Tokens, not literals.** One `:root{}` source of truth for color, spacing, radius, shadow,
  and layout dimensions. Name values that already cluster — don't invent a new palette.
- **A persistent left sidebar.** Fixed/sticky, full viewport height, holding brand → primary nav
  → (spacer) → utility controls + account menu pinned at the bottom.
- **A consistent content column.** One max-width + gutter wrapper and one page-header pattern so
  every route lines up on the same grid.
- **Calm, consistent spacing.** A 4px-based scale; one card spec; even grid gaps.
- **Restraint.** Light surfaces, one accent color for the active/interactive state, subtle
  borders/shadows, no emojis in business UI, accessible contrast and focus states.

## Phase 1 — Discovery (read before editing)

Map the app first; redesigns break on hidden couplings to the old top bar.

1. **Find the shell.** Locate the root layout — usually `App.vue` or a layout component wrapping
   `<router-view>`. Identify the layout mechanism (flex column vs grid).
2. **Global vs scoped styles.** Determine where shared classes live. A single unscoped `<style>`
   block (or global CSS file) is the highest-leverage place to work — restyling it propagates to
   every view that reuses those classes. Note the shared primitives (page header, card, stat/KPI
   card, table, badge, loading/error).
3. **Nav + routes.** Find the nav markup and the router table so the sidebar lists the right
   links with the right labels (respect i18n bindings if present).
4. **Couplings that break when the top bar is removed** — grep for these and fix each:
   - Hardcoded sticky offsets keyed to the bar height (e.g. `top: 70px` on a filter/toolbar).
   - Dropdowns/menus anchored to open **downward / right-aligned** (`top: calc(100% + …); right: 0`)
     — they must be re-anchored once their trigger moves into a sidebar.
   - `margin-left: auto` tricks that push items to the right of a horizontal bar.
5. **Webfont.** Check whether the CSS names a font (e.g. Inter) that the HTML never actually loads.

## Phase 2 — Design-token layer

Add a `:root{}` block to the global stylesheet and repoint the shell's literals to `var(--…)`.
Adapt names/values to the app's existing palette. A solid default scaffold:

```css
:root {
  /* Ink & text */
  --color-ink: #0f172a;          /* headings */
  --color-text: #1e293b;         /* body */
  --color-text-muted: #64748b;   /* secondary */
  /* Surfaces & borders */
  --surface: #ffffff;
  --surface-subtle: #f8fafc;     /* page background */
  --surface-hover: #f1f5f9;
  --color-border: #e2e8f0;
  --color-border-strong: #cbd5e1;
  /* Brand / accent */
  --primary: #2563eb;
  --primary-strong: #1e40af;
  --primary-soft: #eff6ff;
  /* Status */
  --success: #059669; --success-soft: #d1fae5;
  --warning: #ea580c; --warning-soft: #fed7aa;
  --danger:  #dc2626; --danger-soft:  #fecaca;
  /* Spacing scale (4px base) */
  --space-1: 0.25rem; --space-2: 0.5rem;  --space-3: 0.75rem; --space-4: 1rem;
  --space-5: 1.25rem; --space-6: 1.5rem;  --space-8: 2rem;
  /* Radius & shadow */
  --radius-sm: 6px; --radius: 10px; --radius-lg: 12px;
  --shadow-sm: 0 1px 2px rgba(15,23,42,.04), 0 1px 3px rgba(15,23,42,.06);
  /* Layout */
  --sidebar-w: 248px;
  --content-max: 1600px;
  --content-gutter: var(--space-8);
  --header-h: 0px; /* legacy top-bar offset; 0 once converted */
}
```

Refactor incrementally — start with the shell and shared classes. Leaving per-view literals for
later is fine; the tokens are additive.

## Phase 3 — Top-nav → left-sidebar conversion

**Shell layout.** Change the root from a vertical stack to a two-column grid:

```css
.app { display: grid; grid-template-columns: var(--sidebar-w) 1fr; min-height: 100vh; }
.sidebar {
  position: sticky; top: 0; align-self: start;
  height: 100vh; display: flex; flex-direction: column;
  background: var(--surface); border-right: 1px solid var(--color-border);
  padding: var(--space-5) var(--space-3);
}
.main { min-width: 0; display: flex; flex-direction: column; } /* min-width:0 prevents overflow */
```

**Sidebar template** (delegate to vue-expert; keep existing nav bindings & i18n):

```vue
<aside class="sidebar">
  <div class="brand"><!-- logo / company name --></div>
  <nav class="nav">
    <router-link v-for="item in navItems" :key="item.path" :to="item.path"
      class="nav-item" :class="{ active: $route.path === item.path }"
      :aria-current="$route.path === item.path ? 'page' : undefined">
      {{ item.label }}
    </router-link>
  </nav>
  <div class="sidebar-footer"><!-- LanguageSwitcher / ProfileMenu --></div>
</aside>
```

**Vertical nav rows with a left-accent active state** (replaces pills/underlines):

```css
.nav { display: flex; flex-direction: column; gap: var(--space-1); }
.nav-item {
  display: flex; align-items: center; gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  border-radius: var(--radius-sm);
  color: var(--color-text); text-decoration: none; font-weight: 500;
  border-left: 3px solid transparent;        /* accent rail, reserved space */
}
.nav-item:hover { background: var(--surface-hover); }
.nav-item.active {
  background: var(--primary-soft); color: var(--primary-strong);
  border-left-color: var(--primary); font-weight: 600;
}
.sidebar-footer { margin-top: auto; }        /* pins utilities to the bottom */
```

Tips: let labels wrap or use flexible widths so **bilingual / variable-length** labels fit; keep
the existing active-class bindings rather than swapping to `router-link-active` mid-redesign.

## Phase 4 — Content container & page-header

One wrapper and one header spec so all routes align:

```css
.main-content { width: 100%; max-width: var(--content-max); margin: 0 auto;
                padding: var(--space-6) var(--content-gutter); }
.page-header { margin-bottom: var(--space-6); }
.page-header h2 { font-size: 1.875rem; font-weight: 700; letter-spacing: -.025em;
                  color: var(--color-ink); margin: 0 0 var(--space-1); }
.page-header p  { color: var(--color-text-muted); margin: 0; }
```

Card spec (pick ONE and let it propagate via the shared class):

```css
.card { background: var(--surface); border: 1px solid var(--color-border);
        border-radius: var(--radius); padding: var(--space-5);
        box-shadow: var(--shadow-sm); }
```

## Phase 5 — Move & repair chrome

- **Relocate utility menus** (language, account/profile) into `.sidebar-footer`.
- **Re-anchor their dropdowns to open upward** from the sidebar bottom so they don't clip:

  ```css
  .dropdown-menu { position: absolute; bottom: calc(100% + var(--space-2)); left: 0; right: auto;
                   top: auto; }
  ```
- **Re-stick filter/toolbars** that were offset to the old bar height: change `top: <bar>px` to
  `top: 0` (or `var(--header-h)`); they now stick to the top of the content column.

## Phase 6 — Polish

- **Load the webfont** if the CSS references one the HTML doesn't load — add a `<link>` (preconnect
  + stylesheet) in `index.html`.
- **Apply shadow + spacing tokens** for rhythm; unify grid `gap`/`min` across stat/KPI/card grids.
- **Collapsible icons-only sidebar.** Give each nav row an **icon** (inline SVG — no emojis) plus
  a label wrapped in its own element so the label can be hidden independently. Add a toggle button
  and a `collapsed` state; collapsing shrinks the rail to icons only via the layout token:

  ```css
  .app.sidebar-collapsed { --sidebar-w: 64px; }
  .app.sidebar-collapsed .nav-label,
  .app.sidebar-collapsed .brand-text,
  .app.sidebar-collapsed .sidebar-footer .label { display: none; }
  .app.sidebar-collapsed .nav-tabs a { justify-content: center; padding-inline: 0; }
  .nav-icon { width: 20px; height: 20px; flex: none; }
  ```
  Persist the user's choice (e.g. `localStorage`) and add `title` + `aria-label` so icon-only rows
  stay discoverable and accessible. Keep the left-accent active rail visible when collapsed.
- **Auto-collapse on small screens.** Drive `collapsed` from a media query so narrow viewports
  default to icons-only, while still letting the user toggle on wide screens:

  ```js
  const mq = window.matchMedia('(max-width: 1024px)')
  const isNarrow = ref(mq.matches)
  mq.addEventListener('change', e => { isNarrow.value = e.matches })
  const collapsed = computed(() => isNarrow.value || userCollapsed.value)
  ```

## Phase 7 — Verify

1. Run the dev server (and API if the UI needs data).
2. Visit **every** route: the sidebar persists, the active row shows the accent + `aria-current`,
   and content is not clipped by the sidebar.
3. Open each relocated dropdown — it opens upward/inward without clipping.
4. Scroll a long page — the filter/toolbar sticks at the top of the content column.
5. Capture before/after screenshots (Playwright MCP per `CLAUDE.md`, or a dev browser).
6. Check the console for errors and confirm data still loads. Verify long/i18n labels fit.

## Key reminders

- **Token-first:** add `:root{}`, then repoint the shell and shared classes to `var(--…)`.
- **Delegate `.vue` work to vue-expert**; give it exact structure, token names, and selectors.
- **Fix the couplings**: sticky offsets keyed to the old bar, and down/right-anchored dropdowns.
- **Keep behavior intact**: existing active-class bindings, i18n keys, and data flow.
- **No emojis in business UI**; ensure focus states and contrast; one accent color.
- **Scope sensibly**: shell + tokens propagate widely; deep per-view restyles are a separate pass.
