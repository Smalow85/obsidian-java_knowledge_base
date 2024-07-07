Once a remote record has been configured through the use of the `git remote` command, the remote name can be passed as an argument to other [[Git]] commands to communicate with the remote repo. Both `git fetch`, and `git pull` can be used to read from a remote repository.

# Git fetch

The `git fetch` command downloads commits, files, and refs from a remote repository into your local repo. Fetching is what you do when you want to see what everybody else has been working on.
When downloading content from a remote repo, `git pull` and `git fetch` commands are available to accomplish the task. You can consider `git fetch` the 'safe' version of the two commands. It will download the remote content but not update your local repo's working state, leaving your current work intact. `git pull` is the more aggressive alternative; it will download the remote content for the active local branch and immediately execute `git merge` to create a merge commit for the new remote content. If you have pending changes in progress this will cause conflicts and kick-off the merge conflict resolution flow.

```
git fetch <remote>
```

Fetch all of the branches from the repository. This also downloads all of the required commits and files from the other repository.

```
git fetch <remote> <branch>
```

Same as the above command, but only fetch the specified branch.

```
git fetch --all
```

A power move which fetches all registered remotes and their branches:

```
git fetch --dry-run
```

The `--dry-run` option will perform a demo run of the command. It will output examples of actions it will take during the fetch but not apply them.

Example: 

Firstly we will need to configure the remote repo using the `git remote` command.

```
git remote add coworkers_repo git@bitbucket.org:coworker/coworkers_repo.git
```

Here we have created a reference to the coworker's repo using the repo URL. We will now pass that remote name to `git fetch` to download the contents.

```
git fetch coworkers_repo coworkers/feature_branch
fetching coworkers/feature_branch
```

We now locally have the contents of coworkers/feature_branch we will need the integrate this into our local working copy. We begin this process by using the [git checkout](https://www.atlassian.com/git/tutorials/using-branches/git-checkout) command to checkout the newly downloaded remote branch.

```
git checkout coworkers/feature_branch
```

# Git pull

The `git pull` command is used to fetch and download content from a remote repository and immediately update the local repository to match that content. Merging remote upstream changes into your local repository is a common task in Git-based collaboration work flows. The `git pull` command is actually a combination of two other commands, `git fetch` followed by `git merge`.

### How it works

The `git pull` command first runs `git fetch` which downloads content from the specified remote repository. Then a `git merge` is executed to merge the remote content refs and heads into a new local merge commit. To better demonstrate the pull and merging process let us consider the following example. Assume we have a repository with a main branch and a remote origin.

![](https://wac-cdn.atlassian.com/dam/jcr:63e58c34-b273-4e48-a6b1-6e3ba4d4a0ea/01%20bubble%20diagram-01.svg?cdnVersion=1501)

In this scenario, `git pull` will download all the changes from the point where the local and main diverged. In this example, that point is E. `git pull` will fetch the diverged remote commits which are A-B-C. The pull process will then create a new local merge commit containing the content of the new diverged remote commits.

![](https://wac-cdn.atlassian.com/dam/jcr:0269bb2d-eb7f-43d8-80a2-8afa88d11eea/02%20bubble%20diagram-02.svg?cdnVersion=1501)

![Console window](https://wac-cdn.atlassian.com/dam/jcr:7816f6da-4c53-46c3-8df3-c125249a4f87/collaborating-workflows-cropped.png?cdnVersion=1501)


In the above diagram, we can see the new commit H. This commit is a new merge commit that contains the contents of remote A-B-C commits and has a combined log message. This example is one of a few `git pull` merging strategies. A `--rebase` option can be passed to `git pull` to use a rebase merging strategy instead of a merge commit. The next example will demonstrate how a rebase pull works. Assume that we are at a starting point of our first diagram, and we have executed `git pull --rebase`.

# Git push

The `git push` command is used to upload local repository content to a remote repository.
## Git push usage

```
git push <remote> <branch>
```

Push the specified branch to , along with all of the necessary commits and internal objects. This creates a local branch in the destination repository. To prevent you from overwriting commits, Git won’t let you push when it results in a non-fast-forward merge in the destination repository.

```
git push <remote> --force
```

Same as the above command, but force the push even if it results in a non-fast-forward merge. Do not use the `--force` flag unless you’re absolutely sure you know what you’re doing.

## Force pushing

Git prevents you from overwriting the central repository’s history by refusing push requests when they result in a non-fast-forward merge. So, if the remote history has diverged from your history, you need to pull the remote branch and merge it into your local one, then try pushing again.

The `--force` flag overrides this behavior and makes the remote repository’s branch match your local one, deleting any upstream changes that may have occurred since you last pulled. The only time you should ever need to force push is when you realize that the commits you just shared were not quite right and you fixed them with a `git commit --amend` or an interactive rebase. However, you must be absolutely certain that none of your teammates have pulled those commits before using the `--force` option.