---
description: Learn how to manage Error Pages on your Umbraco Cloud projects.
---

# Error Pages

The Error Pages feature lets you serve a custom HTML page to visitors when your site is unavailable or experiences an error. Pages can be assigned per hostname.

An Error Page is typically shown when an environment is restarting. Many deployments require the app service to restart, so this is also shown at some point during deployments.

## Managing Error Pages tab

This tab lists all uploaded custom HTML pages and the built-in Umbraco Default.

All uploaded error pages are stored in the **Live environment's blob storage**, regardless of which environment they are assigned to. Each page is stored at the path:

```
error-pages/{errorPageId}.html
```

### Upload a page

1. Click **Upload**.
2. Select an `.html` or `.htm` file (max **20 KB**).
3. Optionally enter a description.
4. Click **Upload** to confirm.

### Set a default

Click the **Set** button on any uploaded page. That page becomes the default for all new hostnames added to the project. The current default error page has a checkmark.

### Actions

**Preview and replace**

Hover and click on any of the uploaded error pages to open the preview dialog:

- **Preview tab** — shows the HTML content.
- **Replace tab** — upload a new file version and optionally update the description. You do not have to reassign the page if it was already assigned to some hostnames. 

**Edit name or description**

Click the edit icon on a row to update the display name and description without touching the file itself.

**Assign**

Bulk assign the error page to the current hostnames.

**Delete**

Click the delete icon.

## Hostname Assignments tab

This tab shows every hostname across all environments and which error page it currently uses.

### Filter the list

Use the column header dropdowns to filter by:

- **Environment** — show only hostnames from a specific environment.
- **Error Page** — show only hostnames using a specific page, or those using the default.
- **Domain** — filter by registrable domain (e.g. `mysite.co.uk`).
- **TLD** — filter by top-level domain (e.g. `.co.uk`).

Click **Reset filters** to clear all active filters.

### Actions
**Assign to a single hostname**

Click the assign icon on any hostname row, select a page from the dropdown, and confirm.

**Bulk assign**

1. Check the checkboxes for the hostnames you want to update (or use **Select all** to pick everything currently shown).
2. Click **Assign**.
3. Choose a page from the dropdown and confirm.

**Revert to default**

In the assign dialog, select **Use default (remove custom assignment)**. This removes the custom assignment and makes the hostname fall back to the environment default (or the Umbraco built-in default if none is set).

## Error Page Authoring guidelines

Here are some guidelines to help you build a custom Error Page.
- **Max file size** of 20 KB (HTML + CSS + JS combined)
- Only `.html` or `.htm` files are accepted
- Error pages must be **self-contained** — no external resources (see below)

**Why self-contained?** Cloudflare serves your error page directly from blob storage. When your site is unavailable, external fonts, stylesheets, and scripts (Google Fonts, CDN libraries, etc.) are very likely to fail too, leaving your page broken or unstyled. Everything must be inline.

- Inline all CSS in a `<style>` block.
- Inline all JavaScript in a `<script>` block.
- Use system fonts instead of web fonts: `font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`.
- Inline any icons as SVG markup rather than loading an icon library.

### Recommended meta tags

```html
<meta name="robots" content="noindex, nofollow">
<meta http-equiv="Cache-Control" content="no-cache">
```

The `robots` tag prevents search engines from indexing your error page. The `Cache-Control` tag prevents browsers from caching it, so visitors see a fresh load once your site recovers.

### Reload mechanism

Tell visitors what is happening and give them a way back. 

**Auto-poll (recommended)** — JavaScript that periodically sends a `HEAD` request to the current URL and reloads automatically when the site responds with `200`. Use exponential backoff with jitter to avoid hammering the server, and surface a manual refresh button after a set number of failed attempts:

```js
var MAX_ATTEMPTS = 20;
var attempt = 0;

function getDelay(n) {
  var base   = Math.min(5000 * Math.pow(2, n - 1), 60000);
  var jitter = base * 0.1 * (Math.random() * 2 - 1);
  return Math.round(base + jitter);
}

function poll() {
  attempt++;
  fetch(location.href + '?_poll=' + Date.now(), { method: 'HEAD', cache: 'no-store' })
    .then(function(res) {
      if (res.ok) { location.reload(); return; }
      scheduleNext();
    })
    .catch(scheduleNext);
}

function scheduleNext() {
  if (attempt >= MAX_ATTEMPTS) {
    document.getElementById('refresh-btn').style.display = 'inline-block';
    return;
  }
  setTimeout(poll, getDelay(attempt));
}

setTimeout(poll, 5000); // first check after 5 s
```

**Fallback** — Always include a manual "Refresh page" button for visitors without JavaScript, or once polling is exhausted:

```html
<button id="refresh-btn" onclick="location.reload()" style="display:none">Refresh page</button>
<noscript><button onclick="location.reload()">Refresh page</button></noscript>
```

### Size budget

A typical well-crafted error page stays comfortably within the 20 KB limit:

| Part | Typical size |
|---|---|
| HTML structure + text | ~3–4 KB |
| Inline CSS | ~5–8 KB |
| Inline JS (polling) | ~3–5 KB |
| **Total** | **~12–17 KB** |
