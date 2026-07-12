# DensePack
A zero-dependency browser tool that converts text into token-efficient images for AI input, cutting costs by 50 to 80 percent

# DensePack

**[Try it live](https://luigalve.github.io/DensePack/)** &middot; [View the source](https://github.com/luigalve/DensePack/blob/main/densepack.html)

DensePack converts any pasted text into the smallest image an AI can still read accurately. 
It is one HTML file with no dependencies. 
It runs entirely in the browser and works offline. Just about anywhere static files are served, including GitHub Pages.

I built it to cut input token costs when feeding large text to vision capable models. 
Text costs roughly 1 token for every 4 characters. Images cost roughly 1 token per 750 pixels. 
<br>
Normally, text input is much cheaper than its screenshot counterpart but, packed tightly enough,
an image of the same text is much cheaper than the text itself, consistently by 50 to 80 percent! 

DensePack does the packing and shows you the estimated savings live.


## What it does

1. Takes any text: code, markdown, JSON, HTML source, prose. Paste it or drop a file.
>
2. Flattens it into one continuous block. All runs of whitespace collapse to single spaces.
>
3. Renders it to a tightly cropped PNG with near zero margins, in a font and size tuned for machine reading.
>
4. Automatically splits the output into multiple images if it would exceed the 1568 pixel edge that most vision models downscale past.
>
5. Reports character count, pixel count, and estimated token cost for both the text and the image, plus the percentage saved.

<sub>Fable 5 is significantly better at reading text on images because of its pixel-level analysis, spatial reasoning, and iterative tool use, while Opus 4.8 is less effective due to its reliance on generic description, lack of deep structural extraction, and standard OCR processing. Color coding was added specifically to help models like Opus close this gap.</sub>

## Color coding

An optional mode colors ambiguous characters so the AI never confuses a 1 with an l or a 0 with an O. Letters stay black. Digits render blue, symbols render red, and the two markers render green and purple. The palette is fixed and was chosen for legibility on white after downscaling. Each group can be toggled individually. The generated instruction sentence updates when this option is enabled as well.

## Formatting markers

Flattening destroys line breaks, which matters for Python, Markdown, YAML, and anything else where structure lives in the whitespace. I added two optional markers to preserve it:

- Line breaks: each newline becomes a visible marker, "¶" by default.
- Indentation: each indent level (one tab or four spaces) becomes a second marker, "»" by default.

Both markers are editable. The app warns you if your chosen marker already appears in the input, and selecting any generates a short instruction sentence to paste alongside the image so the AI knows what the markers mean.

## Settings

- Font: six system font stacks with guidance in the app: Verdana is the recommended default for its density and distinct letterforms. Consolas is the safe monospace fallback.
- Font size: 5 to 20 pixels. The app marks the sweet spot around 8 to 11 pixels and warns below 7, where AI transcription becomes unreliable.
- Line spacing, letter spacing, and image width are all adjustable. Auto width produces a squarish image, which minimizes the risk of downscaling.

## Token estimates

Text cost is characters divided by four. Image cost is total pixels divided by 750. Both are estimates. Real tokenizers vary by model, so verify against your provider's pricing before relying on the numbers.

## How to use it

1. Open densepack.html in any modern browser, or visit the hosted page.
2. Paste text or drop a file into the input box.
3. Turn on markers if your text is whitespace sensitive.
4. Check the savings meter and the preview.
5. Download the PNG and feed it to your AI along with the generated instruction sentence.

Always verify a sample first: give the image to your AI, ask it to transcribe the text, and compare against the original. If characters come back wrong, raise the font size or switch to Consolas.

## Running locally and on GitHub Pages

No build step and no server. Download the file and open it. To host it, put the file in a repository as index.html and enable GitHub Pages in the repository settings.

## Privacy

Nothing leaves the browser. There are no network requests, no analytics, and no external fonts or libraries.
