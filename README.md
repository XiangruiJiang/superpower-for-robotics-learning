# Superpower for Robotics Learning

Superpower for Robotics Learning is a **Codex-first skill library** for robotics learning, reinforcement learning, and simulation-heavy development workflows.

It is based on the Superpowers workflow, but this repository is tailored for teams that need stronger discipline around:
- robotics / RL planning and review loops
- simulation and training verification instead of code-only claims
- keeping local planning artifacts out of git commits
- safer completion workflows before deployment or merge

## What makes this repo different

Compared with the original general-purpose Superpowers setup, this version is adapted for our robotics learning workflow:

- **Codex-focused installation**: the README only documents the Codex path we actually use.
- **RL / simulation verification**: skills explicitly require small training or rollout evidence, log inspection, and checkpoint play when that is the honest proof.
- **Local-only planning artifacts**: specs and plans are written to local memory paths instead of being committed by default.
- **Cleaner git hygiene**: workflows check staged contents so local-only plans or test artifacts do not slip into commits.
- **Safer completion rules**: finalization emphasizes fresh verification evidence before claiming success.

## How it works

The workflow is built around a small set of cooperating skills:

1. **brainstorming** — refine the task and confirm the design before implementation.
2. **writing-plans** — turn the approved design into small executable steps.
3. **subagent-driven-development** / **executing-plans** — carry out the plan with review checkpoints.
4. **test-driven-development** — require a failing check before code changes.
5. **systematic-debugging** — debug with a structured root-cause process instead of guessing.
6. **verification-before-completion** — require fresh evidence before any success claim.
7. **finishing-a-development-branch** — finish work only after verification and clean staging.

For robotics learning tasks, the intended proof is often not a unit test alone. This repo biases the workflow toward short train/rollout checks, runtime metrics, and observed behavior when those are the honest validation path.

## Installation for Codex

### 1. Clone the repository

```bash
git clone https://github.com/XiangruiJiang/superpower-for-robotics-learning.git ~/.codex/superpower-for-robotics-learning
```

### 2. Expose the skills to Codex

For compatibility with the existing skill namespace, keep the local alias name `superpowers`:

```bash
mkdir -p ~/.agents/skills
ln -s ~/.codex/superpower-for-robotics-learning/skills ~/.agents/skills/superpowers
```

### 3. Restart Codex

Quit and relaunch Codex so it re-discovers the skill directory.

## Verify installation

```bash
ls -la ~/.agents/skills/superpowers
```

You should see a symlink pointing to:

```text
~/.codex/superpower-for-robotics-learning/skills
```

Then start a new Codex session and ask it to do something that should trigger a skill, such as:
- "help me plan this feature"
- "use systematic-debugging on this failure"
- "execute the implementation plan at ..."

## Updating

```bash
cd ~/.codex/superpower-for-robotics-learning && git pull
```

Because Codex reads through the symlink, updates take effect for new sessions after restart.

## Repository layout

- `skills/` — the skill library itself
- `agents/` — reviewer / helper agent prompts
- `commands/` — command shims and compatibility helpers
- `docs/` — supporting documentation
- `.codex/INSTALL.md` — Codex installation instructions

## Notes

- The public repository name is **Superpower for Robotics Learning**.
- The local Codex skill alias remains **superpowers** for compatibility with existing references.
- This repository is intended for **Codex usage first**, not as a cross-platform marketplace package.

## License

MIT License - see `LICENSE`.
