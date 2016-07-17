# 1. init

## 1.1 First Time Git setup

# 2. Git Basics

## 2.1 Getting a git repo:

### 2.1.1 Initializing a Repo in an Existing Repo

* **To track an existing project in Git**, cd into the project's directory and type:

	`git init`

* This creates a sub directory named .git that contains all of your repo files - a Git repo skeleton. 

* **Although, Nothing is tracked yet**

* **To track existing files, type:**

	`git add _filename_`

### 2.1.2 Cloning an existing repo

* You clone a repository with git clone [url] . For example, if you want to
clone the Git linkable library called libgit2, you can do so like this:

	`git clone https://github.com/libgit2/libgit2`

	* This creates a dir named "libgit2", initializes a .git directory inside it and pulls down all the data for that directory

* If you want to clone the repository
into a directory named something other than “libgit2”, you can specify that as
the next command-line option:  
 `git clone https://github.com/libgit2/libgit2 mylibgit`

## 2.2 Recording Changes to Repo

### 2.2.1 Checking the status of files:

To find the state of the files, you use the status command.

`git status`

If you run this command directly after a clone, you should
see something like this:

```
git status
On branch master
nothing to commit, working directory clean
```

This tells us:

1. The working directory is clean - no tracked or modified files.
2. There are also no untracked files, otherwise git would have listed them here.
3. Finally, the command tells you which branch you’re on
and informs you that it has not diverged from the same branch on the server.
For now, that branch is always “master”, which is the default.

### 2.2.2 Tracking new files:

In order to begin tracking a new file, you use the command git add .

`git add _filename_`

### 2.2.3 Staging Modified Files 


Change a previously tracked file named filename.ext and run git status:

```
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)

modified:   filename.ext
```

"filename.ext" is listed under "Changes not staged for commit", which means a file that is tracked has been modified
in the working directory but not yet staged.

git add is a multipurpose command – you use it to begin tracking
new files, to stage files, and to do other things like marking merge-conflicted
files as resolved. It may be helpful to think of it more as “add this content to the
next commit” rather than “add this file to the project”.


**Staged *and* unstaged**

So you have staged the file "filename.ext" and before you modify it, you remember to make a last minute change and now when you run `git status`, you get:

```
$ git status

On branch master

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)


    new file:   README
    modified:   filename.ext


Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)


    modified:   filename.ext
```

Git stages a file exactly as it is when you run the git add command. If you commit now, the version of **filename.ext**
as it was when you last ran the git add command is how it will go into the
commit, not the version of the file as it looks in your working directory when
you run git commit . 

**If you modify a file after you run `git add` , you have to
run `git add` again to stage the latest version of the file.**


### 2.2.4 Short Status

If you run `git status -s` or `git status --short` you get a far more simpli-
fied output from the command.

```
$ git status -s

 M README
MM Rakefile
A lib/git.rb
M lib/simplegit.rb
?? LICENSE.txt
```

There are two columns to the output - 

* the left hand column indicates that the file is staged and 

* the right hand column indicates that it’s modified.

New files that aren’t tracked have a ?? next to them, new files that have
been added to the staging area have an A , modified files have an M and so on.

### 2.2.5 Ignoring Files

* Git usually shows untracked files. But there will be some files(e.g. log files, system generated files, etc.) which you don't want git to automatically track or even show as untracked files.

* In such cases, you can create a file listing patterns to match them named `.gitignore`.

* The rules for the patterns you can put in the .gitignore file are as follows:  
  * Blank lines or lines starting with # are ignored.   
  * Standard glob patterns work.  
  * You can end patterns with a forward slash ( / ) to specify a directory.
  * You can negate a pattern by starting it with an exclamation point ( ! ).

* Glob patterns are like simplified regular expressions that shells use.
  * An asterisk ( * ) matches zero or more characters; 
  * [abc] matches any character inside the brackets (in this case a, b, or c)   
  * a question mark ( ? ) matches a single character; 
  * brackets enclosing characters separated by a hyphen( [0-9] ) matches any character between them (in this case 0 through 9). 
  * You can also use two asterisks to match nested directories; a/**/z would match a/z , a/b/z , a/b/c/z , and so on.

* An example `.gitignore` file is as follows:

	```
	# no .a files
	*.a

	# but do track lib.a, even though you're ignoring .a files above
	!lib.a

	# only ignore the root TODO file, not subdir/TODO
	/TODO

	# ignore all files in the build/ directory
	build/

	# ignore doc/notes.txt, but not doc/server/arch.txt
	doc/*.txt

	# ignore all .txt files in the doc/ directory
	doc/**/*.txt
	```
* GitHub maintains a fairly comprehensive list of good .gitignore file exam-
ples for dozens or projects and languages at https://github.com/github/
gitignore if you want a starting point for your project.
	

### 2.2.6 Viewing your staged and unstaged changes
* `git status` lists the files which were changed whereas the `git diff` lists the exact lines added or removed.

* `git diff` compares what's in the working directory to the staging area. The result tells you the changes you’ve made that you haven’t yet staged.

* `git diff --staged` shows what you've staged that will go into your next commit. This command compares your staged changes to your last commit.

* It’s important to note that git diff by itself doesn’t show all changes
made since your last commit – only changes that are still unstaged. This can be confusing, because **if you’ve staged all of your changes,** `git diff` **will give you no output**.

* `git diff --cached`

### 2.2.7 Committing your changes

* Any unstaged files -- created or modified since last `git add` -- are not put in a commit. They stay on as modified files.

* **To commit** type `git commit`
  *  This launches the text editor of your choice (using command `git config --global core.editor`)
  * 

* you can type your commit message inline with the commit command by specifying it after a `-m` flag `git commit -m "Commit message here"`

  * Example:
	 ```
		$ git commit -m "Story 182: Fix benchmarks for speed"
		[master 463dc4f] Story 182: Fix benchmarks for speed
		 2 files changed, 2 insertions(+)
		 create mode 100644 README
	  ```
  * The commit has given you some output about itself: 
    * which branch you committed to ( master ),
    * what SHA-1 checksum the commit has ( 463dc4f ), 
    * how many files were changed, and 
    * statistics about lines added and removed in the commit.

### 2.2.8 Skipping the staging area (**`-a`**)

* Adding the `-a` option to the git commit command makes Git automatically
stage every file that is already tracked before doing the commit, letting you skip the git add part.

### 2.2.9 Removing Files (`git rm file` or `git rm --cached file`)

* To remove a file from Git, you have to remove it from your tracked files -- a.k.a. from the staging area.

* `git rm` command removes a file from Git and also removes the file from your working directory so you don’t see it as an untracked file the next time around. `git rm` **removes the file from the disk as well as the working tree.**

* If you simply delete the file from your working directory using say `rm`, it shows up under the “Changed but not updated” (that is, unstaged) area of your `git status` output.

* However, **if you want to keep the file in your working tree but remove it from your staging area.** In other words, you may want to keep the file on your hard drive but not have Git track it anymore. Use the `--cached` option of `git rm`.  
Example:
`git rm --cached README`

* You can pass files, directories, and file-glob patterns to the `git rm` command.  
Example: `git rm log/\*.log` This
command removes all files that have the .log extension in the log/ directory.

### 2.2.10 Moving Files

* If you want to rename a file in Git, you can run something like  
`git mv file_from file_to`

* Example:   
```
$ git mv README.md README
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)


     renamed:    README.md -> README
```
This is equivalent to running something like this:
```
$ mv README.md README
$ git rm README.md
$ git add README

```

## 2.3 Viewing the Commit History (`git log`)

* After you have created several commits, or if you have cloned a repository with an existing commit history, you’ll probably want to look back to see what has happened. The most basic and powerful tool to do this is the `git log` command.

* Example:  
	```
	$ git log
	commit ca82a6dff817ec66f44342007202690a93763949
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:  Mon Mar 17 21:52:11 2008 -0700

	    changed the version number

	commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:  Sat Mar 15 16:40:33 2008 -0700

    	removed unnecessary test

	commit a11bef06a3f659402fe7563abf99ad00de2209e6
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:  Sat Mar 15 10:31:28 2008 -0700

		first commit
	```

* By default, `git log` lists the commits made in that repo in reverse chronological order.

* This command lists each commit with its:  
  * SHA-1 checksum
  * author's name and email
  * date of the commit
  * commit message

* git log options:  
  * git log -p is -p , which shows the difference introduced in each commit. It has the same info as git log but a diff directly following each entry.
  * git log --stat gives abbreviated stats for each commit
  * `--pretty` This option changes the log output to formats other than the default. A few prebuilt options are available for you to use.
    * `oneline` The oneline option prints each commit on a single line, which is useful if you’re looking at a lot of commits. 
	    ```
        $ git log --pretty=oneline
		ca82a6dff817ec66f44342007202690a93763949 changed the verison number
		085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
		a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
		``` 
    * In addition, the `short` , `full` , and `fuller` options show the output in roughly the same format but with less or more information, respectively.

	* `format` , which allows you to specify your own log output format. This is especially useful when you’re generating output for machine parsing – because you specify the format explicitly, you know it won’tchange with updates to Git:  

    	```
    	$ git log --pretty=format:"%h -%an, %ar : %s"
    	ca82a6d - Scott Chacon, 6 years ago : changed the version number
	    085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
    	a11bef0 - Scott Chacon, 6 years ago : first commit

		```

  * Useful options for `git log --pretty=format`:  

	| Option | Description of output |
	|----------|----------|
  	|%H |Commit hash|
	|%h |  Abbreviated commit hash|
	|%T |Tree hash
	|%t |Abbreviated tree hash
	|%P |Parent hashes
	|%p |Abbreviated parent hashes
	|%an |Author name
	|%ae |Author e-mail
	|%ad |Author date (format respects the –date= option)
	|%ar |Author date, relative
	|%cn |Committer name
	|%ce |Committer email
	|%cd |Committer date
	|%cr |Committer date, relative
	|%s |Subject
  You may be wondering what the **difference is between author and committer**. The **author** is the *person who originally wrote the work*, whereas the **committer** is the *person who last applied the work*. So, if you send in a patch to a project and one of the core members applies the patch, both of you get credit –you as the author, and the core member as the committer.

  * The oneline and format options are particularly useful with another log option called `--graph` . This option adds a nice little ASCII graph showing your branch and merge history:  
   ```
    $ git log --pretty=format:"%h %s" --graph
    * 2d3acf9 ignore errors from SIGCHLD on trap
    * 5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
    |\
    | * 420eac9 Added a method for getting the current branch.
    * | 30e367c timeout code and tests
    * | 5a09431 add timeout protection to grit
    * | e1193f8 support for heads with slashes in them
    |/
    * d6016bc require time for xmlschema
    * 11d191e Merge branch 'defunkt' into local
   ```

* Common options of `git log`:

	|Option|Description|
	|-|-|
	|-p |Show the patch introduced with each commit.|
	|--stat |Show statistics for files modified in each commit.
	|--shortstat |Display only the changed/insertions/deletions line from the --stat command.
	|--name-only |Show the list of files modified after the commit information.
	|--name-status| Show the list of files affected with added/modified/deleted information as well.|
	|--abbrev-commit | Show only the first few characters of the SHA-1 checksuminstead of all 40.|
	|--relative-date| Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format.|
	|--graph |Display an ASCII graph of the branch and merge history beside the log output.
	|--pretty| Show commits in an alternate format. Options include oneline, short, full, fuller, and format (where you specify your own format).|

### 2.3.1 Limiting Log Output

* In addition to output-formatting options, git log takes a number of useful
limiting options – that is, options that let you show only a subset of commits.
  * -n where n is any integer to show the last n commits.
  * `--since` and `--until` Example: `$ git log --since=2.weeks`
  * `--author` option allows you to filter on a specific author, and `--grep` option lets you search for keywords in the commit messages Note: if you want to specify both author and grep options, you have to add --all-match or the command will match commits with either.)
  * Another really helpful filter is the -S option which takes a string and only shows the commits that introduced a change to the code that added or removed that string. For instance, if you wanted to find the last commit that added or removed a reference to a specific function, you could call: `$ git log --Sfunction_name`
  * The last really useful option to pass to git log as a filter is a path. If you specify a directory or file name, you can limit the log output to commits that introduced a change to those files. This is always the last option and is generally preceded by double dashes ( -- ) to separate the paths from the options.

* **Options to limit the output of `git log`**

	|Option|Description|
	|-|-|
	|-(n)| Show only the last n commits|
	|--since , --after| Limit the commits to those made after the specified date.|
	|--until , --before| Limit the commits to those made before the specified date.|
	|--author| Only show commits in which the author entry matches the specified string.|
	|--committer| Only show commits in which the committer entry matches the specified string.
	|--grep| Only show commits with a commit message containing the string|
	|-S| Only show commits adding or removing code matching the string|

* For example, if you want to see which commits modifying test files in the Git source code history were committed by Junio Hamano and were not merges in the month of October 2008, you can run something like this:  
 `$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
--before="2008-11-01" --no-merges -- t/`  
Its output is:
  ```
  5610e3b - Fix testcase failure when extended attributes are in use
  acd3b9e - Enhance hold_lock_file_for_{update,append}() API
  f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
  d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
  51a94af - Fix "checkout --track -b newbranch" on detached HEAD
  b0ad11e - pull: allow "git pull origin $something:$current_branch" into an   unborn branch```  
Of the nearly 40,000 commits in the Git source code history, this command
shows the 6 that match those criteria.

## 2.4 Undoing Things

At any stage, you may want to undo something. Here, we’ll review a few basic
tools for undoing changes that you’ve made. **Be careful, because you can’t always undo some of these undos.**

* One of the common undos takes place when you commit too early and possibly forget to add some files, or you mess up your commit message. If you want to try that commit again, you can run commit with the --amend option:

  `git commit --amend`

    * This command takes your staging area and uses it for the commit.
	* **If you’ve made no changes since your last commit** (for instance, you run this command immediately after your previous commit), then your snapshot will look exactly the same, and **all you’ll change is your commit message.**
	* The same commit-message editor fires up, but it already contains the message of your previous commit. You can edit the message the same as always, but it overwrites your previous commit.
	* Example: if you commit and then realize you forgot to stage the changes in a file you wanted to add to this commit, you can do something like this:
	  ```
	  $git commit -m 'initial commit'
	  $ git add forgotten_file
	  $ git commit --amend
	  ```
		You end up with a single commit – the second commit replaces the results of the first.

### 2.4.1 Unstaging a staged file


* Use `git reset HEAD <file>` to unstage a staged file.

* git reset can be a dangerous command if you call it with `--hard`. Calling git reset without an option is not dangerous - it only touches your staging area.

	Example:

	```
	$ git reset HEAD benchmarks.rb
	Unstaged changes after reset:
	M       benchmarks.rb
	$ git status
	On branch master
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)
	
	
		renamed:    README.md -> README
	Changes not staged for commit:
	  (use "git add <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working 	directory)


		modified:	benchmarks.rb
	```

### 2.4.2 Unmodifying a Modified File


* So you have made some changes to a git tracked file, but want to revert it back to what it looked like when you last committed or cloned. `git status` tells us how to do that `(use "git checkout -- <file>..." to discard changes in working directory)`

* So we use that `git checkout -- <file>`

* `git checkout -- [file]` **is a DANGEROUS COMMAND**. *Any changes you made to that file are gone – you just copied another file over it*. **Don’t ever use this command unless you absolutely know that you don’t want the file.**

* If you would like to keep the changes you’ve made to that file but still need to get it out of the way for now, look at **branching** and **stashing**.These are generally better ways to go.

* Anything that is *committed* in Git can almost always be recovered. However, anything you lose that was never committed is likely never to be seen again.

* However, anything you lose that was never committed is likely never to be seen again.

## 2.5 Working with Remotes


* Remote repositories are versions of your project that
are hosted on the Internet or network somewhere.

* To be able to collaborate on any Git project, you need to know how to manage
your remote repositories.

* Collaborating with others involves managing these remote repositories and pushing and pulling data to and from them when you need to share work.

* Managing remote repositories includes knowing how to add remote repositories, remove remotes that are no longer valid, manage various remote branches and define them as being tracked or not, and more.


### 2.5.1 Showing Your Remotes

* To see which remote servers you have configured, you can run the `git remote` command. 

* If you have cloned your repository, you should see at least `origin` – that is the default name Git gives to the server you cloned from. In a repo on your computer which was initially copied from a remote, you'll get:

	```
	$ cd repo_name
	$ git remote
	origin
	
	```
* `-v` option, shows you the URLs that Git has stored for the shortname to be used when reading and writing to that remote:

	```
	$ git remote -v
	origin https://github.com/schacon/ticgit (fetch)
	origin https://github.com/schacon/ticgit (push)
	```
  * If you have more than one remote, the command lists them all. For example, a repository with multiple remotes for working with several collaborators might look something like this.

	```
	$ cd grit
	$ git remote -v
	bakkdoor https://github.com/bakkdoor/grit (fetch)
	bakkdoor https://github.com/bakkdoor/grit (push)
	cho45    https://github.com/cho45/grit (fetch)
	cho45    https://github.com/cho45/grit (push)
	defunkt  https://github.com/defunkt/grit (fetch)
	defunkt  https://github.com/defunkt/grit (push)
	koke     git://github.com/koke/grit.git (fetch)
	koke     git://github.com/koke/grit.git (push)
	origin   git@github.com:mojombo/grit.git (fetch)
	origin   git@github.com:mojombo/grit.git (push)
	```
  * This means we can pull contributions from any of these users pretty easily. We may additionally have permission to push to one or more of these, though we can’t tell that here.

### 2.5.2 Adding Remote Repositories

* To add a new remote Git
repository as a shortname you can reference easily, run `git remote add
[shortname] [url]`. 
	Example:
	```
	$ git remote
	origin
	$ git remote add pb https://github.com/paulboone/ticgit
	$ git remote -v
	origin https://github.com/schacon/ticgit (fetch)
	origin https://github.com/schacon/ticgit (push)
	pb     https://github.com/paulboone/ticgit (fetch)
	pb     https://github.com/paulboone/ticgit (push)
	```

* Now you can use the string pb on the command line in lieu of the whole URL.
For example, if you want to fetch all the information that on the remote but that you don’t yet have in your repository, you can run `git fetch pb`
	```
	$ git fetch pb
	remote: Counting objects: 43, done.
	remote: Compressing objects: 100% (36/36), done.
	remote: Total 43 (delta 10), reused 31 (delta 5)
	Unpacking objects: 100% (43/43), done.
	From https://github.com/paulboone/ticgit
	* [new branch]       master     -> pb/master
	* [new branch]       ticgit     -> pb/ticgit
	```

* Paul’s master branch is now accessible locally as pb/master – you can merge it into one of your branches, or you can check out a local branch at that point if you want to inspect it.


### 2.5.3 Fetching and Pulling from Your Remotes


* **To get data from your remote projects**, you can run:

	`$ git fetch [remote-name]`

* The command goes out to that remote project and pulls down all the data
from that remote project that you don’t have yet. After you do this, you should have references to all the branches from that remote, which you can merge in or inspect at any time.



* If you clone a repository, the command automatically adds that remote repository under the name "origin". So, `git fetch origin` fetches any new
work that has been pushed to that server since you cloned or last fetched
from it.

* NOTE that the git fetch command pulls the data to
your local repository – **`fetch` doesn’t automatically merge it with any of your work or modify what you’re currently working on**. You have to merge it manually into your work when you’re ready.

* If you have a branch set up to track a remote branch, you can use the `git pull` command to automatically fetch and then merge a remote branch into your current branch.

* This may be an easier or more comfortable workflow for you; and by default,
the `git clone` command automatically sets up your local master branch to
track the remote master branch (or whatever the default branch is called) on
the server you cloned from. 

* Running git pull generally fetches data from the server you originally cloned from and automatically tries to merge it into the code you’re currently working on.


### 2.5.4 Pushing to Your Remotes (`git push [remote-name] [branch-name]`)

* When you have your project at a point that you want to share, you have to push it upstream using the command `git push [remote-name] [branch-name]`.  
 **Example**: If you want to push your master branch to your origin server
(again, *cloning generally sets up both of those names for you automatically*), then you can run this to push any commits you’ve done back up to the server: `$ git push origin master`

* This command works only if you cloned from a server to which you have
write access and if nobody has pushed in the meantime. 

	* If you and someone else clone at the same time and they push upstream and then you push upstream, your push will rightly be rejected. You’ll have to pull down their work first and incorporate it into yours before you’ll be allowed to push.


### 2.5.5 Inspecting a Remote (`git remote show [remote-name]`)

* If you want to see more information about a particular remote, you can use the `git remote show [remote-name]` command.  
  * Example: If you run this command with a particular shortname, such as `origin`, you get something like this:  
  	```
  	$ git remote show origin
  	* remote origin
    Fetch URL: https://github.com/schacon/ticgit
    Push URL: https://github.com/schacon/ticgit
    HEAD branch: master
	Remote branches:
      master                             tracked
	  dev-branch                         tracked
	Local branch configured for 'git pull':
      master merges with remote master
	Local ref configured for 'git push':
	  master pushes to master (up to date)
  ```  
	``````
   * It lists the URL for the remote repository as well as the tracking branch information. The command helpfully tells you that if you’re on the master branch and you run `git pull`, it will automatically merge in the master branch on the remote after it fetches all the remote references. It also lists all the remote references it has pulled down.  

	* A more complex example:  
		```
		$ git remote show origin
		* remote origin
		URL: https://github.com/my-org/complex-project
		Fetch URL: https://github.com/my-org/complex-project
		Push URL: https://github.com/my-org/complex-project
		HEAD branch: master
		Remote branches:
				master             tracked
				dev-branch      tracked
				markdown-strip   tracked
				issue-43   new (next fetch will store in remotes/origin)
				issue-45 new (next fetch will store in remotes/origin)
				refs/remotes/origin/issue-11 stale (use 'git remote prune' to remove)
	
		Local branches configured for 'git pull':
				dev-branch merges with remote dev-branch
				master   merges with remote master
	
		Local refs configured for 'git push':
			dev-branch	        pushes to dev-branch	      (up to date)
			markdown-strip      pushes to markdown-strip      (up to date)
			master			    pushes to master              (up to date)
		```

	* This command shows
	  * _which branch is automatically pushed to when you
run_ `git push` _while on certain branches_. It also shows you  
	  * _which remote branches on the server you don’t yet have_, 
	  * _which remote branches you have
that have been removed from the server_, and 
	  * _multiple branches that are automatically merged when you run_ `git pull`.

### 2.5.6 Removing and Renaming Remotes

* If you want to rename a reference you can run `git remote rename` to change
a remote’s shortname. For instance, if you want to rename pb to paul , you can
do so with git remote rename :

	```
	$ git remote rename pb paul
	$ git remote
	origin
	paul
	```
* It’s worth mentioning that **this changes your remote branch names**, too.
What used to be referenced at pb/master is now at paul/master .

* If you want to remove a remote for some reason – you’ve moved the server
or are no longer using a particular mirror, or perhaps a contributor isn’t contributing anymore – you can use `git remote rm` :
	```
	$ git remote rm paul
	$ git remote
	origin
	```

## 2.6 Tagging

### 2.6.1 Listing your Tags (`git tag`)(`git tag -l 'search_term'`)

* Listing the available tags in Git is straightforward. Just type `git tag` :
	```
	$ git tag
	v0.1
	v1.3
	```

* This command lists the tags in alphabetical order; the order in which they
appear has no real importance.

* You can also search for tags with a particular pattern using `git tag -l 'search_term'`

### 2.6.2 Creating Tags


* Git uses two main types of tags: lightweight and annotated.

  * A lightweight tag is very much like a branch that doesn’t change – it’s just a pointer to a specific commit.

  * Annotated tags, however, are stored as full objects in the Git database.

	* They’re checksummed; 
	* contain the tagger name, e-mail, and date; 
	* have a tagging message; and 
	* can be signed and verified with GNU Privacy Guard (GPG).

* It’s generally recommended that you create annotated tags so you can have all this information; but if you want a temporary tag or for some reason don’t want to keep the other information, lightweight tags are available too.


### 2.6.3 Annotated Tags

* An annotated tag is created by specifying the `-a` flag in the tag command:

  ```
  $ git tag -a v1.4 -m 'my version 1.4'
  $ git tag
  v0.1
  v1.3
  v1.4
  ```
  * The `-m` specifies a tagging message, which is stored with the tag. If you don’t specify a message for an annotated tag, Git launches your editor so you can type it in.

### 2.6.3Mine Seeing the tag data

* You can see the tag data along with the commit that was tagged by using the
`git show` command:

  ```
	$ git show v1.4
	tag v1.4
	Tagger: Ben Straub <ben@straub.cc>
	Date:   Sat May 3 20:19:12 2014 -0700
	
	my version 1.4

	commit ca82a6dff817ec66f44342007202690a93763949
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Mar 17 21:52:11 2008 -0700
	
		changed the verison number
  ```

* That shows the tagger information, the date the commit was tagged, and the
annotation message before showing the commit information.


### 2.6.4 Lightweight Tags

* Another way to tag commits is with a lightweight tag. This is basically the commit checksum stored in a file – no other information is kept. To create a lightweight tag, don’t supply the -a , -s , or -m option.

* This time, if you run git show on the tag, you don’t see the extra tag information. The command just shows the commit:

	```
	$ git show v1.4-lw
	commit ca82a6dff817ec66f44342007202690a93763949
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Mar 17 21:52:11 2008 -0700

		changed the verison number
	```

### 2.6.5 Tagging Later

* Command - `git tag -a <tag> <checksum>`

* You can also tag commits after you’ve moved past them. Suppose your commit
history looks like this:
	```
	$ git log --pretty=oneline
	15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
	a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
	```
* Now, suppose you forgot to tag the project at v1.2, which was at the “beginning write support” commit. To tag that commit, you specify the commit checksum (or part of it) at the end of the command: `$ git tag -a v1.2 a6b4c97`



## 2.7 Git Aliases

* Git doesn’t automatically infer your command if you type it in partially. If
you don’t want to type the entire text of each of the Git commands, you can
easily set up an alias for each command using git config.

  * Here are a couple of examples you may want to set up:  
	```
	$ git config --global alias.co checkout
	$ git config --global alias.br branch
	$ git config --global alias.ci commit
	$ git config --global alias.st status
	```  
	This means that, for example, instead of typing git commit , you just need
to type git ci . As you go on using Git, you’ll probably use other commands
frequently as well; don’t hesitate to create new aliases.

* This technique can also be very useful in creating commands that you think
should exist. For example, to correct the usability problem you encountered
with unstaging a file, you can add your own unstage alias to Git:
	
	```$ git config --global alias.unstage 'reset HEAD --'```
	* This makes the following two commands equivalent:
		```
		$ git unstage fileA
		$ git reset HEAD fileA
		```

* It’s also common to add a last command, like this: `$ git config --global alias.last 'log -1 HEAD'`. This way you can see the last commit easily:
	```
	$ git last
	commit 66938dae3329c7aebe598c2246a8e6af90d04646
	Author: Josh Goebel <dreamer3@example.com>
	Date:   Tue Aug 26 19:48:51 2008 +0800
	
		test for current head

		Signed-off-by: Scott Chacon <schacon@example.com>
	```