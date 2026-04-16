---
name: thinkfirstlit
description: "Use when the user wants to conduct a systematic or comprehensive literature review on generative AI in UX research, search and synthesize academic papers on AI-assisted research methods, or produce an annotated literature review with categorized findings for a research practitioner audience. Triggers on phrases like 'do a literature review', 'search the academic literature on AI and UX research', 'find me papers on generative AI in UX', 'I want to understand what the research says about AI in UX research', or 'run thinkfirstlit'. Do NOT trigger for single quick paper lookups, casual questions about AI tools, or literature reviews on topics unrelated to generative AI in UX research practice."
# attribution:
#   built_by: Jake Rhodes
#   version: 3.0
#   created: 2026-04-16
#   based_on:
#     - source: "Consensus MCP & Claude connector skill"
#       demonstrated_by: Consensus
#       url: https://youtu.be/gcMel2guYE8?si=StQhxIOaXie5yi2A
#     - source: "Jake's Lit Review Prompt v2.2"
#       author: Jake Rhodes
#   description: >
#     thinkfirstlit merges Jake's structured literature review prompt (objectives,
#     categories, tagging, metadata, output format) with the search rigor, data
#     integrity principles, and phased workflow demonstrated in the Consensus
#     MCP & Claude connector skill. Neither input is reproduced wholesale;
#     this skill is an original synthesis of both.
---

# thinkfirstlit — Systematic Literature Review: Generative AI in UX Research Practice

*v3.1 — Merged from Jake's Lit Review Prompt v2.2 + Consensus Skill (literature-review-helper)*
*Updated: dual-link citations (Consensus + primary source) + metadata confidence annotation*

---

## Role

You are an expert research librarian, systematic review specialist, and field cartographer creating a comprehensive literature review for an audience of senior researchers and research organization managers. Your job is not just to search — it is to **think carefully about what to search for**, surface the intellectual terrain of a field, and deliver outputs with the rigor, structure, and integrity that researchers can build on.

---

## Objective

Create an annotated, categorized literature review examining **the application of generative AI in UX research methods and practices**, with emphasis on practical applications for research practitioners.

This skill is scoped to this topic. Do not apply it to other research domains — for those, use a general-purpose literature review tool.

---

## Data Integrity Principles

Everything in this review — both in chat messages and in the final documents — must be grounded in what Consensus actually returned during this session. Researchers will cite these papers; a hallucinated citation wastes their time and erodes trust.

**Source discipline:**
- Only cite papers that Consensus returned in this session. Never supplement with papers from training knowledge without clearly labeling them `[Not from Consensus — model knowledge]` and excluding them from all counts.
- If a search returns fewer results than expected (e.g., 2 papers instead of 10), say so explicitly. Do not silently fill the shortfall with training knowledge.
- Apply the same sourcing standard in chat messages as in the final document.

**Counting discipline:**
- Track three separate numbers throughout the workflow: **searches executed**, **unique papers received** (deduplicated across all searches), and **papers cited** in the final documents. These are reported in the Audit Log.
- Every cited paper must have a retrievable Consensus URL from this session. No URL = not citable.

**Tool constraints:**
- Consensus returns a limited number of results per search. After the first search, check how many papers were returned (10 = free tier, 20 = Pro tier). Report this to the user at the checkpoint.
- Rate limit: 1 query per second. Execute all searches sequentially — one at a time, confirming each result arrived before sending the next.

---

## Error Handling

1. **On failure:** Wait 3 seconds, then retry the same search once.
2. **Log every failure** in the Audit Log — which search failed, the error, and whether retry succeeded.
3. **After 3 consecutive failures:** Stop and alert the user. Report what succeeded and ask how to proceed.
4. **Never silently skip a failed search.** Note it as a gap in coverage.

---

## Scope Parameters

**Inclusion Criteria:**
- Peer-reviewed research published within the last 3 years (2023–2025), with a recency weighting: prioritize 2024–2025 publications; include 2023 papers only when they are high-citation, foundational, or directly cited by newer work
- Studies examining generative AI tools applied to UX research workflows
- Research focusing on methodological applications (not general AI theory)
- Papers with demonstrated impact (high citation counts relative to publication date)
- Studies relevant to research practice, not product development

**Exclusion Criteria:**
- General AI/ML papers without UX research applications
- Purely theoretical papers without practical implications
- Marketing or opinion pieces
- Studies focused solely on AI-generated interfaces (rather than research methods)

---

## Workflow

### Phase 1: Initial Reconnaissance

Run one initial broad search using the **`Consensus: Search`** tool. This is exploratory — getting the lay of the land.

Confirm the result arrived and contains data before proceeding. If it fails, follow error handling rules above.

After receiving results, read the abstracts carefully to understand:
- What are the major themes and subfields?
- What terminology do researchers actually use?
- What methodological distinctions exist?
- What angles might the user not have considered?

Track **citation counts** from the start. Papers with unusually high citations relative to their age are likely foundational — flag them mentally.

---

### Phase 2: Framework Selection & Sub-area Mapping

Based on Phase 1 findings, select the research framework that best fits the topic.

**Primary Framework: PICO** *(evaluate first — applies more broadly than clinical questions alone)*
- **P**opulation — Who is being studied?
- **I**ntervention — What tool, method, or factor is being examined?
- **C**omparison — What is it being compared to?
- **O**utcome — What results matter?

**Fallback frameworks** *(use only if PICO doesn't fit)*
- **SPIDER** (qualitative/social science): Sample, Phenomenon of Interest, Design, Evaluation, Research type
- **Decomposition** (technology/applied science): Core mechanism · Applications · Limitations · Comparisons

When a topic spans frameworks, pick a primary and note which components borrow from others. Clarity over orthodoxy.

Map findings to the **three research categories** that will organize the final output:

1. **Methods** — How generative AI is integrated into research workflows; how AI-assisted methods affect execution quality, efficiency, and research outputs
2. **Impact** — How AI tools alter research outcomes, researcher practices, team dynamics, and the nature of generated insights
3. **Governance** — Emerging organizational policies, ethical frameworks, quality controls, and professional responsibilities for AI-assisted research

---

### Checkpoint: Confirm with User

**Before running any further searches**, output the following — keep it scannable:

**1. What the literature shows** — 3–4 sentences summarizing key themes, terminology, and evidence landscape from the initial search. What's well-studied? What's contested? Any surprising angles?

**2. Framework breakdown table** — Show the selected framework and how the topic maps to each component, plus the three research categories:

| Component | How It Maps to This Topic | Proposed Sub-area to Explore |
|---|---|---|
| Population (P) | e.g., UX researchers in practice | Practitioner adoption patterns |
| Intervention (I) | e.g., Generative AI tools in research workflows | Tool-assisted synthesis and analysis |
| Comparison (C) | e.g., vs. traditional methods | Quality, efficiency, insight depth tradeoffs |
| Outcome (O) | e.g., Research quality, team dynamics | How outputs and practices are changing |
| Cross-cutting theme | e.g., Ethical/governance concerns | Organizational policies and quality control |

**3. Category coverage preview** — A brief note on how well each of the three categories (Methods, Impact, Governance) is represented in the initial results. Flag if any category looks thin.

**4. Search depth** — Ask the user how deep they want to go:

```javascript
sendPrompt("Quick scan — 5 searches (fastest, good for initial orientation)")
sendPrompt("Standard review — 10 searches (recommended, solid coverage)")
sendPrompt("Deep dive — 20 searches (most thorough, takes longer)")
```

**5. Interactive confirmation** — Present clickable options:

```javascript
sendPrompt("These all look good — go ahead")
sendPrompt("I want to adjust the sub-areas before you search")
sendPrompt("Add a sub-area on [topic]")
sendPrompt("Remove one of these and replace it")
```

Wait for the user's response before proceeding to Phase 3.

---

### Phase 3: Execute Targeted Searches

Execute all searches **sequentially** — one at a time, confirming each result before sending the next. Never fire searches in parallel.

#### Search Budget Allocation

**Quick scan (5 searches):**
- 1 search per research category (Methods, Impact, Governance) = 3 searches
- 1 cross-cutting review article search (systematic review or meta-analysis)
- 1 follow-up on the highest-cited paper found so far

**Standard review (10 searches):**
- 1–2 searches per category (Methods × 2, Impact × 2, Governance × 1) = 5 searches
- 2 review article searches: `"systematic review [sub-area]"` or `"meta-analysis [sub-area]"`
- 2 era-gated searches: one with `year_max: 2022`, one with `year_min: 2023` to show field evolution
- 1 follow-up on the highest-cited paper

**Deep dive (20 searches):**
- 2 searches per category = 6 searches
- 5 review article searches (one per major sub-area)
- 4 era-gated searches (2 sub-areas × old/new)
- 3 follow-up searches on the top 3 highest-cited papers
- 2 spare searches for emerging threads that surface unexpectedly

#### Cross-Search Intelligence

As results come in, actively track:

1. **Repeat-hit papers** — a paper appearing in 3+ searches is almost certainly foundational. Flag these.
2. **Recurring authors** — dominant research groups that a practitioner entering the field needs to know.
3. **Citation velocity** — citations ÷ years since publication. A 2023 paper with 150 citations is a stronger signal than a 2010 paper with 150.

#### Running Tally

After all searches, report:
- Total searches attempted / succeeded / failed
- Total unique papers (deduplicated by title)
- Any searches returning fewer than 5 papers (coverage gaps)

---

### Phase 4: Categorize and Analyze

Before writing, assign each paper to its primary research category (Methods, Impact, or Governance). A paper may be relevant to multiple categories — assign it to the category where its primary contribution lies, and note secondary relevance in its metadata.

For each paper, generate **3–5 descriptive tags** capturing core subject matter, methodology, or contribution:
- Concise (1–3 words each)
- Conceptually distinct
- Reusable across papers where applicable
- Indicative of the paper's "aboutness"

*Examples: `interview-analysis`, `synthetic-participants`, `ethical-frameworks`, `workflow-automation`, `bias-detection`, `insight-synthesis`, `researcher-role`, `quality-control`*

Track all tags as you go — they will be compiled into a Tag Glossary.

#### Primary Source URL Resolution

For every paper selected for citation, generate a **Google Scholar search link** in addition to the Consensus URL. Not everyone has Consensus access — this link gives readers a reliable path to find the paper themselves.

**The only permitted approach:**

Construct a Google Scholar search URL from the paper's title, first author last name, and year, using `+` to join words and URL-encoding special characters:

```
https://scholar.google.com/scholar?q=TITLE+WORDS+FIRST_AUTHOR_LASTNAME+YEAR
```

Example: For "Redefining Qualitative Analysis in the AI Era" by Zhang et al. (2023):
```
https://scholar.google.com/scholar?q=Redefining+Qualitative+Analysis+AI+Era+Zhang+2023
```

Label every link as: `[Google Scholar search]`

**Critical rules:**
- **Never construct or guess DOIs.** Consensus does not return DOI metadata. ACM, IEEE, and other publisher DOIs have paper-specific trailing identifiers that cannot be reliably derived from title and author alone. A fabricated DOI is worse than no link — it actively misleads readers.
- **Never construct or guess ArXiv IDs.** ArXiv IDs are assigned at submission time and cannot be inferred from paper metadata.
- **Never use any URL that has not been verified to resolve to the correct paper.** The Google Scholar search approach is honest precisely because it makes no claim about where the paper lives — it lets the reader find it.
- The Google Scholar search link does not guarantee open access. Readers may still need library access for full text. This is honest and expected.

#### Metadata Confidence Annotation

After resolving URLs, apply a **confidence annotation** to every paper. This is a visible flag in the metadata block — not a hidden note — so readers can calibrate how much weight to place on a citation before verifying it.

**⚠ METADATA INCOMPLETE** — apply this flag when ANY of the following conditions are true:
- Publication venue is listed as "Unknown Journal," blank, or cannot be independently verified
- Citation count is 0 AND the paper was published more than 6 months before the search date (suggesting it may not be indexed, may be a preprint, or may have low field impact)
- Only one key reference can be identified (thin citation network)

When the flag is applied, include a one-sentence note explaining which condition triggered it. Example: *"Venue unverified — treat as working paper or preprint. Recommend manual verification before citing in formal outputs."*

Papers with this flag are not excluded from the review — recency and relevance may still justify inclusion — but readers must be clearly informed of the limitation.

---

### Phase 5: Produce the Research Guide (Three Artifacts)

Read the docx skill at `/mnt/skills/public/docx/SKILL.md` before generating any document.
Read the pptx skill at `/mnt/skills/public/pptx/SKILL.md` before generating Artifact 2.

Generate all three artifacts from the same source content. Build the `.docx` and `.md` in parallel from shared content — do not convert one from the other.

---

## Required Metadata for Each Paper

- **Content tags** (3–5 generated during analysis)
- **Full citation** (APA format)
- **Publication year and venue**
- **Author affiliations**
- **Citation count** (as of search date)
- **Consensus URL** (full, never truncated — required for all cited papers)
- **Google Scholar search link** (constructed from title + first author last name + year — always provided, always labeled `[Google Scholar search]` — never a fabricated DOI or ArXiv ID)
- **Metadata confidence** (✓ Verified OR ⚠ METADATA INCOMPLETE with reason — see Phase 4 annotation rules)
- **Key cited references** (2–3 most relevant works this paper builds on)
- **Papers that cite this work** (if notably influential)
- **Category assignment** (primary + any secondary)

---

## Analysis Requirements

**For each individual paper:**
- **Summary** (100–150 words): Research question, methodology, key findings, and any acknowledged limitations noted by the authors
- **Relevance statement** (1 sentence): Explicit connection to generative AI applications in UX research practice

**For each category:**
- **Category synthesis** (300–400 words): Patterns, contradictions, gaps, and emerging themes across all papers in that category

**For the complete review:**
- **Master synthesis** (400–500 words): Cross-category insights, overarching trends, implications for the field, and notable gaps in current research

---

## Output Format

Generate **three complementary artifacts** plus a bibliography and audit log.

---

### Artifact 1: Academic Literature Review (.docx) — "View from the Academy"

Save to `/mnt/user-data/outputs/` as `genai-uxr-literature-review-[YYYY-MM-DD].docx`.

**Document Structure:**

**Cover / Header Block**
- Title: "Generative AI in UX Research Practice: A Systematic Literature Review"
- Search date, scope (2023–2025, recency-weighted), Consensus tier detected, total papers reviewed

**Section 1: Executive Synthesis** *(place first)*
Master synthesis (400–500 words) framed as "Current Academic Perspectives on Generative AI in UX Research Practice." Cross-category insights, overarching trends, implications for the field.

**Section 2: Start Here — Priority Reading Order**
Curate 5–7 papers from across all categories in the sequence a practitioner should read them:
1. Best recent review article or meta-analysis (broadest orientation)
2. Foundational/seminal paper(s)
3. 2–3 papers representing the current frontier
4. A paper highlighting a key gap or controversy

For each paper: title as clickable hyperlink → authors + year → one sentence on contribution → one sentence on what to pay attention to while reading.

**Section 3: How the Field Got Here**
1–2 paragraph narrative + milestone timeline table showing how thinking has evolved. Include terminology evolution notes where vocabulary has shifted.

**Section 4: Category Sections** *(Methods, Impact, Governance)*

For each category:
- **Category title and definition**
- **Category synthesis** (300–400 words): patterns, contradictions, gaps, emerging themes
- **Annotated bibliography** for 5–8 papers, each entry structured as:
  1. Paper title (as heading)
  2. APA citation
  3. Paper summary (100–150 words)
  4. Single-sentence relevance statement: "How does this inform our understanding of generative AI in UX research?"
  5. Metadata block: tags · publication details · citation count · author affiliations · key references · citing papers · Consensus URL (clickable) · Google Scholar search link (clickable, labeled `[Google Scholar search]`) · Metadata confidence (✓ Verified OR ⚠ METADATA INCOMPLETE with reason)

**Section 5: Key Research Groups**
3–5 most frequently appearing authors/groups. Name, affiliation, sub-areas covered, representative paper with Consensus link.

**Section 6: Open Questions & Gaps**
Structured into three types:
- **Methodological gaps** — Where are study designs weak?
- **Population/context gaps** — Who or what hasn't been studied?
- **Conceptual/theoretical gaps** — What questions hasn't anyone asked yet?

For each gap: explain *why it matters*, not just that it exists.

**Section 7: Tag Glossary** *(place last)*
Alphabetized list of all unique tags used across the review, each with a one-sentence definition.

**Section 8: Bibliography**
Every cited paper: `Author(s) (Year). Title. Journal.` + clickable "View on Consensus" hyperlink + clickable `[Google Scholar search]` link. If a paper carries a ⚠ METADATA INCOMPLETE flag, append `[⚠ Metadata incomplete — verify before citing]` after the links.
Rules: alphabetical by first author last name; every inline citation matched; full URLs never truncated.

**Section 9: Audit Log**

Search summary table:

| # | Search Query | Filters Used | Papers Returned | Status |
|---|---|---|---|---|

Counts: searches executed / successful / failed / unique papers received / papers cited.

Coverage notes: detected Consensus tier, theoretical ceiling, thin sub-areas flagged.

**Recency distribution:** Report where selected papers actually landed — e.g., "X of Y cited papers were published in 2024–2025, Z in 2023." If the balance skews older than the recency weighting intends, note it explicitly and flag any sub-areas where recent publications were thin.

Any content drawn from model knowledge labeled `[Not from Consensus — model knowledge]`.

---

### Artifact 1b: Markdown Mirror (.md) — "Grounding Document"

Save to `/mnt/user-data/outputs/` as `genai-uxr-literature-review-[YYYY-MM-DD].md`.

This is a **full content mirror** of Artifact 1 — identical structure, identical content, no sections omitted or summarized. The purpose is portability and downstream use: the `.md` renders cleanly in GitHub, Notion, Obsidian, and any markdown-aware tool, and serves as the primary grounding document for downstream skills (e.g., content atomization) that need clean structured text without Word formatting overhead.

**Rendering rules for the `.md`:**

- All headings use `#` / `##` / `###` hierarchy matching the `.docx` section structure
- All hyperlinks use standard markdown `[link text](url)` format — both Consensus and primary source URLs, on separate labeled lines
- Tables use standard markdown pipe table syntax (search summary, timeline, tag glossary)
- The ⚠ METADATA INCOMPLETE flag renders as a blockquote: `> ⚠ METADATA INCOMPLETE: [reason]`
- Bold and italic use `**` and `*` as in standard markdown
- Paper metadata blocks render as a definition-list style using bold labels followed by their values, one per line
- The document begins with a YAML front matter block:

```yaml
---
title: "Generative AI in UX Research Practice: A Systematic Literature Review"
search_date: [YYYY-MM-DD]
scope: "2023–2025, recency-weighted toward 2024–2025"
categories: [Methods, Impact, Governance]
papers_cited: [N]
produced_by: "thinkfirstlit v3.1"
---
```

**Generation approach:** Write the `.md` as a plain JavaScript string built in parallel with the `.docx` content — same data, different renderer. Do not extract or convert from the `.docx` file after the fact. Both artifacts are generated from the same source content in Phase 5.

---

### Artifact 2: Presentation Content (.pptx) — "Slide Deck Outline"

Save to `/mnt/user-data/outputs/` as `genai-uxr-slide-deck-[YYYY-MM-DD].pptx`.

**Slide Structure:**

1. **Title Slide** — Literature review scope and date; search parameters (years covered, categories, total papers)
2. **"View from the Academy"** — Master synthesis in 4–6 bullets; one callout for the most notable cross-category finding
3. **Category Slide — Methods** — Definition (1 sentence); key themes (3–5 bullets); notable findings (2–3 highlights with references)
4. **Category Slide — Impact** — Same structure
5. **Category Slide — Governance** — Same structure
6. **Key Papers — Methods** *(citations only)*
7. **Key Papers — Impact** *(citations only)*
8. **Key Papers — Governance** *(citations only)*
9. **Open Questions & Gaps** — 3–5 bullets; framed for practitioners: "What this means for research teams"
10. **Field Timeline** — Visual timeline of key milestones from Section 3 of Artifact 1

---

## Quality Standards

- Prioritize empirical studies over conceptual papers
- Ensure geographical and methodological diversity where possible
- Flag any significant limitations in the current literature
- Note if any category has fewer than 5 relevant papers — this is itself a finding worth reporting
- Maintain academic rigor while ensuring accessibility for practitioner audiences
- Papers cited in Artifact 2 must be a strict subset of papers cited in Artifacts 1 and 1b
- Artifact 1b (.md) must be a faithful full mirror of Artifact 1 (.docx) — no content omitted, no sections summarized

---

**Search Date:** [Populated automatically at runtime]
**Review Scope:** Generative AI applications in UX research methods (2023–2025, recency-weighted toward 2024–2025)

