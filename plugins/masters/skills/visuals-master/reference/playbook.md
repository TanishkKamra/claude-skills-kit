# visuals-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Cost override
Child rules about image counts ("one image per section", "generate as many as needed", "default 6 to 8") are capped here. First pass: at most 3 images in total (e.g. hero + one mid section + one detail, or 3 hero directions). List the remaining sections with an estimated count and wait for a yes. If no image tool or API key exists, skip generation: build HTML/SVG comps, screenshot them with Playwright and label them "coded mockup".

## Text in images
Text in generated images is placeholder. Never copy copy, hex values or font names from an image as truth. Sample colours from pixels (PIL `quantize`), take copy from the brief, pick fonts from Google Fonts, Fontshare or OFL sources.

## Brandkit sequence (overrides the child's single board)
1. One-page strategy: audience, 3 adjectives, anti-references, 3 territories; the user picks one.
2. Build the mark as SVG in code from simple geometry. Test at 16, 32 and 64px, one colour, reversed, light and dark, greyscale.
3. Write `tokens.json` and CSS: colours with a contrast table, 2 fonts, a type scale; clear space, minimum size and lockups.
4. Only then render the presentation board, marked "concept, not final art".
5. Tell the user a trademark search (USPTO, EUIPO, WIPO Global Brand Database) is needed before use.

## Posters and social (canvas-design for functional pieces)
One headline, one date/place block, one action or QR. Sizes: A3 297x420mm at 300dpi + 3mm bleed + 5mm safe area; 1080x1350 Instagram; 1200x628 link preview. Text contrast 4.5:1. Export PNG and PDF; script-check for text overlap and off-canvas elements. canvas-design's art-first rules apply only to art pieces.

## Image-to-code with a provided image
Skip generation. Measure: dimensions, palette via PIL `quantize`, spacing via row/column projection. Write `tokens.css`. Build semantic HTML, not image slices. Playwright screenshot at the source width; diff with pixelmatch or ODiff; list regions more than 4px off. Design the 390px view yourself and label it an assumption. For web, impeccable's measured pipeline (`comp-spec`, `build-phase`, `comp-diff`) is preferred.

## Default coded stack (no image API needed; researched 2026-10)
- **Everything as HTML/CSS/SVG rendered by Playwright** to PNG or PDF at `deviceScaleFactor: 2` or more, with self-hosted or Google Fonts.
- **Bulk social and OG images**: Takumi (Rust, no browser, flex and grid) or Satori (JSX to SVG; CSS subset only).
- **Print-grade PDFs** (brand books, posters, decks): Typst (`winget install --id Typst.Typst`, `typst compile in.typ out.pdf`; also PNG/SVG).
- **Logos**: hand-written SVG from simple geometry, optimised with SVGO, rasterised with resvg for the 16/32/64px test sheet.
- **3D product shots**: Three.js or React Three Fiber rendered through Playwright.
- **Device frames**: MIT-licensed CSS frames (e.g. `css-device-frames`); check each repo's LICENSE; never ship Apple/Google official frames without their terms.
- **Tokens**: DTCG JSON + Style Dictionary; colours in OKLCH with a WCAG 2.x contrast table.
- **Checks**: odiff for comp-vs-build diffs; colour-blind simulation via Chrome DevTools Protocol `Emulation.setEmulatedVisionDeficiency`.

## Routing overrides
- **imagegen-frontend-web** makes comps; **image-to-code** only converts. For "Images + coded build", chain them but skip image-to-code's own generation and variation steps (80% shared text with imagegen-web).
- **imagegen-frontend-mobile**: then hand to impeccable iOS/Android references; use 44pt (iOS) / 48dp (Android) targets, Dynamic Type, safe areas.
- A Figma file exists: prefer the Figma plugin (`figma@claude-plugins-official`) over generated images.
- Image APIs are paid and need a key the user sets themselves; never enter keys.

## Gate additions
- Logo test sheet (16/32/64px, mono, reversed) rendered and inspected; contrast table for every text/background pair; colour-blind simulation check; asset licence ledger in EVIDENCE.md.

## Sources to require
Wheeler *Designing Brand Identity* · Brand New (UnderConsideration) · Pentagram, Collins, Wolff Olins case studies · brand guideline libraries (verify on the owner's site) · WIPO Global Brand Database, USPTO · Apple HIG app icons, Material adaptive icons · WebAIM contrast checker.
