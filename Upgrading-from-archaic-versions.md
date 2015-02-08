// Reset to last point 'site' table was used.

    git reset --hard e54a48


// run patchDB script to bring Db up to date.

    php misc/testing/DB/patchDB.php


// Reset to commit adding 'settings' table.

    git reset --hard f55a07


// run patchDB script to bring Db up to date.

    php misc/testing/DB/patchDB.php


// Reset to latest commit

    git reset --soft HEAD


// run .../cli/update_db.php true to bring db to current.

    php cli/update_db.php true