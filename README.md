# SEO On Page Agent

A multi-phase agent that audits a single landing page, diagnoses why it is losing search clicks, and produces a markdown deliverable with a gap analysis, ready-to-ship copy rewrites, full new sections, internal-linking recommendations, and schema markup. Designed to run inside Claude Code, with Ahrefs, DataForSEO, Firecrawl, and OpenAI as the data layer.

## Why this exists

Most SEO audits stop at "you ranked here, now you rank here." This one tries to answer the harder question: **what changed on the SERP that explains the drop, and what specifically should the page say differently to recover.** It does that by validating the hypothesis the user shows up with (rather than accepting it), running a fresh competitive scrape against the live top 10, comparing structures, classifying every gap as either a copy fix or a product gap (so the agent never claims a feature the product does not actually ship), and writing the new copy itself instead of hand-waving with "rewrite this section to be more comprehensive."

Two principles run through the whole thing:

1. **Diagnose first, prescribe second.** Every recommendation has to trace back to a specific gap, competitor pattern, or diagnosis point. No generic SEO advice.
2. **Never claim a feature the product does not have.** When a competitor pattern surfaces a feature, it gets classified as verified, verifiable in minutes, or product gap. Only verified features end up in copy. Product gaps go to the build team.

## The 10 phases

The agent runs end-to-end through these phases. Phase numbers map to the structure inside `CLAUDE.md`.

| Phase | What it does |
|---|---|
| 0. Input intake | Collects 13 inputs from the user one at a time (URL, primary keyword, click loss, hypothesis, inspirational pages, etc.) and echoes them back for confirmation. |
| 1. Validation | Cross-checks the user's keyword volume against Ahrefs, validates the SERP intent against DataForSEO live SERP, scrapes the audit URL with Firecrawl, and extends the click-loss hypothesis with new drivers (AI Overview presence, intent shifts, position drops, new competitors). |
| 2. Page scrape | Pulls the audit URL through Firecrawl and extracts headings, schema, FAQs, body content, internal links, alt text, word count, and last-modified date. |
| 3. Competitive recon | Pulls the top 10 results via Ahrefs SERP overview and full-scrapes the top N (user-configurable, 1 to 10) with Firecrawl. |
| 4. Gap analysis | Compares ours vs competitors across heading structure, content depth, FAQ coverage, entity coverage, UX elements, schema, and layout patterns. Classifies every gap as verified, verifiable in minutes, or product gap. |
| 5. Keyword usage audit | Checks where primary and secondary keywords appear (slug, title, meta, every heading, first 100 words of body, image alts, internal anchors). Flags stuffing and natural absences. |
| 6. LLM citation recon | Runs four to six structured queries against `gpt-4o-search-preview` (Chat Completions) and the Responses API with `web_search`, logs cited domains and brand mentions, and identifies the content patterns that get cited. |
| 7. Internal linking | Pulls the sitemap, scores relevance to the audit URL's primary topic, recommends contextual and section-based links, and writes the section copy when a dedicated related-links section is warranted. |
| 8. Topical research | A second OpenAI web-search pass aimed at content depth (statistics, benchmarks, sub-topics that competitors cover but the audit URL does not). |
| 9. Synthesis | Builds a prioritized action list: Critical, High, Medium, Low. Every action ties back to a specific diagnosis or gap. |
| 10. Copy rewrite | Writes the actual copy for every change. Uses a section library to reuse layouts across pages. Produces three files: `AUDIT.md`, `REWRITES.md`, and `prototype.html`. |

## Output structure

Every audit produces a folder under `output/<page-slug>/` containing:

- `AUDIT.md` — diagnosis, gap analysis, prioritized actions, schema recommendations, LLM citation insights, research sources, scope caveats. Target under 300 lines.
- `REWRITES.md` — copy ready to ship. Labeled `Current X` / `New X` pairs for every element that changes (title, meta, every heading, every body block, every FAQ Q+A, every alt). New sections delivered as full copy with placement instructions.
- `prototype.html` — production-ready HTML for any new section or design change, built with semantic markup and responsive structure. The developer can read this file and ship the change without re-asking for design intent.

## Section library

The agent maintains a growing library of reusable section templates at `output/_sections-library.md`. Before proposing a new section, the agent checks the library first; if an existing template fits the gap with different copy, images, or data, the agent reuses it and marks the block as `[REUSE: <template name> from <page-slug>]`. Only genuinely new content models get a `[NEW TEMPLATE]` entry.

This is the part that compounds across audits. The first audit on a site creates ten templates. The second audit reuses eight and creates two more. The third audit reuses most of them. Within four or five page audits, the developer is shipping component-level reuse and the rewrites are mostly content swaps.

## What the agent will not do

This is a one-page on-page SEO agent. It does not touch:

- Off-page (digital PR, backlinks, outreach)
- Technical SEO outside of on-page (site speed, indexation, crawl budget)
- Multi-page content strategy beyond the single URL being audited
- Paid or social

When the real fix looks like one of those, the agent says so explicitly in the `AUDIT.md` "scope caveats" section and does not generate action items in those categories.

## Setup

### Required environment variables

Create a `.env` file in the project root (a `.env.example` is included). The agent reads from it at runtime.

```bash
DATAFORSEO_LOGIN=...
DATAFORSEO_PASSWORD=...
FIRECRAWL_API_KEY=...
OPENAI_API_KEY=...
```

For Ahrefs, the agent uses the Ahrefs MCP server. Connect it inside your Claude Code environment first and the agent will discover the tools automatically. No Ahrefs key in the .env file.

### Running the agent

The agent is defined entirely in `CLAUDE.md`. Drop the file into a Claude Code project folder, fill in the `.env`, and trigger the agent by giving it the URL you want to audit (or letting it ask for one).

### How long an audit takes

A typical audit takes 15 to 30 minutes of compute time, depending on how many competitors are full-scraped (the user picks 1 to 10) and how many LLM citation queries get retried. The output is one folder with three to four files.

## A note on the comprehensive product research dependency

The agent has a hard rule that every product claim entering `REWRITES.md` must be cross-checked against a comprehensive product research document at `Research/<your-product>_comprehensive.md`. If you are running this agent on a product you own, write this document once. It is the source of truth for what your product actually ships. Without it, the agent will fall back to inference, which is the failure mode that puts fictional features into your marketing copy.

## License

MIT. Use it, fork it, adapt it. If it saves you a few client hours, that is the whole point.

## Credits

Built by [@harsh719](https://github.com/harsh719). The orchestration runs inside Claude Code; the data layer is Ahrefs MCP + DataForSEO + Firecrawl + OpenAI's web-search-enabled models.
