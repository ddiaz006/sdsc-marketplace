# SDSC Marketplace

Agent skills and plugins for systems run by the San Diego Supercomputer Center. A skill is a folder of
instructions, references and templates that a coding agent (Claude Code, OpenCode, Codex and others) loads when a
task needs it. The aim is twofold:

- **For users:** an agent that already knows the system's access steps, policies, storage and GPU names, so a plain
  request ("run this on a GPU") turns into a correct, policy-compliant workload.
- **For operators:** agents that follow the same rules the admins would teach a new user, so they stop creating the
  patterns that hurt a shared system (huge ConfigMaps, idle GPUs, floods of Jobs, `sleep infinity`).

Skills follow the [Agent Skills specification](https://agentskills.io/specification).

## Plugins

| Plugin | System | Skills |
|---|---|---|
| [`nrp-nautilus`](plugins/nrp-nautilus) | National Research Platform, Nautilus Kubernetes cluster | `nrp` (access, policies, GPUs, storage, managed LLMs), `kubernetes` (Jobs, Indexed Jobs, data, troubleshooting) |

Other SDSC systems (Expanse, Voyager, ...) are not covered yet.

## Install

### Claude Code

From a local clone (this repository is not published yet):

```bash
claude plugin marketplace add /path/to/sdsc-marketplace
claude plugin install nrp-nautilus@sdsc-marketplace
claude plugin list                     # nrp-nautilus should show "enabled"
```

Start a new conversation afterwards; skills load when a conversation starts. `/skills` lists them.

### OpenCode

OpenCode reads skills from `.opencode/skills/` in a project, or `~/.config/opencode/skills/` for every project.
Link the plugin's skills in:

```bash
mkdir -p ~/.config/opencode/skills
ln -s /path/to/sdsc-marketplace/plugins/nrp-nautilus/skills/nrp        ~/.config/opencode/skills/nrp
ln -s /path/to/sdsc-marketplace/plugins/nrp-nautilus/skills/kubernetes ~/.config/opencode/skills/kubernetes
```

### Other agents

Symlink a plugin's `skills/` directory into the agent's skill path, for example
`ln -s $PWD/plugins/nrp-nautilus/skills ~/.agents/skills/nrp-nautilus`.

## Check

```bash
python3 scripts/check.py      # validates manifests and skill frontmatter; standard library only
```

## Credits and license

Apache-2.0. See [CREDITS.md](CREDITS.md) for where each skill came from.
