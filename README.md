# TAU Interactive Campus Map

An interactive SVG map of the Tel Aviv University campus: pan/zoom, toggleable
layers (faculties, libraries, points of interest, gates, parking, movement
axes, east/west division), and zoom-triggered label switching.

**Live map:** `index.html` (open directly in a browser, or via GitHub Pages —
see below).

## What this project actually is

Every page in this repo — the map and every editor — is a **single
self-contained HTML file**. All CSS, JavaScript, and SVG map data are inline
in that one file. There is no build step, no framework, no npm packages, and
no external files loaded at runtime. This was a deliberate result of how the
project was built (iteratively, in a chat with Claude), not a design choice
made now — this repo packages that work as-is, unchanged.

## Folder structure

```
/                          repo root — GitHub Pages serves from here
├── index.html             THE PUBLIC MAP — open this to view/share the map
├── editors/                internal tools, not meant for public visitors
│   ├── mega_editor.html            faculty polygon shapes + all label positions
│   ├── icon_editor.html            drag libraries/POI/gates/buildings/faculty labels together
│   ├── building_names_editor.html  building label positions only
│   ├── faculty_names_editor.html   faculty label positions only
│   ├── combined_names_editor.html  buildings+libraries+POI+parking labels together
│   ├── axes_editor.html            movement-axis path vertices
│   ├── parking_names_editor.html   parking label positions
│   ├── poi_new_editor.html         positioning newly-added POI entries
│   ├── move_buildings_editor.html  repositioning specific added building polygons
│   ├── coordinate_checker.html     click the map, read back X/Y (SVG coordinate space)
│   ├── ortho_trace_editor.html     trace new building shapes over an aerial photo
│   ├── ortho_coordinate_checker.html  same, but reads pixel coords from the aerial photo
│   └── legacy/              superseded/one-off editors, kept for reference — see note below
├── layers/                  the "source of truth" SVGs for each map layer
│   ├── base_layer_FINAL.svg        buildings, streets, campus boundary, building names
│   ├── faculty_layer_FINAL.svg     faculty polygons + faculty labels
│   ├── eastwest_layer_FINAL.svg    east/west division line + labels
│   ├── libraries_layer_FINAL.svg   library icons + names
│   ├── poi_layer_FINAL.svg         points-of-interest icons + names
│   ├── axes_layer_FINAL.svg        movement axes
│   ├── gates_layer_FINAL.svg       campus gate icons + numbers
│   └── combined_ALL_layers_FINAL.svg   all of the above merged into one file
├── exports/                 PDF renders of each layer (for print/reference)
└── assets/                  misc reference images (not used by the live map)
```

### `editors/legacy/`

These were built for a single early task each and haven't been touched since
(e.g. `train_label_editor.html`, `lawn_position_editor.html`,
`neighborhood_editor.html`), or have been replaced by a newer tool that does
the same job (`legend_frame_editor.html` → `legend_position_frame_editor.html`;
`parking_lots_editor.html` → `parking_names_editor.html`;
`faculty_polygon_editor.html`/`faculty_shape_color_editor.html` →
`mega_editor.html`'s polygon mode). Nothing was deleted — they're kept in case
you need to see how something was originally done.

## What's editable, and what isn't (yet)

Every layer that has ever had a dedicated visual editor built for it is listed
in the table above. Two things currently have **no editor at all** — they
were only ever hand-placed once, directly in the SVG source:

- The campus boundary path (inside `base_layer_FINAL.svg`)
- The white background rectangle behind the map

If you want to change those, it currently means editing the raw SVG path/rect
by hand, or asking me (Claude) to do it in a chat, the same as everything else
below.

## How saving currently works — read this before you start editing

**No editor in this project saves anything by itself.** Every editor has an
"Export" button that dumps the new coordinates/paths as plain text into a
textarea on the page. That's it — nothing is written to a file, to
localStorage, or to GitHub automatically.

The actual workflow, today, is:

1. Open the relevant editor page (from `editors/`, in a browser)
2. Drag things into place
3. Click "Export" and copy the resulting text
4. Paste that text to Claude in a chat
5. Claude regenerates the affected file(s) in this repo's structure
6. You upload the changed file(s) to GitHub (overwriting the old version)
7. The live map at `index.html` picks up the change automatically, since it
   has that data baked in — no rebuild needed

This is slower than a "real" save button, but it's what's actually built
right now, and nothing here pretends otherwise.

## Running it locally

There's nothing to install. Two options:

- **Simplest:** double-click `index.html` — it opens directly in your browser.
- **Closer to how GitHub Pages serves it:** run a tiny local server from the
  repo root, e.g. `python3 -m http.server 8000`, then visit
  `http://localhost:8000`. Only needed if something behaves differently as a
  local file vs. a served page (rare, but some browsers restrict local file
  access more than served pages).

## Deploying to GitHub Pages

See the separate setup instructions provided alongside this README. Short
version: push this whole folder to a repo, enable Pages for the root of the
`main` branch, and `index.html` becomes your public map URL automatically.
