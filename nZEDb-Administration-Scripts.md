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

### backfill_predb.php

This script is outdated, but kept as a how to should we need it again.
Please use the predb dump located at nZEDb.com

### change_USP_provider.php

This script is used when you have switched UseNet Providers(USP) so you can pickup where you left off, rather than resetting all the groups.
Only use this script after you have updated your config.php file with your new USP info!!
Make sure you **DO NOT** have any update or postprocess scripts running when running this script!

Usage: php change_USP_provider true

### convert_from_newznab.php

Usage: newznab_schema nZEDB_schema true/false
example: php convert_from_newznab.php newznab nzedb true

newznab_schema: Schema where your newznab install is located. The database name. The current user in your config.php file must have access to this schema
nZEDB_schema: Schema where you want the newznab data converted too. The database name. The schema must be populated and will be wiped clean except the sites and categories tables
true/false: false = Show the queries but do not run.  true means you understand the risks and want to convert the data (Your old data will not be touched

NOTE: This is experimental and there is a possibility that this will not work correctly.  Please let us know if it doesn't work correctly, but we are not responsible for any lost data.
      You will have to start any backfilling and processing over again since we use a different mechanism for processing releases

### convert_mysql_tables.php

```
php convert_mysql_tables.php myisam                     ...: Converts all the tables to Myisam Dynamic.
php convert_mysql_tables.php dinnodb                    ...: Converts all the tables to InnoDB Dynamic.
php convert_mysql_tables.php cinnodb                    ...: Converts all the tables to InnoDB Compressed.
php convert_mysql_tables.php collections                ...: Converts collections, binaries, parts to MyIsam.
php convert_mysql_tables.php mariadb-tokudb             ...: Converts all the tables to MariaDB Tokutek DB.
php convert_mysql_tables.php tokudb                     ...: Converts all the tables to Tokutek DB.
php convert_mysql_tables.php table [ myisam, dinnodb, cinnodb ] ...: Converts 1 table to Engine, row_format specified.
```

### convert_to_tpg.php

This script will allow you to move from single collections/binaries/parts tables to TPG without having to run reset_truncate.
Please STOP all update scripts before running this script.

Use the following options to run:

```
php convert_to_tpg.php true               Convert c/b/p to tpg leaving current collections/binaries/parts tables in-tact.
php convert_to_tgp.php true delete        Convert c/b/p to tpg and TRUNCATE current collections/binaries/parts tables.
```

### copy_from_newznab.php 

Usage php copy_from_newznab.php path_to_newznab_nzbs

### 