# git-worktree-number

**git-worktree-number** exposes a simple command for obtaining a unique number for each [worktree](https://git-scm.com/docs/git-worktree) of a given git repository.

```
# Print a unique (in repo scope) non-negative integer for the current worktree
git worktree-number [num]      # print

# List all worktrees with their assigned numbers
git worktree-number list

# Release numbers for deleted worktrees
git worktree-number prune

# Print a unique (in repo scope) id for the current worktree
git worktree-number id
```

The primary motivation for this is to assign unique port numbers when developing in multiple worktrees in parallel. There are a number of other tools out there that do this, such as [stemps/treehouse](https://github.com/stemps/treehouse), but I wanted one that was written in bash (to minimize dependencies), packaged as a git subcommand (which feels more natural), and only interacts with the `.git` directory using core git commands (no loose files to manage).

Worktree numbers are stored in a rudimentary key-value store build on top of git custom refs. By leaning on git's built-in locking mechanisms, I hope to ensure that it is impossible for concurrent executions of this command to issue overlapping worktree numbers or clobber the store.

Note: When run as `git-kv` or `git-reflock` this command provides different subcommands that give access to the some of the primitives used by the main command.

## TODO

  - [ ] Write tests, especially for concurrent access
  - [ ] Make the kv access between both stores fully atomic
  - [ ] Ensure portability at least back to Bash 3
  - [ ] `prune` automatically in the right contexts
