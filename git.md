# Git Version Control System

## Basics

### **Git Basic Command Syntax**

```console
git fakecommand  (-p|--patch) [<id>] [--] [<paths>...]
```

* *-f* , *--f* → **Flags** that change behavior
* **|** → **OR**
* **[** optional **]**
* **<** placeholder **>**
* **[<** optional placeholder **>]**
* **()** → Grouping(Clarification and disambiguation.)
* **--** → Disambiguate the command.
* **...** → Multiple occurrences possible.

### **Getting Help**

```console
git                 #Overall help
git help [command]  #Takes you to the [documentation](https://git-scm.com/doc) for the given command.
git [command] -h    #Quick help in the Console.  
```

### **Setting Basic User Information**

```console
git config --global user.name "USERNAME"          #setting username
git config --global user.email "USER@EMAIL.COM"   #setting user e-mail
```

### **Get Basic Userinfo**

```console
git config user.name
git config user.email
```

## Git Locations

* **Project Directory**(local)  
  * Working tree → Actual files of the project
  * Staging area → Files that will be included in the next commit (.git)
  * Local repository → All commits (.git)

* **Remote Repository**

### **Creating a Local Repository**

```console
git init                        #Initializing a local repository(working tree, staging area, local repository) with the .git hidden folder.
```

### **Commiting to a Local Repository**

```console
git status
git status -s
```

> Checking the status of files in the `working tree` and the `staging area`.  
> `-s` →  Short status:
>
> * `A`  : Added (staged)
> * `M`  : Modified
> * `AM` : Added and Modified
> * `??` : Untracked
> * `D`  : Deleted

```shell
git add <file-or-directory>     #Adding a file or directory to the `staging area`. → It will be part of the next commit.
```

```shell
git commit
git commit -m "commit-message"  #Commits the changes to the `local repository`. The first version opens the default git text editor to edit the message.
```

```shell
git log
git log --oneline
git log --oneline -n            #Viewing the commit history(last n commits if  given). Second version is condensed.
```

### **Creating a Remote Repository**

Remote repository URLs end with `.git`. (e.g. [bitbucket.org](https://bitbucket.org)/`<team-name`>/repoa.git)  
Remote repositories are often `Bare Repositories`.

#### **Bare Repository**

There is no `working tree` or `staging area` because no one works locally.

```shell
git init --bare
```

### **Push to a Remote Repository**

#### **Add | Clone**

* **Clone** → If there's no existing local repository yet, you have to clone the remote repository. This creates a local repository, which is associated with the remote repository, containing it's commits.
* **Add** → If theres already a local repository, with commits to push to the remote repository, you add the remote repository to your local repository.

##### **Cloning**

```shell
git clone <url/to/projectname.git> [localprojectname]
```

```shell
git remote --verbose
git remote -v
```

> Displays information about the `remote repository` associated with the `local repository`.  
> origin  [https://geabor@bitbucket.org/geabor/projectb.git](https://geabor@bitbucket.org/geabor/projectb.git) (fetch)  
origin  [https://geabor@bitbucket.org/geabor/projectb.git](https://geabor@bitbucket.org/geabor/projectb.git) (push)  
`origin` is a usable alias for the URL.

##### **Adding**

```shell
git remote add <name> <url>
git remote add origin https://me@bitbucket.org/me/repoa.git
```

> Adding information about the `remote repository` to the already existing `local repository`.

#### **Pushing**

```shell
git push [-u] [<repository>] [<branch>]
git push                                  #all modifiers are optional
```

> Writes the commits for a branch to the remote repository from the local repository.
>
> * `<repository>` name(alias) or URL of the remote repository
> * `-u` || `--set-upstream` → track this branch : Notification when the branch is out of sync.

## Git's Graph Model

Git models the relationship of commits in a `Directed Acyclic Graph` (`DAG`), where `every edge points to the given commit's parents`.

### **Branches**

`Branches` occur when a commit has more than one child.

### **Merge**

A `merge` occurs when a commit has more than one parent.

### **Viewing the graph**

```shell
git log --oneline --graph           #View graph on current branch
git log --oneline --graph --all     #View graph on all branches
```

## Git Objects

* `Commit Object` → Contains every information about a commit.
* `Annotated tag` → Reference to a specific commit
* Tree → Dictionaries and filenames in the project.
* Blob → Content of a file in the project.

## Git IDs

* Name of a Git object.
* 40 character hexadecimal string.
* Other names :  `object ID`, `SHA-1`, `hash`, `checksum`
* `git log --oneline` shortens these IDs for better readability

```shell
git show [objectID(can be shortened)]    #Shows the content of the given object.
```

### **SHA-1**

It's a `generated value`, which is `unique for every content`. No files with even slightly different contents can have the same SHA-1 value. This is how `Git` generates it's `IDs`.

```shell
git hash-object <file>                   #To generate an `SHA-1` for any content.
```

## Git References

A `Git Reference` is a `user-friendly name` that points to:

* A commits SHA-1 value
* Another reference → `symbolic reference`  

You can use `references` instead of `SHA-1` values in `Git Commands`.

### **Branch Labels**

A `Branch Label` points to the `most recent commit` of the given branch. Often called the `tip of the branch`. Branch labels `are references`.  
Branch labels are stored and can be viewed in `.git/refs/heads`.

### **HEAD**

A `reference to the current commit`. Usually `points to the branch label` of the current branch. There is `only one` **HEAD** reference in a local repository.

### **Refering to Prior Commits**

```shell
git show HEAD~          #shows the parent of the given commit
git show HEAD~1         #same  

git show HEAD~~         #parent's parent
git show HEAD~2          #same
```

>Any number of `~` or `~<`number`>` can be used.

### **Refering to Merge Commit's parents**

`^parentnum`

```shell
git show master^      #shows a commits first parent(same as ~)
git show HEAD^2       #shows a commits second parent if theres one
git show HEAD^^       #shows the first parent's first parent
```

> `^` and `~` can be combined freely, in any depth or complexity.

### **Git Tags**

* **Lightweight**
  * `Simple reference` to a commit.
* **Annotated**
  * `Git object`
  * `Meta data` : tag author information, tag date, tag message, commit ID
  * Can be `signed` and `verified` with GNU Privacy Guard

```shell
git tag             #View all tags in the reporsitory
git show <tagName>
```

#### **Tagging a commit with a LightWeight tag**

```shell
git tag <tagname> [<commit>]    #<commit> defaults to HEAD

git tag v1.0                    #Tagging the current commit
git tag v0.1 HEAD^              #Tagging the previous commit
```

#### **Tagging a commit with an Annotated tag**

```shell
git tag -a [-m <msg> | -F <file>] <tagname> [<commit>]  #<commit> defaults to head

git tag -a -m "includes feature 2" v2.0               #git show <tagname> shows the tag information and after the commit information
```

#### **Pushing tags to the Remote Repository**

```shell
git push <remote> <tagname>    # to push a single tag
git push <remote> --tags       # to transfer all tags
```

## Branches

A `Branch` is a set of commits that trace back to the project's first commit.

### **`Benefits` of Branches**

* Fast and easy to create
* Enable experimentation
* Enable team development
* Support multiple project versions

### **`Topic` and `Long-Running Branches`**

* **Topic** → A feature,bugfix,hotfix,configuration (small changes)
* **Long-lived** → master,develop,release,etc (longer lifetime)

### **Commands**

```shell
git branch          #list of branches in the project (current branch marked with *)
git branch <name>   #creating a new branch (branch label) with the given name, pointing to MASTER

git checkout <branch_or_commit>    #1.Updates the HEAD reference poiniting to the branch
                                   #2.Updates the working tree with the commit's files

git checkout -b <name>             #combination of git branch and git  (only for new branches)
```

> Commits on the new branch are independent from the commits on the master branch. That enables independent work.

### **Detached HEAD**

Checking out a commit directly, instead of checking out a branch label leads to a `detached HEAD` state, when the `HEAD reference` instead of pointing to a branch label, points to the `SHA-1` of a commit. You can temporarly view the files of the commit, but for changes you should create a new branch originated from that commit and check out that branch. That puts the project back to normal state, where the `HEAD reference` points to the new branch label.

### **Deleting a Branch label**

* Deleting a branch label normally doesn't delete any commints
* Branch labels normally deleted after the topic branch has been merged
* Deleting a branch without merging -> `warning` (commits on that branch won't belong to any branch)
* `Dangling commits` : Commits that don't belong to any branch (will be deleted by `Git's Garbage collector`)
* Leaving `Dangling commits` can be undone by creating a new branch pointing to the `Danling commit` (ref log + new branch)

```shell
git branch -d <branch_label>  #Deletes the referenced branch label
git branch -D <branch_label>  #Force deletes the branch label, even if it leaves some commits without a branch

git reflog   #Local list of recent HEAD commits
git checkout -b <branch_name> <SHA-1 of the recently deleted commit> #Undoing branch delete causing dangling commits
```

## Merging

Merging combines the work of independent branches. Usually involves a `topic branch`, and a `base branch`(usually longer).

**Main types of merges:**

* Fast-forward merge(FF)
* Merge commit
* Squash merge
* Rebase

### **Fast forward merges**

* Moves the master label to the tip of the topic branch
* Only possible if no other commits were made to the base branch since branching
* `Linear commit graph`

```shell
git checkout <base_branch>
git merge <topic_branch>         #attepmting a fast forward merge is the default
git branch -d <topic_branch>     #optionally deleting the topic branch label
```

>Performing an `FF Merge`.

### **Merge commits**

* Combines the commits  at the tips of the merged branches
* Places the result in the merge commit, with two parents
* `Non-linear commit graph`

```shell
git checkout <base branch>
git merge <topic branch>              #then accept the default merge message or modify it 
git branch -d <topic_branch>   

git merge --no--ff <topic_branch>     #force merge commit, even if it's fast forwardable
```

> Performing a `Merge commit`.

### **Resolving a merge conflict**

* `Merge conflicts` occour if different branches change the same parts(`hunks`) of the same files.
* Small, frequent merges are advised
* Involves 3 commits:
  * `ours` / `mine`: Tip of the current branch
  * `theirs`: Tip of the branch to be merged
  * `merge base`: A common ancestor

**Steps to resolve a merge conflict:**

* Checkout `ours` branch
* Merge `theirs` branch
  * `CONFLICT` - Both modified the same `hunk` in the same file
  * The `conflicted file` is in the working tree with conflict markers
  * FIX `conflicting file` with a text editor
* Stage `conflicting file`
* Commit merge commit
* Delete the `theirs` branch label

```shell
git merge <theirs_branch>   #CONFLICT occours and git notifies us
git status                  #view the status of the conflict
```

## Tracking Branches

* `Tracking branches` are named `<remote_repo_name>\<branch_name>`
* They represent the state of the remote repository, but are only updated with network commands like `push` and `pull`
* They aren't always synchronised

```shell
git branch --all             #displays all branches including tracking branches
git log <remote> --oneline   #<remote> can be used instead of <remote>/<branch> in git commands (because of the default remote tracking branch: remotes/origin/head)
git remote set-head <remote> <branch>   #set the default remote tracking branch
git status                              #includes tracking branch status(only locally, still need push or pull)
git log --all                           #combined log of local and tracking branches
```