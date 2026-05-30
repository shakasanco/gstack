# FORK_NOTES — shakasanco/gstack

This is a **personal fork** of [`garrytan/gstack`](https://github.com/garrytan/gstack),
maintained so that local customizations to gstack skills (e.g. `ship/SKILL.md.tmpl`)
**survive `/gstack-upgrade`**.

## Remote layout

In the install dir (`~/.claude/skills/gstack`):

| Remote     | URL                                      | Role                        |
| ---------- | ---------------------------------------- | --------------------------- |
| `origin`   | `https://github.com/shakasanco/gstack.git` | Our fork — source of truth  |
| `upstream` | `https://github.com/garrytan/gstack.git`   | Canonical gstack            |

`/gstack-upgrade` runs, inside this dir:

```bash
git stash
git fetch origin
git reset --hard origin/main   # ← resets to OUR fork, not upstream
./setup
```

## ⚠️ THE GOTCHA — read before every upgrade

Because the upgrade does `git reset --hard origin/main`, **the fork's `main` is the
source of truth**. The fork does **not** auto-track upstream. If you run
`/gstack-upgrade` without first syncing, the "upgrade" just resets you back to the
fork's (possibly stale) `main` and pulls in **zero** new upstream versions.

**Always sync the fork with upstream BEFORE running `/gstack-upgrade`:**

```bash
cd ~/.claude/skills/gstack
git fetch upstream
git merge upstream/main          # resolve conflicts in any files we edited
git push origin main
# only now run /gstack-upgrade
```

## Editing skills

- Edit the **`*.SKILL.md.tmpl`** template (or the relevant `scripts/resolvers/*`),
  **never** the generated `SKILL.md` — `./setup` regenerates `SKILL.md` from the
  template and will overwrite direct edits.
- After editing a template, run `./setup` to regenerate, then commit **both** the
  `.tmpl` and the regenerated `SKILL.md`, and push to `origin` (the fork).
