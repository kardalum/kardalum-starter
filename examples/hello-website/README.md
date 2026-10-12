# Hello website

A normal website on Kardalum's headless API: one file, no framework, no
npm dependencies. `server.js` fetches published content server-side
(your API key never reaches the browser) and renders plain HTML. The
home page lists every content item grouped by document type; click one
to see its fields.

## Run it

1. Run your Kardalum server with `KARDALUM_HEADLESS=true` (start one with
   `npm create kardalum@latest my-site` if you have none yet).
2. In the admin, go to **Settings -> API Keys** and create a key.
3. Copy `.env.example` to `.env`, put your server's origin and the key in
   it, then:

```sh
node server.js         # open http://localhost:4600
```

## Make it say hello world

In the Kardalum admin:

1. **Document types**: create a type, for example `Page` with a text
   field `title` and a rich-text field `body`.
2. **Content**: create a `Page`, type "Hello world", **publish** it.
3. Refresh the site. Your page is listed under **Page**; click it to see
   the fields. No rebuild, no cache to clear.

From here, view source on `server.js` (the whole site is about 120 lines)
and reshape the HTML however you like.

## How the code works (`server.js`, top to bottom)

**1. Load `.env`**: ten lines that read `KEY=value` pairs into
`process.env` (real shell exports win). No dotenv dependency needed.

**2. Configuration**: three values.

| Variable | Meaning |
|---|---|
| `KARDALUM_API_URL` | Your Kardalum server's origin. All requests go to `<origin>/api/v1/...`. |
| `KARDALUM_API_KEY` | An API key from **Settings -> API Keys**, sent as `Authorization: Bearer <key>`. Keys are read-scoped by default, which is all this example needs. Keep it secret either way. |
| `PORT` | Where this site listens (default `4600`). |

**3. `kardalum(apiPath)`**: the only piece of "API client": `fetch` with
the bearer header, throw on non-2xx, return JSON. Everything else is HTML.

**4. `home()`**: two requests in parallel, then a grouped list.

| Request | Returns |
|---|---|
| `GET /api/v1/document-types?limit=100` | `{ items: [{ id, name, ... }], nextCursor }`: every document type the instance has. |
| `GET /api/v1/content?limit=100` | `{ items: [{ id, parentId, path, depth, sortOrder, documentTypeId, publishedAt, url }], nextCursor }`: every **published** content node, in tree order, without field data. |

The page maps each item's `documentTypeId` to its type name and groups by
it. `nextCursor` is how you page through bigger sites: pass it back as
`?cursor=...` until it comes back `null` (this example stops at 100).

**5. `item(id)`**: `GET /api/v1/content/<id>` returns the full item,
`{ id, documentTypeId, locale, publishedAt, version, data, url,
extensions? }` (`url` is the page's address, `null` when it has none;
`extensions`, keyed by plugin id, appears only when an active plugin adds
fields to the response). `data` is
an object keyed by field id (`title`, `body`, ...) whose values are
whatever the field's data type stores: strings, numbers, or nested
objects and arrays for rich fields. The page renders strings as text and
anything structured as pretty-printed JSON, so you can see the real shape
before you design markup for it.

Two more endpoints you'll want as the site grows:
`GET /api/v1/content/by-path?path=/about` (resolve a URL to an item) and
`GET /api/v1/content/<id>/children` (one level of the tree, paginated).

**6. The HTTP server**: one `createServer` with two routes (`/` and
`/item?id=...`). Anything thrown above becomes a readable 500 page instead
of a crash. `esc()` HTML-escapes every value that came from the API;
keep doing that in your own markup.

## Why server-side

The API key is a secret for *your* instance. Fetching on the server keeps
it out of the browser.

The full headless API reference is at <https://docs.kardalum.com>.
