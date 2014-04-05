To use Sharing:

1. NNTPProxy currently does not support all the NNTP commands required, so you will not be able to use it with this.
1. Your NNTP provider must allow you access to post articles to usenet if you wish to enable the "Posting option".

First run 
`/misc/update/postprocess.php sharing true`

Wait until that finishes.

Next go to admin section of your web site, Sharing Settings.

Set up the options you want.

Rerun 
`/misc/update/postprocess.php sharing true`

First run might take a while.

This will also run sharing :
`/misc/update/update_releases 1 true`