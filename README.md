# test_repo

## `index.html`

`index.html` is a single-file demo page that intentionally shows an insecure `message` event listener. It registers a `window` listener for `postMessage` that never checks `event.origin` and writes the received data straight into the DOM with `innerHTML`, so any origin that can open the page can inject HTML and script into it.

This is a deliberately vulnerable example for security testing. Do not deploy it or copy its pattern into production code. The commented-out block at the bottom of the file shows the safer approach: check `event.origin` against an allowlist and avoid dangerous sinks such as `innerHTML`.

To reproduce the issue:

1. Open `index.html` in a browser.
2. In the page's console, run `window.open('https://example.com', 'attacker');`.
3. In the new tab's console, run `window.opener.postMessage('<img src=x onerror=alert(1)>', '*');`.

The vulnerable page renders the payload and the `alert` fires.
