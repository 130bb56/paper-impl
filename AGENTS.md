# Repository instructions

This repository tracks paper implementations as independently versioned Git
submodules. Keep the root repository as a lightweight index; implementation code
belongs in each paper's fork and submodule.

## Current implementation

- Paper: MicroMix
- Submodule: `MicroMix/`
- Fork: `https://github.com/130bb56/MicroMix.git`
- Working branch: `micromix`
- Upstream implementation: `https://github.com/lwy2020/MicroMix.git`

## Working with a paper implementation

Before editing MicroMix, enter the submodule and attach HEAD to its working branch:

```bash
cd MicroMix
git switch micromix
git submodule update --init --recursive
```

Commit implementation changes inside `MicroMix` first. Push those changes to the
fork's working branch. Then return to the root repository and commit only the
updated submodule pointer and root-level metadata.

```bash
cd MicroMix
git add <changed-files>
git commit -m "<implementation change>"
git push origin micromix

cd ..
git add MicroMix
git commit -m "Update MicroMix submodule revision"
```

Do not vendor a submodule's source into the root repository, delete its `.git`
metadata, or flatten nested repositories. Do not commit generated model files,
benchmark outputs, build products, temporary binaries, or local scratch files.

## Remotes and upstream changes

- `origin` is the writable fork.
- `upstream` is the official repository and must not be pushed to.
- Fetch and integrate upstream changes inside the submodule, push the result to
  the fork, and then update the root submodule pointer.

## Documentation

- Keep `README.md` as the compact paper status table requested by the user.
- Update its Branch, Status, Kernels, and Test cells when the pinned implementation
  changes.
- Use a single `branch@short-commit` link in the Branch cell.
- Keep detailed agent-facing repository procedures in this file rather than
  expanding the README.

## Local state

Preserve unrelated user changes in both the root repository and submodules. Check
`git status` in both scopes before staging or committing. Never commit or remove
untracked files without first determining whether they are user work or generated
artifacts.
