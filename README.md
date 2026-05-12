# AgentRoot Documentation

Public documentation for [AgentRoot](https://agentroot.app) — the agentic root of trust. Onchain identity, proxied API keys, sovereign kill switch.

This repository is the source for [docs.agentroot.app](https://docs.agentroot.app), rendered by Mintlify and auto-deployed on every push to `main`.

## Structure

- `mint.json` — Mintlify configuration: navigation, branding, top-bar links, SEO metadata
- `*.mdx` — one file per documentation page; the URL slug matches the filename
- `logo/`, `favicon.svg`, `background.png` — brand assets referenced from `mint.json`

## Contributing

Typos, broken links, unclear explanations, missing edge cases — please open a PR. For larger structural changes (new pages, navigation reshuffles, restructuring a section), please open an issue first so we can discuss before you invest the time.

### Local preview

Install the Mintlify CLI and run from the repo root:

```bash
npm install -g mintlify
mintlify dev
```

This serves a live-reloading preview at `http://localhost:3000`.

### Deploy

Pushes to `main` auto-deploy via Mintlify's GitHub integration. There is no manual deploy step. PR previews are also generated automatically.

## Contact

- **Security disclosures:** security@agentroot.com — see [Vulnerability Disclosure Policy](https://docs.agentroot.app/vulnerability-disclosure)
- **Trust and compliance questions:** trust@agentroot.com
- **Documentation feedback:** open an issue on this repo

## License

License terms for this documentation are being finalized. Until a license file is added to this repo, all rights are reserved by Vitro Technology. If you want to redistribute or reuse content before then, reach out at trust@agentroot.com.
