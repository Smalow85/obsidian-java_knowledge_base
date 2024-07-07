[[Git]] has several mechanisms for storing history and saving changes. These mechanisms include: Commit `--amend`, `git rebase` and `git reflog`.

## Changing the Last Commit: `git commit --amend`

---

The `git commit --amend` command is a convenient way to modify the most recent commit. It lets you combine staged changes with the previous commit instead of creating an entirely new commit. It can also be used to simply edit the previous commit message without changing its snapshot. But, amending does not just alter the most recent commit, it replaces it entirely, meaning the amended commit will be a new entity with its own ref. To Git, it will look like a brand new commit.

The `--amend` flag is a convenient way to fix wrong commit message.

```bash
git commit --amend -m "an updated commit message"
```

Adding the `-m` option allows you to pass in a new message from the command line without being prompted to open an editor.

 ==! Don’t amend public commits==

Amended commits are actually entirely new commits and the previous commit will no longer be on your current branch. This has the same consequences as resetting a public snapshot. Avoid amending a commit that other developers have based their work on. This is a confusing situation for developers to be in and it’s complicated to recover from.

## Changing older or multiple commits

To modify older or multiple commits, you can use `git rebase` to combine a sequence of commits into a new base commit. In standard mode, `git rebase` allows you to literally rewrite history — automatically applying commits in your current working branch to the passed branch head. Since your new commits will be replacing the old, it's important to not use `git rebase` on commits that have been pushed public, or it will appear that your project history disappeared.

Rebasing is the process of moving or combining a sequence of commits to a new base commit. Rebasing is most useful and easily visualized in the context of a feature branching workflow. The general process can be visualized as the following:

![Git rebase](https://wac-cdn.atlassian.com/dam/jcr:4e576671-1b7f-43db-afb5-cf8db8df8e4a/01%20What%20is%20git%20rebase.svg?cdnVersion=1499)

The primary reason for rebasing is to maintain a linear project history.

==! Never rebase public history==
### Git rebase standard vs git rebase interactive

Git rebase interactive is when git rebase accepts an `-- i` argument. This stands for "Interactive." Without any arguments, the command runs in standard mode. In both cases, let's assume we have created a separate feature branch.

Git rebase in standard mode will automatically take the commits in your current working branch and apply them to the head of the passed branch.

```xml
git rebase <base>
```

This automatically rebases the current branch onto `＜base＞`, which can be any kind of commit reference (for example an ID, a branch name, a tag, or a relative reference to `HEAD`).

Running `git rebase` with the `-i` flag begins an interactive rebasing session. Instead of blindly moving all of the commits to the new base, interactive rebasing gives you the opportunity to alter individual commits in the process. This lets you clean up history by removing, splitting, and altering an existing series of commits. It's like `Git commit --amend` on steroids.

```
git rebase --interactive <base>
```

This rebases the current branch onto `＜base＞` but uses an interactive rebasing session. This opens an editor where you can enter commands (described below) for each commit to be rebased. These commands determine how individual commits will be transferred to the new base. You can also reorder the commit listing to change the order of the commits themselves. Once you've specified commands for each commit in the rebase, Git will begin playing back commits applying the rebase commands. The rebasing edit commands are as follows:

```
pick 2231360 some old commit
pick ee2adc2 Adds new feature


# Rebase 2cf755d..ee2adc2 onto 2cf755d (9 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
```

#### Additional rebase commands

As detailed in the [rewriting history page](https://www.atlassian.com/git/tutorials/rewriting-history), rebasing can be used to change older and multiple commits, committed files, and multiple messages. While these are the most common applications, `git rebase` also has additional command options that can be useful in more complex applications.

- `git rebase -- d` means during playback the commit will be discarded from the final combined commit block.
- `git rebase -- p` leaves the commit as is. It will not modify the commit's message or content and will still be an individual commit in the branches history.
- `git rebase -- x` during playback executes a command line shell script on each marked commit. A useful example would be to run your codebase's test suite on specific commits, which may help identify regressions during a rebase.