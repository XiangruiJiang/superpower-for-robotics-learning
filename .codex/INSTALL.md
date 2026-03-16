# Installing Superpower for Robotics Learning for Codex

Enable Superpower for Robotics Learning skills in Codex via native skill discovery. Just clone and symlink.

## Prerequisites

- Git

## Installation

1. **Clone the Superpower for Robotics Learning repository:**
   ```bash
   git clone https://github.com/XiangruiJiang/superpower-for-robotics-learning.git ~/.codex/superpower-for-robotics-learning
   ```

2. **Create the skills symlink:**
   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/.codex/superpower-for-robotics-learning/skills ~/.agents/skills/superpowers
   ```

   **Windows (PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
   cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers" "$env:USERPROFILE\.codex\superpower-for-robotics-learning\skills"
   ```

3. **Restart Codex** (quit and relaunch the CLI) to discover the skills.

## Migrating from old bootstrap

If you installed Superpower for Robotics Learning before native skill discovery, you need to:

1. **Update the repo:**
   ```bash
   cd ~/.codex/superpower-for-robotics-learning && git pull
   ```

2. **Create the skills symlink** (step 2 above) — this is the new discovery mechanism.

3. **Remove the old bootstrap block** from `~/.codex/AGENTS.md` — any block referencing `superpowers-codex bootstrap` is no longer needed.

4. **Restart Codex.**

## Verify

```bash
ls -la ~/.agents/skills/superpowers
```

You should see a symlink (or junction on Windows) pointing to your Superpower for Robotics Learning skills directory (kept under the compatibility alias `superpowers`).

## Updating

```bash
cd ~/.codex/superpower-for-robotics-learning && git pull
```

Skills update instantly through the symlink.

## Uninstalling

```bash
rm ~/.agents/skills/superpowers
```

Optionally delete the clone: `rm -rf ~/.codex/superpower-for-robotics-learning`.
