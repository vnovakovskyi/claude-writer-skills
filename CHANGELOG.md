# Changelog

All notable changes to the plugin in this repository are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and
the versions follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html):

- **Major:** a change that breaks a habit — a skill renamed or removed, a
  different report format.
- **Minor:** a new skill, a new language, or a new capability.
- **Patch:** wording, tells, examples, fixes.

To see which version you have installed, run `claude plugin list`.

## [Unreleased]

### Added

- `writer-skills` plugin, installed from the `novakovskyi-writer-skills`
  marketplace. It replaces the symlink installer: `install.sh` and
  `uninstall.sh` are gone.
- `fact-check` — extracts every checkable claim from the text, its front-matter
  title and excerpt, and its illustrations; runs a sanity pass; verifies the
  rest against primary sources (source code for claims about internals); and
  reports mismatches as "fix before publishing" and "worth fixing", each with
  the correct version and a source.
- `coherence-check` — maps the promise, the through-line, and the job of each
  section, then finds broken promises, lost threads, detours, jumps, repeats,
  drift, a weak ending, and the seams an edited AI draft leaves behind.
  `scripts/skeleton.py` prints the headings and first sentences with line
  numbers, plus back-references and announced counts to verify.
- `voice-check` — finds what makes a text read as AI-written, in English,
  Ukrainian, and Russian, and suggests rewrites aimed at the author's own voice
  when given samples. `references/tells/` holds the tells: `universal.md` plus
  one file per language, and a check loads only the text's language;
  `scripts/text_stats.py` counts rhythm, punctuation, repeated openers, and
  filler words with line numbers.
