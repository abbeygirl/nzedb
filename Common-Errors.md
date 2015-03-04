### Lock Wait Timeout
This is caused when 1 script locks a row and another script is waiting for the row. The script that is waiting, times out.

There are at least 2 things you can do to mitigate this behavior.

1. Increase you lock wait timeout, so the script can wait a little longer.
2. Allow the first script to release the row as soon as it is finished with it.

Do this by setting/adding these to my.cnf
```
innodb_lock_wait_timeout                              = 240
transaction-isolation                                 = READ-COMMITTED
```

These are both InnoDB settings and will reduce the occurrence of lock wait timeouts. The second has the added benefit of releasing the row lock as soon as possible where without it, its not released until the query is finished.

### SPHINX engine missing in MariaDB
If you upgraded your MariaDB and "**SHOW ENGINES;**" doesn't list Sphinx, you have to manually load it again (once):

```
INSTALL SONAME 'ha_sphinx';
```
If that fails with **Duplicate entry 'SPHINX' for key 'PRIMARY'**, check if it's still in the plugins table:

```
MariaDB [nzedb]> select * from mysql.plugin;
+-------------------------------+--------------+
| name                          | dl           |
+-------------------------------+--------------+
| SPHINX                        | ha_sphinx.so |
+-------------------------------+--------------+
1 rows in set (0.00 sec)
```

If it is listed there, delete it and load the plugin again:
```
DELETE FROM mysql.plugin WHERE name = 'SPHINX';
INSTALL SONAME 'ha_sphinx';
```
Now "**SHOW ENGINES;**" should contain a row like this:
```
| SPHINX | YES | Sphinx storage engine 2.2.6-release | NO | NO | NO |
```