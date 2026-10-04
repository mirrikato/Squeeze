# Squeeze

Shrink PDFs and images from the macOS right-click menu. Two Quick Actions, no app, no uploads.


Right-click any PDF or image in Finder, pick **Squeeze**, and a smaller copy appears next to the
original. Everything runs on your Mac - nothing is uploaded anywhere.

## Install

1. [Download the latest release](https://github.com/mirrikato/squeeze/releases/latest) and unzip it.
2. Double-click **Squeeze PDF.workflow** and click Install.
3. Double-click **Squeeze Image.workflow** and click Install.

No Terminal, nothing to configure.

> [!NOTE]
> macOS **moves** the two `.workflow` files into your Services folder rather than copying them, so
> they disappear from the unzipped folder. That's normal and means they installed. Keep the `.zip`
> if you'd like a spare copy.

If the menu items don't appear, relaunch Finder: hold <kbd>Option</kbd>, right-click the Finder icon
in the Dock, and choose **Relaunch**.

## Use

Right-click any PDF or image in Finder:

```
Quick Actions → Squeeze PDF
Quick Actions → Squeeze Image
```

Select several files at once if you like. A smaller copy is saved next to each original with
`-small` in its name, so **your originals are never touched**. A notification tells you how many
files were squeezed.

Some files won't shrink - a text-only PDF or an already-optimized image is about as small as it
gets. Squeeze deletes the copy rather than leaving you a bigger file, and reports `Squeezed 0 of 1`.

A PNG may come back as a `.jpg`. See below for why.

## Quality, and the optional extras

Squeeze works on any Mac using tools built into macOS.

If you have [Homebrew](https://brew.sh) installed, the first time you run Squeeze it offers to add
two small open-source tools - [Ghostscript](https://www.ghostscript.com) and
[pngquant](https://pngquant.org). Click Install and it handles the rest; it takes about a minute.
These give sharper PDFs at a given size, and proper compression for PNGs that have transparency.

Without them, Squeeze falls back to what macOS provides:

| File type | Without extras | With extras |
| --- | --- | --- |
| PDF | Apple's built-in reduction filter | Ghostscript, sharper at the same size |
| JPEG, HEIC, TIFF | Resized to 2560px, saved as JPEG | Identical - no difference |
| PNG, no transparency | Saved as a smaller JPEG | pngquant, stays a PNG |
| PNG, with transparency | Left alone unless over 2560px | pngquant, 60–80% smaller |

Clicked **Not Now** but changed your mind? Delete this folder and Squeeze will ask again next time:

```
~/Library/Application Support/Squeeze
```

In Finder: **Go → Go to Folder…**, then paste that path. Or just install them yourself - Squeeze
picks them up automatically:

```bash
brew install ghostscript pngquant
```

## Tuning

Open a workflow from `~/Library/Services` with Automator (right-click → Open With → Automator) and
edit the script.

- **PDFs** - change `-dPDFSETTINGS=/ebook` to `/screen` for smaller files or `/printer` for higher quality.
- **Images** - `-Z 2560` caps the longest side in pixels; `formatOptions 75` is the JPEG quality out of 100.

## Privacy

Everything runs on your Mac. Nothing is uploaded, and Squeeze makes no network connections apart
from the optional Homebrew install, which you have to approve.

## Remove

Delete these two files:

```
~/Library/Services/Squeeze PDF.workflow
~/Library/Services/Squeeze Image.workflow
```

In Finder: **Go → Go to Folder…**, then paste `~/Library/Services`.

## Troubleshooting

Squeeze writes a log of each run to `~/Library/Logs/Squeeze.log`. If something goes wrong, that file
usually says why.

## License

MIT - see [LICENSE](LICENSE).

Ghostscript and pngquant are optional, installed by you, and carry their own licenses (AGPL and
GPL v3 respectively, both with commercial options). Squeeze doesn't bundle or redistribute them.
