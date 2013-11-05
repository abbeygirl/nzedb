nZEDb has a rich variety of scripts that automate many administrative tasks. We strongly suggest that you take the time to browse the scripts found in the **misc/testing/** directory and familiarise yourself with the various possibilities. In general, whenever each script is called without parameters it will display a small summary of it's function and input parameters.

The scripts are loosely grouped by function into several sub-directories.

## DB_scripts 

### active_groups.php 

This script will show all Active Groups. There is 1 required argument and 2 optional arguments.
The first argument of [date, releases] is used to sort the display by first_record_postdate or by the number of releases.
The second argument [ASC, DESC] sorts by ascending or descending.
The third argument will limit the return to that number of groups.
To sort the active groups by first_record_postdate and display only 20 groups run:
  php active_groups.php date desc 20

### autopatcher.php

This script will automatically do a git pull, patch the DB and delete the smarty folder contents.
If you are sure you want to run it, type php autopatcher.php true
If you want to run a backup of your database and then update, type php autopatcher.php safe

### backfill_groups.php

This script will show all Backfill Groups.
An optional first argument of ASC/DESC is used to sort the display by first_record_postdate in ascending/descending order.
An optional second argument will limit the return to that number of groups.
To sort the backfill groups by first_record_postdate and display only 20 groups run:
  php backfill_groups.php true 20
