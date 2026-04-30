# SEO On Page Agent

A multi-phase agent that audits a single landing page, diagnoses why it lost search clicks, and produces a markdown deliverable with a gap analysis, ready-to-ship copy rewrites, full new sections, internal-linking recommendations, schema markup, and a working HTML prototype of every new section. Runs inside Claude Code. Reads from Ahrefs, DataForSEO, Firecrawl, and OpenAI.

This README is a complete operating manual. It walks through what the agent does, every API account you need, how to get each key, how to run your first audit, what each output file is, how to adapt the agent to your own product, and the gotchas that will trip you up the first time.

If you only have two minutes, read **Why this exists** and **The 10 phases**. If you are setting up for the first time, read top to bottom.

## Table of contents

- [Why this exists](#why-this-exists)
- [The 10 phases](#the-10-phases)
- [What you will need before you start](#what-you-will-need-before-you-start)
- [Cost expectations](#cost-expectations)
- [Step 1: Get the API keys](#step-1-get-the-api-keys)
  - [DataForSEO](#dataforseo)
  - [Firecrawl](#firecrawl)
  - [OpenAI](#openai)
  - [Ahrefs (via MCP, not a .env key)](#ahrefs-via-mcp-not-a-env-key)
- [Step 2: Install the agent](#step-2-install-the-agent)
- [Step 3: Wire up the Ahrefs MCP server inside Claude Code](#step-3-wire-up-the-ahrefs-mcp-server-inside-claude-code)
- [Step 4: Write your product research document](#step-4-write-your-product-research-document)
- [Step 5: Adapt CLAUDE.md to your site and competitors](#step-5-adapt-claudemd-to-your-site-and-competitors)
- [Step 6: Run your first audit](#step-6-run-your-first-audit)
- [Step 7: Read the output](#step-7-read-the-output)
- [The section library](#the-section-library)
- [Common errors and how to fix them](#common-errors-and-how-to-fix-them)
- [What the agent will not do](#what-the-agent-will-not-do)
- [Customising the agent](#customising-the-agent)
- [License and credits](#license-and-credits)

## Why this exists

Most SEO audits stop at "you ranked here, now you rank here." This one tries to answer the harder question: **what changed on the SERP that explains the drop, and what specifically should the page say differently to recover.** It does that by validating the hypothesis the user shows up with (rather than accepting it), running a fresh competitive scrape against the live top 10, comparing structures, classifying every gap as either a copy fix or a product gap (so the agent never claims a feature the product does not actually ship), and writing the new copy itself instead of hand-waving with "rewrite this section to be more comprehensive."

Two principles run through the whole thing:

1. **Diagnose first, prescribe second.** Every recommendation has to trace back to a specific gap, competitor pattern, or diagnosis point. No generic SEO advice.
2. **Never claim a feature the product does not have.** When a competitor pattern surfaces a feature, it gets classified as verified, verifiable in minutes, or product gap. Only verified features end up in copy. Product gaps go to the build team.

## The 10 phases

The agent runs end-to-end through these phases. Phase numbers match the structure inside `CLAUDE.md`.

| Phase | What it does | Tools used |
|---|---|---|
| 0. Input intake | Collects 13 inputs from the user one at a time (URL, primary keyword, click loss, hypothesis, inspirational pages, full-scrape count). Echoes them back for confirmation. | None |
| 1. Validation | Cross-checks the user's keyword volume against Ahrefs. Validates SERP intent against the live DataForSEO SERP. Scrapes the audit URL with Firecrawl. Extends the click-loss hypothesis with new drivers (AI Overview presence, intent shifts, position drops, new ranking competitors). | Ahrefs, DataForSEO, Firecrawl |
| 2. Page scrape | Pulls the audit URL through Firecrawl. Extracts headings, schema, FAQ Q+A, body content, internal links, alt text, word count, last-modified date. | Firecrawl |
| 3. Competitive recon | Pulls the top 10 ranking pages via Ahrefs SERP overview. Full-scrapes the top N (user picks 1 to 10) with Firecrawl. Structure-only scrapes the rest. | Ahrefs, Firecrawl |
| 4. Gap analysis | Compares ours vs competitors across heading structure, content depth, FAQ coverage, entity coverage, UX elements, schema, layout patterns. Classifies every gap as verified, verifiable in minutes, or product gap. | Internal logic |
| 5. Keyword usage audit | Checks where primary and secondary keywords appear (slug, title, meta, every heading level, first 100 words of body, image alts). Flags stuffing and natural absences. | Internal logic |
| 6. LLM citation recon | Runs four to six structured queries against `gpt-4o-search-preview` (Chat Completions) and the Responses API with `web_search`. Logs cited domains and brand mentions. Identifies the content patterns that get cited. | OpenAI |
| 7. Internal linking | Pulls the sitemap. Scores relevance to the audit URL's primary topic. Recommends contextual and section-based links. Writes the section copy when a dedicated related-links section is warranted. | Sitemap fetch |
| 8. Topical research | A second OpenAI web-search pass aimed at content depth (statistics, benchmarks, sub-topics that competitors cover but the audit URL does not). | OpenAI |
| 9. Synthesis | Builds a prioritised action list: Critical, High, Medium, Low. Every action ties back to a specific diagnosis or gap. | Internal logic |
| 10. Copy rewrite | Writes the actual copy for every change. Uses a section library to reuse layouts across pages. Produces three files: `AUDIT.md`, `REWRITES.md`, and `prototype.html`. | Internal |

## What you will need before you start

### Software

- **[Claude Code](https://claude.com/claude-code)** — the CLI or VS Code extension version. The agent runs inside Claude Code; that is non-negotiable, because the agent depends on the Anthropic tool-calling loop and on the Ahrefs MCP server connection.
- **An Anthropic account** with at least the Pro plan, or API credits if you are using Claude Code via API.
- **A code editor** (VS Code, Cursor, or whatever you already use).
- **`bash`, `curl`, and `python3`** on your machine. The agent shells out to all three.

### Paid accounts

You will need active accounts on four services. Three give you a key for the `.env` file. The fourth (Ahrefs) connects via MCP.

- **DataForSEO** — for live SERP data, SERP feature detection, and AI Overview presence.
- **Firecrawl** — for scraping the audit URL and competitor pages.
- **OpenAI** — for the LLM citation recon and topical research phases.
- **Ahrefs** with API access — for keyword volumes, rank tracking, and the SERP overview.

## Cost expectations

Per audit, end to end, expect roughly:

| Tool | Typical cost per audit | What drives it |
|---|---|---|
| DataForSEO | $0.05 to $0.15 | Live SERP request, depth 20 |
| Firecrawl | $0.10 to $0.30 | One audit URL + 1 to 10 competitor scrapes |
| OpenAI | $0.40 to $1.20 | 4 to 6 citation queries plus 2 to 4 topical queries on `gpt-4o-search-preview` |
| Ahrefs | Included in your subscription | API calls count against your monthly Ahrefs API quota |
| **Total** | **~$1 to $2 per audit** | |

If you run 10 audits a month, you are looking at $10 to $20 in pay-as-you-go API spend, plus your fixed Ahrefs subscription (Lite is enough for low volume; Standard if you are heavy).

## Step 1: Get the API keys

Do these in any order. Each should take 5 to 10 minutes.

### DataForSEO

1. Sign up at [dataforseo.com](https://dataforseo.com).
2. Add credits to your account (the SERP API is pay-per-call; $50 buys you a couple of thousand SERP requests, which is far more than you will need for personal use).
3. Open the dashboard. Your **API login** is your account email; your **API password** is on the API Access page.
4. Test the credentials with this curl (replace the two placeholders):
   ```bash
   curl -s -X POST "https://api.dataforseo.com/v3/serp/google/organic/live/advanced" \
     --user "YOUR_LOGIN:YOUR_PASSWORD" \
     -H "Content-Type: application/json" \
     -d '[{"keyword":"test query","location_code":2840,"language_code":"en","device":"desktop","depth":10}]' \
     | python3 -m json.tool | head -40
   ```
   You should see a structured JSON response with `tasks` and `result` objects. If you see `{"status_code": 40101}`, your credentials are wrong.

### Firecrawl

1. Sign up at [firecrawl.dev](https://firecrawl.dev).
2. The Free plan gives you 500 page scrapes per month. The Hobby plan ($19/month) gives 3,000. One audit uses up to 11 scrapes (audit URL + up to 10 competitors), so the Free plan covers about 45 audits per month.
3. Dashboard → **API Keys** → **Create new key**. Copy the value (it starts with `fc-`).
4. Test it:
   ```bash
   curl -s -X POST "https://api.firecrawl.dev/v2/scrape" \
     -H "Authorization: Bearer fc-YOUR_KEY" \
     -H "Content-Type: application/json" \
     -d '{"url":"https://example.com","formats":["markdown"]}' \
     | python3 -m json.tool | head -20
   ```
   You should get `"success": true` and the scraped markdown.

### OpenAI

1. Sign up or sign in at [platform.openai.com](https://platform.openai.com).
2. Add a billing method and at least $5 in credit. Without billing set up, you will not have access to the search-enabled models.
3. Go to **API keys** → **Create new secret key**. Copy it (it starts with `sk-proj-` or `sk-`).
4. Confirm you have access to `gpt-4o-search-preview`. Test with:
   ```bash
   curl -s -X POST "https://api.openai.com/v1/chat/completions" \
     -H "Authorization: Bearer sk-YOUR_KEY" \
     -H "Content-Type: application/json" \
     -d '{"model":"gpt-4o-search-preview","messages":[{"role":"user","content":"What is the capital of France? Cite a source."}]}' \
     | python3 -m json.tool | head -40
   ```
   You should get a response with a `content` field and `annotations[].url_citation` entries. If you see `model_not_found`, your account has not been provisioned for the search models yet (this happens to brand-new accounts; usually clears within a few hours).

### Ahrefs (via MCP, not a .env key)

This is the only one that does not go in `.env`. You connect Ahrefs through Anthropic's MCP server marketplace inside Claude Code.

1. Have an Ahrefs subscription with API access. The Lite plan ($129/month) has limited API quota; Standard ($249/month) gives you enough for 50+ audits/month. Without API access, this agent cannot run.
2. Get your **Ahrefs API token**: Account → API → Generate token.
3. Wiring it up inside Claude Code happens in Step 3.

## Step 2: Install the agent

```bash
# 1. Clone the repo
git clone https://github.com/harsh719/SEO-On-Page-Agent.git
cd SEO-On-Page-Agent

# 2. Copy the env template
cp .env.example .env

# 3. Open .env in your editor and paste your three keys
#    (DATAFORSEO_LOGIN, DATAFORSEO_PASSWORD, FIRECRAWL_API_KEY, OPENAI_API_KEY)
```

The `.env` file is `.gitignore`-blocked by default. It will never accidentally get pushed.

## Step 3: Wire up the Ahrefs MCP server inside Claude Code

This is the trickiest setup step. Take it slow.

1. Open the project folder in Claude Code (the CLI or the VS Code extension; whichever you use).
2. In the chat panel, type `/mcp` and hit enter. This opens the MCP server management UI.
3. Find or search for **Ahrefs** in the available servers list. (As of this writing, Ahrefs ships an official MCP server through claude.ai's directory.)
4. When prompted for credentials, paste the Ahrefs API token you generated in Step 1.
5. Confirm the connection. Claude Code should show "Connected" next to Ahrefs.
6. Verify the tools are discoverable: ask Claude something simple like "list the Ahrefs tools you have access to". You should see a list including `keywords-explorer-overview`, `serp-overview`, `site-explorer-organic-keywords`, and many more. The agent depends on these.

If the Ahrefs MCP is not connected, the agent will fail at Phase 1 and tell you which tool was missing.

## Step 4: Write your product research document

This is the most important step. Skipping it is the single biggest cause of bad output.

The agent has a hard rule: every product claim that ends up in `REWRITES.md` must be verifiable against a comprehensive product research document at `Research/<your-product>_comprehensive.md` in the project root. Without this file, the agent falls back to inference, which is the failure mode that puts fictional features into your marketing copy.

The document should catalogue what your product actually ships, in detail. Aim for these sections:

1. **Product overview** — what the product is, who it serves, where it sits in the market.
2. **Core capabilities and building blocks** — every feature, organised by category (form editor, AI features, analytics, integrations, etc.).
3. **Specialised builders** — surveys, quizzes, polls, anything domain-specific.
4. **Branding, layout, UX features** — branding kit, design controls, embed modes.
5. **Integrations and automation** — every named integration, plus the API and webhook story.
6. **Use cases** — by buyer persona, with the actual workflow.
7. **Pricing model and tiers** — every plan, every feature gate, every quota.
8. **Strengths and differentiators** — the real moats, not marketing copy.
9. **Gaps and limitations** — what the public sources do *not* document. Be honest. The agent reads this section and treats anything in it as "do not write into copy without verification".
10. **Verified facts addendum** — append this at the bottom every time you verify a feature with the product team or via direct testing. Date-stamped. The most recent addendum wins over anything earlier in the document.

Plain markdown. Cite the source for every claim (help center URL, pricing page, DPA, screenshots). The document will live for years and grow with every audit.

If you do not have one yet, ask the product team to walk you through every feature on the marketing site and the help center. One afternoon of work. Pays back across every page audit you run for the next two years.

## Step 5: Adapt CLAUDE.md to your site and competitors

`CLAUDE.md` ships hardcoded with one project's context (Formester, with a competitor set including Tally, Fillout, forms.app, Aidaform, Typeform, Jotform). Before you run an audit on your own site, swap these:

1. Open `CLAUDE.md`.
2. Find the **Hardcoded context** section near the top.
3. Replace `formester.com` with your domain.
4. Replace the sitemap URL with yours.
5. Replace the competitor set with five to seven of your real competitors. The agent uses this list as a fallback when SERP recon does not surface obvious peers.
6. (Optional) Update the agent's tone-of-voice instructions in the **Operating principles** section if your brand voice differs from the default ("clear, practical, direct, no marketing fluff").

Save the file. The agent will pick up the new context on the next run.

## Step 6: Run your first audit

Open Claude Code in the project folder (the same one with the modified `CLAUDE.md`). Tell the agent something like:

> Let's audit a page on my site.

It will start Phase 0 and ask you 13 questions, one at a time. Answer each one carefully (the answers feed every later phase). The 13 inputs in order:

1. **URL to audit**
2. **Primary keyword**
3. **Primary keyword volume** (your guess; the agent will validate against Ahrefs and tell you if it is off)
4. **Secondary keywords** (comma-separated or newline-separated)
5. **Click loss** in the last 3 months vs prior 3 months (a number)
6. **Current page intent** (product, comparison, template, guide, etc.)
7. **SERP intent** (listicles, product pages, comparisons, etc.)
8. **Page type** (product, feature, comparison, alternative, template, blog, pricing, guide, landing)
9. **Free-text observations** about the page or the SERP
10. **Your click-loss hypothesis** — why you think clicks dropped
11. **Your recommendations** — what you think should change
12. **Inspirational pages** — URLs of pages doing well that you want to learn from
13. **Full-scrape count** — how many top-ranking competitor pages to scrape in full (1 to 10)

The agent will echo all 13 answers back and ask you to confirm. Once you confirm, it runs Phases 1 through 10 end to end. Expect 15 to 30 minutes of compute, depending on the full-scrape count and OpenAI rate limits.

You can interrupt at any time. The agent caches Firecrawl scrapes and Ahrefs responses to disk under `output/<page-slug>/_competitors/` and `_research/`, so resuming after an interrupt does not re-spend on the calls already made.

## Step 7: Read the output

Every audit produces a folder at `output/<page-slug>/`. The folder contains three deliverables and two cache directories.

```
output/your-page-slug/
├── AUDIT.md                  ← diagnosis, gap analysis, prioritised actions
├── REWRITES.md               ← copy ready to ship
├── prototype.html            ← working HTML for every new section
├── _competitors/             ← cached competitor scrapes (Firecrawl)
└── _research/                ← cached LLM citation + topical research
```

### `AUDIT.md`

Aim for under 300 lines. Sections:

1. Executive summary (3 to 5 bullets).
2. Diagnosis — your hypothesis validated and extended with new drivers.
3. Prioritised action list — Critical, High, Medium, Low.
4. Gap analysis table (ours vs competitors).
5. Keyword usage audit table.
6. Schema recommendations (rich-result plays, not ranking plays).
7. LLM citation insights (content patterns only, no PR recs).
8. Research sources from the OpenAI topical research phase.
9. Scope caveats — what the audit does not cover.

### `REWRITES.md`

The bulk of the value. Every change is a labeled `Current X` / `New X` pair, so a writer can scan the file and locate each change against the live page without guessing.

Format for every element:
```
**Current title:**

[exact existing text from the live page]

**New title:**

[the rewritten copy]

**Why:** [one-line rationale tied to gap or diagnosis]
```

This applies to title, meta, every heading level, every subhead, every card, every body block, every FAQ Q+A, every alt text, every CTA label.

New sections come with a **Placement** instruction (where on the page they go), full ready-to-paste copy, and a `[REUSE: <template>]` or `[NEW TEMPLATE]` tag pointing to the section library.

### `prototype.html`

A self-contained HTML file with production-ready markup for every new section and every redesigned section. The brand tokens are inlined as CSS variables, the responsive breakpoints work, and a developer can hand the file to engineering and ship the change without re-asking for design intent. Copy-only rewrites (title, meta, FAQ wording) do not need a visual prototype, so they are not in this file.

## The section library

The agent maintains a growing library of reusable section templates at `output/_sections-library.md`. Before proposing a new section, the agent checks the library first; if an existing template fits the gap with different copy, images, or data, the agent reuses it and marks the block in `REWRITES.md` as `[REUSE: <template name> from <page-slug>]`. Only genuinely new content models get a `[NEW TEMPLATE]` entry, which automatically appends to the library file.

This is the part that compounds. The first audit on a site creates ten templates. The second audit reuses eight and creates two more. The third audit reuses most of them. Within four or five page audits, your developer is shipping component-level reuse and the rewrites become mostly content swaps.

The library that ships with this repo has 15 templates already, drawn from prior audits. You can keep growing it, prune it, or fork it for your own brand.

## Common errors and how to fix them

### "OpenAI server error on `gpt-4o-search-preview`"
The search-enabled preview models are flaky. About one in five queries returns a 500. The agent retries automatically and falls back to the Responses API with the `web_search` tool if the preview model fails twice. If both fail, wait five minutes and try again; OpenAI usually recovers within an hour.

### "Firecrawl 429 rate limit"
You hit your monthly scrape quota or per-minute rate limit. Either upgrade your Firecrawl plan or wait for the rate-limit window to reset (one minute for per-minute, end of month for monthly).

### "Ahrefs MCP tools not found"
Restart Claude Code. The MCP connection sometimes drops between sessions. Re-confirm the connection with `/mcp` and re-run.

### "DataForSEO timeout"
The default timeout in the agent's curl calls is 120 seconds. SERP responses on rare keywords can take longer. If you see this, edit the agent's curl call to use `--max-time 240` and retry.

### "Audit ran but `REWRITES.md` claims a feature my product does not have"
You are missing the comprehensive product research document, or the doc is out of date. Add the feature to the **Gaps and limitations** section of the doc with a "do not claim in copy" note. The agent will respect it on the next run.

### "Ahrefs returned `column not found` errors"
Ahrefs occasionally changes the columns on its endpoints. The agent's `select` parameters get out of sync. The fix is to call the Ahrefs `doc` tool first (the agent does this automatically the first time), get the current column list, and update the `select` parameter.

## What the agent will not do

This is a one-page on-page SEO agent. It deliberately does not touch:

- **Off-page** — digital PR, backlink building, outreach, link reclamation
- **Technical SEO outside on-page** — site speed, indexation, crawl budget, robots.txt
- **Multi-page content strategy** beyond the single URL being audited
- **Paid** — Google Ads, Bing Ads, retargeting
- **Social** — LinkedIn, Twitter, anything community-led

When the real fix looks like one of these, the agent says so explicitly in the `AUDIT.md` "scope caveats" section and does not generate action items in those categories. That is on purpose. A surface that scopes itself ships better recommendations than one that tries to cover everything.

## Customising the agent

The agent is a single markdown file (`CLAUDE.md`). All behaviour lives there. To change anything, edit the file:

- **Tone of voice** — find the **Formatting rules for `REWRITES.md`** section and adjust the voice guidance.
- **New phase** — add a Phase 11 (or insert at any point) and describe what it should do, what tools it should use, and what it should output.
- **Different data layer** — swap DataForSEO for Semrush by editing the Phase 1 instructions and the curl examples.
- **Different output structure** — edit Phase 10 and the **Final deliverable structure** section.

The agent reads `CLAUDE.md` at the start of every conversation. No restart needed.

## License and credits

MIT. Use it, fork it, adapt it. If it saves you a few hours on your next page audit, that is the whole point.

Built by [@harsh719](https://github.com/harsh719). The orchestration runs inside Claude Code; the data layer is Ahrefs MCP plus DataForSEO, Firecrawl, and OpenAI's web-search-enabled models.

Issues, pull requests, and forks are welcome. If you adapt the agent for a new domain or extract one of the phases as a standalone agent, ping me and I will link to it from this README.
