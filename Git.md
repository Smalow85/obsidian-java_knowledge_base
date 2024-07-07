Git is a free and open-source version control system, originally created by Linus Torvalds in 2005. Unlike older centralized version control systems such as SVN and CVS, Git is distributed: every developer has the full history of their code repository locally. This makes the initial clone of the repository slower, but subsequent operations such as [[Git]], [[Git]], [[Git]], [[Git]], [[Git]], and [[Git]] dramatically faster.

Git also has excellent support for branching, merging, and rewriting repository history, which has led to many innovative and powerful workflows and tools. [[Pull]] requests are one such popular tool that allows teams to collaborate on Git [[Git]] and efficiently review each other's code. Git is the most widely used [[Version Control Systems]] in the world today and is considered the modern standard for software development.
### Snapshots, Not Differences
The major difference between [[Git]] and any other [[Version Control Systems]] (Subversion and friends included) is the way Git thinks about its data.
Git doesn’t think of or store its data this way. Instead, Git thinks of its data more like a series of snapshots of a miniature filesystem. With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored. Git thinks about its data more like a **stream of snapshots**.

![[Pasted image 20240319002512.png]]


### The Three States

Pay attention now — here is the main thing to remember about Git if you want the rest of your learning process to go smoothly. Git has three main states that your files can reside in: _modified_, _staged_, and _committed_:

- Modified means that you have changed the file but have not committed it to your database yet.
    
- Staged means that you have marked a modified file in its current version to go into your next commit snapshot.
    
- Committed means that the data is safely stored in your local database.
    

This leads us to the three main sections of a Git project: the working tree, the staging area, and the Git directory.

![Working tree, staging area, and Git directory](https://git-scm.com/book/en/v2/images/areas.png)
The working directory is a single checkout of one version of the project. These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.

The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit. Its technical name in Git parlance is the “index”, but the phrase “staging area” works just as well.

The Git directory is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you _clone_ a repository from another computer.

The basic Git workflow goes something like this:

1. You modify files in your working tree.
    
2. You selectively stage just those changes you want to be part of your next commit, which adds _only_ those changes to the staging area.
    
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.







## How it works

`Git merge` will combine multiple sequences of commits into one unified history. In the most frequent use cases, `git merge` is used to combine two branches. The following examples in this document will focus on this branch merging pattern. In these scenarios, `git merge` takes two commit pointers, usually the branch tips, and will find a common base commit between them. Once Git finds a common base commit it will create a new "merge commit" that combines the changes of each queued merge commit sequence.

Say we have a new branch feature that is based off the `main` branch. We now want to merge this feature branch into `main`.

![Merging feature branch into main](https://wac-cdn.atlassian.com/dam/jcr:7afd8460-b7bf-4c42-b997-4f5cf24f21e8/01%20Branch-2%20kopiera.png?cdnVersion=1501)

![Console window](https://wac-cdn.atlassian.com/dam/jcr:7816f6da-4c53-46c3-8df3-c125249a4f87/collaborating-workflows-cropped.png?cdnVersion=1501)

###### RELATED MATERIAL

#### Advanced Git log

[Read article](https://www.atlassian.com/git/tutorials/git-log)

![Bitbucket logo](https://wac-cdn.atlassian.com/dam/jcr:03116c1f-27e5-4a82-9b9b-806786578fb2/logos-bitbucket-icon-gradient-blue-121x109@2x.png?cdnVersion=1501)

###### SEE SOLUTION

#### Learn Git with Bitbucket Cloud

[Read tutorial](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud)

Invoking this command will merge the specified branch feature into the current branch, we'll assume `main`. Git will determine the merge algorithm automatically (discussed below).

![New merge commit node](https://wac-cdn.atlassian.com/dam/jcr:c6db91c1-1343-4d45-8c93-bdba910b9506/02%20Branch-1%20kopiera.png?cdnVersion=1501)

Merge commits are unique against other commits in the fact that they have two parent commits. When creating a merge commit Git will attempt to auto magically merge the separate histories for you. If Git encounters a piece of data that is changed in both histories it will be unable to automatically combine them. This scenario is a version control conflict and Git will need user intervention to continue.



