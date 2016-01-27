
* **To track an existing project in Git**, cd into the project's directory and type:

	`git init`

* **To track existing files, type:**

	`git add <filename>`


Use `.gitignore` files to ignore files or filenames matching specific patterns.




### Command List

	

| Command | Description |
|----------|----------|
|  git init  |  track an existing project in git, cd into the project's directory and type this  |
| git add _filename_ |To **start tracking a new file** called filename or **stage a modified file** called filename. `git add` is a multipurpose command. Think of it more as **add this content to the next commit** rather than “add this file to the project”.|
|`git add filename`|Track file named filename|
|`git clone <link>`|Clone a remote repository at _link_|
| `git clone <link> <newname>` | Clone a remote repo into a directory of different name|
|`git commit -m "commit message"`|Commit with the "commit message"|
|`git commit -a -m "commitmessage"`|Skip the staging area and commit every tracked file.|


**git add**

|Command|Description|
|----|----|
|`git add .`| looks at the working tree and adds all those paths to the staged changes if they are either changed or are new and not ignored, it does not stage any 'rm' actions.|
|`git add -u`| looks at all the already tracked files and stages the changes to those files if they are different or if they have been removed. It does not add any new files, it only stages changes to already tracked files.|
|`git add -A`|equivalent to  `git add .`; `git add -u`.|

#### Checking Status

|Command|Description|
|----|----|
|`git status`|To check the status of the files|
|`git status -s` or `git status --short`|A short description of files' status|

#### git diff

|Command|Description|
|----|----|
|`git diff`|lists the exact lines added or removed. It compares what's in the working directory vs staging area. Hence shows only the unstaged changes|
|`git diff --staged`|shows what you've staged that will go into your next commit. It compares the staging area vs the last commit.|
|`git diff --cached`||

**if you’ve staged all of your changes,** `git diff` **will give you no output**.

#### git log


|Command|Description|
|----|----|
|`git log`|lists the commits made in that repo in reverse chronological order.|


Useful Git log options:  
  
|Command|Description|Example|
|----|----|----|
|`-p`|shows the difference introduced in each commit. It has the same info as git log but a diff directly following each entry.| `git log -p`|
|`--stat`|gives abbreviated stats for each commit|`git log --stat`|
|`--pretty=`|changes the log output to formats other than the default||


* Options for `--pretty=`

|Command|Description|Example|
|----|----|----|
|oneline|prints each commit on a single line, which is useful if you’re looking at a lot of commits.|<code>$ git log --pretty=oneline<br>ca82a6dff817ec66f44342007202690a93763949 changed the verison number|
|`short`, `full`, `fuller`|options show the output in roughly the same format but with less or more information, respectively.|`$ git log --pretty=full`|
|format|allows you to specify your own log output format. Very useful for machine parsing. MOST USEFUL|<code>$ git log --pretty=format:"%h -%an, %ar : %s"<br> ca82a6d - Scott Chacon, 6 years ago : changed the version number```|

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


* **Common options of `git log`**:

	|Option|Description|
	|-----|------|
	|-p |Show the patch introduced with each commit.|
	|--stat |Show statistics for files modified in each commit.
	|--shortstat |Display only the changed/insertions/deletions line from the --stat command.
	|--name-only |Show the list of files modified after the commit information.
	|--name-status| Show the list of files affected with added/modified/deleted information as well.|
	|--abbrev-commit | Show only the first few characters of the SHA-1 checksuminstead of all 40.|
	|--relative-date| Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format.|
	|--graph |Display an ASCII graph of the branch and merge history beside the log output.
	|--pretty| Show commits in an alternate format. Options include oneline, short, full, fuller, and format (where you specify your own format).|

* **Options to limit the output of `git log`**

	|Option|Description|
	|-------|-------|
	|-(n)| Show only the last n commits|
	|--since , --after| Limit the commits to those made after the specified date.|
	|--until , --before| Limit the commits to those made before the specified date.|
	|--author| Only show commits in which the author entry matches the specified string.|
	|--committer| Only show commits in which the committer entry matches the specified string.
	|--grep| Only show commits with a commit message containing the string|
	|-S| Only show commits adding or removing code matching the string|

	* `--since` and `--until` Example: `$ git log --since=2.weeks`
  * `--author` option allows you to filter on a specific author, and `--grep` option lets you search for keywords in the commit messages Note: if you want to specify both author and grep options, you have to add --all-match or the command will match commits with either.)
  * Another really helpful filter is the -S option which takes a string and only shows the commits that introduced a change to the code that added or removed that string. For instance, if you wanted to find the last commit that added or removed a reference to a specific function, you could call: `$ git log --Sfunction_name`
  * The last really useful option to pass to git log as a filter is a path. If you specify a directory or file name, you can limit the log output to commits that introduced a change to those files. This is always the last option and is generally preceded by double dashes ( -- ) to separate the paths from the options.

