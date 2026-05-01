#programming #idioms

# Guard Clause: Replace Nesting with Early Returns

## Definition

A guard clause is a coding technique where you handle edge cases, invalid states, or preconditions at the top of a function by returning (or throwing/panicking) early — before the main logic runs.

> [!summary]
> **Get the "no" out of the way first**, so the rest of the function can be read as a clean, linear happy path with no nesting.

## Examples

### JavaScript

Without guards — the **arrow anti-pattern**:

```javascript
function processOrder(user, order) {
  if (user !== null) {
    if (order !== null) {
      if (user.isActive) {
        if (order.items.length > 0) {
          // actual logic buried 4 levels deep
          return `Processing order for ${user.name}`;
        } else {
          return "Order is empty";
        }
      } else {
        return "User is inactive";
      }
    } else {
      return "Order not found";
    }
  } else {
    return "User not found";
  }
}
```

With guards — flip each condition and return early:

```javascript
function processOrder(user, order) {
  if (user === null) return "User not found";
  if (order === null) return "Order not found";
  if (!user.isActive) return "User is inactive";
  if (order.items.length === 0) return "Order is empty";

  return `Processing order for ${user.name}`;
}
```

> [!tip]
> The happy path is now visually separated and reads like a specification: *given all preconditions pass, do this*.

### Lua

Lua has no static types and no exceptions in the traditional sense. Guard clauses with early `return` and Lua's idiomatic multi-return `nil, error_message` are the primary defensive tool:

```lua
local function process_order(user, order)
    if not user then return nil, "User not found" end
    if not order then return nil, "Order not found" end
    if not user.is_active then return nil, "User is inactive" end
    if #order.items == 0 then return nil, "Order is empty" end

    return "Processing order for " .. user.name
end

local result, err = process_order(current_user, current_order)
if not result then
    log_error(err)
    return
end
```

> [!example]
> In Lua (especially in Roblox, Love2D, or game engine scripting), functions receive arbitrary tables that might be `nil` or malformed. Guard clauses at the top are the standard defensive idiom.

## Trade-offs

### Pros

- Makes the set of valid states explicit at the top of the function
- O(1) mental overhead — each guard is independent, no nesting to track
- Narrows types progressively in TypeScript and Rust
- Separates validation from business logic

### Cons

- Adds indirection that may hurt readability for 2–3 simple cases
- Cannot express complex conditions beyond simple checks (ranges, multi-field validation)
- Multiple exit points can complicate debugging in languages without RAII or `defer`

## Why It Matters

> [!danger]
> Without guards, every new condition adds another level of indentation. You have to track the entire stack to understand what's happening.

### When It Helps

- You have multiple preconditions that must all pass before the main logic
- Async code where nested `.then()` chains or `if` blocks become unreadable
- Languages where each guard also narrows the type (TypeScript, Rust)
- Any function where the happy path is buried under layers of validation

### When Not to Use

- The "negative" case needs substantial logic — extract it into a separate function instead
- Languages without RAII or `defer` where a single exit point ensures cleanup
- There are only 2–3 cases and readability doesn't benefit from the pattern