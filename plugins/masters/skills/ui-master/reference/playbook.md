# ui-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Conflict rule
Done-when gates beat any aesthetic child. A child's bans apply only inside its own lane. Never panel design-taste-frontend or impeccable with minimalist-ui, industrial-brutalist-ui, high-end-visual-design, gpt-taste or stitch-design-taste as co-builders; those are proposers only.

**Always overlay on every child**: a `prefers-reduced-motion` block; a visible `:focus-visible` ring; no text under 12px; no auto-playing loop over 5 seconds without pause (WCAG 2.2.2); sentence-case UI copy unless the project differs; fonts only from Google Fonts, Fontshare or self-hosted OFL files (record the source in EVIDENCE.md).

## Routing overrides
- **impeccable**: default for apps and dashboards; its critique/audit/polish is the reviewer for every landing build. First run downloads a binary: ask first.
- **design-taste-frontend**: primary for landing pages and portfolios. To propose directions use `node scripts/draw.mjs --options 3`.
- **Look = Not sure (propose 3 directions)**: design-taste-frontend `draw.mjs --options 3` for landing/portfolio, impeccable `shape` + `concept-seed` for everything else.
- **design-is**: only for a scored Rams audit or Start = Redesign (verdict before rebuild). Skip its `/make-plan` handoff; copy its verdict and top moves into SPEC.md.
- **frontend-design**: second voice as brief critic.
- **web-design-guidelines**: run last; ignore its Title Case, `&` and `autocomplete=off` rules unless the project uses them. Its findings are code evidence, not rendered proof.
- **redesign-existing-projects**: audit pass only; follow with impeccable `audit`. Ignore its "add grain" and picsum placeholder advice.
- **ui-polish**: add reduced-motion and colour-scheme checks via Playwright `emulateMedia`, and axe in the sweep.
- **high-end-visual-design** only when Look = High-end agency; **stitch-design-taste, gpt-taste** only on explicit request. For all three: strip infinite loops and mandatory eyebrow tags; add reduced motion.
- No image tool or API key on this machine: skip image-generation steps; build HTML/SVG comps and label them "coded mockup".

## Default coded stack (researched 2026-10; check versions in Phase 2)
1. **Components**: shadcn/ui on Base UI + Tailwind CSS v4 (`npx shadcn@latest init`; `-b radix` keeps Radix; React Aria via `--base aria`). Use the project's existing system first; reference systems (Material 3, Fluent 2, Carbon, Primer, GOV.UK) are for reading, never for copying brand assets.
2. **Tokens**: W3C DTCG 2025.10 JSON, built with Style Dictionary 5 (or Terrazzo).
3. **Colour**: OKLCH. Radix Colors for 12-step scales; Adobe Leonardo (`@adobe/leonardo-contrast-colors`) for contrast-targeted palettes. Gate on WCAG 2.x ratios; APCA is a second, advisory check only (WCAG 3 contrast is not final).
4. **Type and space**: Utopia-style fluid `clamp()` scales (compute the values; there is no package).
5. **Motion**: CSS scroll-driven animations and same-document View Transitions first (cross-document transitions are missing in Firefox; give a fallback); then Motion; GSAP (free, all plugins) only when CSS cannot do it.
6. **Icons**: Lucide (ISC) by default; Phosphor for duotone or filled styles; Tabler for breadth.
7. **Docs and regression**: Storybook 10 for component systems; Playwright `toHaveScreenshot` for visual regression (baselines per OS and browser); odiff for raw image diffs.
8. **MCP helpers if installed**: shadcn MCP (`pnpm dlx shadcn@latest mcp init --client claude`), Storybook `@storybook/addon-mcp`, Playwright MCP, Figma MCP (needs a Figma account: ask).

## Test matrix (proof, at 1440 and 390 widths)
- axe: `@axe-core/playwright` inside tests, or the CLI: `axe <url> --exit --chromedriver-path "$(npm root -g)/chromedriver/lib/chromedriver/chromedriver.exe"`. If it is missing, installing counts as rule 6: ask. After a Chrome update, reinstall `chromedriver@<Chrome major>`. axe finds about half of WCAG issues: still do the keyboard and screen-reader checks. Paste the violation count.
- Reduced motion: `page.emulateMedia({ reducedMotion: 'reduce' })`, screenshot, content visible.
- Dark mode and forced colours if supported; 200% zoom without loss; keyboard-only pass with no trap.
- Real-content fuzz: very long names, empty states, RTL sample if i18n is in scope.
- Lighthouse or Core Web Vitals (LCP, INP, CLS) for public pages.

## Corrected numbers
- Type: at most 3 emphasis levels per screen region; scale 5 to 7 steps for apps, 5 max for landing pages.
- Targets: 24px minimum (WCAG 2.5.8 AA); 44px for primary touch controls.
- Contrast: 4.5:1 body text, 3:1 large text and UI components.
- Platform: Apple HIG for Apple, Material 3 for Android, Fluent 2 for Windows desktop (e.g. Cadence). Never mix.

## Sources to require
WCAG 2.2 + Understanding docs · WAI-ARIA Authoring Practices · NN/g · Apple HIG / Material 3 / Fluent 2 · Awwwards, Godly, Land-book, Siteinspire, One Page Love · Mobbin, Refero, Page Flows · web.dev and MDN · Refactoring UI, Laws of UX · Fontshare, Google Fonts, Fonts In Use, Typewolf.
