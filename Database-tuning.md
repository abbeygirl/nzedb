In order to get the most out of nZEDb, you are going to have to tune the database. The default configuration will work, but if you want real speed it will take some time and tuning.

If you're using percona the best place to start is [https://tools.percona.com/wizard](https://tools.percona.com/wizard) with a fresh database install. Be _cautious_ not to check "Enable a strict SQL mode" as it will enforce ONLY_FULL_GROUP_BY which will yield in errors. It may be possible to use "Enable a strict SQL mode" with the ONLY_FULL_GROUP_BY option removed manually (added as last option to the variable sql-mode).

Once you have a base config, use tools such as the phpmyadmin adviser, **[MySQLtuner](http://mysqltuner.com)** and **[Tuning Primer] (https://launchpad.net/mysql-tuning-primer)** to fine tune things further. Note: MySQLtuner recommends setting join_buffer_size too large.

If you are using the multi-threaded scripts it is advised to convert you tables to InnoDB to avoid table locks. There is a script under misc/testing/DB to help you with this.

**Before migrating from MyISAM to InnoDB** be sure to set **innodb_file_per_table** in my.cnf. If its too late, follow these steps to convert: [Howto Clean a MySQL Storage Engine](http://stackoverflow.com/questions/3927690/howto-clean-a-mysql-innodb-storage-engine)

## MySQL Buffer Sizes
At the latest, once the database contains more than 1 million releases you'll need start tuning. The two queries below can provide a simple recommendation. There is also this nZEDb script
```
php /var/www/nZEDb/misc/testing/DB/show_table_sizes.php
```

### Determine Recommended MyISAM Key Buffer Size
```
SELECT CONCAT(ROUND(KBS/POWER(1024,IF(pw<0,0,IF(pw>3,0,pw)))+0.49999),
SUBSTR(' KMG',IF(pw<0,0,IF(pw>3,0,pw))+1,1)) recommended_key_buffer_size
FROM (SELECT SUM(index_length) KBS FROM information_schema.tables
WHERE engine='MyISAM' AND table_schema NOT IN ('information_schema','mysql')) A,
(SELECT 3 pw) B;
```

### Determine Recommended InnoDB Buffer Size
```
SELECT CONCAT(ROUND(KBS/POWER(1024,IF(pw<0,0,IF(pw>3,0,pw)))+0.49999),
SUBSTR(' KMG',IF(pw<0,0,IF(pw>3,0,pw))+1,1)) recommended_innodb_buffer_pool_size
FROM (SELECT SUM(index_length) KBS FROM information_schema.tables WHERE
engine='InnoDB') A,(SELECT 3 pw) B;
```


Unfortunately that's about it for tuning advise. Every machine will need a different config, so don't just blindly use others. Get a base configuration for your system and fine tune it from there. There are more detailed guides out there on the net if you require further info.