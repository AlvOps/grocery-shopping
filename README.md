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

### In the store
- **Sorted by aisle** — items are grouped into Fruit & Veg, Bread, Meat & Fish, Dairy, Frozen, Pantry, Spices, Drinks, Snacks and Household using a keyword map for Spanish, German and English product names. Toggle to "As added" order any time.
- **Search/filter** for when the list gets long and you're standing in the aisle.
- **Big thumb-sized checkboxes** — tapping one moves the item to the "In the basket" group.
- **Live counter and progress bar** — `3 of 12` plus the value of what's left to buy.
- **Item sheet** — tap a product to set quantity, price per unit and aisle. Line totals add up in the header.
- **Undo** on every delete and on both clear actions.
- **Share** — sends the unchecked list via the native share sheet, or copies it to the clipboard.
- **Keep screen on** — requests a Screen Wake Lock so the phone doesn't lock mid-shop.
- **Mobile-first layout** — sticky add bar at the bottom within thumb reach, sticky header, 16px inputs (no iOS zoom), safe-area padding, no hover effects on touch, no particle canvas on phones to save battery.

### Data
- Everything lives in `localStorage` under `shoppingListV2`; the old `shoppingList` key is migrated automatically on first load.
- No accounts, no server, no network calls — works fully offline once loaded (the only external request is the Poppins font).

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
