This a comparison between MyISAM fixed, MyISAM Dynamic, InnoDB Dynamic and InnoDB Compressed mysql engines and row formats. I am testing with Percona Server version: 5.6.15-63.0-log Percona XtraDB Cluster (GPL), Release 25.3, wsrep_25.3.r4034. I am using the same database for each engine and row_format and using mysql_convert_tables.php for the conversion.

The absolute largest data set is MyISAM Fixed row format with 83GB.

| Table | Count                 |
| ------------- |:-------------:|
| Releases      | 6,287,347     |
| NFO's         | 1,993,044     |
| PreDB         | 6,596,484     |
| MovieInfo     | 1,552,054     |
| TvRage(incl images)        | 41,157        |

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

```
php misc/testing/DB/convert_mysql_tables.php fmyisam
php misc/testing/DB/show_table_sizes.php 0
Table Name                 Engine Row_Format       Data Size      Index Size      Free Space      Total Size
========================= ======= ========== =============== =============== =============== ===============
releases                   MyISAM    DYNAMIC     2,182.63 MB     4,200.64 MB         0.00 MB     6,383.27 MB
releasenfo                 MyISAM      FIXED     3,299.91 MB        50.41 MB         0.00 MB     3,350.32 MB
releaseextrafull           MyISAM      FIXED     1,576.63 MB         7.08 MB         0.00 MB     1,583.71 MB
predb                      MyISAM    DYNAMIC       788.93 MB       727.88 MB         0.00 MB     1,516.81 MB
tvrage                     MyISAM      FIXED     1,392.27 MB         3.79 MB         0.00 MB     1,396.07 MB
movieinfo                  MyISAM    DYNAMIC       403.97 MB        57.57 MB         0.00 MB       461.54 MB
releasefiles               MyISAM    DYNAMIC       163.88 MB       134.70 MB         0.00 MB       298.58 MB
releasevideo               MyISAM    DYNAMIC        56.20 MB         6.78 MB         0.00 MB        62.97 MB
releaseaudio               MyISAM    DYNAMIC        36.72 MB        19.59 MB         0.00 MB        56.31 MB
bookinfo                   MyISAM    DYNAMIC        33.59 MB         0.29 MB         0.00 MB        33.89 MB
musicinfo                  MyISAM    DYNAMIC         9.09 MB         0.14 MB         0.00 MB         9.23 MB
anidb                      MyISAM      FIXED         7.90 MB         0.07 MB         0.00 MB         7.97 MB
tvrageepisodes             MyISAM    DYNAMIC         4.89 MB         0.95 MB         0.00 MB         5.84 MB
releasesubs                MyISAM    DYNAMIC         2.43 MB         3.10 MB         0.00 MB         5.54 MB
consoleinfo                MyISAM    DYNAMIC         1.52 MB         0.02 MB         0.00 MB         1.54 MB
allgroups                  MyISAM    DYNAMIC         0.44 MB         0.21 MB         0.00 MB         0.65 MB
upcoming                   MyISAM      FIXED         0.18 MB         0.00 MB         0.00 MB         0.18 MB
groups                     MyISAM    DYNAMIC         0.03 MB         0.04 MB         0.00 MB         0.07 MB
userrequests               MyISAM    DYNAMIC         0.02 MB         0.01 MB         0.00 MB         0.03 MB
country                    MyISAM    DYNAMIC         0.01 MB         0.01 MB         0.00 MB         0.02 MB
shortgroups                MyISAM    DYNAMIC         0.00 MB         0.01 MB         0.00 MB         0.01 MB
site                       MyISAM    DYNAMIC         0.00 MB         0.01 MB         0.00 MB         0.01 MB
tmux                       MyISAM    DYNAMIC         0.00 MB         0.01 MB         0.00 MB         0.01 MB
category                   MyISAM    DYNAMIC         0.00 MB         0.01 MB         0.00 MB         0.01 MB
binaryblacklist            MyISAM    DYNAMIC         0.00 MB         0.01 MB         0.00 MB         0.01 MB
genres                     MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.01 MB
forumpost                  MyISAM      FIXED         0.00 MB         0.01 MB         0.00 MB         0.01 MB
userdownloads              MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.01 MB
users                      MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
content                    MyISAM      FIXED         0.00 MB         0.00 MB         0.00 MB         0.00 MB
binaries                   MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
nzbs                       MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
collections                MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
animetitles                MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
menu                       MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
userseries                 MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
logging                    MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
userroles                  MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
userexcat                  MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
userinvite                 MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
partrepair                 MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
usermovies                 MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
parts                      MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
usercart                   MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
releasecomment             MyISAM    DYNAMIC         0.00 MB         0.00 MB         0.00 MB         0.00 MB
========================= ======= ========== =============== =============== =============== ===============
Table Name                 Engine Row_Format       Data Size      Index Size      Free Space      Total Size
                                                 9,961.25 MB     5,213.39 MB         0.00 MB    15,174.64 MB


The recommended minimums are:
MyISAM: key-buffer-size           = 6G
InnoDB: innodb_buffer_pool_size   = 0G

**Total DB size 14.82GB**
```

With InnoDB Dynamic Row Format:

```
php misc/testing/DB/convert_mysql_tables.php dinnodb
php misc/testing/DB/show_table_sizes.php 0
Table Name                 Engine Row_Format       Data Size      Index Size      Free Space      Total Size
========================= ======= ========== =============== =============== =============== ===============
releasenfo                 InnoDB    DYNAMIC     4,380.00 MB        42.58 MB         5.00 MB     4,422.58 MB
releases                   InnoDB    DYNAMIC     2,543.00 MB       105.67 MB         0.00 MB     2,648.67 MB
releaseextrafull           InnoDB    DYNAMIC     2,045.00 MB         0.00 MB         4.00 MB     2,045.00 MB
predb                      InnoDB    DYNAMIC       936.00 MB       438.00 MB         0.00 MB     1,374.00 MB
movieinfo                  InnoDB    DYNAMIC       465.95 MB         0.00 MB         0.00 MB       465.95 MB
tvrage                     InnoDB    DYNAMIC       342.52 MB         0.33 MB         4.00 MB       342.84 MB
releasefiles               InnoDB    DYNAMIC       210.72 MB         0.00 MB         0.00 MB       210.72 MB
releasevideo               InnoDB    DYNAMIC        70.59 MB         0.00 MB         6.00 MB        70.59 MB
releaseaudio               InnoDB    DYNAMIC        47.58 MB        19.55 MB         5.00 MB        67.13 MB
bookinfo                   InnoDB    DYNAMIC        38.56 MB         0.00 MB         6.00 MB        38.56 MB
anidb                      InnoDB    DYNAMIC        11.52 MB         0.00 MB         4.00 MB        11.52 MB
musicinfo                  InnoDB    DYNAMIC        10.52 MB         0.00 MB         4.00 MB        10.52 MB
tvrageepisodes             InnoDB    DYNAMIC         6.52 MB         1.52 MB         4.00 MB         8.03 MB
releasesubs                InnoDB    DYNAMIC         4.52 MB         3.52 MB         4.00 MB         8.03 MB
consoleinfo                InnoDB    DYNAMIC         2.52 MB         0.00 MB         4.00 MB         2.52 MB
allgroups                  InnoDB    DYNAMIC         1.52 MB         0.41 MB         4.00 MB         1.92 MB
upcoming                   InnoDB    DYNAMIC         0.23 MB         0.02 MB         0.00 MB         0.25 MB
collections                InnoDB    DYNAMIC         0.02 MB         0.11 MB         0.00 MB         0.13 MB
groups                     InnoDB    DYNAMIC         0.06 MB         0.05 MB         0.00 MB         0.11 MB
userrequests               InnoDB    DYNAMIC         0.06 MB         0.03 MB         0.00 MB         0.09 MB
forumpost                  InnoDB    DYNAMIC         0.02 MB         0.06 MB         0.00 MB         0.08 MB
partrepair                 InnoDB    DYNAMIC         0.02 MB         0.06 MB         0.00 MB         0.08 MB
shortgroups                InnoDB    DYNAMIC         0.02 MB         0.06 MB         0.00 MB         0.08 MB
binaries                   InnoDB    DYNAMIC         0.02 MB         0.05 MB         0.00 MB         0.06 MB
nzbs                       InnoDB    DYNAMIC         0.02 MB         0.05 MB         0.00 MB         0.06 MB
category                   InnoDB    DYNAMIC         0.02 MB         0.05 MB         0.00 MB         0.06 MB
binaryblacklist            InnoDB    DYNAMIC         0.02 MB         0.03 MB         0.00 MB         0.05 MB
tmux                       InnoDB    DYNAMIC         0.02 MB         0.03 MB         0.00 MB         0.05 MB
parts                      InnoDB    DYNAMIC         0.02 MB         0.03 MB         0.00 MB         0.05 MB
releasecomment             InnoDB    DYNAMIC         0.02 MB         0.03 MB         0.00 MB         0.05 MB
userdownloads              InnoDB    DYNAMIC         0.02 MB         0.03 MB         0.00 MB         0.05 MB
country                    InnoDB    DYNAMIC         0.02 MB         0.02 MB         0.00 MB         0.03 MB
site                       InnoDB    DYNAMIC         0.02 MB         0.02 MB         0.00 MB         0.03 MB
userexcat                  InnoDB    DYNAMIC         0.02 MB         0.02 MB         0.00 MB         0.03 MB
usermovies                 InnoDB    DYNAMIC         0.02 MB         0.02 MB         0.00 MB         0.03 MB
usercart                   InnoDB    DYNAMIC         0.02 MB         0.02 MB         0.00 MB         0.03 MB
userseries                 InnoDB    DYNAMIC         0.02 MB         0.02 MB         0.00 MB         0.03 MB
userinvite                 InnoDB    DYNAMIC         0.02 MB         0.00 MB         0.00 MB         0.02 MB
genres                     InnoDB    DYNAMIC         0.02 MB         0.00 MB         0.00 MB         0.02 MB
logging                    InnoDB    DYNAMIC         0.02 MB         0.00 MB         0.00 MB         0.02 MB
userroles                  InnoDB    DYNAMIC         0.02 MB         0.00 MB         0.00 MB         0.02 MB
menu                       InnoDB    DYNAMIC         0.02 MB         0.00 MB         0.00 MB         0.02 MB
users                      InnoDB    DYNAMIC         0.02 MB         0.00 MB         0.00 MB         0.02 MB
animetitles                InnoDB    DYNAMIC         0.02 MB         0.00 MB         0.00 MB         0.02 MB
content                    InnoDB    DYNAMIC         0.02 MB         0.00 MB         0.00 MB         0.02 MB
========================= ======= ========== =============== =============== =============== ===============
Table Name                 Engine Row_Format       Data Size      Index Size      Free Space      Total Size
                                                11,117.78 MB       612.34 MB        54.00 MB    11,730.13 MB


The recommended minimums are:
MyISAM: key-buffer-size           =
InnoDB: innodb_buffer_pool_size   = 1G

**Total DB size 11.452GB**
```

With InnoDB Compressed Row Format:

```
php misc/testing/DB/convert_mysql_tables.php cinnodb
php misc/testing/DB/show_table_sizes.php 0

