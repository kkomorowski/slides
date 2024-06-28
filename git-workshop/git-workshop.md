# Basic GIT Workshop

**Introduction to version control**

*Krzysztof Komorowski*

<span style="font-size:0.5em">03/07/2024</span>

----

## About me

----

## Agenda

* Understanding the basics of GIT
* Learning essential GIT commands
* Introduction to Bitbucket
* Lot's of exercises

----

## What is GIT?

Distributed version control system.

----

## Reasons to use GIT

* Track changes of your code
* Maintain history of the project
* Collaborate with others

----

## Installing GIT

```bash
❯ sudo apt install git
```

----

## Basic GIT configuration

```bash[1-4|6-8]
# User configuration

❯ git config --global user.name  "Krzysztof Komorowski"
❯ git config --global user.email "krzysztof@hiquality.dev"

# Default editor

❯ git config --global core.editor "vim"
```

----

## Where default GIT configuration is stored

```bash
❯ cat .gitconfig

[user]
	name = Krzysztof Komorowski
	email = krzysztof@hiquality.dev
[core]
	editor = vim
```

----

### Exercise 1 

1. Install GIT
2. Configure username and email
3. Configure default editor

----

## Creating GIT repository

```bash [1-2|4]
❯ mkdir new-repository
❯ cd new-repository

❯ git init
```

----

## Cloning a repository

```bash
# Cloning over SSH
❯ git clone git@bitbucket.org:kkomorowski/git-workshop.git

# Cloning over HTTPS
❯ git clone \
  https://kkomorowski@bitbucket.org/kkomorowski/git-workshop.git
```

----

## GIT Repository Structure

----

### Working Directory

The working directory is the area where you create, edit, delete, and organize files and directories. It reflects the current state of the files on your filesystem.

----

### Staging Area (Index)

The staging area, also known as the index, is an intermediate area where changes are listed before they are committed. It allows you to prepare and review changes.

----

### Repository (Local Repository)

The repository is where GIT permanently stores snapshots of your project history. Each commit in the repository represents a point in the history of the project.

----

## Tracking Changes

```bash
❯ git status

On branch main

No commits yet

nothing to commit (create/copy files and use "git add" 
to track)
```

----

## Working directory

```bash
❯ echo "Hello, Git." > README.md
❯ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be 
  committed)
	README.md

nothing added to commit but untracked files present 
(use "git add" to track)
```

----

## Staging Area

```bash
❯ git add README.md
❯ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   README.md
```

----

## Creating a commit

```bash
❯ git commit -m "Initial commit"
[main (root-commit) df0ec80] Initial commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
```

Note: Commit hash is calculated from all the file changed in the commit,
commit parents, message, author and timestamp.

----

## Commit is immutable

You cannot change the files in the commit, history or an author of the commit!

Note: if you try to the hash of the commit will be different.

----

## Checking the commit content


```diff
❯ git show
commit 4704e62 (HEAD -> main)
Author: Krzysztof Komorowski <krzysztof@hiquality.dev>
Date:   Wed Jul 3 00:46:05 2024 +0200

    Initial commit

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..9160de0
--- /dev/null
+++ b/README.md
@@ -0,0 +1 @@
+Hello GIT
```

----

## Checking the history

```bash
❯ git log

commit df0ec80 (HEAD -> main)
Author: Krzysztof Komorowski <krzysztof@hiquality.dev>
Date:   Tue Jul 2 22:58:04 2024 +0200

    Initial commit
```

----

### Exercise 2

* Create a new file README.md
* Create a first commit in your local repository
* Check the status and commit history

----

## Branches in GIT

GIT stores commits on branches.
Every branch have:
* name (e.g. `main` or `develop`)
* last commit
* reflog - a commits history

Note: don't confuse the branch and tag! Tag is a simple name for a commit.

----

## Creating Branches
```
❯ git branch my-new-feature
```

----

## Switching Branches
```
❯ git checkout my-new-feature
```

----

## Merging Branches

```
❯ git merge my-new-feature
```

----

## Merge commit

```
*   ccf54e3 (HEAD -> main) Merge branch 'new-branch'
|\  
| * 90b85e0 (new-branch) Update new-branch
* | 7e0d975 Commit on main
|/  
* f956e7d New file
* fe99e84 Updated main
* 4704e62 Initial commit

```

----

## Deleting branch

```
❯ git branch -d new-branch
```

----

## Handling Merge Conflicts

```bash
❯ cat README.md
Hello GIT!
<<<<<<< HEAD
==========

Welcome to the GIT Workshops.
=======

Just a simple workshop.
>>>>>>> new-branch
```

Note: Conflicts may occur not only on merge but also on
rebase, cherry-pick, revert...

----

## Handling Merge Conflicts

```bash
❯ git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   README.md

no changes added to commit (use "git add" and/or 
"git commit -a")
```

----

## Exercise 3

* Create a new branch and switch to it.
* Make changes and commit.
* Merge branches and resolve conflicts.

----

## Rebase your commits

```
* f956e7d (HEAD -> main) New file
| * 3c2d610 (new-branch) Update new-branch
|/  
* fe99e84 Updated main
* 4704e62 Initial commit
```

----

## Rebase your commits

```
* 90b85e0 (HEAD -> new-branch) Update new-branch
* f956e7d (main) New file
* fe99e84 Updated main
* 4704e62 Initial commit

```

----

## Rebase your commits

```bash
❯ git rebase main

Successfully rebased and updated refs/heads/new-branch.
```

----

## Squash your commits

```
* 22081da (HEAD -> new-branch) Commit 2
* 98800a1 Commit 1
* 90b85e0 Update new-branch
* f956e7d (main) New file
* fe99e84 Updated main
* 4704e62 Initial commit
```

----

## Squash your commits

```bash
❯ git rebase -i main

Successfully rebased and updated refs/heads/new-branch.
```

```
* cafeba4 (HEAD -> new-branch) Update new-branch & C1 & C2
* f956e7d (main) New file
* fe99e84 Updated main
* 4704e62 Initial commit
```

----

## Working with Remotes

Adding Remote
```bash
❯ git remote add \
   origin git@bitbucket.org:kkomorowski/git-workshop.git
```

Fetching Changes
```bash
❯ git fetch
```

----

## Working with Remotes

Pulling Changes
```bash
❯ git pull
```

Pushing Changes
```bash
❯ git push
```

Note: `pull --rebase`, `push --force`

----

## Exercise 4

* Add a remote repository.
* Pull changes from the remote.
* Make changes locally and push them.

----

## Additional Resources

https://git-scm.com/doc

https://git-scm.com/book/en/v2

https://learngitbranching.js.org/

