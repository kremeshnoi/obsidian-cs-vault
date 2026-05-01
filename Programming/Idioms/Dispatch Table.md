#programming #idioms

# Dispatch Table: Replace Branching with Data

## Definition

A dispatch table is a data structure that maps keys to behavior. Instead of branching through if/else or switch, you store the mapping in a table and select the right entry by key at runtime.

> [!info]
> The word "dispatch" means to route execution — to send control to the right place. In its classic form, the values in the table are callable: function pointers, closures, method references.

This is the origin of the concept: interrupt vector tables and jump tables in C and assembly, where an index maps directly to a memory address of a routine to execute. Vtables in C++ are dispatch tables — they map method selectors to function pointers.

In modern usage, especially in dynamic languages, the term is applied more broadly. The values can be classes, configuration objects, or any data that determines what happens next — not necessarily something you call directly. At that point the line between a dispatch table and a plain lookup table blurs.

> [!note]
> **Dispatch table** maps keys to *behavior* (what to do).
> **Lookup table** maps keys to *data* (what to return).
> In practice, the two often overlap.

## Examples

### JavaScript

```javascript
const statusConfig = {
  success: { icon: "✓", color: "green", retry: false },
  error:   { icon: "✗", color: "red",   retry: true  },
  pending: { icon: "…", color: "gray",  retry: false },
}

const config = statusConfig[response.status]
if (config) {
  renderBadge(config.icon, config.color)
  if (config.retry) scheduleRetry()
}
```

> [!tip]
> The table maps status strings to configuration objects, not functions. The set of valid statuses and their behavior is declared in one place.

### Lua

Lua has no switch statement at all, so dispatch tables are the idiomatic way to branch on values:

```lua
local commands = {
  greet = function(name) print("Hello, " .. name) end,
  quit  = function() os.exit(0) end,
}

local action = commands[input]
if action then action(arg) end
```

> [!example]
> A very common Lua pattern — you'll see it in game scripting (WoW addons, LOVE2D), configuration-driven systems, and embedded interpreters.

## Trade-offs

### Pros

- Makes the set of valid keys explicit and inspectable in one place
- O(1) lookup vs O(n) if/else chain (in hash-based implementations)
- Easy to extend, merge, and compose at runtime — tables are just data
- Separates the routing decision from the handler logic

### Cons

- Adds indirection that may hurt readability for 2–3 simple cases
- Cannot express complex conditions beyond key equality (ranges, guards, predicates)
- Loses exhaustiveness checking that typed languages provide for free
- Debugging is less obvious — stack traces point to the handler, not the routing logic

## Why It Matters

### When It Helps

- In dynamically typed languages (JavaScript, Lua, Python) where there is no type-level contract — the keys become the implicit contract, the only place that declares which values exist and what they do
- The number of branches is growing and if/else becomes hard to maintain
- Keys are only known at runtime (user input, config files, plugin systems)
- Handlers need to be composed dynamically (merged, overridden, passed around)

### When Not to Use

- In a statically typed language where match/switch + exhaustiveness checking already covers you — the type system already guarantees what a table would
- When branches have complex conditions beyond simple key equality
- When there are only 2–3 cases and readability doesn't benefit from the indirection