# Agents

<p align="center"><strong>English</strong> · <a href="README.zh-CN.md">简体中文</a></p>

A collection of portable, domain-focused AI agents managed by
**[kman](https://github.com/unliftedq/kman)**. Each agent keeps its persona,
skills, tools, and permissions in its own directory, so work can be routed to
the right specialist instead of being pushed through an ever-growing generalist
assistant.

## Why separate agents matter

Most agent setups put every skill into one shared space. A specific task may
only need a few of them, but it still carries dozens of unrelated capabilities:
a video agent drags around coding skills, while a coding agent drags around
slide-deck skills. Those extras sit in context, compete for attention, increase
cost, and make it easier for the model to choose the wrong tool or method.

kman's premise is simple: **a task does not need an assistant with every ability
bolted on. It needs the specialist that actually understands the domain.**

- Video work belongs with a motion and rendering expert.
- Engineering work belongs with an agent that understands development,
  debugging, and testing.
- Interface polish belongs with an agent that cares about visual design,
  interaction, and user experience.

When a project crosses domains, the answer still is not to rebuild one giant
agent. The better pattern is orchestration: break the request into meaningful
pieces, route each piece to the right expert, then bring the results back
together.

## The kman agent boundary

In many tools, an "agent" is closer to a profile: a different system prompt and
maybe a different name, while the underlying skills, tools, and permissions are
still shared. kman treats an agent as a stronger boundary:

| Profile-style "agent" | kman agent |
|-----------------------|------------|
| Every profile shares the same skill space | Each agent loads only its own domain skills |
| Tools are globally available to every profile | Tools are exposed to the agents that need them |
| Permissions are effectively shared | Permissions can be defined per agent |
| Mostly differentiated by a system prompt | Defined by persona, skills, tools, and permissions |
| Tasks carry lots of irrelevant context | Tasks run against a focused specialist |

The result is not an assistant with add-ons. It is a set of isolated,
composable, versionable domain experts.

## Anatomy of an agent

Each agent is a complete, portable folder:

```text
<agent>/
  agent.toml      # name, description, runtime, soul reference, and defaults
  soul.md         # the agent's persona / system prompt
  mcp.json        # MCP server configuration, if any
  skills/         # this agent's domain skills, each containing SKILL.md
  hooks/          # hooks owned by this agent
  scripts/        # helper scripts owned by this agent
```

The core files do the following:

- **`agent.toml`** defines what the agent is, when to use it, its default
  runtime, permission mode, and where its soul file lives.
- **`soul.md`** is the agent's persona and system prompt.
- **`skills/`** contains the domain knowledge, references, templates, and
  scripts that belong only to this agent.
- **`mcp.json`** declares MCP servers the agent should connect to.

## Adding a new agent

Start by making the domain clear. The more precise the boundary, the easier it
is for the agent to stay focused.

1. Define the agent's domain and usage boundary.
2. Create a folder named after the agent.
3. Add an `agent.toml` manifest.
4. Write a `soul.md` that describes its persona, working style, and constraints.
5. Add a `skills/` folder with only the skills this domain actually needs.
6. Add `mcp.json`, `hooks/`, and `scripts/` when the agent needs them.

You can also create one from the CLI:

```bash
kman agent create "some-agent" \
  --runtime copilot-cli \
  --description "description of the agent" \
  --soul "soul of the agent"
```

## Using an agent

Delegate a one-off task to an agent:

```bash
kman -a superpowers run --task "fix the failing tests"
```

Start an interactive session:

```bash
kman -a superpowers chat
```

Learn more about kman at **[unliftedq/kman](https://github.com/unliftedq/kman)**.
