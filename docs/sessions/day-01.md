# Day 01 — 2026-10-20

## Goal
Set up the Mac from scratch, create all free accounts, and scaffold the `compound` repo.

## Done
- Installed Xcode Command Line Tools, Homebrew, git, gh, uv, Python 3.12, and Node 24.
- Logged in to GitHub with `gh auth login`.
- Created accounts: GitHub, Supabase, Vercel (Hobby, via GitHub), Resend, Google AI Studio, Groq. Keys stored in a password manager.
- Created the public repo `compound` and scaffolded the monorepo: `CLAUDE.md`, `.env.example`, `.gitignore`, `docs/` skeleton, `pipeline/`, `web/`, `prompts/`, `.github/workflows/`.
- Recorded ADR-001 to ADR-006 in `DECISIONS.md`.
- Wrote the full Mac setup guide in `SETUP.md`.

## Commands run
```sh
xcode-select --install
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
brew install git gh uv
uv python install 3.12
brew install node@24
echo 'export PATH="/opt/homebrew/opt/node@24/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
gh auth login            # GitHub.com, HTTPS, browser
gh repo create compound --public --clone
cp .env.example .env
git commit -m "chore: scaffold monorepo, CLAUDE.md, env template, decisions, and docs skeleton"
git push
```

## Issues hit
| Issue | Cause | Fix |
| --- | --- | --- |
| `brew: command not found` | Homebrew wasn't installed, and on Apple Silicon `/opt/homebrew` isn't on PATH by default | Installed Homebrew, added `brew shellenv` to `~/.zprofile` |
| `node: command not found` after `brew install node@24` | `node@24` is keg-only, so Homebrew doesn't link it | Added `/opt/homebrew/opt/node@24/bin` to PATH in `~/.zprofile`, then `source ~/.zprofile` |
| The long setup prompt wouldn't paste into Claude Code | Very long terminal pastes get truncated or fail | `pbpaste > prompt.md`, then asked Claude Code to read the file |

## Key learnings
- **LLM cost scales with runs, not users.** Briefs are built per domain and per unique tag on a schedule, so 10 or 10,000 users cost the same in LLM calls.
- **Email once a day.** Content can refresh up to 4x/day, but email goes out once — protects the 3-minute habit and stays within Resend's free limits (ADR-005).
- **Resend needs a verified domain** to email anyone other than the account owner. Open decision: buy a domain or use Gmail SMTP (ADR-003, decide in Sprint 6).

## Decisions
See [DECISIONS.md](../DECISIONS.md): ADR-001 (public repo), ADR-002 (uv), ADR-003 (OPEN: email sending), ADR-004 (Gemini primary, Groq fallback), ADR-005 (refresh vs. email cadence), ADR-006 (audit logging).

## Next
- Day 02: CI skeleton (ruff, mypy, pytest, ESLint on every PR).
