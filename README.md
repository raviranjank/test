# Git Notes

## Books used
1. [Pro Git](https://github.com/progit/progit2/releases/download/2.1.49/progit.pdf)

## Introduction
*Git is distributed versioning system. Every machine has a copy of enitre project database locally. Also commit can be made locally*.

### Three States of Git
The three state in which the file can reside in the git is **committed, modified, staged**.

* ***Committed*** means the data is safely stored in your local database.
* ***Modified*** means that we have changed the file but have not committed it to our database yet.
* ***Staged*** means that we have modified the file but git will use the committed version while committing next snapshot.

### Three main section of Git project
They are **the Git directory, the working tree and the staging area**.
* ***The Git directory*** is where Git stores the metadata and other object database for our project.
* ***The working tree*** or working directory is the single checkout of one version of project. These files are pulled out from Git database and is available for modification.
* ***The staging area*** is present in our Git directory and stores information what will go into our next commit.

### The basic Git workflow
  
1. We modify files in our working tree.
2. We selectively stage just those changes which we want to be part of next commit. This adds only those changes to the staging area.
3. We do a commit. This action takes the files that are in staging are and stores that snapsjot permanently to our Git directory.

So to say if particular version of file is present int the Git directory, it's considered as committed. 
If it has been modified and was added to the staging are, it is staged and present in locally. 
And if it was changed since it was checked out but has not been staged, it is modified.

### This first thing after installing git

Open the powershell or terminal and set user name and email address.
The following are the commands:
```bash
git config --global user.name "Ravi Ranjan Karn"
git config --global user.email ravi@example.com
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -nosession"
```
To check settings
```bash
git config --list
```

## Git Basics

### Getting a Git Repository
There are two ways:
1. We can take a local dir which is not currently under version control and turn it into a Git repository, or
2. We can ***clone*** an existing Git repository from elsewhere.

---
### Initializing a Repository in an existing directory
```bash
for linux: cd /home/user/my_project
for windows: cd c:\user\my_project
```
and run
```bash
git init
```
The ```git init``` creates a sub-dir named ***.git*** that contains all of the necessary repository files (Git repository skeleton).

If you want to commit the existing files in the directory then use following commands.
```bash
# to add all the c files to repository
git add *.c
# to add specific file
git add LICENSE
# initial commit
git commit -m 'initial project version'
```

### Cloning existing repository
The main difference between clone and checkout is that checkout only retrieves working dir whereas clone in git gets full copy of all the data that server has.
```bash
git clone https://github.com/libgit/libgit2
# if you need to put files in some dir then
git clone https://github.com/libgit/libgit2 mylibgit
```

### Recording changes to the Repository
* Each file in the working dir can be in one of the two states: ***traked or untracked***.
* Tracked files are known by Git and can be ***unmodified, modified or staged***.
* Untracked files will not be maintained by the Git.
* Following is the command to check the status of the files
```bash
git status
# for more information
$ git status -s
_M README       # underscore is space
MM Rakefile
A_ lib/git.rb
M_ lib/simplegit.rb
?? LICENSE.txt
```
As seen above there are two columns. the left col indicates staging area status and the right col indicates working tree status. eg. "lib/simplegit.rb" is modified and staged. "Rakefile" was modified then staged then again modified in working tree.

### Ignoring Files
* For files which are auto generated locally or for which tracking is not needed. eg log files.
* We can use **.gitignore file**.
```bash
# following commands works with powershell
# create .gitignore file if not present
echo "*.[oa]" > .gitignore      # this command creates and write string content into .gitignore file
notepad.exe .gitignore          # this opens .gitignore file into notepad
# add "*~" in new line into .gitignore
```
The first line tells Git to ignore any files ending in “.o” or “.a” — object and archive files. The second line tells Git to ignore all files whose names end with a tilde (~), which is used by many text editors such as Emacs to mark temporary files. We may also include a log, tmp, or pid directory; automatically generated documentation; and so on.

The rules for the patterns we can put in *.gitignore* file is as follows:
* Blank lines or lines starting with # are ignored.
* Standard glob patterns work, and will be applied recursively throughout the entire working tree.
* You can start patterns with a forward slash (/) to avoid recursivity.
* You can end patterns with a forward slash (/) to specify a directory.
* You can negate a pattern by starting it with an exclamation point (!).

>Glob patterns are like simplified regular expressions that shells use. An asterisk (\*) matches zero or more characters; [abc] matches any character inside the brackets (in this case a, b, or c); a question mark (?) matches a single character; and brackets enclosing characters separated by a hyphen ([0-9]) matches any character between them (in this case 0 through 9). You can also use two asterisks to match nested directories; a/**/z would match a/z, a/b/z, a/b/c/z, and so on.

Following is a sample .gitignore file
``` bash
# ignore all .a files
*.a
# but do track lib.a, even though you're ignoring .a files above
!lib.a
# only ignore the TODO file in the current directory, not subdir/TODO
/TODO
# ignore all files in the build/ directory
build/
# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt
# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

### Viewing Staged and Unstaged Changes
