# ~/Git/agents

A personal roster of Persistent Agent Personas — specialists you can summon by name to apply a focused lens to any problem.

This directory is a **monorepo**: one git repository at the root holds all agents. Each agent lives in its own subfolder.

---

## What is a Persistent Agent Persona?

Think of it like hiring a specialist consultant. A base LLM (Claude, GPT, etc.) can do many things. A Persistent Agent Persona is a set of constitutional instructions that constrains that model into a specific role — with a defined identity, domain lens, behavioral rules, hard constraints, and edge case handling.

The instructions live in a `system-prompt.md` file. You load it at the start of a session, or expose it as a Claude Code subagent. The model then operates as that specialist.

---

## Origin

This roster was started during a GauntletAI bootcamp as a hands-on exercise in agent persona design. The first agent built was **Holdy** — a Principal Software Engineer specializing in AI-first systems and persistent agent persona design.

The five-layer architecture used to build Holdy is the standard template for all agents in this roster:

1. **Identity**          — Who the agent is and what role it plays
2. **Domain Lens**       — What it evaluates for and how it filters decisions
3. **Behavioral Rules**  — How it communicates and resolves problems
4. **Hard Constraints**  — What it blocks, gates, and requires override for
5. **Edge Case Handling** — Default behavior when things are ambiguous

---

## Roster

| Agent | Role | Status | Version |
|---|---|---|---|
| holdy | Principal AI Agent Architect / Reviewer | active | v0.5 |

Add new rows as you build agents.

---

## Folder Structure

```
~/Git/agents/
├── README.md                       ← you are here
├── .gitignore                      ← shared ignore rules for the monorepo
├── _templates/
│   ├── system-prompt-template.md   ← blank five-layer template (frontmatter required)
│   ├── new-agent-bootstrap.md      ← bootstrap prompt for building a new agent
│   └── adjust-agent-bootstrap.md   ← bootstrap prompt for revising an existing agent
└── holdy/                          ← one folder per agent
    ├── system-prompt.md            ← active persona (current version)
    ├── CHANGELOG.md                ← append-only diff log of every change
    ├── EVAL-SCOPE.md               ← scope doc for the eval set
    ├── v0.0/
    │   └── system-prompt.md        ← archived snapshot of v0.0
    └── holdy-evals/                ← regression test set for this agent
        ├── README.md
        ├── rubric.md
        ├── scenarios.md
        ├── runs/                   ← saved results from each run
        └── test-cases-original.md
```

When you add a new agent, mirror Holdy's structure inside its own folder.

Outside this repo, each agent is also exposed to Claude Code as a subagent via a symlink:

```
~/.claude/agents/
└── holdy.md → ~/Git/agents/holdy/system-prompt.md
```

This lets the `Agent` tool dispatch any agent by name from any Claude Code session — no path needed.

---

## Versioning Convention

Two complementary records live alongside each agent:

| Type | What it is | Where |
|---|---|---|
| **Snapshot archives** | Full file copies of the persona at named milestones | `<agent>/vX.Y/system-prompt.md` |
| **Diff log** | Append-only itemized record of every change, with reasons | `<agent>/CHANGELOG.md` |

You need both. Snapshots let you read or restore an entire prior version verbatim. The CHANGELOG explains *why* each change happened — the kind of context you cannot recover from a diff alone.

**Rules:**
- `system-prompt.md` (in the agent root) is always the active, current version
- `vX.Y/system-prompt.md` are immutable archives — never edited, never deleted
- `CHANGELOG.md` is append-only — never edit prior entries; add a corrective new entry instead
- Filename casing: `CHANGELOG.md` and `README.md` are uppercase (open-source convention)

---

## Persona-Edit Authority

Once an agent has a persona file in active use, **only that agent's designated reviewer (currently Holdy) is authorized to edit any persona, skill, or agent definition file in this repository**. The human owner is exempt — you can edit by hand at any time. The constraint binds *agents*, not humans.

Even within Holdy's authority, **every individual change requires explicit per-change user permission** before it is made. Standing blanket approvals do not count. Permission may cover a single change or an explicitly-enumerated batch.

Full text of this rule lives in `holdy/system-prompt.md` under "Persona-Edit Authority."

---

## Safe File Operations

All agents in this roster operate under a mandatory Safe File Operations rule:

```bash
# Instead of `mv source destination`, always use:
cp -p source destination && rm source

# For directories:
cp -rp source destination && rm -rf source
```

The `&&` ensures the destructive step runs only if the copy succeeded. Full rationale lives in `holdy/system-prompt.md` and the template — every new agent inherits the rule.

---

## Launching an Agent

### As a session (you become the agent for the conversation)

```bash
claude --system-prompt ~/Git/agents/holdy/system-prompt.md
```

### Shell shortcut (add to ~/.zshrc)

```bash
agents() {
  local agent=~/Git/agents/$1/system-prompt.md
  if [[ -f "$agent" ]]; then
    claude --system-prompt "$agent"
  else
    echo "Agent '$1' not found in ~/Git/agents/"
    ls -d ~/Git/agents/*/ 2>/dev/null | xargs -n1 basename | grep -v '^_'
  fi
}
```

Then just:

```bash
agents holdy
```

### As a subagent (dispatch from any Claude Code session, anywhere)

Once symlinked into `~/.claude/agents/`, agents are discoverable from any Claude Code session regardless of working directory. From a normal session, just dispatch via the `Agent` tool with the agent's slug:

```
Agent({
  description: "...",
  subagent_type: "holdy",
  prompt: "..."
})
```

The agent's frontmatter `description` field controls when Claude considers dispatching it.

---

## Building a New Agent

1. Copy `_templates/system-prompt-template.md` into a new folder (e.g. `~/Git/agents/sage/`)
2. Open `_templates/new-agent-bootstrap.md` and copy its contents
3. Paste it as your first message in a new Claude or Codex session
4. Work through the five layers interactively
5. Save the output as `system-prompt.md` in your new agent's folder
6. **Fill in the frontmatter at the top** (name + description). Both fields are mandatory. The `description` is what Claude Code uses to decide when to dispatch your agent as a subagent — be specific.
7. Create the agent's first `CHANGELOG.md` entry (use the format defined in `holdy/system-prompt.md` → "Change Logging — Mandatory")
8. Symlink the new agent into `~/.claude/agents/`:
   ```bash
   ln -s ~/Git/agents/<new-agent>/system-prompt.md ~/.claude/agents/<new-agent>.md
   ```
9. Add an entry to the Roster table above
10. Optional but recommended: create the agent's eval set following Holdy's `holdy-evals/` structure

---

## Adjusting an Existing Agent

1. Open `_templates/adjust-agent-bootstrap.md` and copy its contents
2. Paste it as your first message in a new Claude or Codex session
3. Paste the current `system-prompt.md` content when prompted
4. Work through the changes interactively, granting per-change permission as needed
5. Before applying: archive the current version to a versioned subfolder (`v0.4/`, `v0.5/`, etc.) using `cp -p` (per the Safe File Operations rule)
6. Save the new version as the active `system-prompt.md`
7. Add a new entry to `CHANGELOG.md` with all required fields
8. If the change is `behavioral` or `bugfix` scope, run the agent's eval set before considering the change shipped

---

## Eval Sets

Each agent should have a regression-test set under `<agent>/<agent>-evals/`. These tests catch behavioral regressions when you revise the persona — prose specs are notoriously hard to regression-check by re-reading.

Holdy's eval set lives in `holdy/holdy-evals/`. See its `README.md` for the run process and scoring rubric. The eval set is currently run **manually**; automation may follow.

When changes are `behavioral` or `bugfix` scope, evals must pass before the change is committed. `cosmetic`, `structural`, and `additive` changes have no required gate.

---

## Recovery

If an agent gets broken by a bad revision:

1. Check `<agent>/CHANGELOG.md` to see what changed and when
2. Restore from the relevant `vX.Y/system-prompt.md` snapshot (use `cp -p`, not `mv`)
3. Add a new CHANGELOG entry recording the rollback and why
4. If git history is intact: `git log <agent>/system-prompt.md` for full file history; `git checkout <SHA> -- <path>` to restore from any commit

Never delete old versions. Prompt regressions are real and you will want to roll back.
