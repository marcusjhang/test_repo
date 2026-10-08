# Insecure `postMessage` Demo

A small, self-contained demonstration of a **common web security vulnerability**:
a `message` event listener that does **not** validate `event.origin` and injects
untrusted data straight into the DOM via `innerHTML`.

> ⚠️ **This code is intentionally vulnerable.** It exists for education and
> security training only. **Do not** copy the listener in `index.html` into any
> real application.

## What's here

| File         | Purpose                                                        |
| ------------ | ------------------------------------------------------------- |
| `index.html` | The vulnerable demo page and reproduction steps.              |

## The vulnerability

`index.html` registers a `window` `message` listener that:

1. Accepts messages from **any** origin (it displays `event.origin` but never
   checks it).
2. Writes the received data directly to `innerHTML`, a dangerous sink that
   executes injected markup such as `<img src=x onerror=...>`.

Together these allow a cross-origin page to run arbitrary script (DOM-based XSS)
in the context of the demo page.

## Running it

Open `index.html` in a browser (serving it over any origin works). The page
includes step-by-step instructions for reproducing the injection from a second,
different-origin tab.

## The secure pattern

A correct listener validates the origin against an allow-list and avoids
dangerous sinks:

```js
window.addEventListener('message', (event) => {
  const allowed = new Set(['https://trusted.example']);
  if (!allowed.has(event.origin)) return; // strictly validate origin

  // Prefer structured, expected message shapes and safe rendering
  // (e.g. textContent) over innerHTML.
});
```

The same snippet is kept, commented out, at the bottom of `index.html` for
reference.
