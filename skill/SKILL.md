# Obscura Browser

A headless browser for AI agents that beats bot-blocking. Built in Rust, 36k GitHub stars, drop-in replacement for headless Chrome with Puppeteer/Playwright. Uses 1/15th the memory of Chrome and Cloudflare itself ported it to Workers.

Use this when a normal fetch gets blocked, returns nothing, or takes forever — course catalogs, Amazon/Etsy listings, competitor shops, directory sites.

## Install (one time)

```bash
# Already installed via Homebrew on this Mac:
obscura --version
# If not: brew install h4ckf0r0day/obscura/obscura
# Or download the binary: https://github.com/h4ckf0r0day/obscura/releases
```

## Commands

### Fetch one page
```bash
# Get the page title
obscura fetch https://example.com --eval "document.title"

# Get all links as JSON
obscura fetch https://example.com --dump links

# Get readable text
obscura fetch https://example.com --dump text

# Save to a file
obscura fetch https://example.com --dump text --output page.txt

# Take a screenshot
obscura fetch https://example.com --screenshot page.png
```

### Scrape many pages in parallel
```bash
obscura scrape https://shop-a.com https://shop-b.com \
  --concurrency 10 \
  --eval "document.querySelector('h1').textContent" \
  --format json
```

### Start a CDP server (for Puppeteer/Playwright)
```bash
obscura serve --port 9222 --stealth
```

## Use cases for John

- **College:** Pull course schedules, assignment pages, bookstore prices, professor info — sites that block normal scrapers.
- **Golf-tool business:** Scrape competitor prices on Amazon/Etsy, check what similar divot tools and ball markers sell for, monitor supplier listings.
- **General:** Any page that returns empty or times out with a normal fetch.

## Flags cheat sheet

| Flag | What it does |
|------|-------------|
| `--dump text\|html\|links\|markdown\|assets\|original` | What to extract |
| `--eval "JS expression"` | Run JavaScript and print the result |
| `--screenshot FILE` | Save a PNG |
| `--output FILE` | Write output to a file |
| `--concurrency N` | Parallel workers (scrape) |
| `--stealth` | Anti-detection mode |
| `--timeout SECONDS` | Max navigation time |
| `--proxy URL` | HTTP/SOCKS5 proxy |

## Notes
- macOS: use `--no-gpu` if you hit GGML_ASSERT crashes.
- Private/LAN addresses are blocked by default; use `--allow-private-network` for local dev servers.
- `scrape` needs `obscura-worker` on PATH. On this Mac it looks in `/opt/homebrew/bin/` (protected), so symlink it to `~/bin/`:
  `ln -sf /opt/homebrew/bin/obscura ~/bin/obscura-worker` (and make sure `~/bin` is in your PATH).
- Full docs: https://github.com/h4ckf0r0day/obscura