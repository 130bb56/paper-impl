# paper-impl

A personal collection of research-paper implementations. Each paper is tracked as
an independently-forked, independently-versioned Git submodule — this repository
never vendors or copies implementation source code itself.

See [PAPERS.md](PAPERS.md) for the current list of papers, their status, and links.

## Architecture: fork + submodule

For every paper, the relationship is:

```
official repository  →  fork under 130bb56  →  submodule of paper-impl
```

For example:

```
mit-han-lab/llm-awq  →  130bb56/llm-awq  →  paper-impl/AWQ
```

Each paper directory (`AWQ/`, `SmoothQuant/`, `GPTQ/`, …) is a Git submodule
pointing at a fork under the `130bb56` GitHub account, not a copied or vendored
source tree. Inside each fork:

- `origin` points at the fork (`130bb56/<repo>`) — this is what gets pushed to.
- `upstream` points at the official repository — this is fetched from to pull in
  upstream changes, but never pushed to.

This preserves each upstream project's own history, structure, dependencies, and
build system untouched, while still allowing local modifications (bug fixes,
experiment scripts, dependency changes) to be committed and tracked per paper.

This repository (the superproject) contains only repository-wide management
information: the list of papers, links, implementation status, notes on local
modifications, and the exact submodule commit currently pinned for each paper. It
never contains paper implementation source code directly.

## Cloning

Clone with submodules in one step:

```
git clone --recurse-submodules https://github.com/130bb56/paper-impl.git
```

If you already cloned without `--recurse-submodules`, initialize them after the fact:

```
git submodule update --init --recursive
```

## Updating submodules to their latest pinned/remote commit

To pull each submodule's fork up to the latest commit on its configured branch:

```
git submodule update --remote
```

Review the resulting diff, then commit the updated submodule references as
described below.

## Adding a new paper

1. Identify the official/primary GitHub implementation repository for the paper.
2. Fork it into the `130bb56` account:
   ```
   gh repo fork <official-org>/<repo> --org 130bb56 --clone=false
   ```
   (or use the GitHub UI).
3. Add the fork as a submodule under the corresponding paper directory:
   ```
   git submodule add https://github.com/130bb56/<repo>.git <PaperName>
   ```
4. Inside the new submodule, configure `upstream` to point at the official repo:
   ```
   cd <PaperName>
   git remote add upstream <official-repo-url>
   cd ..
   ```
5. Commit the new submodule and update [PAPERS.md](PAPERS.md) with the paper's
   links, status, and pinned commit:
   ```
   git add <PaperName> .gitmodules PAPERS.md
   git commit -m "Add <PaperName> submodule"
   git push
   ```

## Modifying a paper implementation

All implementation-specific changes are committed **inside the fork** (the
submodule), never in the superproject directly. The superproject only ever commits
an updated submodule commit reference.

```
cd AWQ
git checkout <working-branch>

# modify files

git add .
git commit -m "<message>"
git push origin <working-branch>

cd ..
git add AWQ
git commit -m "Update AWQ submodule revision"
git push
```

## Tracking upstream changes

Each fork keeps `upstream` configured so official changes can be pulled in without
disturbing `origin`:

```
cd AWQ
git fetch upstream
git merge upstream/main   # or: git rebase upstream/main
git push origin <working-branch>

cd ..
git add AWQ
git commit -m "Sync AWQ with upstream"
git push
```

## Constraints this repository follows

- No paper algorithm is reimplemented from scratch here — every paper directory is
  a submodule pointing at a real fork of the official/primary implementation.
- Independent paper implementations are never merged into a shared source tree, and
  no common abstraction layer is introduced across them.
- Each upstream project's original structure, dependencies, build system, and Git
  history are preserved as-is inside its fork.
