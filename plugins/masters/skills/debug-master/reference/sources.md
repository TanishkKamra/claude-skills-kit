# Source quality: use only the best of the best

**Top-tier test** (all four): (1) the primary or normative body (spec owner, standards body, regulator, vendor of the thing), (2) versioned and dated, (3) accountable (named editors, errata or changelog), (4) not paid to rank. Anything below Tier 1 must be cross-checked against a Tier 1 source before it drives a decision. Cite the tier next to every source in `CONTEXT.md`.

| Domain | Tier 1 (prefer) | Tier 2 (cross-check) | Avoid |
|---|---|---|---|
| UI/UX correctness | WCAG 2.2 (w3.org/TR/WCAG22), NN/g research articles (dated), Apple HIG (Apple platforms), Material 3 (Android/Material web); never mix HIG and M3 | Smashing Magazine, web.dev, Baymard (e-commerce UX) | "Top 10 trends" posts, Pinterest |
| UI taste benchmarks | Top priority, search first: Watermelon UI (ui.watermelon.sh), Cult UI (github.com/nolly-studio/cult-ui), Skiper UI (skiper-ui.com), GSAP (gsap.com, madewithgsap.com); then Awwwards Site of the Day (jury-weighted: design 40, usability 30, creativity 20, content 10), Mobbin (shipped products' real flows) | Land-book, recent.design (formerly Godly), Siteinspire | Dribbble/Behance shots as proof a pattern works; AI mockup galleries |
| Visual and brand | The brand owner's current guidelines; type foundry specimens and licences (Klim, Commercial Type, Google Fonts for OFL); CSS Color 4 (oklch/oklab) | Brand-guideline archives (verify against the owner's site), agency case studies | Logo makers, "colour of the year" listicles |
| Software docs | Official docs and changelog for the **exact version** in use (context7), language specs (ECMA-262, PEPs), RFCs (rfc-editor.org), MDN | Core maintainers' blogs and talks; accepted, recent StackOverflow answers that cite docs | Undated tutorials, Medium/dev.to, answers over 3 years old for fast-moving libraries |
| Security | OWASP Top 10:2025, ASVS, Cheat Sheet Series; NIST (NVD, SP 800-63/53, SSDF); reviewed GitHub Advisories (GHSA); vendor advisories | CISA KEV (exploited in the wild), Snyk/Mend databases, researcher write-ups | Vendor marketing "threat reports", SEO CVE aggregators |
| Ops/SRE | Google SRE books (free online), AWS/Azure/GCP Well-Architected frameworks (cite date), the provider's own service docs and quotas | Named-company postmortems, CNCF/Kubernetes docs | Vendor-mixing "best practices" blogs, affiliate tool comparisons |
| Research/academic | Peer-reviewed journals, Cochrane reviews (health), PubMed, official statistics (BLS, Census, Eurostat, ONS, World Bank) | Semantic Scholar and Google Scholar (discovery only; read the publisher's copy), arXiv (cite as preprint; prefer the published version) | Predatory journals (check DOAJ/Scopus), press releases instead of papers, content farms |
| Business/finance facts | SEC EDGAR filings (10-K, 10-Q, 8-K), central banks and statistics agencies (FRED, BLS, BEA, ECB) | Earnings-call transcripts, investor decks (company-authored), Reuters/Bloomberg reporting | Stock-tips sites, AI-written analysis, forum figures |
| Writing and decks | Plain-language guidelines (digital.gov), Minto (answer first), Tufte (data display), Duarte (slides) | Consulting style guides, Chicago Manual of Style | "Viral template" decks, generic tips blogs |

Health claims: Cochrane review > systematic review > RCT > observational study. Always read the paper, not the news coverage. Financial figures always carry their filing, date and period. Never give personalised investment advice.

## One-minute low-quality check (any source)
1. **Who**: a named, verifiable author or organisation? Fabricated author profiles are a red flag.
2. **Original**: first-hand testing, data or experience, or only a summary of others?
3. **Primary link**: does it link to the spec, filing or paper? Follow the claim upstream and cite the original.
4. **Dates and versions**: no date or version on a technical claim means Tier 2 at best.
5. **Scaled-content signals**: many near-identical pages, generic intros, symmetrical 3-bullet lists, fake freshness dates, thin affiliate pages.
6. **Lateral reading**: leave the page and search who runs the site.
A `.gov`, `.edu` or `.org` domain alone does not prove quality.

## Replicate, duplicate, rebuild
When the user asks to replicate, duplicate or rebuild something, do exactly that, to the highest standard: match layout, structure, behaviour, spacing and interaction faithfully, and measure the result against the original (side-by-side screenshots, feature checklist). For third-party work, replicate the design and behaviour but use the user's own or properly licensed assets, text and brand marks, and say which assets need replacing. "Rebuild" means a full rebuild unless the user narrows it; confirm what must be preserved (data, URLs, features) in the Change level questions.
