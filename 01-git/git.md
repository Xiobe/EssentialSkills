# Version Control Systems
## All is Code
An important part of a computer system is its configuration and its applications. Applications are compiled from source files and read out their configuration files. Over time these file tend to change because we want to use the computer system in a different way.

## Centralized vs Decentralized Versioning
Git is decentralized versioning system. The difference with a centralized versioning system is that you have a copy of the complete repository on your local system.

This also means you are personally responsible to synchronize your local repository with the centralized one.

Another important aspect of it is that you have to understand that if a system gets lost or stolen the finder or thief has a complete copy of the software in his or her hands. Therefore it is important to have an encrypted hard drive.

## Change Tracking
To keep track of the code one could simply make a copy of the file and rename the file each time but this would be tedious and chances somebody makes a mistake are high. To solve this a piece of software got developed called version control system or revision control.


In git a change is tracked with a 40 character sha1 hash. Every change the hash is recalculated.

# Git
## Git Structure
The git tracked directory is called a repository. Your repository consists of three trees maintained by git.  The first tree is called the working directory and holds the actual files. The second tree is called the Index which acts as a staging area. The last tree is called the HEAD. It is the last commit you have made.


To put it simple, you work with your files in the working directory. Once you are happy with the change, you add the files to the index and when you are ready to share it you can commit it to the HEAD.

## git commit
A git commit is the final step of tracking. As you can see in the message underneath a commit has an identification ID, which is an MD5 hash, an author, a timestamp and a message.
```
commit d7fc26bf3f1b8293a239fbfadce93e01d8f0219d
Author: user <email address>
Date:   day_of_the_week Month day hh:mm:ss yyyy timezone

	Commit message
```
## git commit --amend
It sometimes happens that right after a commit you realise that you have forgotten something that needed to be part of the same commit. 

1. Make the change you need to make
2. git add file to add the changes
3. git commit --amend

A prompt will follow with the question if you want to change the commit message or if you want to keep it.

## Change the commit message
Sometimes we make typos and realise it when we pressed enter already. You can change it by doing git commit --amend. You will be prompted and can change the message.

## Start all over again
If your repository goes foobar, it is better to start over again, you just delete it and it is gone.

# Configuration of Git
## Global Configuration
If you have no global user name and email set up you will get a message like the following
```
[master (root-commit) ID] git installation, configuration and /etc script
 Committer: user name <username@hostname>
Your name and email address were configured automatically based on your username and hostname. Please check that they are accurate. You can suppress this message by setting them explicitly. Run the following command and follow the instructions in your editor to edit your configuration file:


git config --global --edit


After doing this, you may fix the identity used for this commit with:


    git commit --amend --reset-author
```

If you have thus committed without setting your user name and email address you get a message but you can fix it with
```
git commit --amend --reset-author
```

### User and Email address
When you work on a system like your personal laptop you probably want to configure your git user and email address once and for all. To do this you do
```
git config --global user.name “user name”
git config --global user.email user.name@email.org
```

### Editor
Which editor you use is totally up to you. To set your favorite editor you do
```
git config --global core.editor vim
```

### Checking your global configuration
If you have a doubt about your global configuration you can always check it
```
git config --list
```

## Local configuration
For a specific repository you don’t set the --global
```
git config user.name “user name”
git config user.email user.name@email.org
```

# Working With Local Repositories
The first step to track files is of course to have a repository but for completeness sake we wanted to add this to the document. In the example underneath you see the error that occurs when you have no git initialized repository
```
$ touch file.txt
$ git add file.txt
fatal: Not a git repository (or any parent up to mount point /current/path)
Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYSTEM not set).
```

## Create a new repository
Create a new directory, open it and initialize the repository.
```
mkdir projectname
cd projectname
git init
```

## Add a file to the Index
```
git add file.txt
```
Or if you want to add an entire directory
```
git add *
```

## Remove a file from the Index
```
git reset -- files
```

## Tracking what files are not checked in
To track what files are not checked in you run
```
git status
```

## Ignoring files in your repository
Versioning is amazing technology to track changes but not every type of file possible. The system is great for text based files like source code and configuration files but not for compiled files.
This is why you need to specify a .gitignore file with instructions what to ignore. For example documentation is generated based on tags in the source files and thus it makes no sense to track it.
```
touch .gitignore
echo docs/ >> .gitignore
```

For some files it is obvious like a .exe, .pyc, .class files but often people forget that images and application files (style Microsoft Office) are also binary files. An easy test is to read the file into a text editor (vim, notepad) and check if everything was interpreted correctly. It is a test that will normally be possible to execute on any operating system.

Since it is likely you will be working on multiple projects it can be a good idea to set up an ignore policy on a global level
```
cd $HOME
touch .gitignore_global
echo .exe >> $HOME/.gitignore_global
echo .pyc >> $HOME/.gitignore_global
echo .pyc >> $HOME/.gitignore_global
echo docs/ >> $HOME/.gitignore_global
git config --global core.excludefiles $HOME/.gitignore_global
```

## Commit to HEAD
```
git commit -m “reason for commit”
```

## Checkout a local repository
When you are cloning from a local repository
```
git clone /path/to/repository
```

# Working With Remote Repositories
## Configuring the remote repositories
```
git remote add origin server
```

## Downloading from remote repositories
If the repository is on another server you will want to
```
git clone user@host:/path/to/repository
```

## Pushing your changes to a remote repositories
```
git push origin master
```

# Working With Branches
Branches are used to create copies to develop isolated from each other. The master branch is the “default” branch. It is created when you initialized the repository. Once a feature is developed the branch is merged back into the master.

## Creating a new branch
```
git checkout -b newfeature
```

## Switching between branches
```
git checkout branch
```

## Delete a branch
```
git branch -d newfeature
```

## Push a branch to the remote server
```
git push origin branch
```

## Committing the wrong branch
Sometimes it happens you are in one branch and think you are in another. If you have just committed the wrong branch you can rectify this by undoing the last commit but keeping the changes.

1. git reset HEAD~ --soft
2. git stash

Now we go to the correct branch
1. git checkout branch
2. git stash pop
3. git add file
4. git commit -m “your message”

# Update and Merge
## Synchronize your local repository
```
git pull
```

## Merge a branch to your active branch
```
git merge branch
```
If you want to avoid conflicts you can use diff
```
git diff sourcebranch destinationbranch
```

If you are diff’ing staged files you need it to give the parameter --staged or you will see nothing.
```
git diff --staged sourcebranch destinationbranch
```
## Undoing local changes
### Replacing one file
```
git checkout -- filename
```
### Replacing all files
```
git fetch origin
git reset --hard origin/master
```
# Tags
Tags are useful if you are putting out versions in the sense of version 1.0, 1.1, 2, ect. You put a tag on a commit ID. The ID is the first 10 characters of the commit.
```
git tag 1.0.0 commitID
```

# Logs
## Activities by author
git keeps a log of the repository, if you want to to see what a specific author has created
```
git log --author=user
```

## Graph representation
```
git log --graph --oneline --decorate --all
```

## Figuring out when files have changed
```
git log --name-status
```

## Reflog
The git reflog stands for reference log, it it the record when tips of branches change
```
$ git reflog
d7fc26b HEAD@{0}: commit (amend): git installation, configuration and /etc script
bf83dbd HEAD@{1}: commit (initial): git installation, configuration and /etc script


$ git log
commit d7fc26bf3f1b8293a239fbfadce93e01d8f0219d
Author: erik vanderhasselt <erik.vanderhasselt@xiobe.net>
Date:   Wed Sep 21 16:07:43 2016 +0200


    git installation, configuration and /etc script
```
As you can see the log tells you about the commit but the reflog tells you that that initial commit actually consists of multiple steps.

# Sharing Private Code With Your Own Remote Git Server
If you need to share code with other people on a private server within an organization you need to set up a repository server. This is basically a server that contains a copy and is considered the origin server.


In the setup we will work with ssh keys to identify the machines.

## Installing git
```
sudo apt-get install git-core
```

## Create a user for git and set the password
```
sudo useradd git
passwd git
```

## Generate a private key on the programmer’s machine
```
ssh-keygen -t rsa
```

## Copy over the public key to the remote git server
```
cat ~/.ssh/id_rsa.pub | ssh git@server “mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys”
```
## Creating a repository on the remote git server
Create a directory where you want to create your projects.
```
sudo mkdir /projects
sudo chown git:git /projects
```

Create a directory for your project in our example it is called myproject
```
sudo mkdir-p /projects/myproject
```

## Initializing git on the project directory
```
git init
```

## Adding your files to the repo
On your machine you have to configure for the local repository the server.
```
git remote add origin
ssh://git@remote-server/repo-path-on-server.git
```
## Uploading your project to the remote server
To upload your work to a remote server you use git push
```
git push origin master
```

## Keeping your local repository synchronized with the remote server
Since the idea is that you work together with others on projects they will push their updates to the server too and thus it is important to resynchronize your local repository.
```
git fetch upstream
```
# Sharing Public Code With Github
Github (https://github.com) is a public repository server. A lot of people are sharing their code for free. If you don’t like Github you can check out Bitbucket (https://bitbucket.org/) as a public alternative if you don’t want to host a git server yourself.

## Dual Factor Authentication
When you make an account make sure you use dual factor authentication, it uses the Google Authenticator which makes it easy. Don’t forget to save your backup keys in case something goes wrong.

## Pulling over https
https is available for each project on github. When you git clone, git fetch or git push you pass it the remote repository and will be asked for your username and password. When using you have two-factor authentication enabled, you must create a personal access token to use as password when authenticating to github.

1. To create your personal access token you need to:
2. log in and 
3. go to “settings” (click on your avatar), 
4. in the user settings choose “personal access tokens”
5. Click “generate personal access token”
6. Give the token a descriptive name
7. Set the right scope
8. Click “Generate Token”

This returns a personal access token you use as password.

## Creating a repository
To create a new repository in your github you click on the “+” sign next to your avatar and choose new repository.

You will be prompted for a name and will be asked if it is a private repository or a public repository. A public repository is visible to the whole world where a private repository is only visible to the repository owner and the collaborators indicated by the project owner.

To make the world a better place and code auditable it is better to make public repositories if you need a public git.

To initialize the repository, select the “initialize this repository with a README” option and click “create repository”. 

## .gitignore files
Since github is public it is important to read the section on .gitignore files above. You don’t want to publish binary files if they are not necessary.

## Forking a repository
A fork is a copy of a repository which allows you to experiment with the repository without affecting the original project. Forks are used often to propose a change in the project or as a starting point of your own project.

### Forking for a project change
If you want to improve something or make a bug fix you create a fork. After you have made the necessary changes you can propose your fix by submitting a pull request to the owner.

### Forking
Github provides a special project to try out forking it is a called octacat/Knife-Spoon. When you are on the octacat/Knife-Spoon page you can click on the fork-button in the top-right corner to fork the project.

### Creating your local repo
After you have created a fork you will find a copy of the repo on your github page. To make changes to the code you will need to create a local repo on your system. For the example we stay with the Knife-Spoon project.
```
git clone https://github.com/your-username/Knife-Spoon.git
```

### Keeping a fork synced
Your remote server is the project in your repository. You can see this with
```
git remote -v
```

This will return your remote origin server 

While you are making changes, changes might happen on the original project and to get these changes you need to add upstream servers. This is done with
```
git remote add upstream
https://github.com/octocat/Knife-Spoon.git
```

When you do git remote -v now you will see your origin server and upstream.

## Submitting a Pull Request
Pull Requests are made to tell the owner of a repository that a change exists. The idea is is to discuss the pull request and review the changes. If the changes are accepted they can be merged into the repository.

When you are working on a fork it is recommended to make a topic branch before you start changing anything. Pull requests can be sent from any branch or commit. The advantage of a topic branch is that you can push follow up commits if you need to update the proposed changes.

When pushing commits to a pull request, don’t force the push. Force pushing can corrupt your pull request.

Once you’ve create a pull request, you can push commits to your topic to add them to your existing pull request. These commits will appear in chronological order within your pull request and the changes will be visible in “Files changed” tab.

Other contributors can review your proposed changes, add review comments, contribute to the pull request discussion, and even add commits to the pull request.

You can either merge a pull request into the repo or closing the pull request.

## Closing a Pull Request
When the proposed changes are no longer needed because they are not merged in an upstream repository you need to close a pull request.

1. Under your repository name, click pull requests.
2. In the “pull requests list”, click the pull request you want to close.
3. At the bottom of the pull request, below the comment box, click close pull.
4. To keep everything tidy delete the branch.

## Merging a Pull Request
To merge the pull requests you open the repository page.

1. Click pull requests
2. In the “pull request list”, click on the pull you want to merge.
3. Click “merge pull request”, if the “merge pull request” button is not shown, then click the merge drop down menu and select “create a merge commit”.
4. Confirm the merge
5. To keep everything tidy delete the branch.

## Deleting a Repository
1. Go to https://github.com/your-name/your-repo/settings
2. Under danger zone, click “Delete this repository“
3. Read the warnings
4. Confirm by typing the name
5. Click “I understand the consequences, delete the repository”

## Uploading from your Command Line
1. Go to https://github.com/settings/tokens
2. Click on the “Generate New Token”-button
3. Give the token a name and set the scope. Standard you should be happy with “repo”
4. Confirm by clicking “Generate Token”

This gives you a very long password you will need to be able to upload from the command line. When you do git push origin master you will be prompt for your login, which is you github handle and the long password generated above.

# Information Leakage 
Github dorking is a technique used by threat agents to go and search for an organization’s sensitive data on github. Classic things to search for are usernames, passwords, SSH private keys, SSL/TLS private keys, cookies, other files, ...

## Removing Sensitive Data
Although you can delete a file with git rm, it still exists in the repository history. You should consider the data as compromised but it is recommended to do a clean up anyway. To clean up you will need a tool called BFG Repo-Cleaner (http://rtyley.github.io/bfg-repo-cleaner/).

In the example underneath we have downloaded the jar file to our home directory and will need to remove the sensitive file password.txt from the repository repo: 
```
cd repo
java -jar $HOME/bfg-1.12.13.jar --delete-files password.txt


Using repo : /home/vanderhasselte/scripts/test/.git


Found 2 objects to protect
Found 2 commit-pointing refs : HEAD, refs/heads/master

Protected commits
-----------------


These are your protected commits, and so their contents will NOT be altered:


 * commit d1567fee (protected by 'HEAD') - contains 1 dirty file :
    - password.txt (5 B)


WARNING: The dirty content above may be removed from other commits, but as
the *protected* commits still use it, it will STILL exist in your repository.


Details of protected dirty content have been recorded here :
/home/vanderhasselte/scripts/test.bfg-report/2016-10-10/12-59-33/protected-dirt/


If you *really* want this content gone, make a manual commit that removes it,
and then run the BFG on a fresh copy of your repo.
       
Cleaning
--------
Found 1 commits
Cleaning commits:       100% (1/1)
Cleaning commits completed in 26 ms.


BFG aborting: No refs to update - no dirty commits found??


Has the BFG saved you time?  Support the BFG on BountySource:  https://j.mp/fund-bfg
```

## Cleaning Large Binary Objects
As mentioned before git was not build to manage large binary objects but if an inexperienced user puts a blob in your repository you can clean it with bfg-repo-cleaner  BFG Repo-Cleaner (http://rtyley.github.io/bfg-repo-cleaner/).

1. Clone a fresh copy of the repo with the mirror flag:
```
git clone --mirror server/repo.git
```

2. Clean the repo (here we clean blobs > 100 MB)
```
cd repo
java -jar $HOME/bfg-1.12.13.jar --strip-blobs-bigger-than 100M repo.git
```
bfg will update the commits and all branches but doesn’t physically remove the files. 

3. To remove the file you do
```
git reflog expire --expire=now --all && git gc --prune=now aggressive
```

4. When you are happy with the state of the mirror you need to put it on the server.
```
git push
```
