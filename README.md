<p align="center">
  <strong>kaliber / claude-skills</strong>
</p>

<p align="center">
  Open-source Claude Code skills from an AI-native marketing agency.
</p>

<p align="center">
  <a href="https://kalibergroup.netlify.app/claude-skills">Website</a> &middot;
  <a href="https://kaliber.group">About Kaliber</a> &middot;
  <a href="#install">Install</a>
</p>

---

## What this is

These are the actual Claude Code skills we use daily at [Kaliber](https://kaliber.group) — a specialist marketing group in Singapore. Not demos. Not templates. Production tools that run real campaigns, write real content, and manage real client work.

We're releasing them because we believe AI infrastructure should compound across the industry, not stay locked inside agencies.

## Skills

| Skill | What it does |
|-------|-------------|
| **ghostwriter** | Writes long-form articles, LinkedIn posts, and short-form content in a trained voice. Interview-first workflow — extracts thinking before writing. |
| **static-ad-sequencer** | Generates 3-part static ad sequences (Seed → Personalise → Resolve) for paid social. Visual direction + copy mechanics. Works across any category. |
| **lp-generate** | Builds high-converting landing pages from a URL. Scrapes brand data, extracts design system, optionally pulls Google Ads context, generates production HTML. |
| **prompt-optimizer** | Analyzes raw prompts, identifies gaps, and outputs optimized versions. Advisory only — improves the prompt, doesn't execute the task. |
| **data-scraper-agent** | Builds automated data collection agents for any public source. Runs on GitHub Actions, enriches with LLM, stores in Notion/Sheets/Supabase. Free. |
| **knowledge-capture** | Codifies session learnings into a structured knowledge hub. Extracts principles, processes, and gotchas through guided reflection. |

More skills shipping regularly. Watch this repo for updates.

## Install

Each skill is a self-contained folder. To install:

```bash
# Clone the repo
git clone https://github.com/kaliberasia/claude-skills.git

# Copy a skill into your project
cp -r claude-skills/skills/ghostwriter .claude/skills/ghostwriter
```

Or install a single skill directly:

```bash
# One-liner install
curl -sL https://raw.githubusercontent.com/kaliberasia/claude-skills/main/install.sh | bash -s ghostwriter
```

## How skills work

Claude Code skills are markdown files that extend Claude's capabilities within a project. When you type a trigger phrase (like "write an article" or "build a landing page"), the matching skill activates and guides Claude through a structured workflow.

Each skill contains:
- `SKILL.md` — The skill definition (triggers, workflow, constraints)
- `references/` — Supporting context (templates, examples, frameworks)

Skills live in your project's `.claude/skills/` directory and are automatically available in any Claude Code session.

## Built by

[Kaliber](https://kaliber.group) is an AI-native marketing group in Singapore. We combine the strategic depth of a consultancy with the execution speed of an agency — grounded in evidence-based marketing principles and built on proprietary AI infrastructure.

Every engagement starts with a diagnosis. Every recommendation is earned.

## License

MIT — use these however you want. Attribution appreciated but not required.
