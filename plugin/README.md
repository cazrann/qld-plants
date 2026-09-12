# Queensland Native Plants — TRMNL Private Plugin

Cycles through the 10 QLD native plant cards automatically, one per day, using
date-based rotation. No server or hosting required — everything runs as a
**Static** private plugin.

## What's in here

- `plants.json` — all 10 plants (name, scientific name, range, edible parts,
  taxonomy, illustration URL), taken from your reference cards.
- `images/` — the line-art illustration cropped out of each card, hosted here
  so TRMNL's cloud servers can load them.
- `markup-full.liquid` — the full-screen (800x480) layout: illustration on
  the left, details on the right, mirroring your original card design.
- `markup-half-horizontal.liquid` — an optional compact, text-only layout if
  you ever want this plugin sharing the screen in a Mashup.

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

The template computes the current day-of-year (`'now' | date: '%j'`) and
takes it modulo the number of plants (10), so it picks a different card each
calendar day and cycles back to the start after 10 days. The footer shows
"Card X of 10" so you always know where you are in the rotation.

If you'd rather it change every device refresh instead of once a day, tell me
and I'll swap the rotation key to something else.

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
