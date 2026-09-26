# ♠ 21 — Blackjack in under 2 KB

A complete blackjack game in a **single 1,972-byte HTML file** — no build step, no dependencies, no assets.

**▶ Play it live: https://atom-lovat.vercel.app** — or just open `index.html` and play offline.

## The size, precisely

| Measurement | Size |
|---|---|
| **Uncompressed** (the real file, `wc -c`) | **1,972 bytes** ✅ under 5 KB |
| gzip | 1,115 bytes |

Measured with `wc -c index.html` and `gzip -c index.html | wc -c`. No compression tricks needed — the raw file is ~40% of the budget.

## How to play

- **Deal** starts a round; **Hit**/**Stand** play it; **2×** doubles down (first two cards only).
- **−5 / +5** adjust the bet (clamped to your bankroll).
- Blackjack pays 3:2, dealer stands on 17, ties push.
- Go broke and the house kindly stakes you a fresh $100.

## The trickery

Hand-golfed — no minifier, everything earns its bytes:

- **No optional closing tags**: `</p>`, `</button>`, `</h1>` omitted — consecutive `<p>`/`<button>` elements auto-close each other per the HTML parser rules. Buttons are `<button id=D>Deal<button id=H>Hit…`
- **Unquoted attributes**: `class=x`, `id=t` — legal whenever the value has no spaces.
- **No `html`/`head`/`body`**; the doctype only exists to keep the browser out of quirks mode.
- **Emoji-free card suits**: literal UTF-8 `♠♥♦♣` beats `\u2660…` escapes (2–3 bytes per suit instead of 6).
- **One-expression hand scoring**: `'A23456789'.indexOf(rank)+1||10` scores every rank — the failed lookup (−1+1=0) becomes the 10 for face cards; aces counted separately, then promoted 11→1 while the total would bust: `for(;a--&&w+10<22;)w+=10`.
- **Infinite shoe**: cards are drawn `rank + suit` at random rather than tracking a deck — smaller than a shuffle and the game never runs out.
- **Zero IDs declared**: element IDs (`t`, `d`, `y`, `q`, `D`…) become global variables automatically, so `t.innerHTML=…` needs no `getElementById`.
- **Comma operators** pack sequences into single expressions; **short-circuit `&&`/`?:`** replace `if` blocks; arrow bodies stay brace-free.
- **CSS is about a quarter of the file** — a playing-card look from one `<i>` style plus a red-suit `.x` class, monospace everything, and `border-radius` for card corners.

## Fair play

Nothing is downloaded, fetched, or unpacked: what you see is a self-contained file that works offline. The 1,972 bytes above *are* the whole program.
