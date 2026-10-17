# 🍅 Grocery Shopping list

A fast, offline-first grocery list made for one hand and a shopping cart: paste a whole list at once, get it sorted by store aisle, tick things off, and see what you're spending.

Test it **HERE** ➡️ **https://alvops.github.io/grocery-shopping/**

## ✨ Features

### Adding items
- Type a product and press Enter / Add.
- **Paste a whole list** — one product per line, or comma separated. Bullets, numbering (`1.`, `-`, `•`), blank lines, stray numbers and duplicates are stripped automatically.
- Pasting a multi-line block straight into the input field imports it directly, no dialog needed.
- `Cebolla ×2` sets a quantity while adding (also works in bulk paste).
- Duplicate detection is case- and accent-insensitive (`Sésamo` = `sesamo`).

### Voice and barcode
- **🎤 Voice add** — tap Voice and speak: *"leche, huevos y pan"* is split into three items. Continuous listening, live interim transcript, handles Spanish/German/English separators (`y`, `und`, `and`). Chrome, Edge and Safari only (Web Speech API); the language defaults to `es-ES` and can be changed via `localStorage.sl_voiceLang`.
- **📷 Barcode scan** — opens the rear camera and reads EAN-8/13 and UPC-A/E codes (native `BarcodeDetector` where available, ZXing elsewhere), then resolves the product online instead of just adding a number:
  - **names** come from Open Food Facts first, then Open Beauty Facts, Open Pet Food Facts and Open Products Facts — so a cosmetic or a cleaning product resolves too. German product names win when the database has one, since that is the label on the shelf here.
  - **the shelf photo, brand and Nutri-Score** are stored with the item and shown as a thumbnail in the list and as a product card in the detail sheet.
  - **a crowdsourced price** from [Open Prices](https://prices.openfoodfacts.org) for the most recent matching receipt — store, city and date included, with a one-tap "Use €x,xx" button. It is never applied silently: it may be an old price from another city.
  - Unknown codes are added as `Item <code>`. The sheet then offers a "search it" link and tells you the code is missing from the free databases — **type the name once and the app remembers that barcode forever** (`sl_ean_names`), so you never have to look it up twice.

### Barcode data: what is free and what is not
| Source | Cost | Coverage | CORS from a browser |
| --- | --- | --- | --- |
| Open Food / Beauty / Pet Food / Products Facts | free, no key | branded goods: good, Lidl/Edeka private label: patchy | ✅ |
| Open Prices | free, no key | sparse, no country filter | ✅ |
| UPCitemdb trial | 100 req/day | no EU data in the trial (tested: German EANs return nothing) | ❌ locked to their domain |
| Barcode Lookup | ~$99/mo | broad, incl. EU retail | ❌ |
| Go-UPC | ~$75/mo | broad | ❌ |

A static page can't call the paid ones directly (no CORS) and can't hide their key. If store-brand coverage becomes a real problem, deploy a tiny proxy (Cloudflare Worker) holding the key as a secret, then point the app at it **without touching the code**:

```js
localStorage.setItem('sl_proxy', 'https://your-worker.example.workers.dev/lookup')
```

The app calls `${proxy}?ean=<code>` first and expects `{ "name": "...", "brand": "...", "quantity": "...", "image": "...", "grade": "..." }`; anything it returns wins, and the free chain is the fallback.
- Browsers without `BarcodeDetector` (Safari on iPhone/iPad, Firefox) get the same live camera scan through **ZXing**, loaded on demand from jsDelivr the first time you tap Scan — nothing is downloaded until then. Verified decoding an EAN-13 on the fallback path.
- No camera permission or no camera at all? The scanner drops to a numeric field — type the digits and get the same lookup.

### In the store
- **Sorted by aisle** — items are grouped into Fruit & Veg, Bread, Meat & Fish, Dairy, Frozen, Pantry, Spices, Drinks, Snacks and Household using a keyword map for Spanish, German and English product names. Toggle to "As added" order any time.
- **Search/filter** for when the list gets long and you're standing in the aisle.
- **Big thumb-sized checkboxes** — tapping one moves the item to the "In the basket" group.
- **Live counter and progress bar** — `3 of 12` plus the value of what's left to buy.
- **Item sheet** — tap a product to set quantity, price per unit and aisle. Line totals add up in the header.
- **Undo** on every delete and on both clear actions.
- **Share** — sends the unchecked list via the native share sheet, or copies it to the clipboard.
- **Keep screen on** — requests a Screen Wake Lock so the phone doesn't lock mid-shop.
- **🪙 Coin** — a €1 coin toss for when the list can't decide: the screen dims, the coin tumbles up (growing at the
  apex, shrinking as it falls), lands on a face and announces `Ha salido CARA` / `Ha salido CRUZ`, with "Otra vez"
  and tap-outside-to-close. Respects `prefers-reduced-motion` (instantly lands on the result).
- **Mobile-first layout** — sticky add bar at the bottom within thumb reach, sticky header, 16px inputs (no iOS zoom), safe-area padding, no hover effects on touch, no particle canvas on phones to save battery.

### Data
- Everything lives in `localStorage` under `shoppingListV2`; the old `shoppingList` key is migrated automatically on first load.
- Barcode lookups are cached under `sl_ean`, so a product scanned once resolves offline afterwards.
- No accounts, no server. Only network calls: the Poppins font, and Open Food Facts when you scan a barcode.

### Sending the list to Telegram
Use **Share** — the native share sheet already lists Telegram. On browsers without it, the list is copied to the clipboard and the toast offers a **Telegram** button that opens `t.me/share/url` with the text pre-filled.

That deep link is deliberate: **a static GitHub Pages site cannot keep a bot token secret.** Anything in the repo or the served JS is public, and Pages has no server to hold an environment variable. If you ever want a real "send automatically" button, put the token in a server-side secret — e.g. a Cloudflare Worker with `TELEGRAM_BOT_TOKEN` stored as an encrypted secret and the page calling that Worker — and never in this repo.

## 🚀 Getting started

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

Then add it to your phone's home screen — it runs full-screen as a standalone web app.

## 🛠️ Tech

Plain HTML, CSS and vanilla JavaScript in a single file. No build step, no dependencies.

## 📄 License

MIT — see [LICENSE](LICENSE).
