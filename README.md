# Truncate dates and drop timezones in git commits

These hooks modify the author date and committer date of any commit. An example:

```bash
$ git commit -m 'tmp'
date-truncate: 885c484 -> 546299f (truncated author date and committer date)
  (use "git reset 885c484" to undo)
[master 885c484] tmp
 1 file changed, 5 insertions(+)
 create mode 100644 tmp-file
```

When a commit is made, the author date and committer date are automatically truncated to the date part only, and the timezone information is dropped. The commit hash is changed as a result.

The old commit: (which will not be published if you run `git push`)

```bash
$ git show --format=fuller 885c484
commit 885c484cfc724dd7f4f087c08859b14019eaf3f9
Author:     Hypercube <hypercube@0x01.me>
AuthorDate: Mon Apr 1 11:01:44 2024 +0800
Commit:     Hypercube <hypercube@0x01.me>
CommitDate: Mon Apr 1 11:01:44 2024 +0800

    tmp
```

The new commit:

```bash
$ git show --format=fuller 546299f
commit 546299f946aac739d9ec6a8229434fb0be35963d (HEAD -> master)
Author:     Hypercube <hypercube@0x01.me>
AuthorDate: Mon Apr 1 00:00:00 2024 +0000
Commit:     Hypercube <hypercube@0x01.me>
CommitDate: Mon Apr 1 00:00:00 2024 +0000

    tmp
```

## How it works

The `git-date-truncate` script inspects the last commit, if its author name and author email matches `user.name` and `user.email` in the git configuration, it drops the time and timezone information from the author date. The same is done for the committer.

The hooks call the script after a commit is made. By default, it will output the old and new commit hashes, and a command to undo the operation, in case the user wants to revert the changes.

## Installation

1. Copy the `git-date-truncate` script to somewhere in your `$PATH`.
2. Copy the files in `hooks` directory to `.config/git/template/hooks` or `~/.git-template/hooks`. If you only want to enable the hooks for a single repository, copy the files to `.git/hooks` in the repository.
3. For all existing repositories, run `git init` in the repository to apply the hooks in the template directory.

## Disclaimer

This script is not tested with all possible edge cases. Use at your own risk. In rare cases like complex rebases, the script may not work as expected.
