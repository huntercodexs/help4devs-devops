
# GIT TIPS

##### Commit configuration user

<pre>
git config --global user.name "Hunter Codexs"
git config --global user.email "huntercodexs@gmail.com"
git config --list
</pre>

##### Clone repository

<pre>
git clone https://github.com/{USERNAME_GITHUB}/{REPOSITORY_NAME}
</pre>

##### Check repository status

<pre>
git status
</pre>

##### Check the remote target in the current repository

<pre>
git remote -v
</pre>

##### Change branch

<pre>
git checkout {BRANCH_NAME}
</pre>

##### Check the branch name

<pre>
git branch
</pre>

##### Add changes to the local stage

<pre>
git add .
</pre>

##### Create one commit to send the changes from the local stage to remote origin

<pre>
#Sample
git commit -m 'feat+data: [no-stabled] [work] Added database files to sample tests using API (PHP) + grid component repair'
</pre>

##### Send the changes to remote repository

<pre>
git push
</pre>

##### Synchronize the local repository with remote

<pre>
git pull
</pre>

##### Undo the changes made accidentally or that can broken anything after that - RESET

<pre>
git filter-branch --index-filter 'git rm --cached --ignore-unmatch dbhandler/ipch'
git rm --cached dbhandler/ipch
git commit --amend
git filter-branch ... 0123abcd..HEAD
git reset HEAD
git push -f
</pre>

##### To merge with another branch (it means that the informed branch will be merged in the current branch)

<pre>
git merge {BRANCH_NAME_TO_MERGE}
</pre>

##### Change the repository URL

<pre>
git remote remove origin
git remote add origin https://github.com/{USERNAME_GITHUB}/{REPOSITORY_NAME}
git remote set-url origin https://github.com/{USERNAME_GITHUB}/{REPOSITORY_NAME}
</pre>

##### Create new branch during the push

<pre>
git push --set-upstream origin devel-150621
</pre>

##### Change the commit message after the push command

<pre>
git commit --amend
</pre>

