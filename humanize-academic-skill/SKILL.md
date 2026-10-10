---
name: humanize-academic
description: |
  Remove AI writing tells from academic and research prose (papers, theses,
  proposals, reports) while preserving formal register, citations, technical
  terminology, hedged claims, and factual content. Use whenever the user asks
  to "humanize", "de-AI", "remove AI slop from", or "make my paper/thesis/
  section sound less like ChatGPT" for academic or scientific writing. Unlike
  general-purpose humanizers, this skill treats passive voice, hedging,
  formal vocabulary, and stock transitions as normal academic register and
  does NOT flag them — it only removes genuine AI tells (inflated-importance
  claims, shallow -ing padding, vague-source attribution, formulaic
  challenges/outlook sections, empty rule-of-three lists, chatbot
  signposting). Do not use for casual, marketing, or personal prose — use
  a general humanizer for that instead.
license: MIT
metadata:
  version: "0.1.0"
  derived_from: "humanizer (Wikipedia Signs of AI Writing), humanize-en (coroboros/agent-skills)"
---

# Humanize Academic

Strip AI writing tells from academic/research prose without touching the
things that make academic writing *correctly* sound the way it does:
hedged claims, passive constructions, formal vocabulary, discipline-specific
jargon, citations, and exact figures.

General-purpose humanizers (see: `/humanizer`, `/humanize-en`) are tuned on
blog posts and marketing copy. Applied to a paper, they over-fire: they'll
flag "furthermore," passive voice, or "this suggests" as AI tells, when in
an academic register those are just... correct writing. This skill narrows
the pattern list to the subset that is *actually* a tell in academic prose,
and hard-protects everything a rewrite could otherwise damage.

## Scope — what this skill touches and what it never touches

**Rewrite when found:**
1. **Inflated-importance claims** — "marks a pivotal moment," "underscores the growing significance of," "represents a paradigm shift" attached to an ordinary result or dataset.
2. **Shallow -ing padding** — a clause tacked on that restates the sentence instead of adding information: "…, highlighting the need for further research," "…, demonstrating the robustness of the approach" with no new claim behind it.
3. **Vague-source attribution** — "studies have shown," "research suggests," "it is widely believed" with no citation, where the source text or user-provided draft has (or could plausibly cite) a specific source.
4. **Formulaic challenges/future-work padding** — a stock closing paragraph that restates limitations in generic terms ("despite these challenges, this field continues to evolve") without a concrete limitation or next step.
5. **Empty rule-of-three / listing padding** — three near-synonymous adjectives or clauses where one specific one would do ("a robust, scalable, and efficient framework" with nothing to differentiate the three).
6. **Chatbot signposting** — "In this paper, we will explore...", "Let's dive into...", "It is important to note that," "Without further ado."
7. **Filler phrases** — "in order to" → "to," "due to the fact that" → "because," "at this point in time" → "now." Purely mechanical, safe everywhere.
8. **Repetitive synonym cycling** — restating the same point three times with swapped nouns/adjectives rather than adding a new one.
9. **Overused AI vocabulary in non-technical use** — "delve," "tapestry," "intricate," "pivotal," "testament to," "underscore," "garner," "showcase," "boasts," "interplay" — replace with the plain verb, *unless* the word is a technical term in the field (e.g., "interplay" in a physics paper describing coupled forces may be literal, not filler — judge from context).

**Never rewrite, never flag:**
- **Citations, in-text references, bibliography entries, DOIs.**
- **Numbers, statistics, p-values, confidence intervals, dataset sizes, dates, equations, units.**
- **Quoted material** attributed to a source.
- **Passive voice.** Standard and often required in methods/results sections. Do not convert to active voice.
- **Hedging language** — "may suggest," "appears to," "these results indicate," "it is possible that." This is epistemic honesty, not AI-speak. Removing it changes the paper's truth claims — never do this.
- **Formal/Latinate vocabulary** — "utilize," "demonstrate," "facilitate," "subsequent," "prior to." These are normal academic register, not §9 hits, unless used in the padding constructions above.
- **Standard transitions** — "furthermore," "moreover," "consequently," "however," "in addition." Flag only if three or more stack in adjacent sentences with no other function.
- **Discipline jargon and technical terms**, even if they overlap with a watched word (e.g., "framework," "robust" as a statistical term, "significant" in the statistical sense).
- **Figures, tables, table/figure captions, equations, code/pseudocode blocks, footnotes/endnotes markers.**
- **Author-supplied section structure** (headings, subsection order, IMRaD structure). Rewrite prose within sections; never restructure or reorder sections.
- **Methodology/results claims** — do not add, remove, soften, or strengthen a factual or statistical claim. If a sentence must be shortened, shorten the padding, not the claim.
- **Section roadmap / "organized as follows" paragraphs** — "Subsection 3.1 presents X. Subsection 3.2 specifies Y." This is standard academic convention (often required by journal/thesis style guides), not chatbot signposting (§6). Only flag a roadmap paragraph if it also contains an inflated-importance or vague-attribution claim layered on top of the plain preview.
- **Formulaic notation/definition templates** — repeated constructions like "Let X = {...} denote the set of..., indexed by..." across a notation or glossary subsection are required mathematical convention, not repetitive synonym cycling (§8). Deliberate repetition of a definition template across parallel entries is a feature; do not vary the wording for its own sake.

If you cannot tell whether a phrase is genuine field terminology or AI padding, leave it — a missed pattern is a much smaller error than deleting a hedge or a claim.

## Process

1. **Read the whole passage first.** Academic AI tells compound across sentences (padding + vague attribution + formulaic outlook often appear together in intro/conclusion sections, less so in methods/results).
2. **Mark candidate hits** against the 9 patterns above. For each, check it against the "never rewrite" list before touching it.
3. **Rewrite conservatively.** Prefer deleting empty padding over rewriting a sentence into something new. Prefer shortening over restructuring. Keep every citation, number, and hedge exactly as given.
4. **Self-check before returning:**
   - Did I remove or soften any hedge, claim, citation, or number? → revert.
   - Did I touch anything inside a quote, equation, table, or figure caption? → revert.
   - Does the passage still sound like a paper in this field, not a blog post? → if I added personality, opinion, or contractions, revert; academic register has no first-person color unless the source already used it (e.g., some qualitative/social-science papers use first person deliberately — match what's there, don't add it).
5. **Return format:** the rewritten passage, followed by a short list of which of the 9 patterns were found and fixed (pattern number + one-line example), so the user can verify nothing else was touched.

## When to use a different skill instead

- Casual, marketing, or personal prose → `/humanizer` or `/humanize-en`.
- Structural rewriting (wrong section order, missing sections, IMRaD compliance) → `/academic-paper` or `/author-academic-style`.
- Style/tone calibration to a target journal's house style → `/author-academic-style`.
- File contains code, data files, or non-English text → out of scope; say so rather than guessing.

## Source

Pattern selection adapted from Wikipedia's ["Signs of AI Writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) (WikiProject AI Cleanup) and the `humanize-en` pattern catalogue, filtered down to the subset that remains a genuine tell once you require academic register, hedging, and citation integrity to be preserved by default.
