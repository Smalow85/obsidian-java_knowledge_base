**Basic**

The concept is straightforward: there is a single central repository. Each developer clones the repository from [[Git]], works on the code locally, commits the changes, and pushes it to the central repository for other developers to grab and use.
  
![](https://razorops.com/images/blog/basic-workflow.png)  

**Feature Branches**

This is where Git’s primary feature, branches, comes in handy. Branches are separate project development “tracks.” A new branch should be generated for each new functionality, where the new feature is built and tested. When the feature is complete, merge the branch with the master branch and push it to the production server.

  
![](https://razorops.com/images/blog/feature-branches.png)  

**Git-Flow:**  
Git-Flow is a comprehensive branching strategy that aims to cover various scenarios. It defines specific branch responsibilities, such as main/master for production, develop for active development, feature for new features, release as a gatekeeper to production, and hotfix for addressing urgent issues. The life-cycle involves branching off from develop, integrating features, creating release branches for testing, merging into main/master, and tagging versions.

**Pros:**  
- Well-suited for large teams and aligning work across multiple teams.  
- Effective handling of multiple product versions.  
- Clear responsibilities for each branch.  
- Allows for easy navigation of production versions through tags.

**Cons:**  
- Complexity due to numerous branches, potentially leading to merge conflicts.  
- Development and release frequency may be slower due to multi-step process.  
- Requires team consensus and commitment to adhere to the strategy.

![](https://miro.medium.com/v2/resize:fit:1050/0*Dcet6Kf45N2CSFIm.png)

Git-Flow

**2. GitHub-Flow:**  
GitHub-Flow simplifies Git-Flow by eliminating release branches. It revolves around one active development branch (often main or master) that is directly deployed to production. Features and bug fixes are implemented using long-living feature branches. Feedback loops and asynchronous collaboration, common in open-source projects, are encouraged.

**Pros:**  
- Faster feedback cycles and shorter production cycles.  
- Ideal for asynchronous work in smaller teams.  
- Agile and easier to comprehend compared to Git-Flow.  
  
**Cons:**  
- Merging a feature branch implies it is production-ready, potentially introducing bugs without proper testing and a robust CI/CD process.  
- Long-living branches can complicate the process.  
- Challenging to scale for larger teams due to increased merge conflicts.  
- Supporting multiple release versions concurrently is difficult.

![](https://miro.medium.com/v2/resize:fit:1050/0*nTY2nx3ulEfi5JuT.png)

GitHub-flow

**3. GitLab-Flow:**  
GitLab-Flow strikes a balance between Git-Flow and GitHub-Flow. It adopts GitHub-Flow’s simplicity while introducing additional branches representing staging environments before production. The main branch still represents the production environment.

**Pros:**  
- Can handle multiple release versions or stages effectively.  
- Simpler than Git-Flow.  
- Focuses on quality with a lean approach.

**Cons:**  
- Complexity increases when maintaining multiple versions.  
- More intricate compared to GitHub-Flow.

![](https://miro.medium.com/v2/resize:fit:1050/0*UsNv5Hw6gShxqlE5.png)

GitLab-Flow