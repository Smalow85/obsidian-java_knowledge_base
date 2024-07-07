[[Git]] provides several different strategies to 'undo' a commit.

## git checkout

Using the `git checkout` command we can checkout necessary commit from the history, skipping "corrupted" commit(-s).

```
git checkout -b new_branch_without_crazy_commit
```

Unfortunately, if commit is in your `main` branch, this undo strategy is not appropriate.

## git revert

If we execute `git revert HEAD`, Git will create a new commit with the inverse of the last commit. This adds a new commit to the current branch history.

This solution is a satisfactory undo. This is the ideal 'undo' method for working with public shared repositories. If you have requirements of keeping a curated and minimal Git history this strategy may not be satisfactory.

## git reset

The `git reset` command is a complex and versatile tool for undoing changes. It has three primary forms of invocation. These forms correspond to command line arguments `--soft, --mixed, --hard`. The three arguments each correspond to Git's three internal state management mechanism's, The [[Commit Tree]] (HEAD), [[staging area]] and [[working directory]].

At a surface level, `git reset` is similar in behavior to `git checkout`. Where `git checkout` solely operates on the `HEAD` ref pointer, `git reset` will move the `HEAD` ref pointer and the current branch ref pointer. To better demonstrate this behavior consider the following example:

![[Pasted image 20240320231846.png]]
This example demonstrates a sequence of commits on the `main` branch. The `HEAD` ref and `main` branch ref currently point to commit d. Now let us execute and compare, both `git checkout b` and `git reset b.`

```
git checkout b
```

![4 nodes with main pointing at last node and head pointing at 2nd node](https://wac-cdn.atlassian.com/dam/jcr:f45c4a34-8968-4c81-83cf-d55ebf01a447/02%20git-checkout-transparent%20kopiera.png?cdnVersion=1499)

With `git checkout`, the `main` ref is still pointing to `d`. The `HEAD` ref has been moved, and now points at commit `b`. The repo is now in a 'detached `HEAD`' state.

```
git reset b
```

![2 sets of 2 nodes, with head,main pointing at the 2nd of the 1st set](https://wac-cdn.atlassian.com/dam/jcr:bdf5fda3-4aac-4170-ba35-58f7a66ea3c4/03%20git-reset-transparent%20kopiera.png?cdnVersion=1499)

Comparatively, `git reset`, moves both the `HEAD` and branch refs to the specified commit.

In addition to updating the commit ref pointers, `git reset` will modify the state of the three trees(commit history, staging area and working directory). The ref pointer modification always happens and is an update to the third tree, the Commit tree. The command line arguments `--soft, --mixed`, and `--hard` direct how to modify the Staging Index, and Working Directory trees.

### reset options

The default invocation of `git reset` has implicit arguments of `--mixed` and `HEAD`.

Instead of `HEAD` any Git SHA-1 commit hash can be used.

![diagram of scope of git resets](https://wac-cdn.atlassian.com/dam/jcr:7fb4b5f7-a2cd-4cb7-9a32-456202499922/03%20(8).svg?cdnVersion=1499)

#### '--hard

This is the most direct, DANGEROUS, and frequently used option. When passed `--hard` The Commit History ref pointers are updated to the specified commit. Then, the Staging Index and Working Directory are reset to match that of the specified commit. Any previously pending changes to the Staging Index and the Working Directory gets reset to match the state of the Commit Tree. This means any pending work that was hanging out in the Staging Index and Working Directory will be lost.

#### '--mixed

---

This is the default operating mode. The ref pointers are updated. The Staging Index is reset to the state of the specified commit. Any changes that have been undone from the Staging Index are moved to the Working Directory.

#### '--soft

When the `--soft` argument is passed, the ref pointers are updated and the reset stops there. The Staging Index and the Working Directory are left untouched.

## Resetting vs reverting

If `git revert` is a “safe” way to undo changes, you can think of `git reset` as the dangerous method. There is a real risk of losing work with `git reset`. `Git reset` will never delete a commit, however, commits can become 'orphaned' which means there is no direct path from a ref to access them. These orphaned commits can usually be found and restored using [git reflog](https://www.atlassian.com/git/tutorials/rewriting-history/git-reflog). Git will permanently delete any orphaned commits after it runs the internal garbage collector. By default, Git is configured to run the garbage collector every 30 days.

Make sure that you’re using `git reset ＜commit＞` on a local experiment that went wrong—not on published changes. If you need to fix a public commit, the `git revert` command was designed specifically for this purpose.

