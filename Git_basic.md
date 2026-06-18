# Basic Git Commands Reference

A quick guide to the most commonly used Git commands.

## Getting Started
- `git init`: Initialize a new local Git repository.
- `git clone <url>`: Download a project and its entire version history from a remote repository.

## Staging & Committing
- `git status`: Lists all new or modified files to be committed.
- `git add <file>`: Snapshots the file in preparation for versioning.
- `git add .`: Snapshots all changes in the current directory for versioning.
- `git commit -m "<message>"`: Records file snapshots permanently in version history with a descriptive message.

## Branching & Merging
- `git branch`: Lists all local branches in the current repository.
- `git branch <branch-name>`: Creates a new branch.
- `git checkout <branch-name>`: Switches to the specified branch and updates the working directory.
- `git switch <branch-name>`: An alternative to checkout for switching branches.
- `git merge <branch-name>`: Combines the specified branch's history into the current branch.

## Remote Repositories
- `git remote -v`: Lists all current configured remote repositories.
- `git push <remote> <branch>`: Uploads local repository content to a remote repository.
- `git pull <remote> <branch>`: Fetches and merges changes from the remote server to your working directory.

## Inspection & History
- `git log`: Lists version history for the current branch.
- `git diff`: Shows file differences not yet staged.
- `git show <commit>`: Outputs metadata and content changes of the specified commit.

## Undoing Changes
- `git reset <file>`: Unstages the file, but preserves its contents.
- `git revert <commit>`: Creates a new commit that undoes the changes from a previous commit.
