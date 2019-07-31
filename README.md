# Assorted notes about Git

## General process of using git and github
To create a repository, move to the location that you wish the repository to be in and then use the command git init. If using github, pull and push are used to sync the remote and local repository, with pull going remote -> local and push vice versa. To contribute to other projects, fork the project, clone the project, commit to the fork, push the fork, then make a pull request which the project owner/admin can approve.

## Adding aliases and configuration:
These commands let you change the information about yourself.
```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
git config –list
```
It is possible to create aliases for commands, as a shortcut name:
```
git config --global alias.co checkout
git config --get-regexp alias
git config --global alias.aliases 'config --get-regexp alias'
```
The second command lists all of your aliases.
The last command sets up an alias so that `git aliases` will display all your aliases!

I maintain a [list of git aliases](https://gist.github.com/seankmartin/bc479ba899cf419c7f79d67c84a62a91) that I find useful.
There is also a small number of [config settings](https://gist.github.com/seankmartin/8c711d1f49cc2958dd6c9c46aa7e51b2) that I find useful.

## Staging and committing
To add files to a commit use `git add`, to commit, use `git commit` and `git status` to check status.
You can use `git add -a` to automatically stage all files that have been modified or deleted and commit that. 
You can also use `git commit -m "message"` to submit a commit message while committing without having to open a text editor. 
If you want to add all files to commit, including new files, you could use:
```
git add -A && git commit -m "Your Message"
```

## Managing project version
You can tag commits to indicate that they are a particular version of the project.

To use previous versions of the project, use `git log --oneline` to see which version you are interested in.
Then type `git checkout (Version)` to temporarily use that version. You can also make a new branch with that version. To make the version the current version, use `git revert (Version)`. If you would like to go back to an original commit you can use `git reset --hard (original commit)`. Or you can use `git reset --hard HEAD` to throw away current local changes. Another option to throw away local changes is to use `git clean -x -d -i` after resetting.

## Branches
Branches are essentially pointers to specific commits. To make a branch simply type `git branch (branch name)`. Although just a pointer, this can be used to create divergent history. To change the branch, type `git checkout (branch name)`. We can merge this together later if we wish. To delete branches use `git branch -d (branch name)`. To merge, all you have to do is check out the branch you wish to merge into and then run `git merge (branch_to_merge)`.

## Exploring your repository
Typing `git status` is a good way to see what is currently going on in the directory that you are in – furthermore it suggests how to throw away local changes. Generally this is done by typing `git checkout -- .` (full stop is needed).
If you stage a bunch of files and want to unstage all of them in one go type `git reset`.
There is a good command to list all the files that are on a specific branch, it is:
```
git ls-tree -r master --name-only
```
## Git LFS (Large File Storage)
Git lfs allows you to store large files in git more efficiently, and at all, for sizes >50mb. See more at https://git-lfs.github.com/. Be really careful, if you accidentally add a file during a local commit that is too big to be on github, even if you remove it in a later commit and try to push the changes to github it will fail because the file that is too big is in the project history. To fix this you need to change the history of your repository. To fix this I used `git reset –soft` to undo the history but keep the local file changes.
To view a previous version of a file that has been stored using git lfs, use the pipe operator:
```
git show [sha]:[file_path] | git lfs smudge > [file_to_store_output]
```

## Updating git repo after changing gitignore:
Say you had some files that you were using in git, then you wanted to ignore these files at a later date as you updated the `.gitignore.` Running: (both of those dots are actually needed)
```
git rm -r --cached .
git add .
git commit -m “removed ignored files”
```
Would remove the files from the index that were ignored and then add back all the files, this removing the ignored files from the index. The index is the cache.