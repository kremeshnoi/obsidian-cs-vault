#programming #programming-languages #typescript

# Interface vs Type Alias

> [!info] TL;DR
> `interface` comes from the OOP world (Java, C#). It describes a stable, declarative structure — the shape of an object that you expect to extend, implement, and validate for compatibility at compile time. It's a contract.
>
> `type` (officially **Type Alias** in the TypeScript documentation) was introduced later for computed, composed, and dynamic type-level expressions — unions, intersections, conditionals, mapped types, tuples. It's a name for an arbitrary type construct.
>
> For a plain object shape like `{ id: number; name: string }`, both work identically. Over time their capabilities overlapped significantly, and the choice became purely stylistic for simple cases. But the differences still matter in specific scenarios.

## Exclusive Capabilities

### Only Type Alias

```typescript
// Union
type Status = 'active' | 'inactive' | 'banned'

// Tuple
type Pair = [string, number]

// Function signature
type Handler = (e: Event) => void

// Mapped type
type Readonly<T> = { readonly [K in keyof T]: T[K] }

// Conditional type
type Unwrap<T> = T extends Promise<infer U> ? U : T

// Extracting keys
type Keys = keyof SomeInterface
```

> [!warning] None of the above can be expressed with `interface`.

### Only Interface

#### Declaration Merging

Two declarations of the same `interface` merge into one:

```typescript
// a library declares
interface Window {
  title: string
}

// a plugin extends the same interface
interface Window {
  analytics: Analytics
}

// result: Window has both `title` and `analytics`
```

With a Type Alias, redeclaring the same name is a compile error:

```typescript
type Window = { title: string }
type Window = { analytics: Analytics } // Duplicate identifier
```

> [!tip]
> This is the key mechanism for augmenting third-party types — Vue's `ComponentCustomProperties`, Express's `Request`, Node's `ProcessEnv`, etc.

#### Implements

```typescript
interface Serializable {
  serialize(): string
}

class User implements Serializable {
  serialize() { return JSON.stringify(this) }
}
```

> [!note] Technically `implements` works with a Type Alias too, but only for object shapes — not unions or other complex types.

## Extending / Composing

### Interface — via `extends`

```typescript
interface Base { id: number }
interface Extended extends Base { name: string }
```

### Type Alias — via intersection (`&`)

```typescript
type Base = { id: number }
type Extended = Base & { name: string }
```

The result is the same, but there's an important nuance.

### Conflict Detection

`extends` catches incompatible properties immediately:

```typescript
interface Base { id: string }
interface Extended extends Base {
  id: number // Error: 'number' is not assignable to 'string'
}
```

`&` silently collapses the conflict into `never`:

```typescript
type Base = { id: string }
type Extended = Base & { id: number }
// id: string & number -> never
// no error until you try to assign a value
```

> [!danger] This makes `extends` safer for hierarchical structures where you want early feedback on mistakes.

## Performance

> [!info] How the compiler handles them internally
> `interface` creates a **named type** that the compiler can cache and reference by name.
>
> Type Alias with intersections creates an **anonymous type** that must be flattened and recomputed.
>
> For small projects this is irrelevant. In large codebases with deep type hierarchies, preferring `interface` for object shapes can yield measurably faster type checking. The TypeScript team has mentioned this in their performance guidance.

## Common Convention

Many teams follow this rule (sometimes enforced by ESLint):

```haskell
interface  ->  object shapes (props, API responses, configs)
type alias ->  everything else (unions, tuples, utilities, mapped types)
```

```json
// .eslintrc
{
  "@typescript-eslint/consistent-type-definitions": ["error", "interface"]
}
```

> [!note] This is a style choice, not a technical requirement. Some teams prefer Type Aliases everywhere for consistency. Both are valid.

> [!summary] Practical Takeaway
> For a Vue/Nuxt component's Props and Emits, it genuinely doesn't matter. Pick one style and stay consistent. The real differences show up when you need declaration merging, complex type algebra, or you're optimizing compiler performance in a large monorepo.