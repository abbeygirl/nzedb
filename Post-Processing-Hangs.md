The nZEDb post-processing scripts perform analysis using various in-built routines and 3rd party applications. Given the variety of content posted (some of it totally encrypted) it does occur that some posts cause the post-processing to hang or crash, often within an external application not managed directly by the nZEDb team.

There are several ways to reduce this happening. Firstly ensure all 3rd party applications are updated to their latest versions. The version shipped with your distribution is often not robust enough. Check all those defined in Site-Edit->3rd Party Applications. Also configure the timeout application in that section.

## Finding looping/hung tasks


Below example commands to assist *nix users to find hung post-processing scripts. These examples assume you are using the PHP versions of the multi-threaded PP scripts as these are now recommended over the python versions.

**List PHP PP-Additional processes**
```
ps aux | grep -v grep | grep nzedb | sed -e 's/:/ /g' | sort -nk9,10 | grep ProcessAdditional.php
```
The last field is the release ID number.

**Output PID of the first PHP PP-Additional process running longer than 5 minutes**
```
ps aux | grep -v grep | grep nzedb | sed -e 's/:/ /g' | sort -nk9,10 | grep -m1 ProcessAdditional.php | awk -v min=$( date +%M ) -v hour=$( date +%H ) '{if (hour > $9) min=min+60} {if ((min - $10) > 5) print $2}'
```

**List PHP NFO processes active**
```
ps aux | grep -v grep | grep nzedb | sed -e 's/:/ /g' | sort -nk9,10 | grep "switch.php php  pp_nfo"
```

**Output PID of the first PHP PP-NFO processing running longer than 5 minutes**
```
ps aux | grep -v grep | grep nzedb | sed -e 's/:/ /g' | sort -nk9,10 | grep -m1 "switch.php php  pp_nfo" | awk -v min=$( date +%M ) -v hour=$( date +%H ) '{if (hour > $9) min=min+60} {if ((min - $10) > 5) print $2}'
```

**Displaying details about a particular release**

This SQL query will display the Release ID, name, GUID and searchname
```
> SELECT ID,passwordstatus,guid,name,searchname FROM releases WHERE ID = nnnnnnn;
```

**Deleting a single release**

To delete a particular release, use the misc/testing/DB/delete_releases.php script to remove the collection from the DB and the related meta objects.
```
php ~/misc/testing/Release/delete_releases.php fromname=guid=NNNNNNNNNNNNNNNNNNNNNNNNN
```
