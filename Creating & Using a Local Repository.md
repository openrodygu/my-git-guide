By creating a simple local git repository, you can begin utilizing version control and branching tools.

I will be explaining how I use git through the *Command Line Interface* (CLI). I personally believe it helps you visualize what exactly is being done step-by-step. Once you are comfortable with these topics, you can use *Graphical User Interfaces* (GUIs) like the Virtual Code Studio extension.

Git is useful, even when working solo, to provide version control of your work. 
- By **committing** changes, you are creating snapshots of the project at a time. So if you decide you need to start over, you can revert to a previous state instead of starting over from nothing.
- Another feature (which is why I use it the most for) is branches. It is the ability to create a separate "**branch**" off of a snapshot (or `commit`) that I can freely experiment with, without jeopardizing the functionality of a now separate `main` or known stable version of the code.

Throughout this guide, I heavily encourage you to investigate the [documentation from git](progit.pdf) *(the "Pro Git" book is an awesome resource I highly recommend to read if you are in any way curious about more, and its FREE!)* themselves about anything you want to learn. I also recommend playing the CLI game called **"Git Gud"** if you are a more tactile learner. Its on Steam and it lets you actually write and run git commands for challenges. It's only costs $2.99 USD.

If you want to quickly lookup any CLI options for any git command (or most––if not all––other CLI programs) you can try `git <commmand> --help` or `git <cimmand> -h` for short. For example:

```bash
# Displays the command options and structure for the git CLI command of `git status`
git status --help
# or, for short
git status -h
```

# 1. Navigate to chosen directory & initialize git repository:
	
- The Git repository can be initialized brand new directory or an already working directory.
- Once initialized, a `.git` folder will be made under that directory. This is the git files that designate that directory and all subdirectories to become a *git repository* (or *repo*  for short).
	- This is where all the configurations and version history will be stored and updated.
	
```bash
# If starting a brand new empty repository for a new project

# Navigate to a parent directory where you want the new folder for the project
	# NOTE: Windows uses `\` for their directory paths, git only uses `/`
	# You will need to manually ensure that you are using `/` for git commands
	# OR, you can use the "Git Bash" terminal emulator that automatically fixes paths
# For these code snip-bits, I will assume you are using the "Git Bash" terminal emulator
cd ~/git-projects/
 
# This will initialize the git repo under a new directory `~/git-projects/my-new-git-project/.git`
git init my-new-git-project
```
	
```bash
# If initializing a already populated working directory with existing work

# Navigate to the fodler containing your project files
cd ~/projects/my-working-project/

# This will create the `.git` folder under the current directory `~/projects/my-working-project/.git`
git init
```
	
# 2. Commit files:
	
-  Git works by tracking the changes of files through different `commit` snapshots. 
- To store your first "root" node for your project, you need to create your first commit or snapshot of the working directory.
- To commit any changes, you first need to `stage` changes you want to be recorded on your next `commit`.
- ***TIP:*** *Make it a habit to record detailed and concise commit messages. Your future self will thank you. And remember, **its free!** Commit as often as you need.*
	
```bash
# list any 'unstaged' or umtracked changes
git status --untracked-files
## or, for short
git status -u

# To add untracked files, you use `git add`
## To add specific untracked files, use:
git add example.txt
# To add ALL untracked files, you can use:
## This will 'stage' all modified files for the next commit
git add --all
## or, for short
git add -A

# Finally, you can `commit` your 'staged' files to create a snapshot of your project with these files and at this state:
git commit --message 'Initial Commit Message! :)'
## or, for short
git commit -m 'Initial Commit Message! :)'
# You can even add new lines on the message to add more detail on what changes were made:
	# NOTE: To add a new line on a terminal emulator, you can press `SHIFT + ENTER`
git commit -m 'Initial Commit Message! :)
- Initialized the git repository
- Added `example.txt`
- Commited my first commit'

# You can confirm that the changes were made by checking `git status`
git status

# You can also see a log or history of all the commits you have done so far:
git log
## or, for a little slightly more visual display
git log --graph
```
	
## 2.1. Undeclared Local Author Error
	
- If you committed your first commit to a local repository for the first time on that computer, you might be prompted with an authorship error.
- To resolve it, follow the instructions provided on the error message.
	- Modify the git config to add your Username and Email
		- I recommend using your professional identifiers (Username=Full Name, Email=Work email) *if you ever wish to share your local repositories with others so they can know who made the commits and how to contact you if needed.* 
	- Then amend your last commit with the new authorship.
		- It will open the commit file on a text editor, simply save and quit that file. Or you can modify the commit message if you wish to.
	
# 3. Branches:
	
- Branches can be used to "fork" your project at the current (or previous) state to experiment freely without jeopardizing the `main` or stable version of the project.
- Once you are satisfied with the state of a branch and you would like to `merge` it to the main branch, you can `git merge` (discussed on step 4) the two branches into one. Or branch off of the experimental branch to experiment further.
- You can continue to work on different branches in parallel. 
	- *For example:* You are working on implementing a new feature, but suddenly you need to go back to main and fix a bug for production. 
		- You can simply `git switch main` and commit a bug-fix then switch back to your experimental branch to continue where you left off.
- **TIP:*** *Create branches when implementing a new **untested** feature to your project. Or if you are experimenting with changes you might want to revert later on. And remember, **its free!** Make as many as you need.*
	- **P.S.:** *Just be mindful when working with a team. You probably don't want to be pushing all of your experimental branches. If you do want to experiment, keep the branches local and only push on an agreed remote testing branch.*
	
```bash
# Assuming you are already in your git repository directory. If not, navigate to it:
cd ~/git-projects/my-git-project

# To view the branch you have currently `checkout`, you can use:
git branch

# If you have multiple, `switch` branches to the one you want to `branch` off of:
# If you were on another branch:
git switch main
# Else, skip this setp

# Now to create a new branch from the current state of the working directory:
git switch --create new-branch
# or, for short
git switch -c new-branch
# This will have created a new branch called "new-branch" and automatically `switched` the working directory to it

# You can run `git branch' again to see that you have created a new branch and that it is currently selected
git branch

# You can now modify this verion of the project however you would like without altering the `main` branch
# Once you hava made changes, you can `stage` (`git add`) and `commit` (`git commit`) just like before, but now they will be stored on this new branch. Independent of main.
git add --all
git commit --message 'Fist commit on new-branch :O'

# To see the full "tree" or all commits on all branches (it will look more like a tree, the more parallel commits you have on multiple branches), you can run:
git log --all --graph
```
	
# 4. Branch Merge:
	
- Once you are satisfied with your experimental branches (usually when you are confident you have reached a *stable* point on the project/feature) and you wish to update your `main` branch, you can merge branches into one consolidated branch.
- This is useful when you have finished testing and/or you want to `push` to a `remote` git repo for collaborative work with a team.
	
```bash
# Navigate to the branch you want to `merge` to
# For example: we are merging our previous `new-branch` to `main`
git switch main

# Then we merge
git merge new-branch --message 'Merging `new-branch` to `main`'
# NOTE: Merging will not automatically delete your source branch. You can choose to keep the `new-branch` to continue developing on it, or delete it once you merged

# To delete the merged branch, you use:
git branch --delete new-branch
```
	
## 4.1 Merge Conflicts
	
- If you tried to merge two branches that have two differing versions of the same file (i.e. you modified an already existing file from the branch you are merging to) you will get a `conflict` error when trying to merge.
- Honestly, at this point you should be comfortable enough with the basics of git. I recommend using a Graphical User Interface at this point. Personally, I would use Virtual Code Studio with the git extension. 
- Resolve the merge conflicts and commit.