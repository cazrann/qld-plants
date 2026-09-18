# Queensland Native Plants — TRMNL Private Plugin

Cycles through 116 QLD native plant cards automatically, switching to a new
plant every 6 hours (4 times a day). No server or hosting required —
everything runs as a **Static** private plugin.

There are two kinds of entries in `plants.json`:

- **56 "reconstructed" cards** — structured data (name, range, edible parts,
  taxonomy) plus a cropped illustration in `images/`; the Liquid template
  lays out the card at render time.
- **60 "pre-rendered" cards** — complete, already-composited 800x480
  monochrome PNGs in `full-cards/`, produced by the separate, more rigorous
  `native plant cards ChatGPT/` pipeline (real Bailey/botanical-book
  illustrations with verified page/figure citations baked into the image).
  These entries just carry a `full_card_url`; the template detects that field
  and displays the image directly, full-bleed, instead of reconstructing a
  layout.

## What's in here

- `plants.json` — all 116 plants. The 56 older entries have name, scientific
  name, range, edible parts, taxonomy and an `illustration_url`; the 60
  newer entries have name, scientific name and a `full_card_url`.
- `images/` — cropped illustrations for the 56 reconstructed cards.
- `full-cards/` — the 60 complete pre-rendered card PNGs.
- `markup-full.liquid` — the full-screen (800x480) layout. Branches on
  whether the current plant has a `full_card_url`.
- `markup-half-horizontal.liquid` — an optional compact layout if you ever
  want this plugin sharing the screen in a Mashup (also branches the same
  way, though the pre-rendered cards will be shrunk to fit).

## Setup (5 minutes)

1. On your TRMNL dashboard, go to **Plugins → Private Plugin → Add New**
   (requires the Developer add-on).
2. Name it something like `QLD Native Plants`.
3. **Strategy**: choose **Static**.
4. Open `plants.json` in this folder, copy the whole contents, and paste it
   into the Static Data field.
5. Open `markup-full.liquid`, copy the whole contents, and paste it into the
   **Markup (Full)** editor tab.
6. (Optional) Do the same with `markup-half-horizontal.liquid` in the
   **Markup (Half horizontal)** tab if you want it available for mashup
   layouts.
7. Save, then add the plugin to a **Playlist** so it shows on your device.

That's it — no webhook, no polling URL, nothing to host.

## How the rotation works

The template computes the current Unix epoch time (`'now' | date: '%s'`),
divides it into 6-hour slots (21,600 seconds each), and takes that slot
number modulo the number of plants — so the card changes 4 times a day (at
each 6-hour boundary) and cycles through the whole list before repeating.
The footer (on the reconstructed-style cards) shows "Card X of 116" so you
always know where you are in the rotation; the pre-rendered cards already
have their own citation baked into the image instead.

Note: this only *computes* a new card every 6 hours — your TRMNL device still
needs to actually refresh within that window to pick it up. If your device's
refresh interval is set to longer than 6 hours, lower it (in the device's
schedule settings) or you'll see fewer than 4 changes a day.

## About the illustrations

Each card's line-art was cropped out of your original reference image (text
panel removed, since the Liquid template reconstructs that side) and lives in
`images/` in this repo. `plants.json` points at each one via
`illustration_url` using a `raw.githubusercontent.com` link, since TRMNL
renders from its own servers and can't reach files on your computer directly.

If you ever move or rename the repo, update the base URL in every
`illustration_url` field to match.

## Adding more plants later

Just append another object to the `plants` array in `plants.json` (same
shape as the existing ones) and update the Static Data in the TRMNL
dashboard — the rotation math (`modulo: plant_count`) adjusts itself
automatically.
