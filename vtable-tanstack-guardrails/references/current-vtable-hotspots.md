# Current VTable Hotspots

Use this note when changing `tableDemo/js/vtable.js`.

## Current Strengths

- Keep the existing direction of using `TanStackTableCore.createTable(...)` as the table-state entry point.
- Keep the existing direction of using a virtualizer instance instead of manual row-window math.
- Keep the existing DOM-pool idea because it is compatible with a thin render-adapter design.

## Hotspots To Treat Carefully

### `buildColumns()`

- Keep this method as an adapter from `cols` config to TanStack column definitions.
- Move new table semantics into TanStack column defs or table state, not into ad-hoc render-layer conditionals.
- Do not add manual sort or filter logic here when TanStack already exposes the feature.

### `calculateFixedOffsets()`

- This is currently manual sticky-offset math.
- Treat it as temporary infrastructure, not a place to grow a second pinning engine.
- When pinning, sizing, reorder, or horizontal virtualization becomes more complex, prefer TanStack-driven metadata and virtual ranges over expanding this calculator.

### `initVirtualScroll()`

- The current implementation creates only one row virtualizer.
- Do not stop at row virtualization if the table must support wide ERP schemas.
- Add a dedicated column virtualizer and synchronize header and body rendering around the same horizontal scroll source.

### `renderHeader()`

- It currently rebuilds header HTML in one pass.
- Keep that acceptable for discrete state changes such as schema resets or sorting updates.
- Do not put header rebuilding on the scroll path.
- If column virtualization is added, patch header cells from the visible column range instead of materializing every column.

### `renderBody()`

- This is the primary hot path.
- Keep its work bounded by visible rows and visible columns only.
- Avoid work that scales with total dataset size during scroll.
- Avoid global rerender calls when a smaller dirty-window patch is possible.

### `renderRowCells()`

- It currently loops `row.getVisibleCells()` for each rendered row.
- In a wide table, this becomes the next bottleneck even if rows are virtualized.
- Refactor toward a visible-column window so the cost becomes `visibleRows x visibleColumns`, not `visibleRows x allColumns`.

### `toggleSelect()`, `toggleSelectAll()`, `sort()`, `reload()`

- These methods currently trigger broad rerenders.
- Keep TanStack as the state owner.
- Narrow DOM updates to impacted rows, headers, or current virtual windows whenever the data schema itself did not change.

## Recommended Refactor Order

1. Map each current responsibility to `table-core`, `virtual-core`, or `VTable`.
2. Introduce column virtualization before adding more wide-column features.
3. Replace full row-cell loops with visible-column range rendering.
4. Introduce a dirty-row or dirty-window patch strategy for selection and similar narrow changes.
5. Revisit pinned-column layout only after state ownership and virtual ranges are clean.

## Anti-Patterns

- Adding manual offsets because a library API was not checked first.
- Adding render-layer caches that duplicate TanStack state.
- Using scroll handlers to compute virtual ranges by hand.
- Rebuilding the whole body to update one row.
- Optimizing row virtualization while leaving horizontal rendering unbounded.
