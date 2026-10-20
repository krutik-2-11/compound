# Compound — Project Rulebook

## Product
- Mission: help people get 1% smarter every day.
- A 3-minute morning brief: top 3–5 stories per domain (Politics, Tech, Finance, Sports).
- Each story: a sourced summary + a "why it matters" line.
- Plus: a Big Idea of the Day and conversation starters.
- Users pick domains and tags (e.g. NVIDIA, MU, Cricket).

## Stack ($0 only)
- Pipeline: Python 3.12 + uv.
- Scheduling: GitHub Actions cron (no always-on server).
- Data + auth: Supabase (Postgres + magic-link auth).
- Web: Next.js App Router + Tailwind on Vercel, shipped as a PWA.
- Email: Resend.
- LLM: Gemini (primary) + Groq (fallback) behind ONE provider-agnostic LLM wrapper.
- Model choice is configurable per agent; paid models (e.g. Claude) can be added via config later with no agent code changes.

## Architecture Rules
- Briefs are prebuilt by scheduled jobs and stored.
- The app ONLY reads finished data; it never fetches news live.
- LLM work runs per domain and per unique tag, never per user.

## Agents
- Feed Health, Collector, Editor, Research, Verifier, Writer/Personalizer, Memory.
- Hand-written agent loop using native tool calling.
- No agent framework unless agreed first.

## Non-goals
- No native mobile apps, payments, trading, or buy/sell/hold advice.
- No paid APIs, paywall scraping, or infinite feeds.

## Guardrails
- Every summary line links to a source.
- Finance is awareness only: never buy/sell/hold; always include a "not financial advice" note.
- Store only headlines, short snippets, and links — never full articles.
- Respect site terms; no paywall bypassing.
- Politics stays neutral and presents the main sides fairly.

## Logging & Audit (required)
- Log every pipeline run, agent step, tool call, and LLM call with: run_id, timestamp, agent name, outcome.
- LLM calls also log: provider, model, prompt version, input/output tokens, latency.
- Verifier removals are logged with the reason.
- NEVER log API keys, passwords, or full article text.
- Log user emails only where needed for sending.

## Engineering
- Python: type hints everywhere, ruff (lint + format), mypy, pytest, small single-purpose functions.
- TypeScript: strict mode, ESLint, Prettier.
- Prompts are versioned files in `/prompts`.
- Conventional Commits.
- One feature branch + PR per module; `main` is always deployable.
- Secrets only in `.env` (gitignored) and GitHub/Vercel secrets; keep `.env.example` updated.

## Definition of Done
- Code works, tests pass, evals don't regress, docs updated, committed and pushed.

## Docs to Keep Updated (every session)
- `docs/PROGRESS.md`
- `docs/sessions/day-XX.md`
