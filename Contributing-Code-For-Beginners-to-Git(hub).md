If you are familiar with git/github you can probably skip this, you may want to skim it anyhow.

Contributing code fixes or new features should be done something like the following:

* Fork the project.

* Clone your fork to your computer.

`git clone git@github.com:/nZEDb.git`
* Add the nZEDb repository to your local copy as a remote. This will allow you to pull updates into your fork later.

`cd nZEDb`

`git remote add upstream git@github.com:nZEDb/nZEDb.git`

* Create a new branch to work in. Keeping your changes separate from master/dev will make it much easier to compare them, create pull requests, and merge back. Use a name that tells you and us what its intention is and possibly who it came from. Only work on one bug or feature in each branch. Anything else is just too confusing to read/merge!

`git branch -t dev`

`git checkout dev`

`git branch dev-<your-gitID>-<feature/fix-name>`

* When your fixes/feature are ready you first need to make sure dev didn't change while you were working.

`git pull upstream dev`

* Fix any merge onflicts.

`git push`
* On your github repository change the view so that you are comparing nZEDb:dev to your new branch before sending a pull request.