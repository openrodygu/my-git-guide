By creating a local remote git repo, you are able to easily clone your repository in multiple directories and be able to sync them all with any changes `pushed` to the remote repo.

A practical use-case for this (which is why I started using it) is to have a `remote` backup on a cloud drive, such as OneDrive, and have a `clone`d repository on a robot's UD1 folder for easily deploying and testing code on Roboguide. And most importantly, have a remote back up if Roboguide formats the robot or your computer dies.

This is the entire point of using a service like Github or Gitlab. Nevertheless, it is useful to be able to initialize and deploy custom local git remote repos for your personal or miscellaneous work. And it will help you understand what is happening on the background.
- Most likely use case, if you want to work on projects that wont be collaborative and/or niche to your needs, like Roboguide deploy testing.

# 1. Initializing a bare git repository
	
- A bare repository is a variant of a regular git repository
- You can not access the files from the project as it only stores the nodes and changes to save storage space. (this repo will be running on a cloud or drive meant only for remote `pull`/`push` actions)
	
```bash
# Navigate to a directory you would like to store your remote git repos
# I would suggest doing so on a cloud storage drive like OneDrive
cd ~/git-remote-repos/

# Create your bare repo
git init --bare new-remote-repo
```
	
# 2. Link the new remote repo to your working repo
	
- Since the bare git we just created is literally bare empty, we need to link it to an existing working repo
	
```bash
# Navigate to working git repo
cd ~/projects/git-project/

# Add the bare remote repo as the remote for this git project
# We will call this remote repo as `origin`
	# From now on, `origin` will refer to this specific remote on that directory
	# Make sure to use the full pathname
git remote add origin 'C:/Users/username/git-remote-repos/new-remote-repo'

# Now we can push to this remote repo
git push --set-upstream origin main
# This will set the upstream to be the remote repository on origin
	# This means that in the future we can just run `git push` or `git pull` without specifying which remote repo to push/pull from
	# the main at the end marks that we only want to `push` or sync the main branch from 'git-project'
	# You can choose to push any branch to the remote
```
	
# 3. Cloning from remote repository
	
- Since we now have a populated remote repository, we can now create clones of it
	
```bash
# Navigate to parent folder where you want to hold your cloned repo
cd ~/projects/

# Clone the remote repo
git clone 'C:/Users/username/git-remote-repos/new-remote-repo' cloned-git-project
# This will create a linked instance of the remote repo
# From here on, you can treat this cloned repo as a regular repo. Edit, Add, Commit and Banch.
```
	
# 4. Push/Pull
	
- If you have previously cloned or pushed to a remote repo, your local instance should already be linked to the remote repo.
	
```bash
# To update the remote repo to your local instance
	# To sync only one branch, you can specify the branch name after the remote name
git push origin main
# or, to push all branches
git push --all origin

# To update your local instance to the remote repo
	# To sync only one branch, you can specify the branch name after the remote name
git pull origin main
	# or, to pull all branches
git pull origin
# This will automatically download any changes, stage them, and marge them to your local head.

# If you want to only download remote changes and mannually commit them to your local instance, you can `fetch` them
	# `fetch` will only download any remote changes for the active branch
git fetch origin
```
	
# 5. Using Tags
	
- Tags are a metadata object for a commit snapshot that adds a "tag" for easier cataloging.
- The most common use of tags is to mark and catalogue release versions.
	- If you are sharing your repo to someone else and want them to have a specific version (or snapshot) of your project, you can mark that commit with a tag.
	- They can then clone your repo at that tag version.
	
```bash
# Navigate to your working git repo
cd ~/projects/git-project/

# If you want to assign a tag to the current state of your project:
# For example, we will create a tag called `v1.0`
	# You can choose to annotate the tag with a message if you want, but not mandatory
git tag --annotate v1.0 -m 'Fist Stable Release'
# or, without message
git tag v1.0

# If you want to assign a tag to an older commit (i.e. not HEAD):
# For example, the commit ID will be `748e3dba0c0f75a28fc6991cc059998bfb79bb53`
git tag --annotate v0.1 -m 'Pre-Release Version' 748e3dba0c0f75a28fc6991cc059998bfb79bb53
```
	
- So far these tags are local to your git instance only
- If you want to share these tags, you will either need to clone this repo or push these tags to the remote repo
	
```bash
# To upload your local tags to a remote repo:
git push --tags origin main
# or, to upload all tags from all branches
git push --tags origin

# To download remote tags:
# To only download remote changes and tags
git fetch --tags origin
# or, to pull from remote and include tags
git pull --tags origin
```
	
## 5.1. Checking out a tag
	
```bash
# Navigate to parent location of your desired git location
cd ~/projects/

# To clone from a specific tag
git clone --branch v0.1 'C:/Users/username/git-remote-repos/git-project' Pre-Release-Version
# This will clone project and checkout that specific tagged commit

# If you want to start your spinoff from that version, you can then create a new branch
git switch --create Pre-Release-Experimental
```