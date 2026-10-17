A cursor app I vibe coded to allow for quick and easy creation of linux xcursor icons. Take any image and turn it into a mouse cursor.

# 🖱️ Cursor Forge

**A single-file, in-browser tool that turns PNG, JPEG and GIF images into a Linux mouse cursor theme.**

Cursor Forge runs entirely in your browser. Nothing is uploaded to a server. You pick an image for each cursor, set its click point (hotspot), optionally add animation, and download a ready-to-install `.zip` containing a standard **Xcursor** theme that works on X11 and Wayland, on Ubuntu and other Linux distributions.

---

## Table of contents

- [Features](#features)
- [Quick start](#quick-start)
- [Using the app](#using-the-app)
- [Installing the generated theme](#installing-the-generated-theme)
- [Supported cursors](#supported-cursors)
- [What is in the downloaded zip](#what-is-in-the-downloaded-zip)
- [Technical details](#technical-details)
- [Browser support and limits](#browser-support-and-limits)
- [Testing](#testing)
- [Troubleshooting](#troubleshooting)
- [Project structure](#project-structure)
- [Contributing](#contributing)

---

## Features

- **No install, no server.** One HTML file with inline CSS and JavaScript. Open it in a browser and go.
- **21 cursor slots** covering the pointers Linux desktops use: arrow, link hand (mouse-over), text I-beam, busy, help, crosshair, move, grab, grabbing, not-allowed, copy, alias, context menu, zoom in/out, cell and the four resize directions. Each slot also writes the legacy and hashed names older toolkits look for (88 file names in total).
- **Built-in default art** for every slot, so you can see what each cursor is for.
- **Your own images.** PNG, JPEG or GIF, with transparency preserved.
- **Hotspot picker.** Tap or drag on a large preview to choose the exact click pixel, with Top-left and Centre shortcuts.
- **Animation.** Select several images (frames are ordered by file name) or an animated GIF. A speed slider appears only when a cursor has more than one frame.
- **Multiple sizes.** Choose any of 24, 32, 48, 64 and 96 px. The desktop picks the nearest to the user's cursor-size setting.
- **Linux type selector.** Ubuntu/GNOME, KDE Plasma, or Other (Xfce, Cinnamon, MATE, any Linux). This tailors the on-page instructions and the generated `install.sh`.
- **Partial themes.** By default only the cursors you customised are included and the rest inherit from Adwaita.
- **Built-in instructions** for new users, on how the page works and how to install the result.
- **Light and dark mode**, and a layout that works on phones.

---

## Quick start

1. Open `cursor-forge.html` in a modern browser (Chrome, Edge, Brave, Firefox or Safari).
2. Click a cursor tile, choose an image, and set the hotspot.
3. Enter a theme name, pick your Linux type and sizes, and click **Build & download**.
4. Unzip, then run `sh install.sh` in the extracted folder.

No build step, no dependencies, no package manager.

---

## Using the app

### 1. Pick a cursor
Each tile in the **Cursors** grid is one pointer type. Tiles you have changed show a *custom* tag and a highlighted border.

### 2. Choose your image
In the editor dialog:

| Action | Result |
|---|---|
| **Choose image(s) / GIF** | Replaces the slot's frames. One image gives a static cursor. Several images (or an animated GIF) give an animation. |
| **Add frames** | Appends more images to the existing frames. |
| **× on a thumbnail** | Removes that frame. |
| **Reset to default** | Restores the built-in art and hotspot. |

When several files are selected they are sorted by file name with natural numeric ordering, so `frame2.png` comes before `frame10.png`.

Images are scaled to fit a square (keeping aspect ratio) and centred. A square source of 128×128 px or larger with a transparent background gives the best results.

### 3. Set the hotspot
Tap or drag on the preview. The red crosshair marks the point that counts as the click position. For an arrow this is the tip; for a crosshair it is the centre. The hotspot is stored as a fraction of the square, so it stays correct at every output size.

### 4. Set animation speed
With more than one frame, a highlighted slider appears (20 to 1000 ms per frame, with an fps estimate). The preview in the editor and on the tile animates live. For GIFs, the slider starts at the average of the GIF's own frame delays.

### 5. Build
Set the theme name, Linux type and sizes, then click **Build & download**.

| Option | Meaning |
|---|---|
| **Theme name** | Letters, digits, `_` and `-`. Other characters become `_`, and leading hyphens are removed. |
| **Linux type** | Chooses install location and activation commands in the instructions and `install.sh`. |
| **Sizes** | Pixel sizes written into each cursor file. More sizes give better scaling but larger files. |
| **Only include cursors I customised** | When on (default), unchanged slots are omitted and inherit from Adwaita. When off, the built-in placeholder art is written for every slot. |

---

## Installing the generated theme

Unzip the download. You get a folder named after your theme and an `install.sh` file.

### Easy way
```sh
sh install.sh
```
The script copies the theme into place and tries to activate it.

### Manual way

**Ubuntu / GNOME**
```sh
mkdir -p ~/.local/share/icons
cp -r MyCursor ~/.local/share/icons/
gsettings set org.gnome.desktop.interface cursor-theme 'MyCursor'
```
Or install GNOME Tweaks (`sudo apt install gnome-tweaks`) and choose it under **Appearance → Cursor**.

**KDE Plasma**
Copy the folder to `~/.local/share/icons/`, then open **System Settings → Appearance → Cursors** and select it. `plasma-apply-cursortheme MyCursor` also works.

**Xfce, Cinnamon, MATE, others**
Copy the folder to `~/.icons/`, then choose it in your desktop's mouse or appearance settings:
- Xfce: Settings → Mouse and Touchpad → Theme
- Cinnamon: Themes → Mouse Pointer
- MATE: Appearance → Pointer
- Generic fallback: create `~/.icons/default/index.theme` containing:
  ```ini
  [Icon Theme]
  Inherits=MyCursor
  ```

### System-wide
```sh
sudo cp -r MyCursor /usr/share/icons/
```

### Notes
- If nothing changes, log out and back in.
- Flatpak and Snap apps may need the theme in `~/.icons` as well.

---

## Supported cursors

Each slot is written under its primary name plus aliases. Applications use different names for the same pointer (CSS names, legacy X11 names and hashed names used by older GTK and Qt versions).

| Slot | Primary name | Used for |
|---|---|---|
| Arrow | `left_ptr` | Default pointer |
| Hand | `pointer` | Links, mouse-over |
| Text | `text` | Text fields |
| Busy | `wait` | Application busy |
| Working in background | `progress` | Busy but usable |
| Help | `help` | Help pointer |
| Crosshair | `crosshair` | Precise selection |
| Move | `move` | Moving items |
| Open hand | `grab` | Draggable item |
| Closed hand | `grabbing` | Dragging |
| Not allowed | `not-allowed` | Forbidden action |
| Copy | `copy` | Drag to copy |
| Alias | `alias` | Drag to link |
| Context menu | `context-menu` | Menu available |
| Zoom in / out | `zoom-in`, `zoom-out` | Zoom |
| Cell | `cell` | Spreadsheet cell select |
| Resize ↔ | `ew-resize` | Horizontal resize |
| Resize ↕ | `ns-resize` | Vertical resize |
| Resize ⤢ | `nesw-resize` | Diagonal resize |
| Resize ⤡ | `nwse-resize` | Diagonal resize |

The full alias list lives in the `SL` array near the top of the script in `cursor-forge.html`.

---

## What is in the downloaded zip

```
MyCursor.zip
├── install.sh                 # executable installer (sh)
└── MyCursor/
    ├── index.theme            # [Icon Theme] Name, Comment, Inherits=Adwaita
    └── cursors/
        ├── left_ptr           # Xcursor binary files
        ├── default            # full copies under every alias name
        ├── arrow
        └── ...
```

Aliases are stored as full copies rather than symlinks. The archive is larger, but it extracts correctly with every unzip tool and file manager.

---

## Technical details

Cursor Forge writes binary files directly in the browser. This section documents how.

### Xcursor file format
All multi-byte integers are **little-endian**. In JavaScript this means every `DataView.setUint32` call passes `true` as the last argument, because `DataView` defaults to big-endian.

**File header (16 bytes)**

| Field | Value |
|---|---|
| magic | bytes `58 63 75 72` (`Xcur`) |
| header size | 16 |
| version | `0x00010000` |
| ntoc | number of image chunks |

**Table of contents** has one 12-byte entry per image: `type` (`0xfffd0002`, image), `subtype` (nominal size in px) and `position` (byte offset of the chunk).

**Image chunk (36-byte header, then pixels)**

| Field | Value |
|---|---|
| header size | 36 |
| type | `0xfffd0002` |
| subtype | nominal size |
| version | 1 |
| width, height | pixel size |
| xhot, yhot | hotspot, clamped to `size - 1` |
| delay | milliseconds (0 for static cursors) |
| pixels | `width × height` 32-bit values |

**Pixels** are premultiplied ARGB (alpha in the high byte). Because values are stored little-endian, the bytes on disk are **B, G, R, A**. The canvas returns straight (non-premultiplied) RGBA, so each colour channel is multiplied by alpha / 255 before writing.

**Animation.** Frames of the same nominal size are written consecutively, with the same `delay`. Sizes are written one after another.

### ZIP writer
A small store-only (no compression) ZIP encoder is included:
- CRC-32 with a precomputed table.
- UTF-8 filename flag (bit 11).
- "Version made by" set to Unix (`0x031E`) so `install.sh` keeps mode `0755` and other files `0644`.
- Local headers, central directory and end-of-central-directory record written by hand; the result is passed to a `Blob`.

### Image decoding
- Static images: `createImageBitmap(file)`.
- Animated GIFs: the WebCodecs `ImageDecoder` API, which returns fully composed frames and their durations.
- Rendering to each output size uses `drawImage` with `imageSmoothingQuality = 'high'`.

### Safety limits
- Maximum 60 frames per cursor.
- The build stops if the total would exceed 150 MB.
- Theme names are restricted to `[A-Za-z0-9_-]` so they are safe inside the generated shell script.

---

## Browser support and limits

| Feature | Chrome / Edge / Brave | Firefox | Safari |
|---|---|---|---|
| Static PNG, JPEG, GIF | Yes | Yes | Yes |
| Animated GIF splitting | Yes | No | Depends on version |
| Multiple images as frames | Yes | Yes | Yes |

Where `ImageDecoder` is unavailable, a GIF loads as a single static frame. Upload the frames as separate images instead.

The themes themselves are standard Xcursor and are not tied to any browser.

**Download behaviour.** When opened as a normal file or web page, the download uses a standard anchor with a `Blob` URL. When hosted inside an environment that provides a `window.claude` downloads capability, that is used instead.

---

## Testing

The binary output was validated outside the browser:

- The encoder and ZIP functions were run in Node.js with a stubbed canvas.
- The generated Xcursor file was parsed with an independent Python script that checks the magic, header size, version, table-of-contents offsets, chunk headers, hotspot bounds, pixel byte order, premultiplication and exact file length.
- The ZIP was verified with `unzip -t`, extracted, and inspected with Python's `zipfile` for permissions and flags.
- The CRC-32 routine was checked against the standard test vector (`"123456789"` → `cbf43926`).
- All 88 alias file names were confirmed to be unique.

It has **not** yet been tested against every desktop environment, so real-world reports are welcome.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Cursor does not change | Log out and back in; confirm the folder is in `~/.local/share/icons` or `~/.icons`; re-select it in settings. |
| Wrong click position | Reopen the cursor and move the hotspot. |
| Cursor too small or large | Change the cursor size in your desktop settings; include more sizes when building. |
| Cursor looks blurry | Use a larger, square source image. |
| GIF is static | Use Chrome, Edge or Brave, or upload frames as separate images. |
| Some pointers still use the old look | Build with the "only include customised" option off, or customise more slots. |
| Flatpak or Snap apps ignore it | Also install to `~/.icons`. |
| Build stops with a size message | Use fewer sizes, fewer frames or fewer animated cursors. |

---

## Project structure

```
.
├── cursor-forge.html   # the entire application
└── README.md
```

Inside `cursor-forge.html` the script is organised in sections: slot definitions, default art, canvas and editor, Xcursor encoder, ZIP writer, install-text generator, build handler and initialisation.

---

## Contributing

Issues and pull requests are welcome. Useful areas:

- A pure-JavaScript GIF decoder so animation works in all browsers.
- Per-frame delays instead of one speed per cursor.
- Optional symlink output for smaller archives.
- Drag-to-reorder frames.
- Importing existing Xcursor themes.
- Extra cursor names and verified results on more desktop environments.

When changing binary-writing code, please re-run a round-trip check against the format description above.

