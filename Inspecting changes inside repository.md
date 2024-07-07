## git status

The `git status` command displays the state of the working directory and the staging area.  It lets you see which changes have been staged, which haven’t, and which files aren’t being tracked by [[Git]]. Status output does _not_ show you any information regarding the committed project history. For this, you need to use `git log`.

```
git status
```

```
# On branch main # Changes to be committed: # (use "git reset HEAD <file>..." to unstage) # #modified: hello.py # # Changes not staged for commit: # (use "git add <file>..." to update what will be committed) # (use "git checkout -- <file>..." to discard changes in working directory) # #modified: main.py # # Untracked files: # (use "git add <file>..." to include in what will be committed) # #hello.pyc
```

## Ignoring Files

Untracked files typically fall into two categories. They're either files that have just been added to the project and haven't been committed yet, or they're compiled binaries like `.pyc`, `.obj`, `.exe`, etc. While it's definitely beneficial to include the former in the `git status` output, the latter can make it hard to see what’s actually going on in your repository.

For this reason, Git lets you completely ignore files by placing paths in a special file called [[gitignore]]. Any files that you'd like to ignore should be included on a separate line, and the * symbol can be used as a wildcard. For example, adding the following to a `.gitignore` file in your project root will prevent compiled Python modules from appearing in `git status`:

```undefined
*.pyc
```

## git log

The `git log` command displays committed snapshots. It lets you list the project history, filter it, and search for specific changes. While `git status` lets you inspect the working directory and the staging area, `git log` only operates on the [[committed history]].

![Git status vs git log](https://wac-cdn.atlassian.com/dam/jcr:52d530ce-7f51-48e3-920b-a18f776048d3/01.svg?cdnVersion=1496)

```bash
git log
```

Display the entire commit history using the default formatting. If the output takes up more than one screen, you can use `Space` to scroll and `q` to exit.

```bash
git log -n <limit>
```

Limit the number of commits by . For example, `git log -n 3` will display only 3 commits.