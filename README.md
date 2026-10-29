<h1 align="center">kempt</h1>
<p align="center"><b>Keep your git repository kempt — tidy commits, branches, changelog, and docs.</b></p>

<p align="center">
  <img alt="status" src="https://img.shields.io/badge/status-alpha-orange">
  <img alt="python" src="https://img.shields.io/badge/python-3.10%2B-blue">
  <img alt="license" src="https://img.shields.io/badge/license-MIT-green">
  <img alt="deps" src="https://img.shields.io/badge/dependencies-zero-brightgreen">
</p>

<!-- kempt:toc -->
- [Why](#why)
- [Install](#install)
- [Commands](#commands)
- [Design](#design)
- [Roadmap](#roadmap)
- [License](#license)
<!-- kempt:toc-end -->

## Why

Every repo accumulates the same chores: commit messages that drift from
convention, branches nobody deleted, a README table of contents that's three
headings out of date, a changelog written by hand at release time. `kempt`
is one small, dependency-free CLI that does each of them, and exits non-zero
when something's wrong so it slots into hooks and CI.

It shells out to the `git` you already have — no libraries, no network, no
telemetry.

## Install

```bash
git clone https://github.com/AdityaMekala14/kempt.git
cd kempt
pip install -e ".[dev]"
```

Then, in any repo:

```bash
kempt health
```

## Commands

| Command | What it does |
|---------|--------------|
| `kempt lint-commits` | Check commit subjects against Conventional Commits |
| `kempt stale-branches` | List (or `--prune`) merged / long-idle branches |
| `kempt health` | Score repo hygiene; `--fail-under` for CI |
| `kempt changelog` | Build a grouped changelog from conventional commits |
| `kempt toc` | Generate/refresh a Markdown table of contents |

Full flags and examples: [`docs/usage.md`](docs/usage.md).

```bash
$ kempt health
Repo health: 90%  ██████████████████░░  (9/10)

  ✓ README
  ✓ LICENSE
  ✓ .gitignore
  ✓ CI workflow
  ✓ Tests
  ✗ Contributing guide
```

## Design

One CLI, one sub-command per chore. Each command lives in `kempt/commands/`
and is a small module exposing `add_parser(sub)` plus pure functions that the
tests call directly against a throwaway git repo. Adding a command is adding a
module and one line in `cli.py`. Git access is isolated in `kempt/gitutil.py`.

## Roadmap

- [ ] `kempt hooks install` — wire lint-commits into a pre-push hook
- [ ] `kempt changelog --write` — prepend to `CHANGELOG.md`
- [ ] Config file (`.kempt.toml`) for per-repo thresholds
- [ ] `kempt health` JSON output + SARIF
- [ ] Branch naming-convention linter

## License

MIT © Aditya Mekala. See [`LICENSE`](LICENSE).
