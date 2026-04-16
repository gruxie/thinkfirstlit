---
name: thinkfirstamplify
description: "Use when the user wants to generate social media posts from a thinkfirstlit literature review output, atomize research findings into campaign content for LinkedIn and Teams, or create Think First program posts. Triggers on phrases like 'generate posts from the lit review', 'atomize the findings', 'build the social campaign', 'create the campaign posts', or 'run thinkfirstamplify'. Requires a thinkfirstlit .md file as input and a GitHub URL for the current lit review run. Do NOT trigger for general social media requests unrelated to the Think First program or thinkfirstlit output."
# attribution:
#   built_by: Jake Rhodes
#   version: 1.0
#   created: 2026-04-16
#   part_of: Think First program — View from the Academy
#   companion_skill: thinkfirstlit v3.1
#   description: >
#     thinkfirstamplify takes the .md output from thinkfirstlit and atomizes
#     findings into a ready-to-deploy social media campaign for the Think First
#     program. Produces LinkedIn posts, Teams posts, and ChatGPT illustration
#     prompts for each finding, sequenced thematically and output as both
#     .md and .docx artifacts.
---

# thinkfirstamplify — Social Media Campaign Generator for Think First

*v1.0 — Companion skill to thinkfirstlit v3.1*

---

## Purpose

Take the `.md` output from a thinkfirstlit run and produce a complete, ready-to-deploy social media campaign for the Think First program. Each campaign post atomizes one finding from one paper into content suitable for UX researchers and UX designers — fun to read, professionally substantive, and anchored to the source literature.

This skill is scoped to the Think First program. Do not apply it to general social media or other literature reviews.

---

## Required Inputs

Before running, confirm the user has provided:

1. **The thinkfirstlit `.md` file** — either uploaded or accessible at a path. The skill reads this file for all paper content, Google Scholar links, and YAML front matter metadata.
2. **The GitHub URL for this run** — the permanent link to the `.md` file in the Think First GitHub repo, e.g., `https://github.com/gruxie/thinkfirstlit/blob/main/thinkfirstlit_20260416.md`. This URL is run-specific and must be provided each time.

If either is missing, ask for it before proceeding.

---

## Fixed Assets (hardcoded — do not change per run)

```
Think First LinkedIn newsletter:  https://www.linkedin.com/newsletters/think-first-7419494628061728769/
Think First Microsoft Design page: https://microsoft.design/news-and-stories/tag/think-first/
```

The GitHub lit review URL is **run-specific** — use whatever the user provides, do not hardcode.

---

## About Think First

Think First is a program evangelizing AI use and knowledge in the UX design discipline. It has two sub-programs:

1. **Industry Interviews** — practitioner conversations published on LinkedIn
2. **View from the Academy** — synthesis of scholarly research on AI use in UX, produced by thinkfirstlit

thinkfirstamplify serves the View from the Academy sub-program.

---

## Phase 1: Read and Parse the .md File

Read the thinkfirstlit `.md` file in full. Extract:

- **YAML front matter** — `search_date`, `scope`, `categories`, `papers_cited`, `produced_by` — for use in campaign metadata
- **All paper entries** — title, authors, year, venue, summary, relevance statement, tags, citation count, Google Scholar search link, confidence flag (✓ or ⚠)
- **Category assignments** — Methods, Impact, or Governance for each paper
- **Executive synthesis** — the master synthesis section, for use in a campaign intro post

Do not fabricate or supplement any paper content. Everything in the campaign must come from what is in the `.md` file.

---

## Phase 2: Select Papers for Atomization

For each cited paper, decide: **2 posts, 1 post, or skip.**

**2 posts** — paper has multiple distinct findings that stand alone as separate audience hooks. Apply when the paper covers meaningfully different angles (e.g., a practitioner adoption study might yield one post on the governance gap finding and a separate post on the prompt-quality finding — two different hooks for the same audience).

**1 post** — single strong central finding, clear standalone value for a UX researcher/designer reader.

**Skip** — paper is useful scaffolding in the review but does not produce a compelling standalone hook for a UX practitioner audience. Typical skip candidates: broad governance framework papers from adjacent domains (accounting, healthcare) that were included for conceptual grounding but whose findings don't land with a UX audience.

**⚠ flagged papers** — eligible for posts. The finding may be strong even if the metadata is incomplete. Include a single-line caveat at the bottom of the post: *"Note: this paper's publication venue is unverified — treat as a working paper pending confirmation."*

**Target:** 15–18 posts total. If selection logic yields fewer than 15, revisit skip decisions before under-delivering. If it yields more than 18, tighten the 2-post papers.

---

## Phase 3: Draft Each Post

Draft posts in thematic order: **Methods → Impact → Governance.**

Within each category, order papers from highest to lowest citation count (citation velocity — citations ÷ years since publication — as tiebreaker for recent papers with 0 citations).

Add one additional post at the top of the sequence: a **campaign intro post** drawn from the Executive Synthesis section of the `.md`, introducing the full lit review and the Think First program. This is Post 0 / Week 0.

### Voice and Tone Rules

**Title:** Fun, punny, ironic, insider UX humor welcome. Should make a UX researcher smile or do a double-take. Examples of the right register: "Your AI Thematic Coder Has Trust Issues (And So Do Your PMs)", "The Governance Gap: Where Good Intentions Go to Die", "Synthetic Users: Great for Pilots, Terrible at Small Talk."

**Fun Question:** Provocative, directly relevant to UX researchers/designers, click-bait energy. Should feel like something a smart colleague would ask to start a debate. Not a rhetorical filler — make it actually interesting.

**Summary:** Water cooler register. Two colleagues who know their stuff talking casually about a paper they both just read. Clear, specific, direct. No academic hedging ("this study suggests that perhaps..."). No jargon theater. Option A is the target register:

> *"Researchers found that when AI mediates between co-coders, teams reach agreement faster — but the final codebook ends up narrower. Efficiency up, interpretive diversity down. Worth knowing if you're using AI to speed up your synthesis work."*

Not Option B:

> *"This study demonstrates that AI-mediated collaborative qualitative analysis yields statistically significant improvements in early-stage coding efficiency while potentially constraining the breadth of emergent theme development."*

**LinkedIn summary length:** 3–4 paragraphs. Each paragraph 2–4 sentences. Enough to be substantive, short enough to read in under 5 minutes.

**Teams summary length:** 2–3 sentences total. Punchy. Gets to the point immediately.

**Hashtags (LinkedIn only):** 4–6. Always include `#UXResearch #AI #ThinkFirst`. Add 1–3 paper-specific tags derived from the paper's tags in the `.md` (e.g., if the paper is tagged `de-skilling`, add `#UXDesign` and `#AITools`).

**Emoji:** One per Teams post maximum, relevant to the content, at the end of the conversational closer. None in LinkedIn posts (professional channel).

**Conversational closer (Teams only):** One sentence inviting a reply. Should feel natural, not forced. Examples: "Anyone on your team navigating this? 👇", "Seeing this play out in your sprints? 👇", "Hot take or hard truth? 👇"

---

## Phase 4: Build the ChatGPT Illustration Prompt

For each post, generate a ChatGPT image generation prompt. The prompt should produce a **flat illustration** — clean, minimal, vector-style, professional. Not photorealistic. Not cartoon. Think modern editorial infographic aesthetic.

The illustration should represent the **core concept or tension** in the finding, not a literal depiction of researchers or computers. Think metaphorical but accessible.

Prompt format:
```
Flat illustration, minimal vector style, clean white background, professional editorial aesthetic.
[One sentence describing the concept to visualize.]
[One sentence on mood/tone: e.g., "Tone: slightly wry, not alarming."]
Color palette: [2–3 colors that match the content mood — cool blues for Methods, warm ambers for governance gaps, etc.]
No text in the image.
```

Example for the de-skilling post:
```
Flat illustration, minimal vector style, clean white background, professional editorial aesthetic.
A researcher's hands becoming translucent as a robotic arm types at the same keyboard beside them.
Tone: quietly unsettling, not dramatic.
Color palette: slate blue, warm gray, pale amber.
No text in the image.
```

---

## Post Entry Structure

Every post in the output document follows this exact structure:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
POST [N] OF [TOTAL]  |  Week [N]  |  Category: [Methods / Impact / Governance]
Paper: [Author(s), Year] — [Title]
[⚠ Metadata note if applicable]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

── LINKEDIN ──────────────────────────────────────

**[Title]**

*[Fun Question]*

[Summary — 3–4 paragraphs, water cooler register]

📖 Read the source article here: [Google Scholar search link from .md]

---
*Think First is a program exploring AI's impact on UX design and research. Follow for practitioner interviews and research insights: [Think First on LinkedIn](https://www.linkedin.com/newsletters/think-first-7419494628061728769/)*

*This post is part of "View from the Academy" — our synthesis of what scholarly research says about AI in UX practice. Read the full literature review: [Think First Lit Reviews on GitHub]([USER-PROVIDED GITHUB URL])*

#UXResearch #AI #ThinkFirst [+ paper-specific hashtags]

── TEAMS ─────────────────────────────────────────

**[Title]**

*[Fun Question]*

[Summary — 2–3 sentences, same water cooler register]

Full lit review → [Think First Lit Reviews on GitHub]([USER-PROVIDED GITHUB URL])

[Conversational closer with one emoji]

── CHATGPT ILLUSTRATION PROMPT ───────────────────

[Full prompt text, ready to paste]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Phase 5: Produce the Campaign Artifacts

Read the docx skill at `/mnt/skills/public/docx/SKILL.md` before generating the `.docx`.

Generate both artifacts from the same content in parallel.

**Artifact 1:** `thinkfirstlit-campaign-[YYYY-MM-DD].md`
- Full YAML front matter block at the top
- Campaign metadata: total posts, date, source review, categories covered
- All posts in sequence, using the structure above
- Save to `/mnt/user-data/outputs/`

**Artifact 2:** `thinkfirstlit-campaign-[YYYY-MM-DD].docx`
- Cover page: campaign title, date, total posts, source review link
- Each post as a clearly separated section with visible headers for LinkedIn / Teams / Illustration Prompt
- Visual separation between posts (page break or thick rule)
- LinkedIn and Teams sections visually distinct (different background shading on metadata row)
- Save to `/mnt/user-data/outputs/`

---

## YAML Front Matter for the .md Output

```yaml
---
title: "Think First: View from the Academy — Social Media Campaign"
generated_date: "[YYYY-MM-DD]"
source_review: "[USER-PROVIDED GITHUB URL]"
review_search_date: "[from .md front matter]"
total_posts: [N]
categories_covered: [Methods, Impact, Governance]
produced_by: "thinkfirstamplify v1.0"
fixed_links:
  linkedin_newsletter: "https://www.linkedin.com/newsletters/think-first-7419494628061728769/"
  microsoft_design: "https://microsoft.design/news-and-stories/tag/think-first/"
---
```

---

## Quality Checks Before Output

- Every post links to a real Google Scholar search URL from the `.md` — never fabricate links
- Every post's summary is grounded in the paper content from the `.md` — no invented findings
- Titles are genuinely fun, not just descriptive with an exclamation point
- LinkedIn and Teams versions are meaningfully different — not just truncated versions of each other
- ChatGPT prompts describe a concept, not a literal scene
- ⚠ flagged papers include the one-line caveat
- Post count is between 15 and 18
- Sequence runs Methods → Impact → Governance, intro post first

---

**Search Date:** [Populated from source .md front matter at runtime]
**Produced by:** thinkfirstamplify v1.0
