# Repository instructions

This repository is an index of paper implementations. Each paper implementation
must live in its own Git submodule at the repository root.

## Repository structure

- Treat every paper directory as an independent repository and submodule.
- Keep implementation source, history, dependencies, and build files inside that
  paper's submodule.
- Keep the root repository limited to submodule pointers and repository-wide
  metadata such as `README.md` and `.gitmodules`.
- Do not vendor or flatten submodule source into the root repository.

## Making changes

1. Check `git status` in both the root repository and the target submodule.
2. Make and commit implementation changes inside the target submodule.
3. Push the submodule commit to its configured writable fork and working branch.
4. Return to the root repository and commit the updated submodule pointer.
5. Update the corresponding `README.md` row when its repository, status, or
   implementation summary changes.

Never assume that every paper uses the same branch, remote, dependencies, build
commands, kernels, or tests. Inspect the target submodule before acting.

## README format

Keep `README.md` as a single table with these columns:

`Paper | Repository | Status | Info`

- Paper: short paper name linked to the paper.
- Repository: working branch name linked to that branch in its implementation fork.
- Status: only `In progress` or `Done`.
- Info: one concise sentence describing the implemented kernel or core mechanism.

Add one row per paper submodule. Do not add prose sections to the README.

## Safety

- Preserve unrelated user changes in the root repository and all submodules.
- Do not stage, commit, delete, or overwrite untracked files without determining
  whether they are user work or generated artifacts.
- Do not commit model weights, datasets, checkpoints, benchmark outputs, build
  products, virtual environments, temporary binaries, or scratch files.
- Never delete or move nested Git metadata to make a submodule look like a normal
  directory.
