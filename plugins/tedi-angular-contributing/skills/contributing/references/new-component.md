# Create New TEDI Component

## Prerequisites

A Figma link MUST be provided: `$ARGUMENTS`

If no Figma link was provided, stop immediately and ask the user for one. Do not proceed without design reference.

## Step 1: Gather Context

1. Use `figma-desktop` MCP to fetch design context, screenshots, and metadata from the Figma link.
2. Check if TEDI React (`../react/src/tedi/components/`) has an equivalent component — use as behavioral reference.
3. Check TEDI Core (`../core/src/`) for available design tokens and shared styles.

## Step 2: Plan

Enter plan mode and create a detailed plan covering:

- **Component name** and selector
- **Selector type** — element selector (`tedi-foo`) for wrapper components, or attribute selector (`[tedi-foo]`) for components that enhance an existing native element
- **Category** — which folder under `tedi/components/` it belongs to
- **API design** — all inputs (with types and defaults), outputs, content projection slots (single `<ng-content>` or named slots)
- **Form integration** — does it implement `ControlValueAccessor`? If yes, plan the `NG_VALUE_ACCESSOR` provider (with `forwardRef()` and `multi: true`) and a reactive-forms test host
- **Signal API** — which inputs are `input()`, which are `model()` for two-way binding, which derived state needs `computed()` or `effect()`
- **Change detection** — `OnPush` is the default; flag any state that requires manual `markForCheck()`
- **Accessibility** — ARIA roles, keyboard interactions, screen reader behavior, focus management
- **Dependencies** — existing TEDI components to reuse, third-party libraries if needed
- **File list** — every file to create
- **Test plan** — what to test (inputs, outputs, states, keyboard, a11y, form integration if applicable)
- **Stories plan** — which stories to create (match all Figma variants)

If a new dependency is needed, stop and ask the user for permission.

## Step 3: Scaffold Files

Create the following files in `tedi/components/<category>/<component-name>/`:

```text
component-name.component.ts
component-name.component.html
component-name.component.scss
component-name.component.spec.ts
component-name.stories.ts
index.ts
```

## Step 4: Implement

Follow all patterns from [best-practices.md](best-practices.md) — class structure, signal API, ControlValueAccessor, styling, testing. Key requirements:
- Standalone, OnPush, ViewEncapsulation.None
- Signal-based inputs (`input()`, `model()`, `output()`) — never `@Input()` / `@Output()` decorators
- BEM SCSS with `tedi-` prefix, using design tokens
- Form controls MUST implement `ControlValueAccessor` with `NG_VALUE_ACCESSOR` provider (using `forwardRef()` and `multi: true`) for reactive forms integration. Test with a host component using `ReactiveFormsModule` and `FormControl`.
- Full WCAG compliance (roles, keyboard nav, focus, aria attributes) — see [a11y-review.md](a11y-review.md) for the checklist

## Step 5: Export

1. Create barrel export in `index.ts`
2. Add export to the parent category `index.ts` (e.g., `tedi/components/form/index.ts`)

## Step 6: Verify

1. Run tests: `npx jest tedi/components/<category>/<component-name>/`
2. Fix any failures.
3. Run lint: `npm run lint`
4. Fix any lint errors.

## Step 7: Update Consumer Catalog

Update `skills/tedi-angular/references/components.md` with the new component:
1. Add an entry to the appropriate section (TEDI-Ready or Community) with selector, key inputs/outputs, and a usage example.
2. Follow the format of existing entries in the file.
