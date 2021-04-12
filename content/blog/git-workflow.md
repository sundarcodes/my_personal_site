---
title: "Git workflow - my ramblings"
date: 2017-04-08
tags: ["git", "version-control", "best-practices"]
draft: false
---

In this post, I would be talking about the git workflow that could be adopted when you are working on a project all alone and also when you are working in a team. Before we start, lets try to understand what is Git and why it is becoming the leader in Version control/SCM (Software Configuration Management).

### What is Git ?
Git is a version control system with a de-centralized approach to source control. This means you don't really need a centralized server to work with instead you could have your own local version control system and push to a storage area whenever you feel like. Git was developed by Linus Torvalds, the same guy who wrote Linux Operating System. Git was developed out of frustration as Linus found the existing Source Control Systems were pathetic when it came to managing/collaborating across huge team of developers. If you like to know more, listen to [talk][git-talk] by Linus himself on git.

#### Want to get your hands dirty on git ?
If you are completely new to git, I would recommend you to just try out this 15 minute [git tutorial][git-tutorial] so that whatever I have written below would make sense.

### Workflow when you working as a single man army
The first step would be creating a git repository with the default master branch in a hosting server ([github][github]/[gitlab][gitlab]/[bitbucket][bitbucket]). **Master** branch should always have the copy of the code which is going to go to production. Now create a **develop** branch, which at any point of time would be the working branch for all the new features you would be adding and this should be in sync with the master branch throughout.

#### Working on a feature
Now, let's say you are going to work on a new feature or enhancement. Create a branch with the feature enhancement name from the **develop** branch. For example, if the feature you are going to work is **featureA**, create a branch from **develop** and name it as **develop-featureA**. 
Keep working on this branch till you have completed the development. Once you feel, you are done with your unit/integrating testing and feel its ready for production, submit a PR (pull request) from **develop-featureA** branch to **develop** branch. I am insisting on a PR instead of a merge as you could leverage the review of your peers/client or even you yourself could take that extra mile in reviewing your code from the stand point of a reviewer instead of a developer. Tag this branch with the release name/version and push it to production whenever it needs to. After it has been deployed to production, merge the tagged version to master.

#### Working on a production defect
Its very normal that we would need to send a patch to fix an urgent production issue. During this scenario, the workflow would be this. Sync up your local master and remote master branch. Create a branch from master with the name **hot-fix-A** and implement the hot fix. Once tested, submit a PR From the **hot-fix-A** branch to **master** branch. Review or have some one review it and merge the PR. Now, merge the master branch changes to the feature branch you are working on and continue working.

### Workflow when you working in a team
When you are working in a team, the first step is to create a repo in a hosting server with the default master branch. Create another branch and name it as **develop**. Now ask your fellow developers to fork this repo. This is with respect to the initial setup. If you team has junior developers who are not experienced, this task of setting up a branch should be done by the senior dev or the team lead. The team lead should have all access to the master repo but the junior developers should just be able to colloborate by forking and submitting PRs.

#### Working on a feature
Now lets say your junior devs are working on a feature, they have to create a branch say **feature-aa** in their own repo and keep working along. Once it has been implemented and passed all the tests, the developer has to send a PR from his **feature-aa** branch to the **develop** branch in hosting server/central repo. This PR will undergo the review cycle and the Tech lead/senior dev should merge it with **develop** branch if it is good to go. If multiple devs are working on the same feature, they must collaborate amongst themselves by syncing their changes frequently. The code must be merged to **develop** when it is deemed fit for QA/prod deployment. Other developers working on different features must be syncing with **develop** on a regular/daily basis so that they are up to date with the latest developments. Now when the code is release ready, tag it with the release name. You could continue working on other features in a similar fashion described in this section. Once your code has been deployed to production, merge that tag to **master** and you all set. 

#### Working on a production defect
Now, you have deployed your code to production and out pops a high severity bug. The workflow for this is similar to the one that we had when you are working all alone in a project. You just create a branch from **master** called **hot-fix**, do the fix, test it. Submit a PR to merge it with develop. Deploy the **hot-fix** branch to production. Once its confirmed to be working fine, merge it with **master** and delete the **hot-fix** branch.


### Summary
This git workflow is based on my experience working both in solo projects and also working in a team enviornment. There are lot of blogs which gives you more variations of a git workflow [here][ref1], [here][ref2] and [here][ref3]. Read through all of them and choose whichever suits you best !!



[git-talk]: https://www.youtube.com/watch?v=4XpnKHJAok8
[git-tutorial]: https://try.github.io/levels/1/challenges/2
[github]: https://github.com/
[gitlab]: https://gitlab.com/
[bitbucket]: https://bitbucket.org/
[ref1]: http://www.bitsnbites.eu/a-stable-mainline-branching-model-for-git/
[ref2]: http://nvie.com/posts/a-successful-git-branching-model/
[ref3]: https://www.atlassian.com/git/tutorials/comparing-workflows