If you are familiar with git/github you can probably skip this, you may want to skim it anyhow.

Contributing code fixes or new features should be done something like the following:

* Fork the project. See https://help.github.com/articles/fork-a-repo

* Clone your fork to your computer. It will need a different name if it will be in the same directory as your clone of the main project. e.g. /var/www/nZEDb and /var/www/nZEDb-&lt;git-user-name&gt;

`cd /var/www`

`git clone --branch dev https://github.com/<git-user-name>/nZEDb.git nZEDb-<git-user-name>`

* Add the main project as a remote. This will allow you to pull updates into your fork later:

`cd nZEDb-<git-user-name>`

`git remote add upstream https://github.com/nZEDb/nZEDb.git`

* Create a new branch to work in. Keeping your changes separate from master/dev will make it much easier to compare them, create pull requests, and merge back. Use a name that tells you and us what its intention is and possibly who it came from. Only work on one bug or feature in each branch. Anything else is just too confusing to read/merge! If you are proposing a fix for a known issue (i.e. in the issue tracker), use the issue-ID.

`git branch dev-<git-user-name>-<feature/fix-name>`

`git checkout dev-<git-user-name>-<feature/fix-name>`

### Do your Coding & Testing

`git add -i`

`git commit`

When your fixes/feature are ready you first need to make sure dev didn't change while you were working.

`git checkout dev`

`git pull upstream dev`

`git checkout dev-<git-user-name>-<feature/fix-name>`

`git merge dev`

Fix any merge conflicts.

`./commit`

### Push changes to your Repository

`git push origin dev-<git-user-name>-<feature/fix-name>`

On your github repository page you should now see a green "Compare & Send Pull Request" button. After using this button you need to use the Edit button to ensure the correct branches are compared. Place your fork and branch on one side and nZEDb's fork and dev branch on the other. e.g. 

`Fork:<git-user-name>/nZEDb  Branch:<feature/fix-name>  ==>  Fork:nZEDb/nZEDb  Branch:nZEDb/dev`

This may take several attempts as you need to do a "Cross-fork Comparison".