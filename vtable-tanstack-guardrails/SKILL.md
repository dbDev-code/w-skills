---
name: vtable-tanstack-guardrails
description: Enforce architecture and performance guardrails when optimizing or extending high-performance table or grid code around tableDemo/js/vtable.js and similar VTable modules. Use when adding or refactoring sorting, filtering, selection, pinning, column sizing, column visibility, row models, scroll behavior, virtualization, or rendering performance. Require all table semantics and state to be driven by @tanstack/table-core, all virtualization, range, offset, and scroll calculations to be driven by @tanstack/virtual-core, forbid reimplementing library features, and target bidirectional virtualization plus zero-rerender behavior for large ERP datasets.
---

# VTable TanStack Guardrails

## Mission

Keep VTable fast, predictable, and library-driven. Treat the render layer as a thin DOM adapter over TanStack state and virtual ranges, not as a second table engine.

## Non-Negotiables

1. Use `@tanstack/table-core` for sorting, filtering, selection, pinning, sizing, ordering, visibility, and row-model logic.
2. Use `@tanstack/virtual-core` for row virtualization, column virtualization, range calculation, offsets, measurement, overscan, and scroll helpers.
3. Do not handwrite algorithms that already exist upstream.
4. Keep `tableDemo/js/vtable.js` focused on DOM reuse, event delegation, and minimal patching.
5. Preserve a path to 60fps for large ERP or electronic-components datasets by reducing work on the scroll hot path.

## Workflow

1. Classify each requested change before coding.
   - Table semantics belong to `table-core`.
   - Virtual range and scroll math belong to `virtual-core`.
   - DOM pooling, text patching, and delegated events belong to `VTable`.
2. Read [references/current-vtable-hotspots.md](references/current-vtable-hotspots.md) before large refactors.
3. Design around upstream primitives first. Add an adapter only when the library cannot express the requirement directly.
4. Keep render updates scoped to visible and dirty regions.
5. Read [references/pr-checklist.md](references/pr-checklist.md) before finalizing changes.

## Ownership Rules

### `@tanstack/table-core`

- Own `sorting`, `columnFilters`, `rowSelection`, `columnPinning`, `columnVisibility`, `columnSizing`, `columnOrder`, and any future row-model state.
- Define custom sorting or filtering only through documented TanStack extension points.
- Keep row ids, header groups, visible cells, and derived table state sourced from the table instance.

### `@tanstack/virtual-core`

- Own row and column virtualizers.
- Own visible ranges, total sizes, offsets, overscan, and scroll-to behavior.
- Own horizontal sync strategy for header and body when column virtualization exists.

### `VTable`

- Own DOM pooling and node reuse.
- Own event delegation and lifecycle hooks.
- Own the smallest possible render adapter from visible TanStack state to DOM.
- Do not own duplicated business state when the same state already exists in TanStack instances.

## Zero-Rerender Rules

- Do not rebuild full header or body HTML during scroll.
- Do not call a full-body rerender for one checkbox toggle if a targeted row patch is sufficient.
- Keep allocations near zero on scroll paths. Avoid creating new arrays, maps, or closures in tight render loops unless justified.
- Separate DOM reads from DOM writes and batch writes in `requestAnimationFrame` when updates are frame-bound.
- Keep mutable runtime state in stable instance fields, maps, or refs rather than recreating config objects in hot paths.
- If a React wrapper is introduced later, split context by concern, use selectors, and avoid unstable prop chains that fan out rerenders.

## Large-Dataset Contract

- Implement row and column bidirectional virtualization for wide and tall datasets.
- Render only `visibleRows x visibleColumns` plus overscan.
- Keep pinned-column behavior aligned with TanStack state and virtual ranges.
- Update only affected DOM segments for selection, sorting, filtering, and viewport changes whenever a smaller patch is possible.
- Treat a `10000+` row ERP dataset and a wide electronic-components schema as baseline scenarios, not edge cases.

## Refusal Rules

- Reject or rewrite changes that manually implement sorting or filtering already available in `table-core`.
- Reject or rewrite changes that manually compute virtual ranges, offsets, or scroll math already available in `virtual-core`.
- Reject changes that iterate all columns for every visible row in wide-table scroll paths.
- Reject changes that add full rerender loops for selection, hover, or small state mutations.
- Reject changes that turn DOM cache fields into a second source of truth for business state.

## Deliverables

When using this skill, produce:

1. A short architecture note mapping each changed concern to `table-core`, `virtual-core`, or `VTable`.
2. The code change.
3. A short performance note explaining why the scroll and selection paths did not become more expensive.
