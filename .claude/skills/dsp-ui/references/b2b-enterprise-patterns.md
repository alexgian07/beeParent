# B2B & Enterprise UI Patterns

Patterns for data-dense, power-user focused applications.

## Dashboard Design

### Dashboard Anatomy
```
┌─────────────────────────────────────────────────────────┐
│ [Logo] Navigation                    [Search] [Profile] │ ← Global nav
├─────────────────────────────────────────────────────────┤
│ Page Title              [Date Range ▼] [Filter] [Export]│ ← Page header
├───────────────┬───────────────┬───────────────┬─────────┤
│   KPI Card    │   KPI Card    │   KPI Card    │ KPI Card│ ← Metrics row
│   $1.2M ↑12%  │   847 ↓3%     │   94.2%       │ 23 New  │
├───────────────┴───────────────┴───────────────┴─────────┤
│                                                         │
│           Primary Chart / Visualization                 │ ← Main insight
│                                                         │
├─────────────────────────────────┬───────────────────────┤
│     Secondary Chart             │    Supporting Table   │ ← Detail row
│                                 │                       │
└─────────────────────────────────┴───────────────────────┘
```

### KPI Cards
**Essential elements**:
- Metric label (what it measures)
- Primary value (large, prominent)
- Comparison/trend (vs. previous period)
- Sparkline (optional, shows trajectory)

**Layout**:
```
┌──────────────────────┐
│ Total Revenue        │  ← Label
│ $1,247,392          │  ← Primary value (largest)
│ ↑ 12.4% vs last mo  │  ← Comparison (green/red)
│ ▁▂▃▂▄▅▆▅▇           │  ← Sparkline (optional)
└──────────────────────┘
```

### Dashboard Best Practices
- **Above the fold**: Most important metrics visible without scrolling
- **Progressive disclosure**: Summary → Detail on drill-down
- **Consistent time periods**: All metrics should reference same period
- **Comparison context**: Raw numbers need benchmarks to be meaningful
- **Actionable insights**: Dashboards should answer "what should I do?"

## Data Tables

### Table Anatomy
```
┌─────┬──────────────┬────────────┬──────────┬─────────┬────────┐
│ [ ] │ Name ^v      │ Status     │ Created  │ Value   │ Actions│
├─────┼──────────────┼────────────┼──────────┼─────────┼────────┤
│ [ ] │ Vessel Alpha │ * Active   │ Jan 15   │ $45,200 │ ...    │
│ [ ] │ Vessel Beta  │ o Pending  │ Jan 12   │ $38,100 │ ...    │
│ [x] │ Vessel Gamma │ * Active   │ Jan 10   │ $52,800 │ ...    │
└─────┴──────────────┴────────────┴──────────┴─────────┴────────┘
      │              │            │          │         │
      Multi-select   Sortable     Status     Formatted Actions
                     indicator    badges     numbers   menu
```

### Table Features Checklist
- [ ] **Sorting**: Click header to sort, show direction indicator
- [ ] **Selection**: Checkbox column, "select all" in header
- [ ] **Bulk actions**: Appear when items selected
- [ ] **Pagination**: Or infinite scroll with virtualization
- [ ] **Column resizing**: Drag handles between columns
- [ ] **Column visibility**: Toggle columns on/off
- [ ] **Row actions**: Hover to reveal, or persistent "..." menu
- [ ] **Empty state**: Helpful message + action when no data
- [ ] **Loading state**: Skeleton rows, not spinner
- [ ] **Density toggle**: Let users switch compact/default/comfortable
- [ ] **Virtual scrolling**: Required for 50+ rows (use TanStack Virtual or similar)

### Table Density Options
```
Compact:     32px row height  — Maximum data, power users
Default:     40px row height  — Balanced
Comfortable: 48px row height  — Easier scanning, touch-friendly
```

Provide a density toggle control — power users demand compact, new users prefer comfortable.

### Number Formatting in Tables
- Right-align numeric columns
- Use consistent decimal places
- Add thousand separators (1,234,567)
- Abbreviate large numbers (1.2M, 45K)
- Currency symbols once (in header) or per cell (if mixed)

## Filters & Search

### Filter Bar Pattern
```
┌───────────────────────────────────────────────────────────────┐
│ [🔍 Search...          ] [Status ▼] [Date Range ▼] [+ Filter] │
├───────────────────────────────────────────────────────────────┤
│ Active filters: [Status: Active ×] [Date: Last 7 days ×]     │
└───────────────────────────────────────────────────────────────┘
```

### Filter Types
- **Search**: Free text, searches across multiple fields
- **Dropdown select**: Single value from predefined list
- **Multi-select**: Multiple values (checkboxes in dropdown)
- **Date range**: Preset ranges + custom picker
- **Numeric range**: Min/max inputs or slider
- **Boolean toggle**: On/off states

### Filter UX Best Practices
- Show active filters as removable chips
- "Clear all" option when filters applied
- Persist filters in URL (shareable, bookmarkable)
- Show result count updating as filters change
- Remember recent/saved filter combinations

### Advanced Filter Builder
For complex queries:
```
┌─────────────────────────────────────────────────────────────┐
│ Where ALL of the following match:                           │
│ ┌─────────────┬────────────┬─────────────────┬───┐         │
│ │ Status      │ is         │ Active          │ × │         │
│ ├─────────────┼────────────┼─────────────────┼───┤         │
│ │ Created     │ is after   │ Jan 1, 2025     │ × │         │
│ ├─────────────┼────────────┼─────────────────┼───┤         │
│ │ Value       │ is greater │ $10,000         │ × │         │
│ └─────────────┴────────────┴─────────────────┴───┘         │
│ [+ Add condition]                                           │
└─────────────────────────────────────────────────────────────┘
```

## Complex Forms

### Multi-Step Forms
```
Step indicator:
──●────────●────────○────────○──
  1         2         3         4
Details   Settings  Review   Complete
```

- Show progress (steps completed / total)
- Allow back navigation without data loss
- Validate each step before proceeding
- Summary/review step before final submission

### Form Section Organization
```
┌─────────────────────────────────────┐
│ Section Header                      │
│ Brief description of this section   │
├─────────────────────────────────────┤
│                                     │
│  Label              Label           │
│  [Input field]      [Input field]   │
│                                     │
│  Label                              │
│  [Wider input field              ]  │
│                                     │
└─────────────────────────────────────┘
```

### Inline Editing
For settings and configuration:
```
Read mode:                    Edit mode:
┌────────────────────────┐   ┌────────────────────────┐
│ Company Name           │   │ Company Name           │
│ Acme Corp        [Edit]│   │ [Acme Corp        ] ✓ ✗│
└────────────────────────┘   └────────────────────────┘
```

## Navigation Patterns

### Sidebar Navigation
```
┌──────────────┬────────────────────────────────────────┐
│ [Logo]       │                                        │
│              │                                        │
│ ● Dashboard  │         Main Content Area              │
│ ○ Vessels    │                                        │
│ ○ Analytics  │                                        │
│ ○ Reports    │                                        │
│              │                                        │
│ ─────────    │                                        │
│ ○ Settings   │                                        │
│ ○ Help       │                                        │
│              │                                        │
│ [User ▼]     │                                        │
└──────────────┴────────────────────────────────────────┘
```

### Collapsible Sidebar
- Expand: Full labels + icons
- Collapse: Icons only (with tooltips)
- Persist user preference
- Keyboard shortcut to toggle

### Tab Navigation (for sub-sections)
```
┌─────────────────────────────────────────────────────┐
│ Vessel Detail                                       │
├──────────┬───────────┬────────────┬─────────────────┤
│ Overview │ Voyages   │ Documents  │ Activity        │
├──────────┴───────────┴────────────┴─────────────────┤
│                                                     │
│              Tab content here                       │
│                                                     │
└─────────────────────────────────────────────────────┘
```

## Detail Views

### Master-Detail Pattern
```
┌──────────────────────┬──────────────────────────────────┐
│ List / Table         │  Detail Panel                    │
│                      │                                  │
│ ○ Item 1             │  Item 2 Details                  │
│ ● Item 2  ←selected  │  ─────────────                   │
│ ○ Item 3             │  [All properties and actions]    │
│ ○ Item 4             │                                  │
│                      │                                  │
└──────────────────────┴──────────────────────────────────┘
```

### Side Panel / Drawer
- Slides in from right (typically)
- Width: 400-600px for forms, up to 50% for complex detail
- Overlay with backdrop, or push content
- Close on Escape, click outside, or explicit close button

### Full-Page Detail
Use when:
- Detail view is primary workspace
- Many tabs/sections of information
- Users spend significant time on single record

## Bulk Operations

### Selection + Action Bar
```
When items selected, show action bar:
┌─────────────────────────────────────────────────────────────┐
│ ☑ 3 items selected    [Archive] [Delete] [More ▼]   [× Clear]│
└─────────────────────────────────────────────────────────────┘
```

### Bulk Action Best Practices
- Show count of selected items
- Disable invalid actions for selection
- Confirmation for destructive bulk actions
- Progress indicator for long operations
- Allow cancel mid-operation when possible
- Report results (success/failures) after completion

## Real-Time Updates

### Live Data Indicators
```
● Live    — Green dot, real-time updates active
◐ Syncing — Animated, sync in progress  
○ Paused  — Gray, updates paused
⚠ Stale   — Warning, data may be outdated
```

### Update Patterns
- **Silent updates**: Data refreshes without visual indication
- **Highlight updates**: Flash/highlight changed values briefly
- **Toast notification**: "3 new items available" [Refresh]
- **Inline indicator**: Show "Updated 2s ago" timestamp

### Handling Conflicts
When data changes while user is editing:
- Show notification: "This item was updated by another user"
- Options: [Keep my changes] [Load their changes] [Compare]

## Productivity Patterns

### Command Palette (Cmd+K)
The most important power-user pattern in B2B software.

```
┌─────────────────────────────────────────────────┐
│ [>] Search or run a command...              Esc │
├─────────────────────────────────────────────────┤
│ Recent                                          │
│   > Dashboard                           Ctrl+D │
│   > Fleet Overview                      Ctrl+F │
├─────────────────────────────────────────────────┤
│ Navigation                                      │
│   > Vessels                                     │
│   > Analytics                                   │
├─────────────────────────────────────────────────┤
│ Actions                                         │
│   > Create new vessel...                        │
│   > Export report...                    Ctrl+E │
│   > Switch workspace...                         │
└─────────────────────────────────────────────────┘
```

- **Trigger**: `Cmd+K` / `Ctrl+K`
- **Modes**: Navigation, Actions, Data search (auto-detect from input)
- **Results**: Grouped by type with keyboard shortcut hints
- **UX**: Autofocus input, fuzzy matching, arrow key navigation, Enter to select
- **Width**: 560-640px, centered vertically in upper third
- **Animation**: 150ms scale+fade in (see `component-visual-specs.md`)

### Keyboard First Design
Power users rely on keyboard efficiency.
- **Global Shortcuts**:
  - `/`: Focus search
  - `?`: Show keyboard shortcuts dialog
  - `Esc`: Close modal/clear selection
  - `j` / `k`: Next/previous item (Gmail style)
- **Focus Management**:
  - Focus trapping in modals.
  - Logical tab order (top-left to bottom-right).
  - Visual focus indicators at all times.

## Empty States

### First Use (Onboarding)
- **Goal**: Teach value and drive first action.
- **Content**:
  - Illustration (welcoming)
  - "No [items] yet"
  - Description of benefit
  - [Primary Action Button]

### No Results (Search/Filter)
- **Goal**: Help user recover.
- **Content**:
  - "No results found for 'xyz'"
  - "Try adjusting filters or search term"
  - [Clear Filters] button

### Error State
- **Goal**: Communicate failure and offer recovery.
- **Content**:
  - Error icon + clear message (not technical jargon)
  - [Retry] button
  - Link to help/support if persistent

---

## Loading & Skeleton Patterns

### Skeleton > Spinner

Skeleton screens are perceived as loading **50% faster** than spinners with identical actual load times.

```
Loading state:                  Loaded:
┌──────────────────────┐       ┌──────────────────────┐
│ ████████████░░░░░░░  │       │ Total Revenue        │
│ █████████████████░░  │  -->  │ $1,247,392           │
│ ██████░░░░░░░░░░░░░  │       │ +12.4% vs last mo    │
└──────────────────────┘       └──────────────────────┘
```

**Rules**:
- Match skeleton dimensions to actual content layout
- Use shimmer/pulse animation (1.5s ease-in-out infinite)
- Only show if load > 500ms (avoid flash for fast responses)
- Load above-fold skeletons first (progressive)
- Fade transition (150ms) from skeleton to content

### Virtual Scrolling

Required for tables/lists with 50+ items. Render only visible rows + small buffer.

- Use TanStack Virtual, react-virtual, or platform equivalent
- Maintain scroll position on data updates
- Show total count even when only rendering visible subset
- Combine with `content-visibility: auto` for additional performance

---

## Bento Grid Dashboard Variant

An alternative to the traditional KPI → Chart → Table stack. Uses asymmetric cards for visual interest and implicit hierarchy.

```
┌────────────────────────────┬──────────┬──────────┐
│                            │ KPI      │ KPI      │
│   Primary Chart/Metric     │ $1.2M    │ 847      │
│   (2x2 feature card)       │ +12%     │ -3%      │
│                            ├──────────┴──────────┤
│                            │                     │
├──────────┬─────────────────┤  Activity Feed      │
│ KPI      │ KPI             │  (tall card)        │
│ 94.2%    │ 23 New          │                     │
├──────────┴─────────────────┴─────────────────────┤
│                                                   │
│              Data Table (full width)              │
│                                                   │
└───────────────────────────────────────────────────┘
```

- Larger cards = more important metrics
- Responsive: collapses to 2 columns → 1 column on mobile
- See `modern-css-2026.md` for CSS Grid implementation