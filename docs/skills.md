# The skills in detail

What each skill checks, what it gives back, and how the three fit together. The
[README](../README.md) has the short version.

Run them in this order: get the facts right, then the structure, and only then
polish sentences that may still move or go.

```text
fact-check → coherence-check → voice-check
```

## What the three have in common

- **They report; you decide.** A skill never edits the text on its own. After the
  report it offers to apply the fixes, and applies only the ones you pick.
- **The text can come from anywhere:** a file, a pull request (URL or number),
  `branch:path`, pasted text, or nothing at all, which means the draft from the
  current conversation. A draft in a pull request is read straight from its
  branch (`git show`, or `gh` when the repo isn't cloned). Nothing is checked out.
- **Front matter is part of the text.** The title and the excerpt are what a
  reader sees first, so they are checked too.
- **The report follows your conversation's language.** Quotes from the text and
  suggested rewrites stay in the language of the text.
- **Nothing is made up for you:** no invented sources, no invented experiences
  or opinions, no missing sections written on your behalf.
- **A slash command is optional.** "Fact-check this article" or "does this sound
  like AI?" starts the right skill as well.

## Fact-checking a text

```text
/writer-skills:fact-check <file | PR URL or number | branch:path | pasted text | nothing = the draft in this conversation>
```

`fact-check` extracts every checkable claim — dates, ages, people, companies,
product names (including outdated or renamed ones), versions, numbers, terminal
commands and flags, code/API names, quotes, links, and technical statements
about how things work — from the text, its front-matter title and excerpt, **and
its illustrations** (alt texts, SVG labels, screenshots). It runs a sanity pass
(impossible values, internal contradictions, arithmetic), verifies the rest
against primary sources — source code for claims about internals — and returns
tables of mismatches, split into "fix before publishing" and "worth fixing":

```text
| # | Where | Fragment | Verdict | What's wrong | Correct version | Source |
```

Verdicts: ❌ false · 🕰 outdated · ⚠️ inaccurate · ❓ unverifiable. The report
states which versions it checked against. Verified claims are only counted, and
claims about your own experience are listed separately, since only you can check
them.

`fact-check` also:

- never runs commands from the text — it checks them against documentation;
- uses `WebSearch`/`WebFetch` and read-only `git`/`gh` commands without a
  permission prompt while it runs (downloading raw source files with `curl`
  still asks).

## Checking that a text holds together

```text
/writer-skills:coherence-check <file | PR URL or number | branch:path | pasted text> [draft: <earlier version>]
```

`coherence-check` reads a text as one piece. It first builds a map — what the
title, excerpt, and intro promise, the one-sentence through-line, and the job of
each section — and then looks for what tears it: broken promises, lost threads,
detours, jumps without a bridge, wrong order, repeats, drift, an ending that
misses the opening. It pays special attention to **seams** left when a person
edits an AI draft: a term that changes halfway, a back-reference to a removed
passage, "two decisions" followed by four points.

`scripts/skeleton.py` prints the headings and the first sentence of every
paragraph with line numbers — they should tell the story on their own — plus
every back-reference and announced count to verify. Given an earlier draft
(`draft:`, the file's git history, or the PR's commits), it checks the edges of
every edited region. Fixes are structural — move, cut, merge, or a one-sentence
bridge — and anything that needs content only you can write is marked ✍️.

## Checking the voice of a text

```text
/writer-skills:voice-check <file | PR URL or number | branch:path | pasted text> [style: <your own texts>]
```

`voice-check` reads an article, front-matter title and excerpt included, for the
things that make it sound AI-written —
dash-heavy punctuation, "not X, but Y" antitheses, rhetorical questions answered
at once, aphoristic closers, stock phrases, officialese, translationese, and a
text with no person in it — in English, Ukrainian, and Russian. A small script
(`scripts/text_stats.py`) counts what is hard to see by eye: sentence rhythm,
punctuation density, repeated openers, and filler words, each with line numbers.

Pass `style:` with texts you wrote yourself (ideally without AI help — notes,
messages, drafts) and it builds a short profile of your voice and aims its
suggestions at it, without copying your phrases. Without samples it takes the
target from the text itself — its genre and its audience — and keeps the wording
plain and direct.

The report has two tables — "gives away AI" and "polish" — each with a concrete
rewrite in the text's language, plus what already sounds alive and should be
kept, and places where a detail only you know would make the text yours. Like
`fact-check`, it never edits the text without asking, keeps every fact intact,
and never invents experiences or opinions for you.

The full list of tells lives in
[skills/voice-check/references/tells.md](../plugins/writer-skills/skills/voice-check/references/tells.md).

## Languages

The skills are written for texts in **English, Ukrainian, and Russian**:

- `voice-check` has a separate list of tells for each of the three, because a
  tell in one language can be plain grammar in another (a dash in "X — це Y" is
  normal Ukrainian).
- Both helper scripts detect the language of the text on their own, or take
  `--lang en|uk|ru`.

## The helper scripts on their own

Both scripts are plain `python3` with no dependencies, and they are handy
outside the skills too:

```bash
python3 plugins/writer-skills/skills/coherence-check/scripts/skeleton.py article.md
python3 plugins/writer-skills/skills/voice-check/scripts/text_stats.py article.md --lang uk
```

They print signals with line numbers. The judgment stays with the reader.
