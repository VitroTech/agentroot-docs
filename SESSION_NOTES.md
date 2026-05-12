# Session Notes — Documentation Build

Working document for the AI assistant helping David work on `agentroot-docs`. Read this first at the start of every new session that touches this repo. Not part of `docs.json` — does not render in Mintlify.

## Current state

The docs are connected to Mintlify (custom config in `docs.json`, custom domain is `docs.agentroot.app` once DNS is wired, current Mintlify-hosted URL is `vitrotechnology.mintlify.app`). Every push to `main` auto-deploys. Four rounds of content work plus a visual-polish pass are complete; the doc set is launch-shaped but a few items require David's personal verification before public release.

**Project repo:** `/Users/dhg/Documents/GitHub/agentroot-docs`
**Operator workflow:** David commits and pushes from GitHub Desktop. The AI never runs `git commit` / `git push` / any write-side git command. When work is ready, the AI says "ready to commit in GitHub Desktop" and stops.

## Decisions

- **No AgentRoot SDK.** (2026-05-12) The project will not ship `@agentroot/sdk`. All proxied API examples use raw HTTP — `fetch()` in TypeScript, `requests` in Python, `curl` for shell. For provider-specific official SDKs (OpenAI, Anthropic, etc.) the pattern is: set `baseURL` to `${process.env.AGENTROOT_PROXY}/<provider>/<version>`, set `defaultHeaders` to `{ Authorization: 'Bearer ${AGENTROOT_TOKEN}', 'X-AgentRoot-ID': '${AGENTROOT_AGENT_ID}' }`. Do not reintroduce `ar.proxy()`, `ar.openai()`, `npx @agentroot/sdk init`, or any `@agentroot/sdk` import. The `sdk.mdx` page is gone, the npm anchor is gone, and `docs.json` / `mint.json` no longer list `sdk` in nav.

## What's done

### Round 1 — Restructure for narrative
- Rewrote `introduction.mdx` with the **Pinocchio Problem** framing (obedience, not malice; Fox and Cat as prompt injection; "the threat model isn't a hostile AI, it's an obedient one")
- Wrote `action-classes.mdx` documenting the six classes (Spend / Communication / Writes / Transactions / Provisioning / Destruction) with the 27/54 thesis
- Wrote `lifecycle.mdx` promoting **Mint / Disable / Kill / ReMint** as first-class concepts with state machines
- Closer in introduction: **"AgentRoot keeps the strings in your hands"** — David's sovereignty-aware revision of the original "AgentRoot is the strings"
- Defined EAS inline in `Why onchain matters` so cold readers aren't stranded

### Round 2 — Voice pass on core pages
- Wrote `playbooks.mdx` — six operator runbooks: 3am rogue agent, investigating a suspicious call, leaving for the weekend, rotating a compromised key, deploying a new agent version, killing and ReMinting after an incident
- Rewrote `kill-switch.mdx` — lifecycle nouns lead, tiers are technical sub-classification, "Kills do not unsign in-flight authorizations" boundary section
- Rewrote `architecture.mdx` — opens with the agent's-eye view, two Mermaid diagrams (two-plane isolation flowchart + 11-step request-flow sequence)
- Rewrote `dashboard.mdx` — restructured around four operator scenarios (onboarding, daily ops, triage, sleep)
- Rewrote `sdk.mdx` — real 30-line customer-support agent example, full framework integrations (LangChain, CrewAI, OpenAI SDK direct, Anthropic SDK direct). **[Superseded 2026-05-12 — see Decisions: No AgentRoot SDK.]**

### Round 3 — Providers catalog
- Added top-level **Providers tab** in `docs.json` (alongside Documentation)
- Wrote 4 catalog admin pages: `providers/overview`, `providers/conventions`, `providers/action-class-mapping`, `providers/custom`
- Wrote 51 provider pages organized into 6 category groups (LLMs / Actions / Tools / Media / Frameworks / Wallets)
- **OpenAI is the reference page** — fully filled out with `<AccordionGroup>` of 6 endpoints, each with `<CodeGroup>` of cURL/TypeScript/Python tabs, action-class annotations, drop-in SDK integration block
- Other 50 providers are templated stubs — setup section, action-class note, one representative call in a CodeGroup, link to upstream docs, "full endpoint catalog coming soon" callout
- Frameworks and Wallets have distinct templates (client-side integration / identity-layer respectively — not API-proxying)

### Round 4 — Fact cleanup
- Added 3 missing providers (Cohere, Fireworks AI, Pinecone) — brings total to **54** matching marketing site
- Updated `providers/overview.mdx` counts to 54 with correct action-class math (27 / 19 Spend / 8 client-side)
- Filled every `[VERIFY — ...]` placeholder in `trust-center.mdx` and `subprocessors.mdx` with industry-standard defaults
- Trimmed `api-reference.mdx` — removed redundant provider table, defers to `providers/conventions` for the proxy contract
- Bumped "Last reviewed" dates to 2026-05-12 across trust-center, subprocessors, vulnerability-disclosure

### Visual polish pass
- Converted ASCII state machines in `lifecycle.mdx` to **Mermaid `stateDiagram-v2`** — one for agents (Mint/Disable/Kill/ReMint with note about onchain permanence), one for keys (Activate/Disable/Delete)
- Added a **Mermaid flowchart decision tree** in `kill-switch.mdx` colored by tier
- AgentRoot brand SVGs (logo, favicon) already installed from Round earlier in the conversation

## Files in the repo

```
agentroot-docs/
├── docs.json                          # Mintlify config — single source for navigation
├── README.md                          # Repo readme (unchanged)
├── SESSION_NOTES.md                   # This file
├── favicon.svg                        # From agentroot.app/icon.svg
├── logo/
│   └── agentroot-logo.svg             # From agentroot.app/landing/ar-logo.svg
├── introduction.mdx                   # Pinocchio Problem
├── action-classes.mdx                 # Six-class thesis
├── lifecycle.mdx                      # Mint / Disable / Kill / ReMint + Mermaid
├── quickstart.mdx                     # Splits into the two below
├── quickstart-social.mdx              # Google/GitHub sign-in path
├── quickstart-siwe.mdx                # Wallet sign-in path
├── architecture.mdx                   # Two-plane model + Mermaid diagrams
├── dashboard.mdx                      # Scenario-based dashboard guide
├── playbooks.mdx                      # Six operator runbooks
├── kill-switch.mdx                    # Four mechanisms + Mermaid decision tree
├── security.mdx                       # Threat model and mitigations
├── api-reference.mdx                  # Lean HTTP-level reference
├── compliance.mdx                     # OWASP / EU AI Act / NIST mapping
├── trust-center.mdx                   # Security posture + roadmap
├── subprocessors.mdx                  # Vendor list — VERIFY THESE
└── vulnerability-disclosure.mdx       # Security disclosure policy

providers/
├── overview.mdx
├── conventions.mdx
├── action-class-mapping.mdx
├── custom.mdx
├── llms/
│   ├── openai.mdx                     # ★ Reference — full AccordionGroup pattern
│   ├── anthropic.mdx
│   ├── google.mdx
│   ├── xai.mdx
│   ├── groq.mdx
│   ├── openrouter.mdx
│   ├── mistral.mdx
│   ├── perplexity.mdx
│   ├── cohere.mdx
│   └── fireworks.mdx
├── actions/   (19 pages: gmail, google-calendar, drive, slack, discord, github,
│              notion, linear, jira, asana, stripe, twilio, sendgrid, resend,
│              supabase, airtable, plaid, calendly, bluesky)
├── tools/     (8 pages: tavily, exa, firecrawl, browserbase, e2b, apify,
│              serpapi, pinecone)
├── media/     (9 pages: x, replicate, fal, stability, elevenlabs, cartesia,
│              deepgram, huggingface, together)
├── frameworks/(4 pages: langchain, crewai, autogpt, openclaw)
└── wallets/   (4 pages: metamask, coinbase, rainbow, walletconnect)
```

**Total content pages: 75** (21 top-level + 54 provider + 4 admin pages in providers/) — `sdk.mdx` removed 2026-05-12

## David must personally verify before public ship

The Trust Center and Subprocessor list went from `[VERIFY — ...]` placeholders to filled-in values. **Every one is a factual claim a security buyer will rely on.** Confirm each before the docs go on `docs.agentroot.app`:

### In `subprocessors.mdx`
- **Cloudflare logs region** — written as "primarily United States." Verify against your Cloudflare account.
- **HCP Vault region** — written as "US-East (AWS us-east-1)." Confirm against your actual HCP cluster region.
- **AWS as primary application host** — I named AWS explicitly with full cert list (SOC 1/2 Type II, ISO 27001/17/18, PCI DSS L1, FedRAMP High, HIPAA). If you're on Fly.io / Railway / GCP / Render / Vercel / etc, swap the vendor and update the cert list accordingly.
- **Resend as email vendor** — plausible (you list Resend in your own catalog) but verify it's actually wired up.
- **Sentry as error monitoring** — verify or swap to whatever you actually use (Honeybadger, Bugsnag, Datadog, none).
- **No third-party product analytics** — verify you don't currently send data to PostHog / Mixpanel / Plausible / GA.

### In `trust-center.mdx`
- **Status page URL `status.agentroot.app`** — I assumed this URL. Provision it (Statuspage, BetterStack, Better Uptime, self-hosted) or change the URL to where it actually lives.
- **Log retention 90 days** — confirm operational reality matches the documented policy.
- **Data residency US-East primary** — confirm.

### Other facts to spot-check
- **Provider list (54 launch services)** — confirm the catalog matches what you actually proxy on day one. If a provider in the catalog isn't actually wired up, mark it "Coming soon" via a Mintlify badge or remove the page.
- **`status.agentroot.app`, `proxy.agentroot.app`, `ux.agentroot.app`** — confirm all three subdomains are real.
- **EAS schema fields on the agent attestation** — `architecture.mdx` says the attestation includes "agent public key, operator wallet, schema-defined policy fields (per-tx caps, scope flags, sub-agent permissions), revocationTime." Match against your actual EAS schema.

## Outstanding decisions / open questions

1. **Provider icons.** 54 SVG files in `/logo/providers/` would let each provider page have a per-provider icon in the sidebar and on the page. None are in the repo yet. Each provider's brand assets are gettable but tedious. Could be a quarter of a Saturday.
2. **OpenAPI playground upgrade for top providers.** Round 5 in the original plan — replace the templated stubs for OpenAI, Anthropic, Stripe, Slack, GitHub with full OpenAPI spec integration so Mintlify renders the interactive "Try It" playground. Substantial work (per-spec auth swap, server URL rewrite, ongoing maintenance as upstream APIs evolve). Deferred until launch traffic justifies the cost.
3. **`security.mdx` voice match.** Round 2 covered architecture/dashboard/kill-switch/sdk. `security.mdx` was deemed acceptable as-is in the Round 1 review ("compliance/trust-center/subprocessors/vulnerability-disclosure are good — don't touch") but the same standard wasn't applied to security.mdx, which has some overlap with architecture.mdx now. Worth a re-read.
4. **Marketing site update.** "The Pinocchio Problem" is strong enough to become the marketing hero. Earlier session discussion proposed: hero replacement on agentroot.app, foundational blog post titled "The Pinocchio Problem: A Field Guide to Confused-Deputy Attacks in Agentic Systems," sales-deck cold open. Not docs work — repo to update is whichever holds the marketing site (not `agentroot-docs`).
5. **Provider stubs → full endpoint catalogs.** Each non-OpenAI provider page currently has one representative endpoint. Filling each with the full upstream endpoint catalog is mechanical work — best done one category at a time, possibly with help from a research-oriented sub-agent that pulls each upstream's API docs and templates the accordions.

## To ship the current state

1. **Read** `subprocessors.mdx` and `trust-center.mdx` in full. Fix anything that doesn't match reality.
2. **Open GitHub Desktop.** You'll see ~80 changed files across rounds 1-4 + this polish pass.
3. **Commit and push.** Suggested message: `"Initial docs structure: Pinocchio Problem, lifecycle, providers catalog (54), playbooks"`
4. **Watch the Mintlify dashboard.** Build should succeed within 30 seconds.
5. **Visit `vitrotechnology.mintlify.app`.** Click through Providers → LLMs → OpenAI to see the reference accordion pattern. Click through to a stub provider (Slack, Stripe) to see the templated pattern.
6. **Set up the custom domain `docs.agentroot.app`** (Mintlify dashboard → Settings → Domain setup → enter URL → add CNAME at your DNS provider → verify).

## What to do in the next session

Pick from:

**Highest value:** Fact-check & ship. Read the trust-center and subprocessor pages, fix anything wrong, push, set up custom domain, then take a screenshot pass through every page and note what needs visual or structural follow-up.

**Medium value, mechanical:** Provider stubs → fuller endpoint catalogs. Pick one category (e.g., Actions — Slack, Stripe, GitHub, Gmail are the most-clicked) and convert each stub to the full AccordionGroup pattern that OpenAI uses. ~30 minutes per provider.

**High value, larger lift:** Round 5 OpenAPI playground integration for OpenAI + Anthropic. Source modified OpenAPI specs (server URL rewritten to `proxy.agentroot.app/<provider>`, auth scheme swapped to AgentRoot Bearer + X-AgentRoot-ID), drop them in `/openapi/`, point `docs.json` at them. Mintlify auto-generates per-endpoint pages with "Try It" buttons.

**Polish:** Provider icons. Grab each upstream's brand SVG, drop in `/logo/providers/<slug>.svg`, add `icon` field to each provider page frontmatter. Visual upgrade across the whole catalog.

## Conventions

- **Markdown is the default.** No PDF / DOCX / PPTX exports unless explicitly requested.
- **GitHub is manual.** AI writes files, David commits and pushes from GitHub Desktop.
- **No emojis in files** unless David asks.
- **Slack is read-only.** Drafts go to chat for David to copy.
- See `/Users/dhg/Documents/GitHub/ai-infra/CLAUDE.md` for the canonical session protocol.

---

*Last updated: 2026-05-12 (post SDK-removal sweep). Maintained by the AI helping David with the docs build. When you (the next session's AI) update this, replace this file with your own write-up of what changed.*
