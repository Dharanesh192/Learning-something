# Thing's that I learn
The complete motive of this repo is to `record the things that I learn throughout my journey`. This are the small skill that I need to remember over the long time. This readme tells the `one line of each thing` that I'm going to learn and they is a `dedicated file for each`. Then each file is like a quick refreshment in that topics, So feel free to learn with me 😁

And finally I mark the topic with this (⚠️). If you are at `your starting point of learning and exploring new things` you must need to learn its basics. And I describe each repo based on the order that I create them.

---

# Kwallet used in Linux
`Simplt workflow`
1.  Application like `VS code `need to store the `user login` details in the `system`
2.  So first `VS code encrypt` the `user detail` and send it to the `Kwallet` through `System API`
3.  The `Kwallet` again encryption data received from the `API` for safety perpose
4.  Then it `store the encrypted` in its `own database` in the `disk`
5.  When the `VS code wants the data` it ask the `Kwallet` then it `decrypt it` and give it to the `VS code`.


---

# ⚠️ Basic Git commands
`Simple workflow`
1.  `git init` or `git clone <url>` starts a `local repository`, either fresh or copied from a `remote source`
2.  `git add <file>` `stages` a changed file, taking a `snapshot` in preparation for the `next commit`
3.  `git commit -m "message"` `permanently records` the staged snapshot into `version history` with a description
4.  `git remote add origin <url>` `connects` the local repo to a `remote GitHub repository` so commits can be shared
5.  `git push` `uploads` local commits to the `remote`, while `git pull` `fetches and merges` remote changes back down; `git branch`/`checkout`/`merge` handle working on and combining separate `lines of development`

---

# `pacman -Syu` system upgrade (Arch Linux)
`Simple workflow`
1.  `-S` tells `pacman` to operate on the `sync databases` (install, update, search)
2.  `-y` `refreshes` the local package database by downloading `fresh package lists` from the mirrors
3.  `-u` `upgrades` every installed package to the `latest version` found in that refreshed database
4.  Running only `pacman -Sy <package>` without `-u` causes a `partial upgrade`, which can `break dependencies` since some libraries stay outdated while others jump ahead
5.  Always run the `full pacman -Syu` together (never split `-Sy` and `-u` apart) to keep the `rolling-release system` fully consistent; AUR helpers like `yay -Syu` do the same for `AUR packages` too

---

# ⚠️ Markdown (.md) basics
`Simple workflow`
1.  A `.md` file is just `plain text` with special symbols like `#`, `*`, `-`, `` ` `` that mark up `structure`
2.  A `renderer` (GitHub, VS Code, Obsidian, `mermaid.live`, etc.) reads that `plain text` and turns it into `formatted output`
3.  `Core syntax` — headings, lists, links, images, tables, code blocks — renders `almost everywhere`
4.  `Extended syntax` like `Mermaid diagrams`, `math (LaTeX)`, `task lists`, and `collapsible sections` only renders if the tool `supports GFM/extensions`
5.  Diagrams written as `mermaid code blocks` (flowchart, class diagram, ER diagram, etc.) can be previewed live for free at `mermaid.live` or right inside `VS Code` with the `Mermaid extension`, no paid plan needed

---

# ⚠️ JSON (.json) basics
`Simple workflow`
1.  A `.json` file is just `plain text` built from two shapes — `objects {}` and `arrays []` — nested inside each other
2.  A `parser` (built into every language — `json.load()` in Python, `JSON.parse()` in JS, etc.) reads that `plain text` and turns it into `native data` (dicts/objects, lists/arrays)
3.  `Core syntax` — key/value pairs, strings, numbers, booleans, `null`, nested objects, arrays — is the `entire spec`, no variants, renders/parses `identically everywhere`
4.  Unlike Markdown, there's `no "extended syntax"` — `no comments`, `no trailing commas`, `no unquoted keys`; any tool that deviates (JSON5, JSONC) is a `different format`, not an extension
5.  A `.json` file can be validated live for free at `jsonlint.com` or `jsonformatter.org`, or right inside `VS Code` (built-in, flags syntax errors as you type), no paid plan needed

---

# HTTP status codes
`Simple workflow`
1.  Every `request` sent by a browser/app to a `server` gets back a `3-digit status code` telling it `what happened`
2.  The `first digit` shows the `category` — `1xx` info, `2xx` success, `3xx` redirect, `4xx` client error, `5xx` server error
3.  Common ones to remember: `200` = OK, `201` = Created, `301`/`302` = moved/redirected, `400` = bad request, `401` = need to log in, `403` = forbidden, `404` = not found, `429` = too many requests
4.  `4xx` codes mean `you (the client) made a mistake`; `5xx` codes mean `the server broke`, not you
5.  Full breakdown with every code and its meaning is in `HTTP_status_codes.md`

---

## HTMX basics
`Simple workflow`
1.  `htmx` is a small, dependency-free `JavaScript library` that lets HTML attributes trigger `AJAX`, `WebSockets`, and `SSE` directly — no separate JS needed
2.  Attributes like `hx-get`, `hx-post`, `hx-trigger`, `hx-target`, and `hx-swap` map straight to the HTTP verb, the firing event, and where/how the response gets swapped into the DOM
3.  The server responds with `HTML fragments`, not `JSON` — htmx just swaps that HTML into the page, unlike typical JS frameworks
4.  Fits best for `server-rendered apps` (Django, Flask, Rails, Express) needing interactivity without a full `SPA` frontend or a build step
5.  Differs from `React`: htmx keeps the `source of truth` on the server and swaps HTML fragments; React renders UI from client-side `JS state` — the two can even be combined (htmx page + React "island")
6.  Since `hx-*` attributes expand what raw HTML can do, any untrusted content must be escaped to avoid `XSS`
7.  Official docs: `htmx.org/docs` — worked examples at `htmx.org/examples`

8.  ---
