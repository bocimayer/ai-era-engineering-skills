# AI-Era Engineering Skills

Claude skills built on [The AI-Era Engineering Playbook](https://aieraengineering.com) — a practitioner framework for software engineering in the AI era.

Two skills, one for each side of the interview table:

| Skill | Audience | What it does |
|---|---|---|
| [`interview-prep`](skills/interview-prep/SKILL.md) | Engineers | Coaches you through scored practice drills for the three skills modern interviews actually test: specification quality, output evaluation, failure-mode reasoning. Drills use your own stack. |
| [`interview-designer`](skills/interview-designer/SKILL.md) | Hiring teams | Builds company-specific question sets, scorecards, and job descriptions under five hard generation rules; audits an existing process for interview drift. |

## Install

**Claude Code:** copy the skill folder(s) into your skills directory:

```bash
git clone https://github.com/bocimayer/ai-era-engineering-skills.git
cp -r ai-era-engineering-skills/skills/interview-prep ~/.claude/skills/
cp -r ai-era-engineering-skills/skills/interview-designer ~/.claude/skills/
```

Then invoke with `/interview-prep` or `/interview-designer`, or just describe what you want ("help me practice for an engineering interview" / "build an interview question set for my team").

**claude.ai:** upload the skill folder under Settings → Capabilities → Skills.

**Any other LLM:** the same instructions work as plain system prompts — copy-paste versions live at
[aieraengineering.com/ai](https://aieraengineering.com/ai/).

## MCP servers

This repo ships a project-scoped `.mcp.json` with:

- **`figma`** — Figma's remote MCP server (`https://mcp.figma.com/mcp`) for
  reading design context (components, variables, layout). Approve it when
  Claude Code prompts you; you'll go through Figma's OAuth flow on first use.
  See [Figma's MCP server docs](https://developers.figma.com/docs/figma-mcp-server/).
- **`playwright`** — the [Playwright MCP server](https://github.com/microsoft/playwright-mcp)
  (via `npx @playwright/mcp`), for driving a real browser to check anything
  built from a Figma design (a Sites export, a coded prototype, a component
  library preview) actually renders and behaves as designed.

## How it works

Both skills fetch the full methodology corpus from
[aieraengineering.com/llms-full.txt](https://aieraengineering.com/llms-full.txt)
at session start, so they always reflect the current published playbook. The
core rules are embedded in the skill files as offline fallback.

## License

The playbook content is © Gabor Mayer, licensed
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). These skill files
are free to use and adapt with attribution.
