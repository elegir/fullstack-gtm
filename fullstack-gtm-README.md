# Fullstack GTM

**AI-native GTM agency. Replaces the go-to-market department for B2B companies.**

Three production systems built and operated by one person. Content publishes on cron schedules, outbound runs on signal triggers, web infrastructure executes via SSH automation — without daily intervention. The interface is the outcome.

→ **[fullgtm.ai](https://fullgtm.ai)** · **[AI SDR demo](https://aisdr.fullgtm.ai)**

---

## What this is

Most GTM software sells you a fishing rod. Clients don't want to fish.

Fullstack GTM operates the full GTM stack as a service: finds clients, builds content authority, manages web infrastructure. Clients pay for results on monthly retainers, not software seats. Traditional agencies do this manually at $8–15K/month with 20–35% margins. We deliver at 65–75% margins with near-zero marginal cost per additional client.

Built solo using **Cursor + Claude** as the engineering environment. ~95% AI-generated codebase, with `.cursor/rules` and `.cursor/skills` production guardrails defining agent behavior, naming conventions, and skill packs across all three repos.

---

## The three systems

### 1. AI SDR (`ai-sdr`)
Signal-based outbound engine. Monitors LinkedIn posts, news articles, and RSS feeds for trigger events. Qualifies candidates through configurable LLM filter tools. Resolves contact emails via a cost-ascending provider waterfall (Tomba → Findymail → Apify). Auto-enrolls into personalized email campaigns.

Zero templates. Every email written from a fresh, unused signal specific to that prospect. Signal memory prevents repeating the same personalization cue across runs.

**Stack:** Python · FastAPI · SQLAlchemy · APScheduler · PostgreSQL · Redis · Anthropic Claude API · OpenAI API · Gmail API · Tavily · Apify  
**Infra:** DigitalOcean (Ubuntu 22.04)  
**Key features:**
- Radar pipeline: source-agnostic autonomous detection (LinkedIn, news, webhooks)
- Configurable LLM filter tools with fail-closed behavior
- Run-level deduplication — same signal never hits two contacts at the same firm in one batch
- 5-attempt retry budget with exponential backoff
- Campaign intelligence: $/reply and funnel metrics per campaign, attributed to radar-sourced contacts only
- Unified Learn workspace: AI proposes playbook rewrites, human approval required — no auto-apply

---

### 2. Content Central (`content-central`)
Automated editorial pipeline. Per-client configuration: author persona, editorial guidelines, RSS feed selection, GPT Researcher integration, EEAT validation, and cron-based publishing to WordPress via REST API + JWT. Multilingual. Cost-tracked per article.

Produces SEO and GEO (Generative Engine Optimization) content — content that ranks on Google and appears in AI search answers (ChatGPT, Perplexity, Gemini).

**Stack:** Python · Anthropic Claude API · OpenAI API · GPT Researcher · WordPress REST API · JWT · SOPS + age  
**Hosting:** DreamHost · Cloudways · Contabo  
**Key features:**
- Per-client pipeline: feeds → research → EEAT enrichment → article → image → QA → publish
- Editor's Notes: per-post observability layer recording which pipeline and which source triggered each article (authenticated WordPress users only)
- Multilingual production (EN/ES confirmed)
- Cost tracking per article per client

---

### 3. WordPress Fleet Control (`wordpress-fleet-control`)
43 live WordPress sites under active management across 4 hosting providers. 172 Python scripts covering migrations, security hardening, DNS management, and credential rotation.

Architected to scale to hundreds or thousands of sites with zero additional headcount: stateless operations, Redis job queue, PostgreSQL fleet registry with no practical site count ceiling.

**Stack:** Python · FastAPI · Redis · PostgreSQL · Paramiko · WP-CLI over SSH · Playwright · Cloudflare API · SOPS · Docker Compose  
**Hosting managed:** Contabo · DigitalOcean · Cloudways · DreamHost  
**Key features:**
- 8-phase automated site creation pipeline: brief → brand → design (Claude Opus) → WordPress provisioning → AI-generated page copy → DNS — full site live in one command
- Gate-based validation with rollback logic at every phase
- SOPS-encrypted credentials per site
- Full post-audit security hardening (8 checks per provisioned site)
- Static site type support (deployed fullgtm.ai on the same fleet)
- SMTP2GO lead capture integration

---

## Why the repos are private

All three systems operate live client workloads. The repos contain:

- **Client-specific configurations** — editorial guidelines, ICP definitions, signal keywords, campaign playbooks
- **Encrypted credential bundles** (SOPS + age) for 43 production sites across 4 hosting providers
- **Proprietary signal data** — the operational dataset accumulated across client campaigns is the moat; making it public would be the moat

Standard practice for production systems handling client data. The architecture, stack decisions, and implementation approach are documented here. Full sessions showing the engineering process are available on request.

---

## Engineering approach

Everything built in **Cursor agent mode** with Claude Sonnet as the primary engineering agent. The human layer is what makes the difference — not vibe coding wholesale, but structured agent direction:

- `.cursor/rules` — production guardrails: naming conventions, error handling patterns, migration style, test requirements
- `.cursor/skills` — skill packs per repo: API integration patterns, WordPress automation conventions, email compliance rules
- Every significant feature documented with architecture decision records inline in the code

Sessions covering all three repos in depth — including design decisions, implementation, and post-implementation bug fixes — are documented separately.

---

## Live systems

| System | Status | URL |
|--------|--------|-----|
| AI SDR | 🟢 Live | [aisdr.fullgtm.ai](https://aisdr.fullgtm.ai) |
| Content Central | 🟢 Live | Serving paying clients |
| WordPress Fleet | 🟢 Live | 43 sites under management |
| fullgtm.ai | 🟢 Live | [fullgtm.ai](https://fullgtm.ai) |

---

## Current state (April 2026)

- **Paying clients:** 4 active + 1 in onboarding
- **MRR:** $10,406 (April 2026, 3 contracts activating)
- **Churn:** Zero cancellations since first client (January 2026)
- **Sites managed:** 43 across DigitalOcean, Cloudways, DreamHost, Contabo
- **Fastest close:** MUNDI ($7,500/month) — committed 3 weeks after first demo

---

## Stack summary

```
Languages:     Python (primary)
Frameworks:    FastAPI · SQLAlchemy · Alembic · APScheduler
AI:            Anthropic Claude API (claude-sonnet) · OpenAI API
Databases:     PostgreSQL · Redis
Automation:    Gmail API · WordPress REST API · WP-CLI · Paramiko · Playwright
DNS/Infra:     Cloudflare API · DigitalOcean · Contabo · Cloudways · DreamHost
Security:      SOPS + age (secrets encryption)
AI coding:     Cursor + Claude Sonnet (agent mode)
               .cursor/rules + .cursor/skills (production guardrails)
```

---

## About

Built by **Martin Weidemann** — [linkedin.com/in/martinweidemann](https://linkedin.com/in/martinweidemann)

Previously: ElegirSeguro.com (insurtech, ~$1M raised, FinTech Global InsurTech100, Zurich Innovation Challenge winner, Startup Chile, Parallel18).

The problem Fullstack GTM solves is the one ElegirSeguro couldn't: B2B distribution at scale without linear headcount. That failure is not background context — it's the origin of this system.
