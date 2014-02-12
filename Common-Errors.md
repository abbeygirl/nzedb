Lock Wait Timeout Errors
This is caused when 1 script locks a row/table and another script is waiting for the row/table. The script that is waiting, times out.

There are at least 2 things you can do to mitigate this behavior.
1. Increase you lock wait timeout, so the script can wait a little longer.
2. Allow the first script to release the row as soon as it is finished with it.

Do this by setting/adding these to my.cnf
```
innodb_lock_wait_timeout                              = 240
transaction-isolation                                 = READ-COMMITTED
```

These are both InnoDB settings are will reduce the occurrence of lock wait timeouts. The second has the added benefit of releasing the row lock as soon as possible where without it, its not releases until the query is finished.