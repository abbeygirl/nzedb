**Note**: This feature currently in development testing.


To use Sharing:

1. NNTPProxy currently does not support all the NNTP commands required, so you will not be able to use it with this.
1. Your NNTP provider must allow you access to post articles to usenet if you wish to enable the "Posting option".

First run 

`php /misc/update/postprocess.php sharing true`


Wait until that finishes.

<br>
Next go to admin section of your web site, Sharing Settings.

Set up the options you want.

<br>
Rerun 

`php /misc/update/postprocess.php sharing true`

First run might take a while.

<br>

This will also run sharing:

`php /misc/update/update_releases.php 1 true`