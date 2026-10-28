# Claude Writer Skills

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

A [Claude Code](https://code.claude.com) plugin that reviews an article or a
draft before you publish it, especially one written with AI help.

```text
fact-check → coherence-check → voice-check
```

Each skill reads the text as a different kind of editor and returns a report:
what is wrong, where, and how to fix it. None of them changes your text unless
you ask.

> **No release yet.** Everything lives on the `dev` branch for now, so the
> install commands below start to work with the first release. Until then,
> install from a clone: see [docs/unreleased.md](docs/unreleased.md).

## Requirements

- **Claude Code.** The shell commands on this page and in the docs use the
  `claude` command. If you have only the desktop app, `claude` is not on your
  `PATH`; install it with
  `curl -fsSL https://claude.ai/install.sh | bash`.
- **`python3`, for `coherence-check` and `voice-check`.** Their helper scripts
  use the standard library only; there is nothing to `pip install`.
- **Optional: `git` and the GitHub CLI ([`gh`](https://cli.github.com)),** to
  read drafts that live in a branch or a pull request.

`fact-check` relies on web search to verify claims. Without it, the report says
so at the very top and marks every verdict "from memory".

## Install

In a Claude Code session:

```text
/plugin marketplace add vnovakovskyi/claude-writer-skills
/plugin install writer-skills@novakovskyi-writer-skills
```

Type `/` and the commands appear as `/writer-skills:fact-check`,
`/writer-skills:coherence-check`, and `/writer-skills:voice-check`. The plugin
adds skills only: no hooks, no servers.

From a shell, the same two commands start with `claude plugin` instead of
`/plugin`. Once installed, the plugin is also there in the desktop app and the
VS Code extension on the same machine.

## Stay up to date

You keep the version you installed until you update it. Pick one of two ways
not to miss a release.

**Turn on auto-update (recommended).** Run `/plugin`, open the **Marketplaces**
tab, select `novakovskyi-writer-skills`, and choose **Enable auto-update**.
Claude Code then fetches new versions on its own and tells you when one is
ready.

**Or watch the releases.** On the
[repository page](https://github.com/vnovakovskyi/claude-writer-skills), click
**Watch → Custom → Releases**. GitHub emails you when a version comes out, and
you update by hand:

```bash
claude plugin marketplace update novakovskyi-writer-skills
claude plugin update writer-skills@novakovskyi-writer-skills
```

Then start a new session, or run `/reload-plugins` in an open one.

`claude plugin list` shows the version you have, and
[CHANGELOG.md](CHANGELOG.md) says what changed in each one.

Want a change before it is released? See
[docs/unreleased.md](docs/unreleased.md).

## Skills

| Command | The question it answers | What you get |
|---|---|---|
| `/writer-skills:fact-check` | Is what the text says true? | Every checkable claim verified against sources, and tables of mismatches with the correct version and a source for each. |
| `/writer-skills:coherence-check` | Does the text hold together as one piece? | A map of the argument, and a table of structural breaks with a fix for each. |
| `/writer-skills:voice-check` | Does it sound like a person wrote it? | Tables of AI tells with a rewrite for each, aimed at your own voice when you give it samples. |

A slash command is optional: "fact-check this article" or "does this sound like
AI?" starts the right skill as well.

## How to use it

Run Claude Code where the text is, and go in order:

```text
/writer-skills:fact-check <file, PR, branch:path, or pasted text>
/writer-skills:coherence-check <the same text>
/writer-skills:voice-check <the same text> [style: <texts you wrote yourself>]
```

Each skill reports and stops. It offers to apply its fixes and applies only the
ones you pick. Reports follow the language of your conversation; quotes and
suggested rewrites stay in the language of the text. English, Ukrainian, and
Russian are supported.

The details are in [docs/skills.md](docs/skills.md).

## Uninstall

```text
/plugin marketplace remove novakovskyi-writer-skills
```

This removes the marketplace and the plugin. Your texts are left untouched.

## More

- [docs/skills.md](docs/skills.md) — what each skill checks and returns, the
  languages, and the helper scripts on their own
- [docs/unreleased.md](docs/unreleased.md) — running `dev`, a branch, or your
  own clone before a release
- [CHANGELOG.md](CHANGELOG.md) — what changed in each version
- [CONTRIBUTING.md](CONTRIBUTING.md) — how to propose a change
- [claude-dev-skills](https://github.com/vnovakovskyi/claude-dev-skills) —
  sibling skills for software development

## License

Licensed under the [MIT License](LICENSE): free to use, copy, modify, and
distribute for any purpose, including commercial use. © 2026 Vadym Novakovskyi.
