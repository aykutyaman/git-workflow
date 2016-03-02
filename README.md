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
```
  $ git checkout -b myFeature develop
  .... do some work ...
  $ git rebase develop
  ..CONFLICT.. after fixing...
  $ git rebase --continue
```
- PULL REQUEST, someone should review our changes first, discuss, and collaborate
  on individual features, embed photos, comment directly on lines of code, use
  GIFS and emojis! :-1:
  When it comes to merging pull requests, it is preferable for the merge to be
  authored by the same person who opened the pull request. To achieve this, reviewers
  should leave a comment approving the new code, but not actually hit the merge button.
  Once a teammate gives the code a “thumbs up” :+1: the pull request opener
  can then go ahead and merge:
```
  $ git checkout develop
  $ git merge --no-ff myFeature
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
