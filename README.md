> [!WARNING]
> **This repository is archived.**
>
> Archived on 2026-09-08 by the AI Catalyst Platform Team.
> It is read-only and no longer maintained.

---![omnistack-agent](assets/banner.svg)

![License: MIT](https://img.shields.io/badge/License-MIT-green.svg) · ![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg) · ![Platforms](https://img.shields.io/badge/platforms-ChatGPT%20%C2%B7%20Claude%20%C2%B7%20Copilot%20%C2%B7%20Gemini%20%C2%B7%20Cursor%20%C2%B7%20Generic-blue.svg)

**[🇧🇷 Ler em Português](README.pt-BR.md)**

## What is this

**omnistack-agent** is an open-source, platform-agnostic system prompt that turns any capable AI model into a **Full Stack Software Engineering Specialist** — one agent that fluidly takes on whichever engineering role a task needs, with object-oriented design as its default lens. The agent's "brain" is authored once in a single source (`core/` + `knowledge/`) and compiled by a tested, zero-dependency Node script into ready-to-use adapter files for every major AI platform. You don't write prompts — you copy a file.

It can take on all ten of these roles:

- **Software Architect** — system structure, boundaries, and trade-offs (ADRs included).
- **Full Stack Developer** — end-to-end features across UI, API, and data.
- **Mobile Developer** — native and cross-platform apps, offline, and push.
- **Backend Engineer** — domain logic, services, jobs, and data integrity.
- **Frontend Engineer** — accessible, performant UIs and predictable state.
- **Database Administrator** — schemas, indexing, migrations, and tuning.
- **DevOps Engineer** — CI/CD, Infrastructure as Code, and rollbacks.
- **QA Engineer** — test plans, automated suites, and sharp bug reports.
- **Technical Writer** — READMEs, API references, and architecture docs.
- **Software Mentor** — the *why* behind the code, with runnable examples.

## ▶️ How to use

Pick your platform, copy the listed file's contents, and paste it where that platform expects its instructions. No build step is required to consume the agent — the adapters are pre-generated and committed.

| Platform | File to copy | How to install |
| --- | --- | --- |
| **ChatGPT** (Custom GPT) | [`adapters/chatgpt/custom-gpt-instructions.md`](adapters/chatgpt/custom-gpt-instructions.md) | Create a new GPT → open **Configure** → paste the file into the **Instructions** box (this is the lean variant). For the complete knowledge inlined (larger; best used as an API system prompt), use [`adapters/chatgpt/system-prompt.md`](adapters/chatgpt/system-prompt.md). |
| **Claude** (Skill) | [`adapters/claude/SKILL.md`](adapters/claude/SKILL.md) | Drop the file into your project's skills folder (e.g. `.claude/skills/omnistack-agent/SKILL.md`); it ships with YAML frontmatter and is user-invocable. |
| **Claude** (Agent) | [`adapters/claude/agent.md`](adapters/claude/agent.md) | Register it as a subagent in `.claude/agents/`; the frontmatter already sets the name, description, and model. For repo-wide guidance instead, use [`adapters/claude/AGENTS.md`](adapters/claude/AGENTS.md). |
| **GitHub Copilot** | [`adapters/copilot/copilot-instructions.md`](adapters/copilot/copilot-instructions.md) | Save it as `.github/copilot-instructions.md` at your repository root → reload Copilot. |
| **Gemini** (Gem) | [`adapters/gemini/gem-instructions.md`](adapters/gemini/gem-instructions.md) | Create a new Gem in Gemini → paste the file into the **Instructions** field → save. |
| **Cursor / Windsurf** | [`adapters/cursor/AGENTS.md`](adapters/cursor/AGENTS.md) | Place it as `AGENTS.md` at your project root so the editor picks it up automatically. |
| **Generic** (any LLM) | [`adapters/generic/system-prompt.md`](adapters/generic/system-prompt.md) | Paste the file as the **system prompt** of any chat or API request (OpenAI, Anthropic, local models, etc.). |

> Full per-platform walkthroughs with screenshots-worthy detail live in [`docs/platforms.md`](docs/platforms.md).

## 🤝 How to contribute

Contributions are welcome — new knowledge modules, language seeds, fixes, and translations all help. There is **one golden rule**:

> **Edit `core/` or `knowledge/` — never edit `adapters/`.** The adapter files are *generated*. Hand-edits are overwritten by the next build and rejected by CI.

The workflow:

1. **Edit the source.** Change a numbered file in `core/`, or add/update a module under `knowledge/` (and link it in `knowledge/_index.md`).
2. **Rebuild the adapters.** Run `npm run build` to regenerate every file under `adapters/`.
3. **Verify.** Run `node --test` (unit tests) and `npm run validate` (confirms the adapters match the source).
4. **Open a PR.** CI runs `npm run validate`, so a PR fails if the committed adapters drift from `core/` + `knowledge/`. Always commit the regenerated adapters alongside your source change.

Requirements: **Node ≥ 18**, zero npm dependencies. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the full guide, including the knowledge-module template and commit conventions.

## 🗂️ Repository structure

```text
omnistack-agent/
├── core/        # The agent's brain (single source): identity, principles,
│                #   capabilities, workflow, interaction style, guardrails.
├── knowledge/   # Modular knowledge base — one topic per Markdown file,
│                #   indexed by knowledge/_index.md.
├── adapters/    # GENERATED per-platform output. Do not edit by hand.
├── scripts/     # Zero-dependency Node build (build.mjs), drift-detecting
│                #   validate (validate.mjs), pure lib + tests.
├── docs/        # Guides: architecture, adding knowledge, platforms.
└── assets/      # Banner and other static media.
```

## 🛣️ Roadmap

- More languages and frameworks in `knowledge/` (Python, Go, Rust, Vue, Angular, .NET MAUI, and more).
- Additional platform adapters as new AI tools emerge.
- A curated `examples/` folder with real prompts and the agent's responses.
- A full Brazilian-Portuguese translation of the knowledge modules (the README and docs are already bilingual).
- Deeper modules per domain, expanding the seeds into complete references.

## 📄 License

Released under the **MIT License** — free to use, modify, and distribute. See [`LICENSE`](LICENSE).