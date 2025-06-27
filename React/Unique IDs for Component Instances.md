---
authors:
  - Anton Petrov
status: note
tags:
  - javascript
  - react
  - preact
---

In reactive UI frameworks, rendering dynamic lists or form components requires unique identifiers for consistent DOM behavior and accessibility compliance. This applies to scenarios such as generating `<option>` elements in a `<select>` menu or associating a `<label>` with its corresponding `<select>` element via the `id` attribute.

## Original Method

A frequently used approach for generating short, pseudo-unique IDs:

```ts
Math.random().toString(36).substring(2, 8);
```

- Compact and easy to implement
- Does not guarantee uniqueness
- Regenerates on each render unless explicitly preserved

> While suitable for simple applications, this method may introduce inconsistencies and potential collisions in re-rendered components.

## Recommended Approach

To maintain ID consistency across renders, a persistent reference should be used to store the generated ID. In React, this can be implemented with the `useRef` hook:

```tsx
const uniqueId = useRef(Math.random().toString(36).substring(2, 8));
```

- Ensures ID stability throughout the component lifecycle
- Prevents redundant ID generation
- Makes the identifier accessible during all render phases

## Use Case

In dynamic lists generated via `.map()`, each item should be assigned a stable `key`. This enables the rendering engine to track changes accurately and maintain efficient DOM updates:

```tsx
<select>
  {options.map((option) => (
    <option key={option.id} value={option.value}>
      {option.label}
    </option>
  ))}
</select>
```

> Stable `key` values ensure predictable reconciliation behavior and prevent visual inconsistencies during re-renders.

## TL;DR

A persistent reference (e.g., `useRef`) can be used to store a randomly generated identifier, supporting stable, render-safe IDs. This technique enables reliable DOM associations and effective list reconciliation in reactive UI components.
