# Contributing to Claude Writer Skills

Thanks for your interest in improving this project! It's a small, personal
collection of reusable [Claude Code](https://code.claude.com) skills for
reviewing articles and drafts, packaged as a plugin that anyone is welcome to
use and improve.

**Every change comes in through a pull request against the `dev` branch.**
`main` holds released versions only; see [Branches and releases](#branches-and-releases).

Contributions are reviewed personally by the maintainer. Not every change will
be merged, and response times may vary — but well-scoped, tested PRs are always
appreciated.

## Project scope

This repo is intentionally **generic and reusable across texts and blogs**.
Good contributions keep it that way.

**In scope:**

- New or improved skills for writing, editing, and reviewing texts
  (`plugins/writer-skills/skills/<name>/SKILL.md`).
- Better lists of tells, and support for more languages.
- The helper scripts (`plugins/writer-skills/skills/*/scripts/*.py`).
- The plugin and marketplace manifests.
- Documentation, examples, and clarity fixes.

**Out of scope:**

- Skills for software development, the messages around it included — those live
  in [claude-dev-skills](https://github.com/vnovakovskyi/claude-dev-skills).
- Anything tied to one blog, publication, company, or house style (keep that in
  your own repo).
- Skills that rewrite a text without asking, or that invent facts, sources, or
  experiences for the author.
- Helper scripts that need third-party packages.
- Large new subsystems without prior discussion (open an issue first).

## How to contribute

1. **Fork** the repository and clone your fork.
2. Create a **feature branch** off `dev`
   (e.g. `git checkout -b add-polish-tells origin/dev`).
3. Make a **small, focused change** — one logical improvement per PR is much
   easier to review than a large mixed one.
4. **Test** your change (see below).
5. Add a line under **Unreleased** in [CHANGELOG.md](CHANGELOG.md) when the
   change is visible to users. Leave the version number alone.
6. Open a **Pull Request against `dev`**. Describe the *motivation*, *what was
   done*, and *how to verify* it — the pull request template has these three
   sections.
7. The maintainer reviews, may ask for changes, and merges when it's ready.

**Issues:** direct PRs are welcome for small fixes. For large, breaking, or
design-level changes, please **open an issue first** so we can agree on the
approach before you invest time.

## Testing your change

There's no heavy test framework — keep it simple and verify locally before
submitting:

- **Manifests and skills:** run the validator from the repository root. It
  checks the marketplace and the plugin.

  ```bash
  claude plugin validate . --strict
  ```

- **Skills:** run the changed skill from your working copy on a real draft,
  before and after your change, and compare the two reports. Say in the PR what
  kind of text you used and in which language. Don't paste someone else's
  unpublished draft into a public PR. [docs/unreleased.md](docs/unreleased.md)
  shows how to make Claude Code read the plugin straight from your clone, or
  load it for a single session:

  ```bash
  claude --plugin-dir /path/to/claude-writer-skills/plugins/writer-skills
  ```

- **Helper scripts:** run every script you touched on a Markdown file — with
  front matter, a code block, a list, and a table — in each language whose
  patterns you changed.

  ```bash
  python3 plugins/writer-skills/skills/coherence-check/scripts/skeleton.py article.md
  python3 plugins/writer-skills/skills/voice-check/scripts/text_stats.py article.md --lang uk
  ```

## Conventions

- **Skills** live in `plugins/writer-skills/skills/<name>/SKILL.md` with YAML
  frontmatter:
  - `name`, `description`, `argument-hint`, and `allowed-tools`.
  - The frontmatter must be valid YAML: quote a value that contains `: `.
  - Claude starts these skills on its own when a request matches, so the
    `description` decides when a skill triggers. Say what the skill does and
    when to use it.
  - Keep `allowed-tools` read-only: reading files, searching, fetching pages,
    and read-only `git`/`gh` commands.
  - When a skill names another skill's command, it uses the full form:
    `/writer-skills:fact-check`, not `/fact-check`.
- **Report first.** A skill returns a report and never edits the text without
  permission. It keeps the author's facts and meaning, and doesn't write content
  only the author can write.
- **Reference material** that a skill reads on demand goes in
  `skills/<name>/references/`; scripts go in `skills/<name>/scripts/`.
- **Helper scripts** are `python3`, standard library only. They print signals
  with line numbers and leave the judgment to the skill.
- **Languages:** English, Ukrainian, and Russian are supported today. Adding a
  language means adding it everywhere it is listed: a section in
  `skills/voice-check/references/tells.md`, the word and pattern tables plus
  `detect_lang` and `--lang` in both scripts, and [docs/skills.md](docs/skills.md).
- **Markdown:** plain GitHub-flavored Markdown, English, wrap reasonably.
- **Keep the README short.** It says what the project is, how to install it, and
  how to stay up to date. Details go in `docs/`.

## Branches and releases

| Branch | What it holds |
|---|---|
| `main` | Released versions. This is what people install. It changes only when a release is merged. |
| `dev` | The next release, collected one pull request at a time. |

The split exists because of how plugin versions work. Someone who installs a
plugin keeps those files until its version number changes, while a new install
always takes the current `main`. If `main` changed between releases, two people
on the same version would have different skills. So `main` never changes without
a new version number.

The version number lives in `plugins/writer-skills/.claude-plugin/plugin.json`.
It follows [semantic versioning](https://semver.org); `CHANGELOG.md` says what
counts as major, minor, and patch here.

A release is made by the maintainer:

1. On `dev`, set the new version in `plugin.json`, rename **Unreleased** in
   `CHANGELOG.md` to the version and date, and open a new empty **Unreleased**.
2. Open a pull request from `dev` into `main`, titled `Release X.Y.Z`.
3. Merge it **with a merge commit, not a squash**. The two branches then keep a
   shared history, and the next release pull request shows only what is new.
4. Tag the merge commit `vX.Y.Z` and publish a GitHub release with that changelog
   entry. People who watch the repository's releases are notified.

## Repository layout

```text
.claude-plugin/marketplace.json     the marketplace: lists the plugin
plugins/writer-skills/              the plugin with the three skills
  .claude-plugin/plugin.json
  skills/fact-check/SKILL.md
  skills/coherence-check/SKILL.md
  skills/coherence-check/scripts/skeleton.py
  skills/voice-check/SKILL.md
  skills/voice-check/references/tells.md
  skills/voice-check/scripts/text_stats.py
docs/                               user documentation beyond the README
CHANGELOG.md                        what changed in each version
```

## Commits & pull requests

- Write clear, present-tense commit messages explaining the *why*.
- Keep PRs focused; split unrelated changes into separate PRs.
- A [DCO](https://developercertificate.org/) sign-off (`git commit -s`) is
  welcome but not required.
- By contributing, you agree your contribution is licensed under the project's
  [MIT License](LICENSE) (inbound = outbound).

## License

This project is licensed under the [MIT License](LICENSE). Contributions are
accepted under the same license.
