# Git rule

### Precondition
- Init central repository on Github (or Bitbucket) first
- Default branch of central repository is master
- New member will fork central reposity
- Decided reviewer and person who will merge pull request

### Pinciple
- Each task will create a new commit
- Title of the commit need very clean (example: `"create a new navbar button"`)
- Updating code of master branch on local is prohibited, please create a new branch to do your task.

### Prepare
1. Fork central repository to your account on Github (Bitbucket)

2. Clone forked repository to local. At this time, forked repository will be automatically registered as `origin` on local.
    ```sh
    $ git clone [URL of forked repository]
    ```

3. Enter the directory created by `clone` command, register central repository as `upstream`.
    ```sh
    $ cd [created directory]
    $ git remote add upstream [URL of central repository]
    ```

## For multi branch rule

### Develop

From now, we will call central repository as `upstream`, forked repository as `origin`.

1. Sync local's master branch with upstream's master branch.
    ```sh
    $ git checkout master
    $ git pull upstream master
    ```

2. Create new branch to do task from master branch on local. Name of branch should be number of that task.(For example: `task/1234`)
    ```sh
    $ git checkout master # <--- unnecessary in case already on master branch
    $ git checkout -b task/1234
    ```

3. Do your task (Freedomly create multiple commits).

4. If you created multiple commits when doing task, please use `rebase i` command to group them into 1 commit before moving to step 5.
    ```sh
    $ git rebase -i [hash value of the commit before the first commit you created or the number of commits needs to be grouped]
    ```

5. Move to local's master branch and update to latest version.
    ```sh
    $ git checkout master
    $ git pull upstream master
    ```

6. Move back to your task branch, rebase this branch with master branch.
    ```sh
    $ git checkout task/1234
    $ git rebase master
    ```
    **If conflict error occurs when rebasing, please refer to "fix conflict error when rebasing" procedure.**

7. Push to `origin`

    ```sh
    $ git push origin task/1234
    ```

8. On Github (Bitbucket), create pull request from origin's  `task/1234` to upstream's `master` branch.

9. Paste pull request page's URL to chatwork group, ask reviewer to have code-review.

    9.1. If reviewer asks you to fix something, do 3. ~ 6. step.

    9.2. Use `push -f` command to force push to the same remote branch.
    ```sh
    $ git push origin task/1234 -f
    ```

    9.3. Repaste pull request page's URL to chatwork group, ask reviewer to have code-review.

10. When more than 2 reviewers gave you OK comment, reviewer who gave the final OK comment will merge that pull request.
11. Back to 1 step.

### Fix conflict error when rebasing

If conflict error occurs when rebasing, it will be displayed as below (at this moment, you will be moved to anonymous branch automatically).
```sh
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: refs #1234 Can not remove cache
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging path/to/conflicting/file
CONFLICT (add/add): Merge conflict in path/to/conflicting/file
Failed to merge in the changes.
Patch failed at 0001 refs #1234 Can not remove cache
The copy of the patch that failed is found in:
    /path/to/working/dir/.git/rebase-apply/patch

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
```

1. Fix all the conflicts manually (code which is surrounded by <<< an >>>)
Use `git rebase --abort` if you want to abort rebase process.

2. After fixing all conflicts, continue your rebase process.

    ```sh
    $ git add .
    $ git rebase --continue
    ```

## For single branch rule

### Develop
1. Working on your code and don't change too much before doing a commit.
2. Stage and commit your changes:
```sh
$ git add .
$ git commit -m "Commit your changes"
```
4. Grabs changes from remote repository and puts it in your repository's object database. It also fetches branches from remote repository and stores them as remote-tracking branches.
```sh
$ git fetch
```
When you are fetching git tells you where it stores each branch on remote repository it fetches. For example you should see something like:
```sh
   7987baa..2086e7b  master -> origin/master
```
5. Merge your branch and fix conflig:
```sh
$ git merge
```
6. Push local respository:
```sh
$ git push
```