# GitFlow

Test complete Gitflow workflow with hotfix releases and cherry-pick.

- [Git version control: Ultimate Guide](./git.md)
- [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/)

## Initialize the repository

```sh
$ mkdir Workflow ; cd Workflow ; git init
Initialized empty Git repository in /home/user/Workflow/.git/

$ echo "# Workflow" > README.md

$ echo "1.0.0-develop.1" > version.txt

$ git add .

$ git commit -m "chore: initial commit"
[main (root-commit) 8dac8e0] chore: initial commit
    2 files changed, 2 insertions(+)
    create mode 100644 README.md
    create mode 100644 version.txt

$ git branch
* main

$ git checkout -b staging
Switched to a new branch 'staging'

$ git checkout -b develop
Switched to a new branch 'develop'

$ git branch
* develop
    main
    staging
```

## Feature and bug fix on develop branch, and create a release with only the bug fix

### Feature 1

```sh
echo "Feature 1" > feature1.txt

$ git add .

$ git commit -m "feat: feature1.txt"
[develop 1a370ad] feat: feature1.txt
    1 file changed, 1 insertion(+)
    create mode 100644 feature1.txt
```

### Bug fix 1

```sh
echo "" >> README.md
echo "Bug fix 1" >> README.md

$ git add .

$ git commit -m "fix: Bug fix 1"
[develop 8f1fbbc] fix: Bug fix 1
    1 file changed, 2 insertions(+)
```

### Release 1.0.0

```sh
$ git diff-commits develop staging

Commits in staging not in develop (0)

Commits in develop not in staging (2)
+ 1a370ad3b922e7971308c2c6a4feac9476ec8c10 feat: feature1.txt
+ 8f1fbbcdeeda74eb936c93d3e34a86f0b3a09ee6 fix: Bug fix 1

$ git checkout staging
Switched to branch 'staging'

$ git checkout -b release/1.0.0
Switched to a new branch 'release/1.0.0'

$ git cherry-pick 8f1fbbcdeeda74eb936c93d3e34a86f0b3a09ee6
[release/1.0.0 5feb159] fix: Bug fix 1
    Date: Tue Oct 8 10:29:21 2024 +0200
    1 file changed, 2 insertions(+)

$ git diff-commits develop release/1.0.0

Commits in release/1.0.0 not in develop (1)
- 5feb15971742bace3e3b07c5fb498cf185aa60c2 fix: Bug fix 1

Commits in develop not in release/1.0.0 (2)
+ 1a370ad3b922e7971308c2c6a4feac9476ec8c10 feat: feature1.txt
- 8f1fbbcdeeda74eb936c93d3e34a86f0b3a09ee6 fix: Bug fix 1

$ echo "1.0.0" > version.txt

$ git add .

$ git commit -m "chore(release): 1.0.0 [skip ci]"
[release/1.0.0 bf32be6] chore(release): 1.0.0 [skip ci]
    1 file changed, 1 insertion(+), 1 deletion(-)

$ git checkout staging
Switched to branch 'staging'

$ git merge --no-ff release/1.0.0
Merge made by the 'ort' strategy.
    README.md   | 2 ++
    version.txt | 2 +-
    2 files changed, 3 insertions(+), 1 deletion(-)

$ git branch -d release/1.0.0
Deleted branch release/1.0.0 (was bf32be6).

$ git checkout develop
Switched to branch 'develop'
```

Release process finished. Now, we need to merge the staging branch into the develop branch.

### Merge staging into develop, 1st approach (`merge`)

```sh
$ git merge --no-ff staging
Merge made by the 'ort' strategy.
    version.txt | 2 +-
    1 file changed, 1 insertion(+), 1 deletion(-)

$ git diff-commits develop staging

Commits in staging not in develop (0)

Commits in develop not in staging (2)
+ 1a370ad3b922e7971308c2c6a4feac9476ec8c10 feat: feature1.txt
+ 8f1fbbcdeeda74eb936c93d3e34a86f0b3a09ee6 fix: Bug fix 1
```

Bad, there are redundant entries for `fix: Bug fix 1`.

### Merge staging into develop, 2nd approach (`rebase`)

To clean up the history so that commits that have already been cherry-picked into the staging branch don't appear in the `git diff-commits` output, you can avoid redundant entries by using `git rebase` instead of a merge in your workflow.

```sh
# This will reapply the commits in develop (such as the feature commit) on top of the staging branch, but it won't include the bugfix commit that was already cherry-picked.
$ git rebase staging
warning: skipped previously applied commit 8f1fbbc
hint: use --reapply-cherry-picks to include skipped commits
hint: Disable this message with "git config advice.skippedCherryPicks false"
Successfully rebased and updated refs/heads/develop.

$ git diff-commits develop staging

Commits in staging not in develop (0)

Commits in develop not in staging (1)
+ 02b9c46ba577d982f85e3f3568b85e4ecf51d3d5 feat: feature1.txt
```

### Merge staging into develop, 3rd approach (`squash`)

```sh
$ git merge --squash staging
Squash commit -- not updating HEAD
Automatic merge went well; stopped before committing as requested

$ git commit -m "Merge 'staging' into 'develop'"
[develop ccf2001] Merge 'staging' into 'develop'
    1 file changed, 1 insertion(+), 1 deletion(-)

$ git diff-commits develop staging

Commits in staging not in develop (2)
- e63913d6184ff72fbe9adb24eda9c7025f714d40 fix: Bug fix 1
- 897f35ac2d5c83a18da2279e0606d360b78378fa chore(release): 1.0.0 [skip ci]

Commits in develop not in staging (3)
+ 1a370ad3b922e7971308c2c6a4feac9476ec8c10 feat: feature1.txt
- 8f1fbbcdeeda74eb936c93d3e34a86f0b3a09ee6 fix: Bug fix 1
- ccf2001bf33ee2d60abad9059c74b23e2dfc73df Merge 'staging' into 'develop'
```
