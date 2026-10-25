# Rules

Guidance for the AI filling in the furniture sale template page. The prompt in the README links here. The page code already handles layout, totals, currency formatting and photo shapes; these rules cover what goes into the fields.

## Writing

- **Intro:** one short, warm paragraph in the seller's voice: why they're selling, who gets the first chance (for example the next tenant), the discount, the date to answer by, and anyone else to contact. First person, like a note from a neighbour. No sales words, no exclamation marks: photos, links and prices speak for themselves.
- **Signature:** the seller's first name only.
- **Item titles:** everyday words ("Sofa bed with storage"), not product names. An optional subtitle says what it's made of ("Wardrobe frame + custom doors").
- **Bought or installed:** month and year.
- **Condition:** one word ("like new", "excellent").
- **Note:** a sentence or two for anything worth knowing: how it's used, what's included, and flaws, said plainly ("A small part has come off, but it isn't visible").
- **Tooltips** (`info`, `conditionInfo`) are only for facts about the item: colour, fabric, model, size, "includes the original remote", why a link points where it does. Condition, flaws and deals go in the note.

## Prices

- **Break each item into its parts** (doors, frame, knobs, shelves), each with its original price.
- **`original`** is the price new, per unit. Leave it out if unknown.
- **`price`** is the seller's price for all units together (`qty` for multiples). `0` shows "Free". Or use **`discount`** (percent off) on the part or the whole item instead; a typed price wins.
- **Electronics:** use today's used price as `original` and set `originalLabel` to "Used price now", so nobody feels misled.
- **Things the seller doesn't own:** list them with `priceText` ("Owned by the landlord") so nobody assumes they're included.
- **`currency`:** the 3-letter code (EUR, USD, GBP…). Plain numbers, no currency signs.

## Links

- Put a part's product page in `link`. If the exact product is gone, link the closest one and explain that in the part's `info`.

## Photos

- 1–3 real photos of the actual item, in the room, best photo first: the whole thing, opened or in use, and one detail.
- Use only photos that really exist: the attached files if you can build them into the page, otherwise the exact file names or links the seller gives. Photo files go in the same folder as the page. Never invent file names.
- Brighter is better: no yellow lamp tint, no dark shadows.

## Privacy

- Use only first names, no address, and contact details only if the seller is fine with strangers seeing them.

---

## How the page works

Not needed for filling in the page. This is for anyone changing the template itself.

### Pricing: show the original, then the deal

- **Big price per item:** the original total crossed out, your price next to it, and a green discount badge ("-64%"). You type each part's original price and either your price or a discount; the page adds up the parts and works out the other number and the badge.
- **Parts list:** each part shows its original price crossed out and your price; "priceText" parts (not for sale) show that text instead and don't count in the totals. `originalLabel` renames the original-price column.
- **Quantities:** "287 € × 2 = 574 €" (or "$287 × 2 = $574": each currency is written the way readers expect it). The reader can check the maths.
- **Totals** appear only when an item has more than one part.
- **Round numbers.** No cents.

### Layout: calm and scannable

- **One column of items**, each a block: photos on the left (300 px), details on the right.
- **Inside each item, top to bottom:** title and subtitle (bought/condition on the right) → big price → parts list → note.
- **Lots of space** between items. No boxes, no dividers between items, no summary table at the end.
- **Typography:** one sans-serif (Geist), near-black on warm off-white (#F7F7F5). Bold for titles, prices and totals.
- **Colour** is used for one thing: green means "your price" (tags, badge). Everything else is black and off-white.
- **Rounded corners:** 6 px on photos and price tags; the discount badge is a full pill.

### Photos

- **Automatic layout, even shapes:**
  - Photos keep their order.
  - One photo: shown in its own shape, uncropped.
  - Two or more photos: two per row, and no photo is ever enlarged; with an odd number the last one sits alone in the last row.
  - Each row picks its own shape (4:3, 1:1 or 3:4) from just its own photos, using their real proportions: the shape that crops the worse-hit photo least, then the one that crops least in total; a full tie goes to 1:1. A lone last photo gets the shape closest to its own. Later photos never change an earlier row.
  - Height depends only on the number of rows and each row's own shape; rows never affect each other. There is no manual override.
  - With two or more photos, they're cropped to fill their shape on the page; the full-screen viewer always shows the whole original.
  - Why: items line up, mixed shapes never stack or grow out of control, and adding a photo never re-crops the ones before it.

### Interactivity: only what helps

- **Full-screen photo viewer** with swipe, arrow keys and a photo counter.
- **Tooltips** that never get cut off at the screen edge.
- **Language switch** (e.g. EN / ES) in the top right when the reader might prefer another language. The first language listed is the default. The choice is remembered. With one language, there's no switch.
- **Tap to call, message or email** for whichever contacts you include.
- Nothing else. No forms, no cart, no accounts.

### Phones (most readers open it there)

- Items stack: photos on top, filling the full width, then the details.
- Bought/condition move under the subtitle.
- The parts table turns into a list: part name on one line, old and new price below it. No column headings, no lines.
- Nothing scrolls sideways.

### The template: how it's built

- **One HTML file.** The whole page is one file: it opens in any browser, AI apps that show web pages can display it, and it can be uploaded to any free host. Nothing to install or build. All content sits in one data block; the page computes totals, discounts and layout from it, so any AI, free or paid, or a person by hand can fill it in.
- **The template page (`template.html`) has no data.** The example page (`index.html`) is built by `scripts/build-example.py`, which puts the example data (`example/data.json`) into the template page.
- **Photos are usually separate files.** The data lists each photo's file name or web link, and the page loads it from there. A seller's photos are saved in the same folder as their page; the example's photos are in `example/photos/`. Why: most AI chats can see an uploaded photo but can't put it into the page; some apps and plans can, so the prompt asks the AI to say what it can do.
- **Labels live in their own block** (`sale-labels`), with one set per language. These are the fixed words on the page that the seller never types, like "Part", "Original price", "Price for you", "Total", "Free" and "Close photo". To add a language, add a translated set; the page code never needs to change.

### What you can fill in

The comment above the data block in `template.html` lists every field and what it does.

### Files

| File | What it's for |
| --- | --- |
| [`template.html`](template.html) | Template page |
| [`RULES.md`](RULES.md) | Rules for the AI filling in the page, and how the page works |
| [`example/data.json`](example/data.json) | Example data |
| [`example/photos/`](example/photos/) | Example photos |
| [`index.html`](index.html) | Example page |
| [`scripts/build-example.py`](scripts/build-example.py) | Builds `index.html` from `template.html` and `example/data.json` |
