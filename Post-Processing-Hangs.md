### Finding looping/hung tasks

An example *nix ps command to display any post-proccessing task taking longer than 60 seconds:
```
ps aux | grep postprocess_new.php | grep -v grep | sed -e 's/://g' | awk '{if ($10 > 60) print $13,$15 }'
```

This outputs the ID and guid of the release. This SQL query will display the name
```
> SELECT ID,passwordstatus,name,searchname FROM releases WHERE ID = nnnnnnn;
```
nnnnnnn = ID, the first number displayed by the ps command

To delete a particular release, use the misc/testing/DB/delete_releases.php script to remove the collection from the DB and the related meta objects.
