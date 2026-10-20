# Setup

Start-to-finish setup on a Mac (Apple Silicon). Every step has a $0 cost.
Run each block in Terminal, in order.

## 1. Xcode Command Line Tools

Provides the compilers and base `git` that Homebrew needs.

```sh
xcode-select --install
```

Click **Install** in the dialog and wait for it to finish.

## 2. Homebrew

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

On Apple Silicon, Homebrew installs to `/opt/homebrew`, which is not on your PATH by default.
Add it to `~/.zprofile` so every new terminal can find `brew`:

```sh
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

## 3. git, GitHub CLI, uv, and Python 3.12

```sh
brew install git gh uv
uv python install 3.12
```

uv manages the Python version for the pipeline; you don't need a separate Python install.

## 4. Node 24

```sh
brew install node@24
```

`node@24` is "keg-only", so Homebrew does not link it onto your PATH. Add it yourself:

```sh
echo 'export PATH="/opt/homebrew/opt/node@24/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
```

## 5. Log in to GitHub

```sh
gh auth login
```

Answer the prompts:
- Where do you use GitHub? **GitHub.com**
- Preferred protocol? **HTTPS**
- Authenticate Git with your GitHub credentials? **Yes**
- How would you like to authenticate? **Login with a web browser** (copy the one-time code, then paste it in the browser)

## 6. Accounts

Create a free account on each. Store every API key in a password manager — **never in code, chat, or commits**.

| Service | Plan | Used for | Key goes in `.env` as |
| --- | --- | --- | --- |
| [GitHub](https://github.com) | Free | Repo + Actions cron | — (uses `gh auth`) |
| [Supabase](https://supabase.com) | Free | Postgres + magic-link auth | `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY` |
| [Vercel](https://vercel.com) | Hobby (sign up with GitHub) | Hosts the web app | — (set in Vercel dashboard) |
| [Resend](https://resend.com) | Free | Email delivery | `RESEND_API_KEY` |
| [Google AI Studio](https://aistudio.google.com) | Free | Gemini (primary LLM) | `GEMINI_API_KEY` |
| [Groq](https://console.groq.com) | Free | Fallback LLM | `GROQ_API_KEY` |

Note: Resend only sends to the account owner's address until you verify a domain (see ADR-003 in `DECISIONS.md`).

## 7. Create the repo and scaffold

```sh
cd ~/Documents/"One Percent"
gh repo create compound --public --clone
cd compound
```

Then scaffold the monorepo (done with Claude Code on Day 01):

```
compound/
├── .github/workflows/   # CI + cron jobs
├── docs/                # PRD, architecture, decisions, progress, session logs
├── pipeline/            # Python 3.12 + uv
├── prompts/             # versioned prompt files
├── web/                 # Next.js app
├── .env.example         # template for secrets
├── .gitignore           # includes .env
├── CLAUDE.md            # project rulebook
└── README.md
```

Create your local secrets file from the template and fill it in from your password manager:

```sh
cp .env.example .env
```

`.env` is gitignored. Commit and push the scaffold:

```sh
git add -A
git commit -m "chore: scaffold monorepo"
git push
```

## 8. Verify

Open a **new** terminal and run:

```sh
xcode-select --version
brew --version
git --version
gh --version
uv --version
uv run --python 3.12 python --version
node --version
npm --version
gh auth status
```

Expected (versions as installed on Day 01; newer patch versions are fine):

| Command | Expected |
| --- | --- |
| `xcode-select --version` | `xcode-select version 2416.` |
| `brew --version` | `Homebrew 7.0.x` |
| `git --version` | `git version 2.56.x` |
| `gh --version` | `gh version 2.102.x` |
| `uv --version` | `uv 0.13.x` |
| `uv run --python 3.12 python --version` | `Python 3.12.x` |
| `node --version` | `v24.x` |
| `npm --version` | `11.x` |
| `gh auth status` | `Logged in to github.com` |

## Common errors

### `zsh: command not found: brew` (or `node`)
The PATH line in `~/.zprofile` only applies to terminals opened after it was added.
Open a new terminal, or reload it in the current one:

```sh
source ~/.zprofile
```

If it still fails, check the lines exist: `cat ~/.zprofile`. You should see both the `brew shellenv` line and the `/opt/homebrew/opt/node@24/bin` PATH line.

### A long prompt won't paste into Claude Code
Very long pastes can get truncated or fail in the terminal. Save the clipboard to a file instead, then point Claude Code at it:

```sh
pbpaste > prompt.md
```

Then in Claude Code: `Read prompt.md and follow it.` Delete the file afterwards (or keep it out of git).

## Environment Variables
_TBD: copy `.env.example` to `.env` and fill in values._

## Run the Pipeline
_TBD: local pipeline commands._

## Run the Web App
_TBD: local web commands._
