<p align="center">
  <img src="assets/logo.svg" alt="Narrate" width="108" height="108" />
</p>

<h1 align="center">Narrate</h1>

<h3 align="center">Turn silent tool execution into concise, real-time step-by-step updates.</h3>

<p align="center">
  Stop staring at a silent spinner while your AI coding agent chains 15 tool calls in a row.<br/>
  <strong>Narrate</strong> makes your agent report key steps, intermediate findings, and next actions in 1–2 crisp sentences as it works—so you can follow its thought process in real time and steer early when needed.
</p>

<p align="center">
  <a href="#quick-start"><strong>Quick Start</strong></a> &nbsp;·&nbsp;
  <a href="#before-vs-after"><strong>Before vs. After</strong></a> &nbsp;·&nbsp;
  <a href="#how-it-works"><strong>How It Works</strong></a> &nbsp;·&nbsp;
  <a href="#always-on-mode"><strong>Always-On Rule</strong></a> &nbsp;·&nbsp;
  <a href="./README.md"><strong>English</strong></a> &nbsp;·&nbsp;
  <a href="./README_ZH.md"><strong>简体中文</strong></a>
</p>

<p align="center">
  <a href="https://github.com/starsrc/narrate/stargazers"><img src="https://img.shields.io/github/stars/starsrc/narrate?style=flat-square&amp;color=E5B650&amp;logo=github&amp;label=Stars" alt="GitHub stars" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-22c55e?style=flat-square" alt="MIT License" /></a>
  <a href="#quick-start"><img src="https://img.shields.io/badge/works_with-Claude_Code_%7C_Cursor_%7C_Codex_%7C_Antigravity_%7C_OpenCode-6366F1?style=flat-square" alt="Works with Claude Code, Cursor, Codex, Antigravity, and OpenCode" /></a>
  <a href="plugin.json"><img src="https://img.shields.io/badge/version-1.0.0-0891b2?style=flat-square" alt="Version 1.0.0" /></a>
</p>

---

<a id="quick-start"></a>

## Quick Start

### 1. Install

#### Option A — Universal Agent Skills CLI (`skills.sh`)

Works across **Cursor**, **Claude Code**, **Codex CLI**, **Google Antigravity**, and **OpenCode**:

```bash
npx skills add starsrc/narrate -g
```

#### Option B — Google Antigravity Native Plugin (`agy`)

Install directly as a native Antigravity plugin bundle:

```bash
agy plugin install https://github.com/starsrc/narrate
```

<details>
<summary>More installation options (Single-agent, Project-scoped, or One-off use)</summary>

Install non-interactively to a specific agent (e.g., Cursor or Claude Code):

```bash
npx -y skills add starsrc/narrate --skill narrate --agent cursor --global --copy --yes
npx -y skills add starsrc/narrate --skill narrate --agent claude-code --global --copy --yes
```

Install into the current workspace only (omit `-g`):

```bash
npx skills add starsrc/narrate
```

Try once without installing:

```bash
npx skills use starsrc/narrate@narrate --agent codex
```

</details>

### 2. Use `/narrate` in Any Task

Prepend `/narrate` (or mention `narrate`) to any everyday coding prompt—feature implementation, bug diagnosis, or refactoring:

```text
/narrate Implement Redis-backed sliding-window rate limiting for the upload endpoint and add unit tests.
```

```text
/narrate Diagnose why the authentication middleware drops the session cookie on token refresh.
```

```text
/narrate Refactor the synchronous CSV export endpoint into an async background job with progress polling.
```

---

<a id="before-vs-after"></a>

## Before vs. After

| Without `narrate` (Silent Black Box) | With `narrate` (Real-Time Step-by-Step Updates) |
| :--- | :--- |
| `Read auth/middleware.ts`<br/>`Grep "refreshToken"`<br/>`Read auth/session.ts`<br/>`Read config/cookies.ts`<br/>`Run npm test`<br/>`Edit auth/session.ts`<br/>`Run npm test`<br/><br/>*(2 minutes of silence before a wall of text)* | **Agent:** Checking `auth/middleware.ts` and `auth/session.ts` to trace how `Set-Cookie` headers are attached during token refresh.<br/>`Read auth/middleware.ts` · `Read auth/session.ts`<br/><br/>**Agent:** Found the issue: `rotateRefreshToken()` overwrites `res.headers` instead of appending to `Set-Cookie`. Patching `auth/session.ts` and running the auth test suite to verify.<br/>`Edit auth/session.ts` · `Run npm test` |

---

<a id="how-it-works"></a>

## How It Works

Modern agentic models are post-trained with **interleaved tool-use preambles**, but default system prompts bias them toward silent tool chaining until the final response.

`narrate` activates structured, low-noise verbal updates governed by four rules:

1. **Interleaved Tool Preambles**: Emits a 1–2 sentence status update in the same turn as tool calls (`read_file`, `grep`, `edit_file`, `run_command`).
2. **Finding + Next Action**: Every update states **(a)** what was just discovered or verified and **(b)** what the next tool batch will inspect or change.
3. **Anti-Spam Batching**: Parallel lookups in a single turn are grouped under one concise update instead of narrating every file read individually.
4. **Zero Redundant Echo**: Never re-summarizes rendered artifacts or diffs that are already visible in the UI.

---

<a id="always-on-mode"></a>

## Optional: Always-On Mode (Global Rule)

By default, agent skills are triggered on demand or when you type `/narrate`. If you want your agent to **automatically invoke and follow the `narrate` skill on every task** without typing `/narrate` each time, add this pointer to your global `CLAUDE.md`, `AGENTS.md`, or `~/.gemini/GEMINI.md`:

```markdown
- Always activate and follow the `narrate` skill whenever executing multi-step tasks, researching code, debugging, or making changes.
```

---

## Repository Structure

```text
narrate/
├── plugin.json               # Google Antigravity native plugin manifest
├── assets/
│   └── logo.svg              # Plugin & repository vector icon
├── skills/
│   └── narrate/
│       └── SKILL.md          # Agent Skill definition (skills.sh & Antigravity compatible)
├── README.md                 # Documentation (English)
├── README_ZH.md              # Documentation (Simplified Chinese)
└── LICENSE                   # MIT License
```

## License

[MIT](LICENSE)
