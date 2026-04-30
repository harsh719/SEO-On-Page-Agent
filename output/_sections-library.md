# Formester section library

Reusable section templates from prior page audits. **Always check this file before proposing a new section in Phase 10.** If one of the templates below fits the gap with different copy, images, or data, reuse it and mark the block in REWRITES.md as `[REUSE: <template name> from <page-slug>]`. Only create a new template when the content model genuinely does not fit anything here. Every `[NEW TEMPLATE]` gets appended to this file.

Each entry below gives: the template name, what it is for, the layout pattern, the data slots the developer swaps, and the reference file that already implements it.

---

## 1. Problem framing / attention-grab headline

- **Purpose:** a single-H2 hero-adjacent block that names the reader's pain in one line. Sits between the hero and the first feature section.
- **Layout:** full-width block, one H2 (two sentences max), no supporting body, optional one-line sub. Centered, neutral background. High visual quiet.
- **Data slots:** H2 text (one sentence + one follow-up sentence), optional sub.
- **Reference:** [ai-form-generator.html](../ai-form-generator.html) — BLOCK 2, "Most forms take an afternoon to build. And lose half their respondents anyway."
- **When to reuse:** any page where the audit diagnosis is a specific, nameable reader frustration that belongs up top.

## 2. Interactive on-page generator (live tool embed)

- **Purpose:** let the visitor try the tool without signing up. Highest-leverage section on any product page where SERP winners run the tool inline.
- **Layout:** 960px-max centered card, soft purple glow behind. Header block with eyebrow pill + H2 + sub. Tab row (3 tabs max, e.g., Paste / Upload / URL). One input zone per tab. Option row of 2-3 compact selects. Primary CTA. Sample output block below the CTA showing a real generated item (question, form field, etc.) with the correct answer highlighted.
- **Data slots:** tab labels, placeholder copy per tab, option labels + values, CTA label, one sample output block (title + body + correct-answer row).
- **Reference:** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 1, "See it work. No sign-up."
- **Variant:** [ai-form-generator.html](../ai-form-generator.html) BLOCK 3 "Generate forms from a prompt, PDF, image, or URL" is a lighter-weight version without a live sample-output block.
- **When to reuse:** any AI tool product page (form generator, quiz maker, survey builder, any "turn X into Y" tool).

## 3. Three-card feature row (icon tile + heading + body)

- **Purpose:** the "Why use us" block. Three concrete differentiators presented as equal-weight cards.
- **Layout:** three equal columns, each a white card with top-left icon tile (44px purple-tint square + line-icon), H3 heading, body paragraph. Equal heights. Hover lifts the border to purple-edge.
- **Data slots:** section H2 + intro paragraph, plus three cards (icon, heading, body) each.
- **Reference:** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 2, "Why teachers and trainers pick Formester."
- **When to reuse:** any page that needs to deliver three clear differentiators after the hero.

## 4. Four-step "How it works" stepper

- **Purpose:** replace long, wordy how-it-works flows with a tight 4-step sequence.
- **Layout:** four equal columns, each a card with a purple numbered tile (1, 2, 3, 4) in the top-left corner, H3 heading, body paragraph. Hover raises the card.
- **Data slots:** section H2 + one-sentence intro, plus four step cards (heading + body).
- **Reference:** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 3, "How to create a test in under a minute."
- **Constraint:** keep it to four steps. If the real flow has 5 or 6 steps, merge (design + scoring → review; publish + analyze → share/analyze).
- **When to reuse:** any tool page that currently has a 5-to-6-step how-it-works section.

## 5. Persona grid (Who this is for)

- **Purpose:** segment the audience into specific personas with a concrete outcome per persona.
- **Layout:** 3 columns × 2 rows on desktop (6 personas), drops to 2 cols on tablet, 1 on mobile. Each persona card has an icon tile, H3 persona name, body paragraph naming a concrete source document and outcome, and an optional deep-link to a relevant template/resource.
- **Data slots:** section H2 + intro, plus 4-8 persona cards (icon, name, body, optional link).
- **Reference (6 personas):** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 4, "Who this is for."
- **Reference (8 tiles variant):** [ai-form-generator.html](../ai-form-generator.html) — BLOCK 5, "Who is this for?"
- **When to reuse:** any product page where the tool serves distinct buyer segments with different use cases. Almost every Formester page should have this.

## 6. Four-row attribute block (inputs / types / integrations)

- **Purpose:** scannable capability reference. Replaces buried feature text.
- **Layout:** white rounded container, four horizontal rows separated by hairlines. Each row has a left label column (icon tile + short label + sub-label) and a right value column (body text + optional chip row).
- **Data slots:** section H2 + intro, plus four rows (label, sub-label, body text, optional chips).
- **Reference:** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 5, "Inputs, question types, and exports."
- **When to reuse:** any product page where the tool has multiple input types, output types, export destinations, or integration lists that need to be visible at a glance.

## 7. Dark trust / quality block with peer-reviewed citation

- **Purpose:** name the reader's concern about AI quality or data safety and address it directly with a cited source.
- **Layout:** full-width rounded dark block (radial purple gradient on navy). Two-column on desktop: left = eyebrow + H2 + two body paragraphs, right = lighter "proof" card containing an eyebrow pill, italic blockquote, and cite line with a small badge icon. Collapses to one column on mobile.
- **Data slots:** eyebrow, H2, two paragraphs of body, proof-card pill label, blockquote text, cite line.
- **Reference:** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 6, "How we keep AI-generated questions honest."
- **Variant:** [ai-form-generator.html](../ai-form-generator.html) — BLOCK 6, "Your prompts, your forms, your data." (data-privacy version, lighter visual treatment)
- **When to reuse:** any AI-related page where users have a trust concern (accuracy, privacy, bias, data ownership, hallucination).

## 8. Three-card feature row — alternate framing

- **Purpose:** second 3-card grid when the page needs two distinct 3-card rows. Same visual as template 3, different framing (operational rather than promotional).
- **Layout:** identical to Template 3.
- **Reference:** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 7, "Built for the way tests actually get run."
- **Note:** this is not a separate template from Template 3; it is Template 3 used a second time on the same page. Listed here only so a future auditor sees it can be used twice per page when the content supports it.

## 9. Three-column form-preview grid (scrollable card + external CTA)

- **Purpose:** show real generated output side-by-side across different use cases. Used for "see it work" proof sections.
- **Layout:** 3-column grid on desktop, 2 cols on tablet, 1 col on mobile. Each column contains (a) a fixed-height card (620px desktop, 580px mobile) with header + scrollable body + footer, and (b) a full-width primary CTA button below the card (outside it). Card header shows category pill + card title + two meta lines + two stat pills. Card body scrolls internally through 3 sample items. Card footer shows "X more questions" hint.
- **Data slots:** section H2 + intro, plus three columns each with: category label, card title, source line, output line, two stat pills (count + time), 3 sample items, footer hint, CTA button label.
- **Reference:** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 8, "See it work on three real inputs."
- **When to reuse:** any AI tool page where three distinct input types produce three distinct outputs worth previewing side by side (classroom / corporate / coaching, or marketing / product / support, etc.).

## 10. Comparison table (us vs alternative)

- **Purpose:** side-by-side feature comparison against a named alternative. Useful where SERP competitors include category pages and the page must defend a point of view.
- **Layout:** two-column comparison table (Us vs Them), row headers on the left, checkmarks or X marks with short explainer text. Header row labels both columns. Mobile stacks.
- **Data slots:** H2 + intro, column headers, 6-10 comparison rows (row label, us value, them value).
- **Reference:** [ai-form-generator.html](../ai-form-generator.html) — BLOCK 7, "AI form generator vs traditional form builder."
- **When to reuse:** pages targeting "X vs Y" intent, alternative pages, or any page where a structural comparison is the best proof format.

## 11. Related templates / cross-sell grid

- **Purpose:** surface related templates or sister products. Internal-link-driven.
- **Layout:** 2-to-4 column grid of template cards. Each card is a clickable link with an icon tile (left) + card title (middle) + right-arrow (right).
- **Data slots:** section H2 + intro, plus 6-10 link items (anchor text, icon, target URL).
- **Reference (link-list version):** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 10, "More from Formester for teachers and trainers."
- **Reference (template-preview version):** [ai-form-generator.html](../ai-form-generator.html) — BLOCK 8, "Related templates. Start from a proven structure."
- **When to reuse:** bottom of every product page.

## 12. Cross-sell band (more AI tools from Formester)

- **Purpose:** horizontal cross-sell to sister AI products (AI form builder, AI test generator, AI quiz maker, etc.).
- **Layout:** full-width band, 3-4 tiles per row, each tile a product thumbnail + name + one-line descriptor + deep link.
- **Data slots:** section H2, plus product tiles.
- **Reference:** [ai-form-generator.html](../ai-form-generator.html) — BLOCK 9, "More AI tools from Formester."
- **When to reuse:** any AI product page within the Formester family.

## 13. FAQ (flattened accordion)

- **Purpose:** answer reader questions, map cleanly to FAQPage JSON-LD.
- **Layout:** max-820px centered column, each question is a `<details>` accordion with a chevron tile that rotates to purple when open. No sub-group headings (flat list only). One Q per row.
- **Data slots:** section H2 + intro, plus N Q/A pairs. Mirror in FAQPage JSON-LD.
- **Reference:** [test-creator/prototype-v2.html](test-creator/prototype-v2.html) — Section 9, "Frequently asked questions."
- **When to reuse:** every page with 5+ FAQs.

## 14. Feature callout highlight row

- **Purpose:** design change applied to an existing feature grid, promoting one feature to hero-within-the-grid treatment.
- **Layout:** existing feature grid, but one tile is enlarged to 2x width with a stronger background and a "learn more" deep link. Other tiles remain standard size.
- **Data slots:** which tile to promote, promoted tile's extended copy + CTA.
- **Reference:** [ai-form-generator.html](../ai-form-generator.html) — BLOCK 4, feature grid DESIGN CHANGE.
- **When to reuse:** pages with an existing feature grid that under-serves a specific high-value feature.

## 15. AI agent / API / MCP differentiator block

- **Purpose:** surface technical depth (REST API + MCP server) as a buyer-facing differentiator, not as a developer-doc page. Designed for AI-tool product pages where competitors lack MCP support and the buyer is increasingly likely to be plugging things into Claude or ChatGPT.
- **Layout:** two-card grid on desktop (1.1fr each), stacks to single column under 760px. Each card is a slightly off-white card with subtle vertical gradient, a colored badge pill at the top (one solid brand purple, one solid ink-black for visual differentiation), an H3 heading, a row of 3 small chips naming the use-case attributes (e.g., "Claude · ChatGPT · Custom agents" on Card 1; "Self-service token · Org-level scope · Business plan +" on Card 2), a body paragraph, and a primary-color "→" CTA link to the relevant docs page.
- **Data slots:** section eyebrow, H2, intro line, two cards (each with: badge label, badge variant solid or alt, H3, three attribute chips, body paragraph, CTA label, CTA URL).
- **Reference:** [ai-quiz-maker/prototype.html](ai-quiz-maker/prototype.html) — BLOCK 7B, "Your quiz data, ready for the AI you already use."
- **When to reuse:** any AI-tool product page on Formester (AI quiz maker, AI form generator, AI survey generator, future AI tools). The MCP angle is unique-to-Formester on every comparable SERP, so it works as a defensive moat on every AI-tool page. Use the same pair of cards across pages and only swap (a) the example questions in the MCP card body to match the page topic (quiz scores, form completions, survey responses) and (b) the page-specific use-case framing in the intro.

## 16. Three-column decision matrix block

- **Purpose:** stake a methodological point of view by comparing three closely-confused concepts (e.g., questionnaire vs. survey vs. form) across the dimensions a buyer cares about. Designed for category-defining pages that need to win the definitional question without trying to outrank Wikipedia, SurveyMonkey, or Qualtrics directly.
- **Layout:** centered table inside a rounded white card with subtle shadow. Header row is brand-tinted; first column (dimension labels) sits on a soft surface tone with bold ink-color labels. 5-7 rows of dimensions × 3 columns of options. Below the table, a one-paragraph "concrete example" land that names a real-world case for each of the three options. Mobile collapses cleanly by removing the fixed first-column width.
- **Data slots:** section eyebrow, H2, intro line, three column headers, 5-7 rows (dimension label + 3 cell values), one closing paragraph with a concrete tri-part example.
- **Reference:** [questionnaire-maker/prototype.html](questionnaire-maker/prototype.html) — Section 6, "Questionnaire vs survey vs form. Pick the right tool for the job."
- **When to reuse:** any page whose primary keyword sits in a confused category (questionnaire/survey/form, quiz/test/assessment, form/landing page) where competitors own definitional searches and our page needs a defensible point of view.

## 17. Industry benchmark table block

- **Purpose:** ship a researcher-credible benchmark table that names the realistic completion-rate (or comparable industry KPI) by use case. Earns LLM citation and serves as the trust anchor for the methodology block that typically follows.
- **Layout:** centered table with a dark header row (ink black, white text), benchmark column rendered in brand-color tabular-nums for scanability, three columns total (use case, typical rate, what lifts it). Below the table, a small note line cites the source. A brand-tinted callout block sits beneath the note with a one-paragraph synthesis (rule of thumb + supporting stat). Mobile stacks the table.
- **Data slots:** section eyebrow, H2, intro line, three column headers, 5-9 benchmark rows, source note, callout paragraph with a bolded number or two.
- **Reference:** [questionnaire-maker/prototype.html](questionnaire-maker/prototype.html) — Section 7, "What completion rate should you expect?"
- **When to reuse:** any page where industry-specific benchmarks differentiate the buyer's expectation. Works for completion rates (questionnaires, surveys), open rates (email tools), conversion rates (lead-gen forms), and similar KPI comparisons by industry.

---

## How to reference a template in REWRITES.md

Prefix the section header with one of:

- `[REUSE: <template name> from <page-slug>]` when you are reusing an existing template. Example: `[REUSE: Persona grid (6 tiles) from test-creator]`. Then list only the new content (icons, names, body, links) that the developer swaps into the existing component.
- `[NEW TEMPLATE]` when no existing template fits. Then describe the layout in REWRITES.md, produce the HTML in `prototype.html`, and append a new entry to this library file.

## How to add a new template to this library

When a `[NEW TEMPLATE]` section ships, add it to this file with:

- A sequential number.
- One-line purpose.
- Layout description (columns, responsive behavior, key visual cues).
- Data slots the developer swaps.
- Reference file path and section number.
- Variant references if any.
- "When to reuse" guidance so the next auditor spots the fit.
