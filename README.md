# DensePack
Pack text into the smallest image an AI can still read. Same content, 50 to 80 percent fewer input tokens.<br>

[**Try it live**](https://luigalve.github.io/DensePack/) | [View the source](https://github.com/luigalve/DensePack/blob/main/index.html)
<br>
<br>
<br>


<!-- Banner slot: drop a FoundryStudio capsule here, same as the FoundryStudio README.
![Alt Text](images/DensePack-intro-capsule.svg) -->

## Table of Contents

- [Quick start](#quick-start)
- [Input: paste, drop, flatten](#input)
- [Render settings: font, size, spacing, width](#render-settings)
- [Color coding](#color-coding)
- [Formatting markers](#formatting-markers)
- [Output: download, copy, meter, preview](#output)
- [The complementary prompt](#the-complementary-prompt)
- [Token math](#token-math)
- [Seven things the buttons do not tell you](#seven-things-the-buttons-do-not-tell-you)
- [License](#license)

<br>
<br>

Open it in a browser and you have a packer: 
1. Paste text or drag a file in,
2. pick a font and a size,
3. watch the token estimate move,
4. download a PNG that a vision model can read.
<br>

**No install, no build step, no npm, no login.**

<br>

I built it to cut input token cost when feeding large text to vision capable models.<br>

- Text costs roughly 1 token for every 4 characters. 
- Images cost roughly 1 token per 750 pixels.
- A screenshot of text is normally the expensive way to send a prompt, but packed tightly enough the image wins, consistently by 50 to 80 percent.<br>
<br>

DensePack does the packing and shows the estimate live.

| What | Why it matters |
|---|---|
| Offline | The tool makes no network requests. No analytics, no external fonts, no libraries. Nothing you paste leaves your machine. |
| Zero dependency | One file, no framework, no build. Save it and it still opens in five years. |
| Cheaper input | Same content, 50 to 80 percent fewer input tokens, once the text is long enough to fill an image. |
| Honest about it | The meter compares both costs live, and says so when the image would cost more than the text. |
| Verifiable | The preview is exactly what downloads. Transcribe a sample back before you trust a big run. |
<br>

## Quick start

1. Click here to try it live, or download `index.html` and double click it to open it in a browser.
2. Paste your text, or drop a file into the input box.
3. If whitespace carries meaning in your text, Python, Markdown, YAML, turn on **Mark line breaks**, then **Mark indentation**.
4. Watch the savings meter. If it reads MORE than text, your text is too short or your font is too big.
5. Click **Download PNG**.
6. Click **Copy** on the complementary prompt and paste it next to the image.
7. Ask the model to transcribe the image back to you. Compare it against the original before you trust the method on anything that matters.

<sub>No build step and no server. To host your own copy, put index.html in a repository and turn on GitHub Pages.</sub>
<br>
<br>

## Input

| Control | What it does | How to use |
|---|---|---|
| Text box | Holds the source text: code, markdown, JSON, HTML source, prose | Paste. The render updates itself a moment after you stop typing |
| Drop a file | Reads any text file into the box | Drag it onto the box. It replaces whatever was in there. Files are read as raw source, so an HTML file stays HTML source, it does not get rendered |

Whatever lands in the box gets flattened before anything else happens: every run of spaces and tabs collapses to one space, every line is trimmed, every blank line is dropped, and what is left is joined into one continuous block. That block is what gets drawn, and the character count on the right counts that block, not what you pasted.
<br>
<br>

## Render settings

| Control | What it does | How to use / default |
|---|---|---|
| Font | Six system font stacks | Default Verdana. The note under the picker changes with the choice: green means recommended, red means think first. See the font table below |
| Font size | Type size in px, 5 to 20, half px steps | Default 9. The colored bar under the slider is the map: below 7 is risky, 8 to 11 is the sweet spot, up to 13 is safe, past that you are buying pixels and getting nothing back. Warnings appear under the slider |
| Line spacing | Multiplier on the font size, 0.95 to 1.35 | Default 1.05. Line height is font size times this, rounded. Tightening it is the cheapest pixel you will ever save |
| Letter spacing | Extra px between characters, -0.5 to 2 | Default 0. Negative packs the glyphs tighter and costs accuracy. Test before you trust it |
| Image width | Wrap target: Auto, 512, 768, 1024, or 1536 px | Auto is the default and aims for a squarish image, which is the shape least likely to get downscaled. A fixed number is a ceiling for wrapping, not the width you get |
<br>

**Fonts**

| Font | Note |
|---|---|
| Verdana | Recommended default. Proportional and dense, and I, l, 1 and O, 0 all look different. Best density to accuracy balance |
| Tahoma | Verdana's narrower sibling. About 10 percent more characters in the same pixels. Good second pick, test both |
| Trebuchet MS | Compact, distinctive glyphs. The one to reach for if Verdana or Tahoma misrender on your system |
| Arial / Helvetica | Densest option, and capital I and lowercase l are the same pixels. Risky for code unless color coding is on |
| Consolas / Menlo | Monospace, safest. Every character unambiguous, holds up at tiny sizes, costs 20 to 30 percent more pixels. Use it when the AI misreads proportional output |
| Courier New | Monospace, but thin strokes fade at small sizes. Consolas beats it in almost every case |
<br>

## Color coding

Off by default. Turn it on and ambiguous characters get colored so the AI cannot confuse a 1 with an l or a 0 with an O. Letters stay black. The palette is fixed, chosen to survive downscaling on white.

| Control | What it does | How to use |
|---|---|---|
| Color coding | Master toggle. Turning it on reveals the three group toggles | Click. All three groups start on |
| Numbers | Digits 0 to 9 render blue | Toggle |
| Symbols and punctuation | Anything that is not a letter, a digit, a space, or a marker renders red | Toggle |
| Markers | The line break marker renders green, the indent marker renders purple | Toggle. Does nothing until markers are on |

Color coding also changes how the text is drawn. With it on, the app places every character itself so it can color each one. With it off, it hands whole lines to the browser. Line widths can move by a pixel or two when you flip it.
<br>
<br>

## Formatting markers

Flattening destroys line breaks, which matters for Python, Markdown, YAML, and anything else where the structure lives in the whitespace. Two optional markers put it back.

| Control | What it does | How to use |
|---|---|---|
| Mark line breaks | Each newline becomes one visible marker instead of a space | Off by default, which is correct for CSS, JSON, HTML, and prose. Required for Python, Markdown, YAML, and anything else that reads by line |
| Line break marker | The character used for it, `¶` by default | Type any one or two characters. Pick something your text does not already contain |
| Mark indentation | Each indent level becomes one marker at the start of the line | Appears only once line breaks are marked. Required for Python. Without it, indent depth is gone even when the line breaks survive |
| Indent marker | The character used for it, `»` by default | Type any one or two characters |
| Clash warning | Fires when your chosen marker already appears in the input | Pick a different character. If the marker also exists in the content, the AI cannot tell which is which |

Levels are counted as one tab or four spaces per level, and the total for a line rounds to the nearest whole marker.
<br>
<br>

## Output

| Control | What it does | How to use |
|---|---|---|
| Download PNG | Saves the render at exact pixel size | Click. One image saves as `densepack.png`. A split render saves as `densepack-1.png`, `densepack-2.png`, and so on, a fraction of a second apart. The button label tells you how many are coming |
| Copy image | Puts the PNG on the clipboard | Single image output only. The button is dead when the render splits. If the browser blocks clipboard writes, it says so and you fall back to Download |
| Token savings meter | The headline number: image cost against text cost | Reads live. It flips and says MORE than text when the image would cost you more than pasting the text |
| Characters | Length of the flattened text | Read only. Not the length of what you pasted |
| Image size | Width x height, or the number of images if it split | Read only |
| Total pixels | Summed across every image | Read only |
| Token cost, text and image | The two estimates the meter is comparing | Read only |
| Preview | Every image, scaled to fit, captioned with its true dimensions | Look at it. The download is full size, the preview is not |

Any render that would run taller than 1568 px is split into more images automatically. 1568 px is the long edge past which most vision models downscale, and downscaling is what destroys small text. There is no manual splitting to do.
<br>
<br>

## The complementary prompt

Appears under the preview the moment markers or color coding are on. It is one sentence telling the model what the markers mean and that the colors are disambiguation, not meaning. Copy it, paste it next to the image, edit it however you like. It is a suggested phrasing, not a magic string. Skip it and the model sees a wall of pilcrows with no idea what they are for.
<br>
<br>

## Token math

| Quantity | Formula |
|---|---|
| Text tokens | characters divided by 4 |
| Image tokens | total pixels divided by 750 |
| Savings | text tokens minus image tokens, over text tokens |

Both are estimates. Real tokenizers vary by model and by provider, so check the pricing you are actually billed against before you lean on these numbers. The savings hold up on long input. Short input does worse, and can do worse than nothing.
<br>
<br>

## Seven things the buttons do not tell you

1. Blank lines do not survive. Flattening drops every empty line before it joins the rest, so a blank line produces no marker and no gap. Paragraph breaks in prose and the space between functions in code both disappear.
2. The indent marker assumes four spaces or one tab per level. A tab counts as a level, each space counts as a quarter, and the line total rounds. Two space indentation collapses under that math: two spaces and four spaces both come out as one marker. Convert to four spaces or tabs before you paste.
3. Indent marking is nested inside line break marking. You cannot keep indentation without also marking newlines, and that is correct, since an indent marker means nothing without a line for it to start.
4. Image width is a wrap target, not the output width. The PNG is cropped to the longest line that actually rendered, plus 2 px of margin on each side. Choose 1024 and you can still get a 700 px image.
5. Splitting happens on height, not width. Each image is capped at 1568 px tall, so the page count follows from your line height and your wrap width. Narrow the wrap and you get more lines, more height, and more images.
6. The meter can go negative. Short text, a big font, or a wide image all spend pixels the raw text was never paying for. When the image costs more, the meter says MORE than text, and it means it. Paste less than a screenful and expect exactly that.
7. Letter spacing quietly does nothing in some browsers while color coding is off. With color coding on, the app positions every character itself, so the slider always bites. With it off, it relies on the canvas letterSpacing property, which not every browser has. If the slider feels inert, that is why.

## License

MIT. Take it, fork it, ship it.
