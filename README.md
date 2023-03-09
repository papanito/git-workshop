# Git Workshop for Newbies

## Intro

This is a small workshop we created for new git users. The goal of this is to learn the "git-flow" and the most important commands.

## Before we start

Throughout the workshop we will use the (git) bash

> Remark for Windows users: You may perform the excrcise also on a Powershell console

## git init

> **Goal**: Learn to initialize a git repo

1. Create a **private repo** on your repo server (github, gitlab, github), name it `java-example`.

   Your git server usually presents you with some instructions but the repository is empty (no files, no branches)

1. Copy Folder `\\Tutorials\git-tutorial\java-example` to `$HOME/sandbox/`
   TODO: update

    ```bash
    cp -a java-example $HOME/sandbox
    ```

    Check the folder with `ls -a` and see which files you have

1. Initialise the project

    ```bash
    cd $HOME/sandbox/java-example
    git init
    ```
    Check the folder again with `ls -a` and see what has changed: There is now a folder `.git`.

    within this folder have a look at `.gitconfig`

1. First we tell it to add all files from the example project to be under source control

    ```bash
    git add --all
    ```

1. Check your git status which should show 4 "new files"

    ```bash
    git status
    ```

1. Then we commit these changes and tell git what are these changes about (commit message)

   ```bash
   git commit -m "Initial Commit"
   ```

1. Push the commits (project) to your repo:

    ```bash
    git remote add origin https://<username>@github.com/<username>/java-example.git
    ```

   A `remote` is a pointer to repository (server) - in this case github `https://<username>@github.com/<username>/java-example.git`

   `origin` is an alias for the remote location

   `main` is the branch name

1. View the .git/config file again and see what has changed since before

1. Push changes to github

    ```bash
    git push -u origin main
    ```

1. Check in your project in github

   You should see that
   * the repo contains now the files you commited
   * there is a main branch
   * there is a single commit with text `Initial Commit`

1. We don't need this anymore so delete `$HOME/sandbox/java-example`

    ```bash
    rm -rf java-example
    ```

## Form Teams

Form Teams of two or three.

### git clone

Got to github Tutorials Project and clone [java-example](https://github.com/papanito/java-example.git) to `$HOME/sandbox`

### Add Team App Class

1. One of the Team Members create a branch and adds a new class

    ```bash
    git checkout -b myshortname/randomfeature
    cp SimpleGUI.java MyTeamsRandomAppName.java
    git add .
    git commit -m "Added <Teamname> App Class"
    git push
    ```

1. Create Pull Request, merge, checkout main and pull

   ```bash
   git checkout main && git pull
   ```

### Create Buttons with git add, commit and fixup, squash.

1. Create a feature branch, edit your Teams Class and add a button labeled with your First Name

   ```bash
   git checkout -b myinitials/mybutton
   ```

1. Add a new Button

    ```java
    JButton myNewButton = new JButton("My First Name");
    frame.getContentPane().add(myNewButton);
    ```

1. Add and commit the file

    ```bash
    git add .
    git commit -m "Add a new Button with Name My First Name"
    git push
    ```

1. Now prepare for pull request

    ```bash
    git fetch
    git rebase origin/main
    git push -f
    ```

1. Create a Pull Request on github.

    Usually, you should write a Skype/Teams Message to your Reviewers and paste the link with the PR and ask for ack

1. Review your Team Mates Pull Request and **comment** that he missed the "Last Name", **deny the PR**

    > You "forgot" to add your Last Name, add your second name

1. Fix the problem by adding the Last Name and then `squash` the changes

    ```bash
    git add .
    git squash HEAD
    git push
    ```

1. Take a look to your commit Messages and notice the `squash!` prefix

    ```bash
    git log
    git log --pretty=oneline
    ```

1. Now your Team Mate should **review your PR again** after the squash.

    Review your Team mates PR and comment that he missed the comment section, **deny the PR**

    > You "forgot" to add the comments in your code. 

1. Add a comment before your code line. Then `fixup` the commit.

    ```bash
    git add .
    git fixup HEAD
    git push
    ```

1. Take a look to your commit Messages and notice the fixup! prefix

    ```bash
    git log
    git log --pretty=oneline
    ```

1. Now your Team Mate should review your PR again after the fixup and **accept the PR**

1. Usually, you should write to your Team Mate via Skype/Teams an "ACK" Message so he knows he can continue.

1. Prepare for Merge one by one

    ```bash
    git fetch
    git rebase -i origin/main
    git push -f
    ```

1. If you notice merge conflicts, review each merge conflict by remove/keep the conflicting section.
   1. Read the output, git gives you carfully. Git will mostly show you the right workflow.

   2. After remove the conflicts, save the file and `git add .` the file. Then `git rebase --continue`

1. After you successfully finished the rebase, try to `git push` your branch. It will not work, because your branch changed history, thus is no longer aligned with the origin branch. So you have to `git push -f`, effecively overwriting the history of your feature branch.

   **So never share a branch with others, never push a branch you don't own, you will create race conditions**

1. You can tag your feature branch HEAD before merging the pull request, omitting the shortCommitHash, but you have to ensure that you don't rebase after that otherwise the Tag reference will be lost (Rewriting History).

    After interactive rebasing, tag and push tag

    ```bash
    git tag -a v0.1-yourName -m "MyName Version 0.1"
    git push origin v0.1-yourName
    git show v0.1-yourName
    ```

1. Now merge your PR and delete your remote branch

### Stash

You forgot to switch to the main branch to create a new one from the HEAD of main, staying on your local outdated feature branch.
Edit the Class and add another comment. Save the class. Try to checkout main

`git checkout main`

So you want to include your changes in another feature branch, branching from main

```bash
git stash
git checkout main
git pull
git checkout -b myinitials/myNewfeature
git stash list
git stash apply
git add .
git commit -m "My Changes"
```

### Reset to HEAD

You want to discard the changes without keeping them

`git reset --hard HEAD`

You can now also reset `--soft` so the tip of the branch will be reset to HEAD but your changes will stay to be commited. This applies only for uncommited changes, e.g. after `git add .`

If you want to reset to a previous commit, use

`git reset --hard HEAD~1`

This will reset to the previous commit. Check with `git log`

### Tag a specific commit

Now you want to tag the second commit of main HEAD before with your name and beta version

```bash
git checkout main
git log --pretty=online
git tag -l
git tag -a v0.1-yourName-beta -m "MyName Version 0.1 Beta" shortCommitHash
git tag -l
git push origin v0.1-yourName-beta
git show v0.1-yourName-beta
```

### Checkout files from other branch

The `git-checkout` command can be used to update specific files or directories in your working tree with those from another branch, without merging in the whole branch.

1. Each team member creates a new branch and adds a file {myhortname}.txt` which contains some text

   ```bash
    git checkout -b {myhortname}/selective-checkout
    echo My name is $(whoami) > $(whoami).txt
    git add .
    git commit -m "Add text file for {myhortname}"
    git push
    ```
1. Now checkout the text file from the branch of your team member

    ```bash
    git checkout {teammates-shortname}/selective-checkout -- {yoursteammates-shortnamehortname}.txt
    git commit -m "Add text file for {teammates-shortname}"
    git push
    ```

More info at http://nicolasgallagher.com/git-checkout-specific-files-from-another-branch/

### cherry-pick

With `cherry-pick` you can pick a single commit from another branch into your branch

1. Each team member creates a new branch and adds a 2 files which contains some text

    ```bash
    git checkout -b {myhortname}/selective-checkout
    echo My name is $(whoami) > $(whoami)_1.txt
    echo My name is $(whoami) > $(whoami)_2.txt
    git add .
    git commit -m "Add text file for {myhortname}"
    git push
    ```
1. Now cherry pick the commit from your teammate into your branch

    ```bash
    git cherry-pick {short hash of commit of your teammate}
    ```

    With the option `-x` you treat the cherry-pick as a bugfix e.g. the commit will containe something like `{commit message} (cherry picked from commit 3158f27)`

1. Check the content of your workdir

    ```bash
    $ ls
    ...
    $(teammates-shortname)_1.txt
    $(teammates-shortname)_2.txt
    ....
    ```

1. Check your git log so you can see the commit

    ```bash
    git log --oneline
    ```

More info at https://www.ralfebert.de/git/cherry-pick/

### Git LFS

If you want to commit large (binary) files over 100MB, you have to use git large file support
Check in your Repo Settings if Large File Storage is checked. Initialize git lfs support locally (only once)

`git lfs install`

1. Create a file containing 1Mb zeros in your repo:

    ```bash
    dd if=$HOMEev/zero of=myname.bin count=1024 bs=1024
    ls
    ```

2. Ensure git lfs tracks bin files, this is only an entry in the file `.gitattributes`

    ```bash
    git lfs track "*.bin"
    cat .gitattributes
    git add .gitattributes
    ```

3. Add psd files as well

    ```bash
    git lfs track "*.psd"
    cat .gitattributes
    git add .gitattributes
    ```

4. Now you can work with the files as usual

    ```bash
    git checkout -b myinitials/largefile
    git add .
    git commit -m "myNamebinfile"
    git push -u origin myinitials/largefile
    ```

### Git Branches

Now you want to delete your remote and local largefile branch

```bash
git branch -a -vv
git checkout main
git branch push
git branch -d myinitials/largefile
git push origin :myinitials/largefile
```

### Git Bisect

We want to find out a specific commit which introduced a class with a specific name which simulates a commit which broke something.

1. Choose some Name, somewhere in the middle of the Teams

1. Checkout main and `git pull`

1. Tell git that HEAD is bad, e.g. it contains the specific name

    ```bash
    git bisect start
    git bisect bad
    ```

1. Choose a tag which clearly was created before the name occured in the code, e.g. a version which did not include the bug. You can also use a commit hash instead of the tag, known to not include the bug.

    ```bash
    git tag -l
    git log --pretty=oneline --abbrev-commit
    git bisect good v0.1-somenamebefore
    ```

1. Now check if the name is included by ls your folder and see if the class is there and contains the button. This only simulates to compile and unit test the code in your repository to find out if the bug is included or not.

    ```bash
    ls
    cat theTeam.java
    ```

1. If it contains the class and button, e.g. its the broken code, type `git bisect bad`, if not, type `git bisect good`

1. Repeat the process, compile and test the code (or cat the file). If there is nothing more to inspect, `git bisect` will spit out the commit hash of the commit where the bug was introduced.

1. You can now either analyze the changes in the commit by checking out the respective commit `git bisect reset badcommithash` or copy the commithash for later use.

1. Checkout the bad commit hash

    `git bisect reset badcommithash`

1. Now blame the file which introduced the bug

    `git blame TheBadClass.java`

### The .git folder

Inspect the folder

```bash
tree -L 1 .git
tree -L 3 .git/refs
tree -L 3 .git/hooks
cat .git/config
cat .git/HEAD
cat .git/logs/HEAD
```

## Links
