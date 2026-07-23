# XSS Lab

Intentionally vulnerable static site used to test XSS scanners and manual
testing techniques against **DOM-based** cross-site scripting patterns.

> ⚠️ **This site is deliberately insecure.** Every vulnerability here is intentional and exists for security testing and learning purposes only.
> Do not reuse this code in production.

## Why this Lab exists?

Server-side reflected XSS test targets (like testphp.vulnweb.com) are plentiful, but static-site DOM XSS is a different attack surface: there's no backend to reflect input, so the vulnerability lives entirely in
client-side JavaScript reading `location.search` or `location.hash` and writing it into the DOM unsafely.

This repository is a small, self-contained page with a few classic vulnerable patterns side by side with a fixed/safe equivalent, for comparison.

## Vectors included

| # | Sink | Source | Notes |
|---|------|--------|-------|
| 1 | `innerHTML` | `?q=` query param | Classic query-string DOM XSS |
| 2 | `innerHTML` | `#` URL fragment | Never sent to the server — purely client-side, invisible to server logs/WAFs |
| 3 | `document.write` | `?name=` query param | Different sink behavior than `innerHTML` (can inject raw `<script>` tags) |
| 4 | `textContent` | `?safe=` query param | **Safe** — same input, properly escaped, shown for comparison |

## Usage

Visit the deployed page and append a payload like:

```bash
https://xss-lab.pages.dev/?q=<img src=x onerror=alert(1)>

https://xss-lab.pages.dev/?name=<script>alert(2)</script>

https://xss-lab.pages.dev/#<img src=x onerror=alert(3)>
```

## Dalfox

Or scan with [Dalfox](https://dalfox.hahwul.com/)

```bash
dalfox scan "https://xss-lab.pages.dev/?q=<img src=x onerror=alert(1)>" --deep-scan
```

## Deployment

This is a static site with no build step. Deployed via Cloudflare Pages
directly from this repository.

## License

MIT License, please check [LICENSE](./LICENSE)
