If you are using backfill_safe_threaded.py, one of these 2 queries is what determines if you have any backfill capable groups, according to the settings you have set.

```
SELECT g.name, g.first_record AS our_first, MAX(a.first_record) AS thier_first, MAX(a.last_record) AS their_last FROM groups g INNER JOIN shortgroups a ON g.name = a.name WHERE g.first_record IS NOT NULL AND g.first_record_postdate IS NOT NULL AND g.backfill = 1 AND g.first_record_postdate != '2000-00-00 00:00:00' AND (NOW() - INTERVAL backfill_target DAY) < g.first_record_postdate
```

or
```
SELECT g.name, g.first_record AS our_first, MAX(a.first_record) AS thier_first, MAX(a.last_record) AS their_last FROM groups g INNER JOIN shortgroups a ON g.name = a.name WHERE g.first_record IS NOT NULL AND g.first_record_postdate IS NOT NULL AND g.backfill = 1 AND g.first_record_postdate != '2000-00-00 00:00:00' AND (NOW() - INTERVAL datediff(curdate(),(SELECT value FROM site WHERE setting = 'safebackfilldate')) DAY) < g.first_record_postdate
```

```
'g.first_record IS NOT NULL'
```
 means group has not run update_binaries
```
'g.first_record_postdate IS NOT NULL'
```
means group has not run update_binaries
```
'g.backfill = 1'
```
means backfill is activated
```
'g.first_record_postdate != '2000-00-00 00:00:00''
```
is an old setting in binaries, used to set this when deactivating the group
```
'(NOW() - INTERVAL backfill_target DAY) < g.first_record_postdate'
```
means only if the group has not exceeded the settings you for group backfill days
```
'(NOW() - INTERVAL datediff(curdate(),(SELECT value FROM site WHERE setting = 'safebackfilldate')) DAY) < g.first_record_postdate'
```
means only if the group has not exceeded the settings you for group safe backfill days


of the last 2 only 1 is used, depending on setting in tmux-edit
I will not change the query, because then you will backfill a group that should not be backfilled