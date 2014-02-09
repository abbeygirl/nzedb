This a comparison between MyISAM fixed, MyISAM Dynamic, InnoDB Dynamic and InnoDB Compressed. I am using the same database for each engine and row_format and using mysql_convert_tables.php for the conversion.

Releases    6,287,347
NFO's       1,993,044
PreDB       6,596,484
MovieInfo   1,552,054
TvRage         41,157 *including images


With MyISAM Fixed Row Format:
```
php misc/testing/DB/convert_mysql_tables.php fmyisam
php misc/testing/DB/show_table_sizes.php 0
Table Name                 Engine Row_Format       Data Size      Index Size      Free Space      Total Size
========================= ======= ========== =============== =============== =============== ===============
releases                   MyISAM      FIXED    21,453.98 MB     6,378.22 MB         0.00 MB    27,832.19 MB
predb                      MyISAM      FIXED    25,968.82 MB       727.70 MB         0.00 MB    26,696.52 MB
movieinfo                  MyISAM      FIXED    20,195.22 MB        57.36 MB         0.00 MB    20,252.58 MB
releasenfo                 MyISAM      FIXED     3,299.91 MB        50.41 MB         0.00 MB     3,350.32 MB
releasefiles               MyISAM      FIXED     1,721.55 MB       134.62 MB         0.00 MB     1,856.17 MB
releaseextrafull           MyISAM      FIXED     1,576.63 MB         7.08 MB         0.00 MB     1,583.71 MB
tvrage                     MyISAM      FIXED     1,392.27 MB         3.79 MB         0.00 MB     1,396.07 MB
releaseaudio               MyISAM      FIXED       618.89 MB        19.56 MB         0.00 MB       638.45 MB
releasevideo               MyISAM      FIXED       511.52 MB         6.78 MB         0.00 MB       518.30 MB
bookinfo                   MyISAM      FIXED       471.40 MB         0.29 MB         0.00 MB       471.69 MB
musicinfo                  MyISAM      FIXED       320.26 MB         0.14 MB         0.00 MB       320.40 MB
tvrageepisodes             MyISAM      FIXED        94.03 MB         0.95 MB         0.00 MB        94.98 MB
consoleinfo                MyISAM      FIXED        27.33 MB         0.02 MB         0.00 MB        27.35 MB
releasesubs                MyISAM      FIXED        14.60 MB         3.10 MB         0.00 MB        17.70 MB
anidb                      MyISAM      FIXED         7.90 MB         0.07 MB         0.00 MB         7.97 MB
site                       MyISAM      FIXED         7.20 MB         0.01 MB         0.00 MB         7.21 MB
allgroups                  MyISAM      FIXED         5.81 MB         0.21 MB         0.00 MB         6.02 MB
groups                     MyISAM      FIXED         0.62 MB         0.04 MB         0.00 MB         0.66 MB
menu                       MyISAM      FIXED         0.41 MB         0.00 MB         0.00 MB         0.41 MB
country                    MyISAM      FIXED         0.18 MB         0.01 MB         0.00 MB         0.19 MB
upcoming                   MyISAM      FIXED         0.18 MB         0.00 MB         0.00 MB         0.18 MB
userrequests               MyISAM      FIXED         0.15 MB         0.01 MB         0.00 MB         0.16 MB
genres                     MyISAM      FIXED         0.13 MB         0.00 MB         0.00 MB         0.13 MB
binaryblacklist            MyISAM      FIXED         0.09 MB         0.01 MB         0.00 MB         0.10 MB
category                   MyISAM      FIXED         0.09 MB         0.01 MB         0.00 MB         0.10 MB
tmux                       MyISAM      FIXED         0.06 MB         0.01 MB         0.00 MB         0.08 MB
users                      MyISAM      FIXED         0.07 MB         0.00 MB         0.00 MB         0.07 MB
shortgroups                MyISAM      FIXED         0.00 MB         0.01 MB         0.00 MB         0.02 MB
logging                    MyISAM      FIXED         0.01 MB         0.00 MB         0.00 MB         0.01 MB
forumpost                  MyISAM      FIXED         0.00 MB         0.01 MB         0.00 MB         0.01 MB
userdownloads              MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
content                    MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
binaries                   MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
nzbs                       MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
collections                MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
animetitles                MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
userseries                 MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
userroles                  MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
releasecomment             MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
userexcat                  MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
userinvite                 MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
partrepair                 MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
usermovies                 MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
parts                      MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
usercart                   MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
========================= ======= ========== =============== =============== =============== ===============
Table Name                 Engine Row_Format       Data Size      Index Size      Free Space      Total Size
                                                77,689.31 MB     7,390.47 MB         0.00 MB    85,079.78 MB


The recommended minimums are:
MyISAM: key-buffer-size           = 8G
InnoDB: innodb_buffer_pool_size   = 0G

**Total DB size 83.08GB**
```

With MyISAM Dynamic Row Format: