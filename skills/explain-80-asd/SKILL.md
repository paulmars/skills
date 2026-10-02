---
name: explain-80-asd
description: Use when the user asks to explain the last answer or a selected topic in roughly 80% ASD-STE100 English, with limited vocabulary and very few new terms.
---

# Explain in 80% ASD-STE100 English

Explain the most recent substantive answer, result, or topic in the conversation. If the user selects other text, explain that text instead. Preserve its meaning, important conditions, and uncertainty. Do not repeat the work that produced it.

## What 80% means

Use ASD-STE100 as a guide, with room for natural English. This is a style target, not a measured compliance score or permission to make 20% of the words difficult.

- Use short sentences, usually under 20 words, with one main idea each.
- Prefer active voice, concrete words, and simple verbs.
- Use the same word for the same meaning. Do not add synonyms for variety.
- Explain the main point first, then how it works and why it matters.
- Avoid idioms, decorative language, and long groups of nouns.
- Allow a longer sentence or a normal English form when it makes the meaning clearer.

Accuracy comes first. Do not remove a necessary distinction just to make the text simpler.

## Base vocabulary

Use the STE dictionary as the preferred base when it is available. Follow its word meanings, not just its spellings. If it is unavailable, use a conservative set of common, concrete English words as an approximate base. Do not claim that you checked dictionary membership or achieved formal STE compliance.

Keep this base stable while drafting. Do not expand it to excuse difficult words. Treat specialist terms, uncommon words, acronyms, and unfamiliar meanings of common words as additions. A term in the source still counts: seeing it does not mean the reader understands it.

## Exponential cost for new vocabulary

Count distinct additions across the entire explanation, including headings, examples, and definitions. Let N be that count. The total cost is `2^N - 1`; the next addition costs `2^N`. This is an editing penalty, not money or a token charge.

| New terms | Total cost | Decision |
| --- | ---: | --- |
| 0 | 0 | Preferred when clear |
| 1 | 1 | Fine when useful |
| 2 | 3 | Fine when useful |
| 3 | 7 | Keep only for a clear gain in meaning |
| 5 | 31 | Strong reason to rewrite |
| 10 | 1,023 | Rewrite around fewer concepts |
| 20 | 1,048,575 | Failed draft; rebuild the explanation |

Aim for zero to two additions. Each further addition needs a much stronger reason. Do not give yourself a fresh allowance for each paragraph or section. Start a new count for a new explanation; do not silently add words to a permanent dictionary.

- Count a repeated word only once. Normal plurals and verb forms share one entry when their meaning stays the same.
- Count a fixed multiword term as one concept. Do not bundle unrelated terms to reduce the count.
- Use one label per concept. An extra synonym or acronym is another addition if you introduce it.
- Define an added term at first use with base words, then reuse that term consistently.
- Keep exact names, code, paths, and quoted labels when required. These literal references are exempt; surrounding explanations and optional jargon are not. Do not use quotes or code formatting to hide new vocabulary.

Keep a term only if a plain alternative would lose important meaning or become harder to understand. Never omit required facts to meet the target. If many terms are truly essential, explain the core idea first and define the required terms where needed.

## Final pass

Privately list the additions and review their total cost. Remove avoidable terms, synonyms, and definitions that introduce more difficult words. Return the explanation itself; show the vocabulary count or cost only if asked.

Example: “The cache mitigates redundant computation” becomes “The app keeps past results so it does not do the same work again.” If the term matters: “A cache is a place where the app keeps past results. The app checks the cache before it does the work again.”

Background: [Official ASD-STE100 overview](https://www.asd-ste100.org/about.html). The 80% target and exponential cost are custom rules for this skill.
