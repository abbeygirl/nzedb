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

### delete_releases.php

This script removes all releases and nzb files from a poster or by searchname or by name or by groupname or by guid or by categoryid or newer than x hours adddate/postdate.
If you are sure you want to run it, type php delete_releases.php [ fromname, searchname, name, groupname, guid, categoryid, adddate/postdate ] equals [ name, guid, 7010, hours(number) ]
You can also use like instead of equals by doing type php delete_releases.php [ fromname, searchname, name, groupname, guid ] like [ name/guid ]

### mysqldump_tables.php

**Single File
To dump the database run: php mysqldump_tables.php db dump /path/to/save/to
To restore the database run: php mysqldump_tables.php db restore /path/where/saved

**Individual Table Files
To dump all tables run: php mysqldump_tables.php all dump /path/to/save/to
To restore all tables run: php mysqldump_tables.php all restore /path/where/saved

**Three Tables (collections, binaries, parts)
To dump collections, binaries, parts tables run: php mysqldump_tables.php test dump /path/to/save/to
To restore collections, binaries, parts tables run: php mysqldump_tables.php test restore /path/where/saved

**Individal Files - OUTFILE/INFILE - No schema
To dump all tables, using OUTFILE run: php mysqldump_tables.php all outfile /path/to/save/to
To restore all tables, using INFILE run: php mysqldump_tables.php all infile /path/where/saved

### setUserPasswordHash.php

usage: php setUserPasswordHash.php password (id | username)

ID and username are the entries in the users table. Either can be used.

Allows you to set the password hash on a particular user's account. Used, for example, when upgrading from earlier version of nZEDb (pre-crypt() use) or when a user forgets their password. Admin should set a random password and give that to the user to log in and reset it how they like.

### setUserPasswordHashesToEmail.php

See the warning in the file for usage!

This script is an alternative for people with large numbers of the old SHA1 hashed passwords, that do not want to change them manually with the above script. It does what its name suggests, scans the users table for old-style passwords (ignoring new ones completely) and changes the hash to that of the user's email address. This allows the user to log in with their email address as a password and then change it manually to whatever they like.

### 