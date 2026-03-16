---
name: finishing-a-development-branch
description: Use when implementation is complete, fresh verification evidence exists, and you need to decide how to integrate the work - guides completion of development work by presenting structured options for merge, PR, or cleanup
---

# Finishing a Development Branch

## Overview

Guide completion of development work by presenting clear options and handling chosen workflow.

**Core principle:** Verify fresh evidence and staged contents → Present options → Execute choice → Clean up.

**Announce at start:** "I'm using the finishing-a-development-branch skill to complete this work."

## The Process

### Step 1: Verify Fresh Evidence and Staged Contents

**Before presenting options, verify fresh evidence that matches the project:**

```bash
# Conventional software: run the relevant suite
npm test / cargo test / pytest / go test ./...

# RL / simulation projects: run the smallest meaningful runtime proof
python scripts/train.py <env> --env.scene.num-envs=<small-gpu-run> --agent.max-iterations=<small-run>
python scripts/play.py <env> --checkpoint_file=<checkpoint>
```

For RL or simulation projects, inspect reward, episode, termination, NaN, and divergence logs, then report the physical quantities or behaviors you observed during play.

Also verify commit hygiene:
```bash
git diff --cached --name-only
```

**If verification fails or local-only planning/test artifacts are staged:**
```
Verification incomplete or dirty staging detected. Must fix before completing:

[Show failing command output and/or staged local-only files]

Cannot proceed with merge/PR until fresh evidence is clean and local-only artifacts are out of git.
```

Stop. Don't proceed to Step 2.

**If evidence is clean and staging is clean:** Continue to Step 2.

### Step 2: Determine Base Branch

```bash
# Try common base branches
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

Or ask: "This branch split from main - is that correct?"

### Step 3: Present Options

Present exactly these 4 options:

```
Implementation complete. What would you like to do?

1. Merge back to <base-branch> locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

**Don't add explanation** - keep options concise.

### Step 4: Execute Choice

#### Option 1: Merge Locally

```bash
# Switch to base branch
git checkout <base-branch>

# Pull latest
git pull

# Merge feature branch
git merge <feature-branch>

# Re-run verification on merged result
<verification command>

# If verification passes
git branch -d <feature-branch>
```

Then: Cleanup worktree (Step 5)

#### Option 2: Push and Create PR

```bash
# Push branch
git push -u origin <feature-branch>

# Create PR
gh pr create --title "<title>" --body "$(cat <<'EOF'
## Summary
<2-3 bullets of what changed>

## Verification
- [ ] <verification steps>
EOF
)"
```

Keep worktree for follow-up changes.

#### Option 3: Keep As-Is

Report: "Keeping branch <name>. Worktree preserved at <path>."

**Don't cleanup worktree.**

#### Option 4: Discard

**Confirm first:**
```
This will permanently delete:
- Branch <name>
- All commits: <commit-list>
- Worktree at <path>

Type 'discard' to confirm.
```

Wait for exact confirmation.

If confirmed:
```bash
git checkout <base-branch>
git branch -D <feature-branch>
```

Then: Cleanup worktree (Step 5)

### Step 5: Cleanup Worktree

**For Options 1 and 4:**

Check if in worktree:
```bash
git worktree list | grep $(git branch --show-current)
```

If yes:
```bash
git worktree remove <worktree-path>
```

**For Options 2 and 3:** Keep worktree.

## Quick Reference

| Option | Merge | Push | Keep Worktree | Cleanup Branch |
|--------|-------|------|---------------|----------------|
| 1. Merge locally | ✓ | - | - | ✓ |
| 2. Create PR | - | ✓ | ✓ | - |
| 3. Keep as-is | - | - | ✓ | - |
| 4. Discard | - | - | - | ✓ (force) |

## Common Mistakes

**Skipping fresh verification**
- **Problem:** Merge broken code, create failing PR, or miss RL runtime regressions
- **Fix:** Always verify fresh evidence before offering options

**Staging local-only artifacts**
- **Problem:** Plan docs or formal test files end up in the commit by accident
- **Fix:** Check `git diff --cached --name-only` before merge or PR creation

**Open-ended questions**
- **Problem:** "What should I do next?" → ambiguous
- **Fix:** Present exactly 4 structured options

**Automatic worktree cleanup**
- **Problem:** Remove worktree when might need it (Option 2, 3)
- **Fix:** Only cleanup for Options 1 and 4

**No confirmation for discard**
- **Problem:** Accidentally delete work
- **Fix:** Require typed "discard" confirmation

## Red Flags

**Never:**
- Proceed with failing verification
- Merge without re-running verification on the result
- Leave local-only plan docs or formal test files staged for commit
- Delete work without confirmation
- Force-push without explicit request

**Always:**
- Verify fresh evidence before offering options
- Check staged contents before offering options
- Present exactly 4 options
- Get typed confirmation for Option 4
- Clean up worktree for Options 1 & 4 only

## Integration

**Called by:**
- **subagent-driven-development** (Step 7) - After all tasks complete
- **executing-plans** (Step 5) - After all batches complete

**Pairs with:**
- **using-git-worktrees** - Cleans up worktree created by that skill
