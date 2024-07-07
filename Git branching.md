[[Git]] branches are effectively a pointer to a snapshot of your changes. When you want to add a new feature or fix a bug—no matter how big or how small—you spawn a new branch to encapsulate your changes. This makes it harder for unstable code to get merged into the main code base, and it gives you the chance to clean up your future's history before merging it into the main branch.

![Git branch](https://wac-cdn.atlassian.com/dam/jcr:a905ddfd-973a-452a-a4ae-f1dd65430027/01%20Git%20branch.svg?cdnVersion=1501)

## How it works

A branch represents an independent line of development. Branches serve as an abstraction for the edit/stage/commit process. You can think of them as a way to request a brand new working directory, staging area, and project history.

```
git branch
```

List all of the branches in your repository. This is synonymous with `git branch --list.`

```
git branch <branch>
```

Create a new branch called `＜branch＞`. This does _not_ check out the new branch.

```
git branch -d <branch>
```

Delete the specified branch. This is a “safe” operation in that Git prevents you from deleting the branch if it has unmerged changes.

```
git branch -D <branch>
```

Force delete the specified branch, even if it has unmerged changes. This is the command to use if you want to permanently throw away all of the commits associated with a particular line of development.

```
git branch -m <branch>
```

Rename the current branch to `＜branch＞`.

```
git branch -a
```

List all remote branches.

## Creating branches

It's important to understand that branches are just pointers to commits. When you create a branch, all Git needs to do is create a new pointer, it doesn’t change the repository in any other way. 
If you start with a repository that looks like this:

![Repository without branches](https://wac-cdn.atlassian.com/dam/jcr:547aa16b-4bdd-45bc-9fbc-18e795dd9df1/02%20Creating%20branches.svg?cdnVersion=1501)

Then, you create a branch using the following command:

```
git branch crazy-experiment
```

The repository history remains unchanged. All you get is a new pointer to the current commit:

![Create new branch](https://wac-cdn.atlassian.com/dam/jcr:387f080e-19b8-43ab-a7a3-0921ffd7298a/03%20Creating%20branches.svg?cdnVersion=1501)

Note that this only _creates_ the new branch. To start adding commits to it, you need to select it with `git checkout`, and then use the standard `git add` and `git commit` commands.