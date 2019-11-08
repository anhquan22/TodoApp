# Git workflow branches

![alt text](./images/git-model@2x.png)

# I. The main branches

The central repo holds two main branches with an infinite lifetime

#### 1. master:

A master branch is the main branch where the source code of HEAD always reflects a *production-ready state*.

#### 2. develop:

A develop branch is the main branch where the source code of HEAD always reflects a state with the latest delivered development changes for the next release.

# II. Supporting branches

#### 1. Release:

A release branch is a fork from ***develop*** which contains one or multiple features, no commit or push shouldn’t be done in this branch. Only documentation and bug fix are allowed on a release branch.

**Script**:

```
$ git checkout -b release-v0.0.1 develop // create a branch release with a version name$ git checkout master // change branch to master
$ git merge release-v0.0.1 // Merge the release branch into the master
$ git push //Push the merge to the master
$ git tag -a v0.0.1 -m “Tag master version 0.0.1” master // tag the master as reference point
$ git push — tags // push the tag to the master
$ git checkout develop // Change branch to develop
$ git merge release-v0.0.1 // Merge the release branch into the develop branch
$ git push //Push the merge to the develop branch
```

**Rules**:

* Parent (checkout) Branch: **develop**
* Merge back Branch: **develop** and **master**
* Name Convention: Every release branch should have ‘release’ as a prefix, ‘-’ (dash) between the prefix and the release version and the version should have this format (v.X.X.X)
Example: release-v0.0.2 or release-v1.0.1

#### 2. Hot-fix:

A hotfix branch is created only if a bug is found in production with the new release by the end users. 
The parent is the master branch.

```
$ git checkout -b hotfix-blocking_bug_from_v0.0.1 master 
//Fix the issue and merge back to master and develop
$ git checkout master
$ git merge hotfix-blocking_bug
$ git push
$ git tag -a v0.0.2 -m “Tag master version 0.0.2 bug fix “ master 
$ git push — tags // push the tag to the master
// Don’t forget to merge back to develop
$ git checkout develop
$ git merge hotfix-blocking_bug
$ git push
```

**Rules**:

* Parent (checkout) Branch: master tag
* Merge back Branch: develop and master
* Name Convention: Every hotfix branch should have ‘hotfix’ as a prefix, ‘-’ (dash) between the prefix and the hotfix name and ‘_’ (underscore) between the words of the hotfix name
Example: hotfix-crash_on_server_call

#### 3. Features:

Every feature should have a unique branch, and it has ‘develop’ like parent (checkout) branch.

```
$ git checkout -b feature-my_feature_name develop
$ git push origin feature-my_feature_name 
//Every pushes and commits should be done on this branch when the development of the feature is finished, it should be merged back to develop
$ git checkout develop //Change branch -> go to develop
$ git merge — no-ff feature-my_feature_name //Create a commit during the merge
$ git push origin develop // push the merge
```
**Rules**:

* Must create **merge request** to review code before merge
* Must rebase before merge to develop
* Parent (checkout) Branch: **develop**
* Merge back Branch: **develop**
* Name Convention: Every feature branch should have ‘issue code’ as a prefix, ‘-’ (dash) between the prefix and the feature name and ‘_’ (underscore) between the words of the feature name
Example: #101-new_conversation_design or #102-new_login_screen