# git-transform

Automation tool to copy and transform branch and tag refs from a source
repository to a destination repository. This can be useful to automatically
mirror/build public documentation from a private repository.

## Usage

git-transform is a standalone bash (and probably others) script that will
perform the mirroring task. The script reads a configuration file and
maintains local state within the working directory where it is invoked.

### Configuration

To use first create a "git-transform.env" file to configure several variables.
See "git-transform help" for a complete listing of available variables and their
semantics.

Sample git-transform.env:

```bash
# Where to access the source repo and where to write results
SRC_REPO=git@github.com:msg555/private-repo.git
DST_REPO=git@github.com:msg555/public-docs.git

# Data to copy into each result commit (e.g. a README or LICENSE file)
DST_REPO_OVERLAY=docs-overlay

# Tell git-transform to only checkout the "docs" and "src" folders
TRANSFORM_PATHSPEC=(
  "docs"
  "src"
)

# Build the docs and delete the src directory so only docs are public
transform-worktree() {
  make -C docs
  rm -rf src
}
```
### Commands

The workflow of `git-transform` can be broken down into four commands;
`init`, `sync`, `transform`, `push`. See `git-transform help` for details
on what each individual command does.

The combined workflow, `git-transform mirror`, is what should typically be
used in automation. This will pull all changes from the source repository,
transform all new commits, and push the updated heads and tags to the
destination repository.
