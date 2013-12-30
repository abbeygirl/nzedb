## Database Administration for nZEDb



### Miscellaneous SQL statements that you may find useful. Cut & paste ready.



### Mysql 5.6+ Options, add to my.cnf and restart
```
#dump/restore buffer pool, faster buffer pool warmup
innodb_buffer_pool_dump_at_shutdown = ON
innodb_buffer_pool_load_at_startup  = ON

innodb_checksum_algorithm           = crc32
```

### Calculate size of tables and indexes
found here: http://blog.elijaa.org/index.php?post/2013/08/22/Calculate-Optimize-MySQL-Database-Size
```
SELECT TABLE_NAME AS "Table", 
       TABLE_ROWS AS "Rows", 
       CONCAT((FORMAT((DATA_LENGTH) / POWER(1024,2),2)), ' Mb') AS "Data Size", 
       CONCAT((FORMAT((INDEX_LENGTH) / POWER(1024,2),2)), ' Mb') AS "Index Size",
       CONCAT((FORMAT((DATA_LENGTH+ INDEX_LENGTH) / POWER(1024,2),2)), ' Mb') AS "Total Size",
       TRIM(TRAILING ', ' FROM CONCAT_WS(', ', ENGINE, TABLE_COLLATION, CREATE_OPTIONS)) AS "Type"
FROM information_schema.TABLES
WHERE information_schema.TABLES.table_schema = "nzedb"
```
or
```
DELIMITER $
DROP FUNCTION IF EXISTS byteResize$
CREATE FUNCTION byteResize(bytes FLOAT(9)) 
RETURNS VARCHAR(50)
 
BEGIN
        # Unit list
        DECLARE unit INTEGER UNSIGNED DEFAULT 1;
 
        # Resizing
        WHILE bytes > 1024 DO
            SET bytes = bytes / 1024;
            SET unit = unit + 1;
        END WHILE;
 
RETURN CONCAT(ROUND(bytes, 2), ' ', ELT(unit, '', 'K', 'M', 'G', 'T'), 'b');
 
END$
DELIMITER ;
 
SELECT TABLE_NAME AS "Table", 
       TABLE_ROWS AS "Rows", 
       byteResize(DATA_LENGTH) AS "Data Size", 
       byteResize(INDEX_LENGTH) AS "Index Size",
       byteResize(DATA_LENGTH+ INDEX_LENGTH) AS "Total Size",
       TRIM(TRAILING ', ' FROM CONCAT_WS(', ', ENGINE, TABLE_COLLATION, CREATE_OPTIONS)) AS "Type"
FROM information_schema.TABLES
WHERE information_schema.TABLES.table_schema = "nzedb";
```
### Check progress while populating guid in releases table
```
SELECT count(*) from releases where nzb_guid is null and nzbstatus = 1;
```

### Request-ID lookups 
_Count_
```
SELECT count(*) FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);
```
_View_
```
SELECT r.ID,r.passwordstatus,name FROM releases r left join category c on c.ID = r.categoryID where (r.passwordstatus between -6 and -1) and (r.haspreview = -1 and c.disablepreview = 0);
```