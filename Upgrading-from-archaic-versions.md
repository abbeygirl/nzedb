// Reset to last point 'site' table was used.

git reset --hard e54a48

// run patchDB script to bring Db up to date.

// Reset to commit adding 'settings' table

git reset --hard f55a07

// run patchDB script to bring Db up to date.

git reset --hard HEAD

// run .../cli/update_db true to bring db to current.