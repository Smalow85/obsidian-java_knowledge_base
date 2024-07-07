## What is Git Head?

The most recent commit to the current checkout branch is indicated by the HEAD.
## Git Refs and Heads

**HEAD** is the reference to the most recent commit in the current branch. This means HEAD is just like a pointer that keeps track of the latest commit in your current branch. However, this definition just gives us a basic overview of HEAD, so before deep diving into HEAD let us learn about two things before that is **refs** and **head.**

A [[Git]] repository is a collection of **objects** and **references**. Objects have relationships with each other, and references point to objects and to other references. The main objects in a Git repository are commits, but other objects include [blobs](https://developer.github.com/v3/git/blobs/) and [trees](https://developer.github.com/v3/git/trees/). The most important references in Git are branches, which you can think of as labels you put on a commit.

HEAD is another important type of reference. The purpose of HEAD is to keep track of the current point in a Git repo. In other words, HEAD answers the question, “Where am I right now?”

For instance, when you use the **log** command, how does Git know which commit it should start displaying results from? HEAD provides the answer. When you create a new commit, its parent is indicated by where HEAD currently points to.
### Are you in ‘detached HEAD’ state?

You’ve just seen that HEAD in Git is only the name of a reference that indicates the current point in a repository. So, what does it mean for it to be attached or detached?

Most of the time, HEAD points to a branch name. When you add a new commit, your branch reference is updated to point to it, but HEAD remains the same. When you change branches, HEAD is updated to point to the branch you’ve switched to. All of that means that, in these scenarios, HEAD is synonymous with “the last commit in the current branch.” This is the _normal_ state, in which HEAD is _attached_ to a branch.

![Attached head vs detached head](https://wac-cdn.atlassian.com/dam/jcr:d93bca4e-11ed-4cd6-a690-e97444755429/01-02%20Detached%20HEADS.svg?cdnVersion=1501)

```
git checkout <commit>
```

Above command makes `HEAD` detached, that means that HEAD now points to commit, not branch (most recent commit). It is always recommended, do not commit on detached Head.


