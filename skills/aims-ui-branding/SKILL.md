---
name: aims-ui-branding
description: Use when building, styling, or modifying any UI component, page, tab, card, button, form, table, or layout in the AIMS ERP (aimscore-nexus). Required any time CSS classes, Tailwind tokens, gradients, borders, colors, or visual design is involved.
---

# AIMS UI Branding

## Overview

Every pixel in aimscore-nexus must use the AIMS brand color system. Never use raw hex codes, arbitrary Tailwind colors (blue-500, gray-300), or inline styles. Always use the AIMS CSS utility classes from `src/index.css` or the HSL CSS variables defined in `:root`.

---

## Brand Color System

### Core Palette (Hex Reference)

| Token | HSL | Hex (approx) | Usage |
|-------|-----|--------------|-------|
| Primary Teal | `hsl(197 100% 41%)` | `#0096D1` | Buttons, links, active states, primary actions |
| Primary Light | `hsl(197 82% 50%)` | `#16B5E8` | Gradient midpoint, hover highlights |
| Accent Gold | `hsl(36 93% 54%)` | `#F5A21C` | Gradient end, warnings, highlights, KPI accent |
| Navy Dark | `hsl(197 55% 10%)` | `#0B2430` | Sidebar background, dark headers |
| Background | `hsl(210 25% 97%)` | `#F4F6F9` | Page background |
| Card | `hsl(0 0% 100%)` | `#FFFFFF` | Card/panel surfaces |
| Muted | `hsl(210 20% 93%)` | `#E8EDF3` | Subtle backgrounds, dividers |

### The AIMS Signature Gradient

This gradient is the AIMS brand mark. It appears on active tabs, sidebar items, section headers, KPI cards, and any element that represents the current/selected/primary state.

```css
/* The signature gradient — teal → lighter teal → gold */
linear-gradient(90deg, hsl(197 100% 41%) 0%, hsl(197 82% 50%) 48%, hsl(36 93% 54%) 100%)
```

**Direction variants:**
- `90deg` — horizontal (tabs, buttons, banners)
- `135deg` — diagonal (hero sections, module headers)
- `150deg` — shallow diagonal (brand page backgrounds)
- `180deg` — vertical (sidebar gradient)

---

## CSS Utility Classes — When to Use Each

All classes are defined in `src/index.css`. Do NOT recreate them inline.

### Backgrounds & Gradients

| Class | Use case |
|-------|----------|
| `.aims-brand-gradient` | Full-page or module background tint (subtle dark navy→gold) |
| `.aims-hero` | Module hero/banner section (bold teal→gold, 135deg) |
| `.aims-header` | Module page header bar (dark navy, 135deg) |
| `.aims-section-gradient` | Section title bar / sub-header with white text |
| `.aims-soft-gradient` | Subtle background for info panels, filter bars |
| `.sidebar-gradient` | Sidebar background — apply to the sidebar wrapper |

### Tabs

| Class | Use case |
|-------|----------|
| `.aims-tab-strip` | Wrap around the tab list (`<TabsList>`) |
| `.aims-tab-trigger` | Apply to every `<TabsTrigger>` |
| `.aims-tab-active` | Manually applied active state (use instead of `data-state="active"` when not using Radix) |

The `.aims-tab-trigger[data-state="active"]` rule is already in CSS — Radix tabs auto-apply this.

### Cards & Borders

| Class | Use case |
|-------|----------|
| `.aims-gradient-border` | Card/panel that needs a teal→gold gradient border |
| `.aims-card-border` | Border-image approach (for elements where padding-box trick won't work) |
| `.aims-glow` | Ambient glow on hover or for highlighted KPI cards |
| `.border-ghost` | Accessible ghost border (15% primary opacity) |
| `.shadow-ambient` | Soft shadow for cards (not heavy drop shadow) |

### Interactive States

| Class | Use case |
|-------|----------|
| `.aims-sidebar-active` | Currently active sidebar nav item |
| `.aims-sidebar-item` | Every sidebar nav item (hover state applied via CSS) |
| `.aims-sidebar-hover` | Manually applied hover state (rarely needed) |
| `.aims-soft-hover` | Light surface hover (filters, list rows, soft interactions) |
| `.aims-btn` | Primary action button (use instead of `bg-primary`) |

### Icons & Text

| Class | Use case |
|-------|----------|
| `.aims-icon-bg` | Icon container background (teal→gold, for colored icon badges) |
| `.aims-text-primary` | Text in primary teal color |
| `.aims-text-accent` | Text in accent gold color |

### Typography & Numbers

| Class | Use case |
|-------|----------|
| `.tabular-nums` | All financial/numeric columns in tables |

---

## Tailwind Token Usage

Use these CSS variable-backed Tailwind tokens — never raw color names.

```tsx
// ✅ CORRECT
<div className="bg-primary text-primary-foreground" />
<div className="bg-accent text-accent-foreground" />
<div className="bg-sidebar text-sidebar-foreground" />
<div className="border-border" />
<div className="text-muted-foreground" />

// ❌ WRONG — never do this
<div className="bg-blue-500" />
<div className="border-gray-300" />
<div style={{ background: '#0096d1' }} />
```

---

## Component Recipes

### KPI Card

```tsx
<Card className="aims-gradient-border aims-glow">
  <CardContent className="pt-6">
    <div className="flex items-center gap-3">
      <div className="aims-icon-bg p-2 rounded-lg">
        <Icon className="h-5 w-5 text-white" />
      </div>
      <div>
        <p className="text-sm text-muted-foreground">Label</p>
        <p className="text-2xl font-bold tabular-nums">1,234</p>
      </div>
    </div>
  </CardContent>
</Card>
```

### Tab Bar

```tsx
<Tabs defaultValue="overview">
  <TabsList className="aims-tab-strip">
    <TabsTrigger value="overview" className="aims-tab-trigger">Overview</TabsTrigger>
    <TabsTrigger value="details" className="aims-tab-trigger">Details</TabsTrigger>
  </TabsList>
  <TabsContent value="overview">...</TabsContent>
</Tabs>
```

### Module Page Header

```tsx
<div className="aims-header rounded-xl p-6 mb-6">
  <div className="flex items-center gap-3">
    <div className="aims-icon-bg p-3 rounded-xl">
      <ModuleIcon className="h-7 w-7 text-white" />
    </div>
    <div>
      <h1 className="text-2xl font-bold text-white">Module Name</h1>
      <p className="text-sm text-white/70">Subtitle / description</p>
    </div>
  </div>
</div>
```

### Section Sub-Header

```tsx
<div className="aims-section-gradient rounded-lg px-4 py-2 mb-4">
  <h2 className="text-sm font-semibold tracking-wide uppercase">Section Title</h2>
</div>
```

### Primary Button

```tsx
// Use shadcn Button with default variant — it maps to CSS --primary which is the AIMS teal
<Button>Save Changes</Button>

// Or use .aims-btn for explicit gradient
<button className="aims-btn px-4 py-2 rounded-lg font-medium">Save</button>
```

### Data Table

```tsx
<Table>
  <TableHeader>
    <TableRow className="bg-muted/40">
      <TableHead className="font-semibold text-foreground">Column</TableHead>
      <TableHead className="font-semibold text-foreground tabular-nums">Amount (AED)</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    {rows.map(row => (
      <TableRow key={row.id} className="aims-soft-hover cursor-pointer">
        <TableCell>{row.name}</TableCell>
        <TableCell className="tabular-nums">AED {row.amount.toLocaleString()}</TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

### Status Badge

```tsx
// Use semantic color tokens — never raw colors
<Badge className="bg-success/10 text-success border-success/20">Active</Badge>
<Badge className="bg-destructive/10 text-destructive border-destructive/20">Inactive</Badge>
<Badge className="bg-warning/10 text-warning border-warning/20">Pending</Badge>
<Badge className="bg-primary/10 text-primary border-primary/20">Draft</Badge>
```

---

## Dark Mode

All `.aims-*` classes already include dark mode variants in `src/index.css`. You do NOT need to add `dark:` prefixes to these classes — they handle it internally.

For custom Tailwind classes you add, always include a `dark:` variant:

```tsx
// ✅ Dark mode aware
<div className="bg-slate-50 dark:bg-slate-900" />

// ✅ AIMS classes — dark handled internally
<div className="aims-soft-gradient" />  {/* no dark: needed */}
```

---

## Rules

1. **No raw hex codes** in JSX or CSS. Use HSL variables or utility classes.
2. **No arbitrary Tailwind colors** (`blue-500`, `gray-300`, etc.). Use semantic tokens.
3. **All financial numbers** use `.tabular-nums`.
4. **All module pages** use `<ERPLayout>` wrapper.
5. **All active states** (tabs, sidebar) use `.aims-tab-trigger` / `.aims-sidebar-active`.
6. **All cards** that need brand emphasis use `.aims-gradient-border`.
7. **New utilities** go in `src/index.css` under the appropriate `@layer utilities` block.
8. **Never use `!important`** outside of the existing `.aims-*` color-override rules.

---

## Adding New Utilities to index.css

When a pattern repeats 3+ times and isn't covered above, add a utility:

```css
/* src/index.css — add inside @layer utilities */
.aims-your-new-class {
  /* light mode */
}
.dark .aims-your-new-class {
  /* dark mode */
}
```

Name convention: `aims-` prefix for brand-specific, no prefix for generic utilities (`tabular-nums`, `shadow-ambient`).
