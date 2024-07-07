Any [[Git]] project will consist of three sections: the Git directory, the working tree, and the staging area. The Git directory contains the history of all the files and changes. The working tree contains the current state of the project, including any changes that have made. The staging area contains the changes that have been marked to be included in the next commit.

Each file in the working directory can be in one of two states:

- **Tracked**
- **Untracked**

#### Untracked state

Untracked files are any files in the working directory that were not in the last snapshot and are not in the staging area. Whenever a new file is added in the working directory which doesn’t exist before, it is considered as an untracked file. This is because Git sees this as a file that didn’t have in the previous snapshot(commit).

#### Tracked state

Tracked files are files that were in the last snapshot. These are files that Git knows about. Each track file can be in one of three sub-states, modified, staged, or committed.



