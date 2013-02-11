# Git Command Line Notes

## Global setup

Set up your user name and email locally.  Use the same details that you used to set up your github account.

```
$ git config --global user.name 'Your Name'
$ git config --global user.email your.name@emailaddress.com
```

### to view:

```
$ git config user.name
```

### set colours:

```
$ git config --global color.status auto
$ git config --global color.branch auto
```

## SSH

After installing Git and creating a personal account on GitHub, you'll need to set up your SSH to link your git installation to your GitHub account.

### Linux

Generate your key:

```
$ cd ~/.ssh
$ ssh-keygen -t rsa
```

Give your key a name when prompted.  For example:

```
mysshkey
```

Don't enter a password when prompted, just hit enter.

Add the key so it will be used:

```
$ ssh-add mysshkey
```

Open the mysshkey.pub in a text editor and copy all the text.  Then paste this in as an SSH Public Key on your github account page.

To check if you're good to go, open the command line and run:

```
$ ssh git@github.com
```

## Set up your local project directory

Assuming you have git installed: 

```
$ cd /path to your local server root 
$ mkdir project name 
$ cd project name 
```

To initialise git for this project type: 

```
$ git init 
```

...this creates a dir called .git in your project directory

Copy your code into the project directory

## Tell Git what to ignore

### .gitignore

To tell Git to ignore files / directories specific to your project directory create a text file called .gitignore in your project's root directory.

**Quirks:** 

'folderA/*.*' will ignore files in folderA, but not in sub-directories

'file.a' will ignore 'file.a' in all directories.
 
'!file.a' will not ignore file.a if it's been ignored in any rules above it.
 
...In other words, if a rule contains a directory, it won't be applied to sub-directories, whilst root rules apply to all sub-directories. 

The following is an example of the contents of a .gitignore file: 

```
# random stuff you might want to keep local and not index
other_stuff/ 

# we don't need to track the temporary files 
app/tmp/ 

#profile pics 
app/webroot/img/account_photos/25x25_rc/*.* 
app/webroot/img/account_photos/70x70_rc/*.* 
app/webroot/img/account_photos/90x90_rc/*.* 
app/webroot/img/account_photos/150x150_rc/*.* 
app/webroot/img/account_photos/orig/*.* 

# keep the default image files 
!default.jpg 
```

### .gitexcludes

To tell git to ignore things that will apply to various projects you can put a shared file on your system called .gitexcludes 

An example of the contents of a .gitexcludes file follows: 

```
# Komodo project file 
*.kpf
```
 
Each project needs to be configured to know where your .gitexcludes file is. To do this, open this file: 

/path to server/project name/.git/config 

...and add this line under [core] 

```
excludesfile = "/path to your gitexcludes file/.gitexcludes" 
```

## Add and commit locally

To add all files in your project to the 'staging area' of Git: 

```
$ cd /path to your project 
$ git add . 
```

To commit all files to Git (You must add a message when you commit!):

```
$ git commit -m "This is the first commit" 
```

...or add and commit at once:

```
$ git commit -a -m "This is the first commit"
```

## Removing files

This removes the file in the staging area:

```
$ git rm file-to-be-removed.php
```

...then commit

```
$ git commit -m “Removed that file!”
```

**Or:**

If you remove a file in the file system, you can commit like this 

```
$ git commit -am “Removed that file!”
```

## Adding the remote repository

To add a shortcut to the github repo: 

```
$ cd /path to your project
$ git remote rm github
$ git remote add github git@github.com:idio/reponame.git 
```

## Pushing and pulling from github

To push the master branch to github's master:

```
$ git push github master 
```

This calls 'fetch' followed by 'merge' to update your local master branch with github's master branch:

```
$ git pull github master 
```

## Git collaborator set up

Collaborators need to be added on the idio git repository page.  Click to 'edit' the repository and add project collaborators (Each collaborator must have their own github account and must have entered their SSH key into their account) 

Then the collaborator needs to:

```
$ cd /path to local server root
$ git clone git@github.com:idio/reponame.git 
```

This will make the project directory and clone the Git repository inside it.

## Branching

```
$ git branch
$ git show-branch
```

## Create new branch

```
$ git checkout -b “branchname”
```

## Switch branches

```
$ git checkout branchname
```

## Push branch to github for first time

```
$git push github branchname
```

## Pull branch from github to another machine

Create a branch with the same name as the one you want to pull

```
$ git checkout -b branchname
```

Pull from the branch

```
$ git pull origin branchname
```

## Merging

```
$git merge master
```

## Conflicts
When merging branches or pulling from github you may have to resolve conflicts.  Git will tell you when there is a conflict and what file the conflict is in.  Open the file and look for where git has marked the conflict.  It will look something like this:

```
<<<<<<< HEAD:app/config/core.php

the code from your local branch will be here

=======
        
the code from the remote branch or branch you are merging will be here

>>>>>>> f0dc663bebe673e6f4d8dce5fe99a767da02d926:app/config/core.php
```

Resolve the conflict and delete the code that git has put in place to mark the conflict.  Once resolved and tested commit the changes and push back to github.

## Submodules
Submodules of code can be added to a git repository by adding a .gitmodule file to the root of the project.  The main git repository will ignore the code in the submodules so they can be independently updated.  An example of the contents of a .gitmodule file follows:

```
[submodule "app/vendors/php-openamplify"]
path = app/vendors/php-openamplify
url = git://github.com/idio/php-openamplify.git
[submodule "app/vendors/php-twitter"]
path = app/vendors/php-twitter
url = git://github.com/tcdent/php-twitter.git
```

To initialise a submodule:
```
$ git submodule init
```

To update a submodule:
```
$ git submodule update
```
