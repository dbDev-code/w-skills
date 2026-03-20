# PR Checklist

Use this checklist before finalizing any VTable optimization.

## Ownership

- Does every table feature in the change map to `@tanstack/table-core` or a documented extension point?
- Does every virtual-scroll or range-calculation concern map to `@tanstack/virtual-core`?
- Did the render layer stay a thin adapter instead of becoming a second table engine?

## Anti-Reimplementation

- Did the change avoid custom sorting logic when TanStack sorting already exists?
- Did the change avoid custom filtering logic when TanStack filtering already exists?
- Did the change avoid manual offset, range, and scroll math when Virtual Core already exposes it?
- Did the change avoid writing new stateful business logic into DOM cache fields?

## Performance

- Does the render cost scale with visible rows and visible columns instead of total rows or total columns?
- Is bidirectional virtualization present or preserved for large datasets?
- Did the scroll path avoid full header or body rerenders?
- Did the change avoid unnecessary allocations in tight loops?
- Did the change keep DOM reads and writes separated or batched?

## Interaction

- Did selection changes patch only affected rows or windows where possible?
- Did sorting, filtering, and pinning update from TanStack state instead of manual DOM state?
- Did header and body stay horizontally aligned under column virtualization and pinning?

## Validation Scenarios

- Test with `10000+` rows.
- Test with a wide schema that makes horizontal virtualization necessary.
- Test with pinned left and right columns.
- Test with fast wheel scrolling and scrollbar dragging.
- Test row selection, select-all, sorting, and reload flows.
- Confirm no obvious white-screen gap or frame collapse appears during fast scroll.
