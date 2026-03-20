<p align="center">
  <strong>kaliber</strong>
</p>

<h1 align="center">Claude Skills by Kaliber</h1>

<p align="center">
  Open-source Claude Code skills built by an AI-native marketing agency.<br>
  Practitioner-tested. Production-grade. Free to use.
</p>

<p align="center">
  <a href="https://kalibergroup.netlify.app/claude-skills">Website</a> · <a href="https://kalibergroup.netlify.app">About Kaliber</a> · <a href="https://apacmarketers.com">APAC Marketers</a>
</p>

---

## What is this?

Kaliber is a marketing group in Singapore that runs on Claude Code. These are the skills we use daily — ghostwriting, ad creation, data scraping, prompt refinement, knowledge management, and campaign auditing.

We're open-sourcing them because the best tools get better when more people use them.

## Skills

| Skill | What it does | Install |
|-------|-------------|---------|
| **[Kaliber Ghostwriter Pro](skills/ghostwriter-pro/)** | Long-form articles, LinkedIn posts, and short-form content in any voice. Interview → outline → draft workflow with research gap-filling. | `claude install-skill kaliberasia/claude-skills/skills/ghostwriter-pro` |
| **[Kaliber Ad Creator](skills/ad-creator/)** | 3-part static ad sequences (Seed → Personalise → Resolve) for Meta, TikTok, Instagram. Scroll-stopping creative without video. | `claude install-skill kaliberasia/claude-skills/skills/ad-creator` |
| **[Kaliber Instant Auditor](skills/instant-auditor/)** | Structured campaign diagnostics across performance, creative, intelligence, and infrastructure. Walk away with an action plan. | `claude install-skill kaliberasia/claude-skills/skills/instant-auditor` |
| **[Kaliber Prompt Perfector](skills/prompt-perfector/)** | Analyzes raw prompts, identifies gaps, matches optimal Claude Code components, and outputs a production-ready prompt. | `claude install-skill kaliberasia/claude-skills/skills/prompt-perfector` |
| **[Kaliber Auto-Scraper](skills/auto-scraper/)** | Fully automated data collection agent for any public source. Scrapes on schedule, enriches with AI, stores anywhere. Runs free on GitHub Actions. | `claude install-skill kaliberasia/claude-skills/skills/auto-scraper` |
| **[Kaliber Knowledge Vault](skills/knowledge-vault/)** | Captures session learnings into a structured knowledge hub. Extracts principles, processes, and gotchas from any working session. | `claude install-skill kaliberasia/claude-skills/skills/knowledge-vault` |

## Quick start

```bash
# Install any skill with one command
claude install-skill kaliberasia/claude-skills/skills/ghostwriter-pro
```

## How these are built

Every skill follows the same structure:

```
skills/ghostwriter-pro/
├── SKILL.md          # The skill definition (triggers, workflow, rules)
└── references/       # Supporting context (voice guides, frameworks, templates)
```

Skills are plain markdown files — no dependencies, no build step, no API keys needed. They run inside Claude Code and extend its capabilities through structured prompts and workflows.

## Contributing

Found a bug? Have an improvement? PRs welcome. 

If you build something cool with these skills, tell us — we feature community work on [our website](https://kalibergroup.netlify.app/claude-skills).

## About Kaliber

We're an AI-native marketing group in Singapore. Founded by ex-Google leadership, we combine the strategic depth of a consultancy with the execution speed of an agency — powered by proprietary AI infrastructure.

Every engagement starts with a diagnosis. Every recommendation is earned.

[Get a diagnosis →](https://kalibergroup.netlify.app/contact)

---

<p align="center"><sub>Built by <a href="https://kalibergroup.netlify.app">Kaliber</a> — Singapore's AI-native marketing group</sub></p>
