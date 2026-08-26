---
name: chat-to-raw
description: Distill the current conversation into a single clean, self-contained .md "raw file" for ingestion into the user's second brain / knowledge base. Trigger whenever the user asks to "make this into a raw file", "turn this chat into a .md", "capture this for my second brain", "save this as a note", "make this ingestible", or any request to convert the discussion so far into a durable markdown note with sources. Use this even if the user's phrasing is casual (e.g. "raw this", "note this up") — the intent is to preserve the current chat as a revisitable knowledge artifact.
---

# Chat → Raw File

Convert the current conversation into ONE markdown file the user can drop into their second brain. The file must stand on its own: someone (or a downstream ingestion pipeline) revisiting it months later should understand the topic without needing the original chat.

## What to produce

A single `.md` file, written to `/mnt/user-data/outputs/`, then presented with `present_files`. Filename: `YYYY-MM-DD-kebab-topic.md` (use today's date).

Do NOT dump the raw transcript. Distill it. The user asked *you* the things they didn't already know, so the value is the answered knowledge, cleanly restated — not the back-and-forth.

## Process

1. **Identify the core topic(s)** of the conversation. If the chat wandered across several unrelated topics, ask the user whether they want one file or several. If topics are related, one file with sections is fine.
2. **Extract the durable knowledge** — the explanations, conclusions, decisions, and facts that are worth keeping. Drop pleasantries, dead ends, and corrections that were superseded (keep only the corrected final answer).
3. **Gather sources.** This is the part the user explicitly cares about. For every non-trivial claim, concept, tool, paper, or person mentioned, find a real reference:
   - If web search / web_fetch is available, search for authoritative primary sources (papers on arXiv/DOI, official docs, original blog posts, standards). Verify links resolve before including them.
   - Prefer primary sources over aggregators. Include enough that a future revisit doesn't start from zero.
   - If a claim came only from the chat and you can't source it, mark it `[unverified]` rather than inventing a citation. Never fabricate a URL, DOI, or author.
4. **Add revisit scaffolding** — the things the user will want when they come back: open questions, next steps, related topics to explore, and key terms to look up.
5. **Write the file** using the template below, save to outputs, present it.

## Template

```markdown
---
title: <concise descriptive title>
date: <YYYY-MM-DD>
source: chat
tags: [<3-6 lowercase kebab tags>]
status: raw
---

# <Title>

## Summary
<2-4 sentence plain-language summary of what this note is about and why it mattered.>

## Key Points
<The distilled knowledge as prose and/or tight bullets. This is the substance.
Restate what was learned in the user's-future-self voice — clear, self-contained,
no "as I said above". Include concrete details: numbers, definitions, mechanisms.>

## Open Questions / To Explore
<What's still unresolved, what to dig into next, adjacent threads worth pulling.>

## Sources
<Numbered list. Each: title — author/org (year) — URL/DOI. Mark [unverified] where
you could not confirm. Group as Papers / Docs / Articles / Tools if there are many.>

## Related / Keywords
<Terms, people, projects, and concepts to cross-link or search later.>
```

## Rules

- **One file, self-contained.** No dependency on the chat existing.
- **Sources are mandatory when the topic has any.** The user specifically wants papers/sources for revisiting. Do the searching yourself in this turn — don't offer to find them later.
- **Never fabricate a citation.** Real links only; mark `[unverified]` otherwise.
- **Match the user's technical depth.** Keep their terminology; don't dumb it down.
- **Respect copyright** — summarize sources in your own words, don't paste long excerpts.
- **Ask before splitting** into multiple files, but otherwise just produce the file without a lot of preamble.
- If the user has a preferred frontmatter schema or tag vocabulary (from earlier in the chat or a prior note they showed you), match it instead of the default template.
