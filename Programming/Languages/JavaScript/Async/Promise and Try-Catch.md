#programming #programming-languages #javascript

# Promise and Try-Catch

> [!summary] TL;DR
> Both `try/catch` and `Promise` describe two paths (success / failure), but they operate at fundamentally different levels of abstraction. `Promise` encodes a contract *into a value*. `try/catch` is just syntax for reacting to an event that already happened.

## The Surface-Level Similarity

At first glance, both constructs seem to express the same contract:

```javascript
try {
  const data = doSomething()
  // success path
} catch (e) {
  // error path
}

fetch('/api/user')
  .then(data => /* success path */)
  .catch(e => /* error path */)
```

Two branches. Success or failure. Looks equivalent.

> [!warning]
> But they're not — and the difference matters.

## Two Different Kinds of Contract

### try/catch — a contract on *control flow*

`try/catch` is a **syntactic construct**. It says:

> "If an exception is thrown inside this block, jump to `catch`."

That's it. The contract lives only at the **site of execution** — it cannot be passed around, composed, or reasoned about outside its lexical scope. Once the block finishes, the contract is gone.

### Promise — a contract on *a value*

`Promise` is a **first-class value**. It carries its contract with it:

> "This value will eventually be either resolved or rejected — exactly once — and that outcome is immutable."

The contract doesn't live at the call site. It lives *in the value itself*, traveling with it wherever it goes.

## The Key Difference: Portability

```javascript
// try/catch dies at the call site — you cannot hand it to someone else
try {
  const data = await fetchUser()
  process(data)
} catch (e) {
  handleError(e)
}

// Promise is a value — the contract travels with it
const userPromise = fetchUser()   // contract already exists here

scheduleOtherWork()               // do something else

const data = await userPromise    // handle later...
pipe(userPromise, andThen(process), orElse(handleError)) // ...or compose it
```

> [!tip]
> With `try/catch`, you must handle the outcome *immediately and locally*.
> With `Promise`, you can **defer, transform, and compose** the contract freely.

## Promise ≈ Result\<T, E\>

> [!info]
> If you come from Rust, this maps cleanly:

| Concept                | Rust                   | TypeScript                    |
| ---------------------- | ---------------------- | ----------------------------- |
| Value-level contract   | `Result<T, E>`         | `Promise<T>`                  |
| Control flow reaction  | `match` / `?` operator | `try/catch` / `.then/.catch`  |

`Result<T, E>` encodes the contract into the type system — the compiler enforces it.
`Promise` encodes the contract into the runtime value — you can inspect, pass, and compose it.

`try/catch` in both worlds is just a **mechanism for reacting**, not a contract itself.

## Where This Matters in Practice

### The hidden statefulness problem

When you use `try/catch` in UI code without an abstraction like `useAsyncState`, you don't just lose contract portability — you also lose any contract on *state*:

```javascript
// What is the contract here? Nobody knows.
const [data, setData] = useState(null)
const [loading, setLoading] = useState(false)

try {
  setLoading(true)
  const res = await fetchUser()
  setData(res)
  // forgot setLoading(false)...
} catch (e) {
  // swallowed the error, loading stuck at true forever
}
```

> [!danger]
> `useAsyncState` (or TanStack Query) adds a *second contract* on top of Promise — a contract on UI state: `loading / data / error` transition atomically and predictably.

So the layers are:

```haskell
Promise          → contract on the async value
useAsyncState    → contract on the UI state derived from that value
try/catch        → just the mechanism tying them together
```

> [!note]
> `try/catch` is a perfectly valid tool — but it's a *tool for reacting*, not a *contract for describing*. When you reach for it as a primary abstraction, you're describing behavior implicitly, per call site, with no guarantees to lean on.
>
> `Promise` makes the contract **explicit, portable, and composable**. That's the real difference.