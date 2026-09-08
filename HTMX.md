# HTMX Basics

## 1. What is HTMX

`htmx` is a small, dependency-free JavaScript library (~14kb min.gz'd) that lets you access modern browser features — AJAX, CSS Transitions, WebSockets, and Server-Sent Events — directly from HTML attributes, without writing JavaScript.

The core idea: a normal `<a href="/blog">` tag already tells the browser "on click, GET `/blog` and load the response." HTMX generalizes this same hypertext idea to *any* element and *any* event:

```html
<button hx-post="/clicked"
    hx-trigger="click"
    hx-target="#parent-div"
    hx-swap="outerHTML">
    Click Me!
</button>
```

This says: "On click, POST to `/clicked`, and replace the element with id `parent-div` using the response."

Key attributes to know:

- `hx-get` / `hx-post` / `hx-put` / `hx-patch` / `hx-delete` — issue the corresponding HTTP request
- `hx-trigger` — which event fires the request (defaults: `change` for inputs, `submit` for forms, `click` for everything else)
- `hx-target` — CSS selector for where the response gets swapped in (defaults to the requesting element itself)
- `hx-swap` — how the response is inserted (`innerHTML`, `outerHTML`, `beforeend`, `afterend`, etc.)

Unlike typical JS frameworks, the server responds with **HTML fragments**, not JSON. HTMX just swaps that HTML into the DOM.

⚠️ Since HTMX increases what plain HTML attributes can do, any untrusted content injected into the page must be escaped — otherwise `hx-*` attributes become an XSS risk. Always escape user content, and use `hx-disable` around any raw/unescaped HTML you must render.

## 2. When to Use It

HTMX fits well when:

- You're building a **server-rendered** app (Django, Flask, Rails, Express, Laravel, etc.) and want interactivity without spinning up a full SPA frontend
- You want **progressive enhancement** — `hx-boost` upgrades normal links/forms to AJAX, but they still work if JavaScript is disabled
- The UI is mostly **CRUD-style**: forms, live search, infinite scroll, polling, partial page updates
- You want to avoid a build step (webpack/vite) — a single `<script>` tag is enough
- You want to keep logic close to the HTML (`Locality of Behaviour`) instead of splitting it across component files
- The team is more comfortable with backend templating than a JS component model

It's a weaker fit when you need a highly stateful, offline-capable, or heavily animated client-side app (e.g. a Figma-like editor), since HTMX assumes the server is the source of truth for most UI state.

## 3. How It Differs from React

| Aspect | HTMX | React |
|---|---|---|
| Core model | Server renders HTML; client swaps HTML fragments into the DOM | Client renders UI from a JS virtual DOM/state tree |
| Source of truth | Usually the server/database | Usually client-side state (`useState`, Redux, etc.) |
| Data format exchanged | HTML fragments | Typically JSON, rendered into HTML by React |
| Language | HTML attributes (`hx-get`, `hx-trigger`, ...) | JavaScript/JSX components |
| Build tooling | None required — one script tag | Usually needs a bundler (Vite, webpack) and JSX transform |
| State management | Lives on the server between requests | Lives in the browser (component state, stores) |
| Interactivity granularity | Whole elements/fragments swapped per request | Fine-grained reactive re-renders of components |
| Learning curve | Small — mostly just HTML attributes | Larger — JSX, hooks, component lifecycle, state patterns |
| Best for | Content-driven, server-rendered, CRUD-heavy apps | Complex, highly interactive, client-heavy apps (dashboards, editors) |

In short: HTMX pushes work back to the server and treats HTML itself as the interactivity layer (`HATEOAS`-style), while React moves rendering and state into the browser and treats the UI as a function of JS state. They can even be combined — e.g. HTMX for most of a page, with a React "island" for one complex widget.
