# hyperapp-ssr

Server-side rendering helpers for [Hyperapp](https://github.com/jorgebucaran/hyperapp).

This package was extracted from [`vite-plugin-hyperapp`](https://github.com/zaceno/vite-plugin-hyperapp). It has no build-tool or DOM dependency; it runs Hyperapp's `app({ init })` synchronously and serializes the resulting virtual tree to HTML.

## Install

```sh
npm install hyperapp-ssr
```

`hyperapp` is a peer dependency.

## Render an app

```js
import { renderToString } from 'hyperapp-ssr'
import { init, view } from './app.jsx'

const html = renderToString({ init, view })
```

`renderToString` runs the `init` action through Hyperapp, waits for synchronous initialization effects to dispatch, and then renders the view for the resulting state. It does not start subscriptions or use the DOM.

Because `init` is the same action the browser app uses, synchronous effects in `init` also run on the server. Guard browser-only APIs before scheduling them:

```js
const focusInput = () => document.querySelector('#new-item')?.focus()

export const init = () => [
  { items: [] },
  !import.meta.env.SSR && focusInput,
]
```

## Serialize an existing virtual tree

```js
import { vdomToHtml } from 'hyperapp-ssr'

const html = vdomToHtml(view(state))
```

`vdomToHtml` accepts an existing Hyperapp `VNode` and serializes it without starting an app. It escapes text and attributes, handles boolean and SVG attributes, serializes style objects, and supports raw-text elements such as `<script>` and `<style>`.

## Package exports

| Import | Purpose |
| --- | --- |
| `hyperapp-ssr` | `renderToString`, `vdomToHtml`, and `ServerAppProps` |
| `hyperapp-ssr/vdom-html` | `vdomToHtml` only |

## License

MIT
