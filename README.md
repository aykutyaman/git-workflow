# git-workflow
This document describes the git workflow.

- There are two main branches. master and develop

- The master branch is always deployable. Always.
- Consider develop to be the main branch where the source code always reflects a state with the 
  latest delivered development changes for the next release. Some would call this the “integration branch”.

- FEATURE BRANCHES are created from and merged back into develop. Before
  merging into develop, make sure that your feature branch has
  the latest changes from develop because there may be conflicts.
  All conflict resolution should happen in your feature branch.
``` bash
  $ git checkout -b feature/awesome develop
  # Adding an empty commit (with a meaningful name) to make the pull-request possible
  $ git commit --allow-empty -m "Feature awesome: Basic teaser component"
```

 The point of this commit is to initialize the branch and the feature so that a pull-request can be created on GitHub.
At this point, head onto the home of the GitHub repository and click on the big ol’ green button. Then, create a pull-request from the relevant branch to the main one (automatically selected).

## Naming the pull-request
Name the pull-request after the feature name, and prefix it with [WIP] for Work In Progress. This will then be changed to [RFR] for Ready For Review once the story is done. ([WIP] Feature awesome: Basic teaser)

## Filling the description
In the description of the story, create a list of tasks where a task is a checkbox, a short description and importantly enough, one or several persons involved in the making. From the Markdown side, it might look like this:
```
* [ ] Create the basic React component (@hugogiraudel)  
* [ ] Design the icons (@sharonwalsh)  
* [ ] Integrate component in current page (@mattberridge)  
* [ ] Clarify types of teasers with client (@moritzguth)
```
## Reviewing pull request
Once all checkboxes from the description have been checked, the name of the pull-request can be updated to [RFR] for Ready For Review.

``` bash
  # .... do some work ...
  # incorporate all of the new commits in develop with rebase
  $ git rebase develop
  # ..CONFLICT.. after fixing...
  $ git rebase --continue
```
- someone should review our changes first, discuss, and collaborate
  on individual features, embed photos, comment directly on lines of code, use
  GIFS and emojis! :-1:
  When it comes to merging pull requests, it is preferable for the merge to be
  authored by the same person who opened the pull request. To achieve this, reviewers
  should leave a comment approving the new code, but not actually hit the merge button.
  Once a teammate gives the code a “thumbs up” :+1: the pull request opener
  can then go ahead and merge:
```
  $ git checkout develop
  $ git merge --no-ff feature/awesome
  $ git push origin develop
```
- SHIP IT, once develop is ready for a release, do a merge into master:
```
  $ git checkout master
  $ git merge --no-ff develop
  $ git tag -a vX.X.X -m 'Version X.X.X'
```
- VERSIONING: MAJOR(massive changes,milestones).MINOR(new features).MATCH(small changes,fixes)

- then merge master back into develop so that develop has the version commit:
```
  $ git checkout master
  $ git merge develop
```



- If there is something critical and urgent you need a HOTFIX

```
  $ git checkout -b hotfix-1.2.1 master
  $ git commit -m "Fixed severe production problem"
  $ git checkout master
  $ git merge --no-ff hotfix-1.2.1
  $ git tag -a 1.2.1
```

- Next include the bugfix in develop, too:
```
  $ git checkout develop
  $ git merge --no-ff hotfix-1.2.1
```
- Finally, remove the temporary branch:
```
  $ git branch -d hotfix-1.2.1
```
