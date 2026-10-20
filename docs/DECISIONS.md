# Decisions

## ADR-001: Public GitHub repo
- **Decision:** Host the repo publicly on GitHub.
- **Options:** Public repo; private repo.
- **Why:** Unlimited GitHub Actions minutes and a portfolio showcase.

## ADR-002: uv for Python
- **Decision:** Use uv for Python versions, dependencies, and running tools.
- **Options:** uv; pip + venv; Poetry.
- **Why:** One fast tool, and a lockfile that keeps laptop and CI identical.

## ADR-003 (OPEN): Email sending to other users
- **Decision:** Pending — decide in Sprint 6.
- **Options:** Buy a domain (~$10/yr) and verify it in Resend; use Gmail SMTP.
- **Why:** Resend needs a verified domain to email users other than the account owner.

## ADR-004: Gemini Flash primary, Groq fallback
- **Decision:** Gemini Flash as the primary LLM, Groq as fallback, behind a provider-agnostic wrapper.
- **Options:** Gemini; Groq; paid models (e.g. Claude).
- **Why:** Fits the $0 budget. Per-agent model config allows paid models (e.g. Claude) later via config only.

## ADR-005: Refresh cadence vs. email cadence
- **Decision:** Content refreshes up to 4x/day (start with 2); email is sent once per day only.
- **Options:** Single daily refresh; multiple refreshes with multiple emails; multiple refreshes with one email.
- **Why:** Protects the 3-minute habit and stays within Resend's free limits.

## ADR-006: Audit logging
- **Decision:** Every run, agent step, tool call, and LLM call is audit logged with run_id; secrets are never logged.
- **Options:** Minimal error logging; full audit logging.
- **Why:** Enables debugging, evals, cost tracking, and trust in what the agents did.
