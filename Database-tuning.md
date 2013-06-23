In order to get the most out of nZEDb, you are going to have to tune your database for your machine. The default configuration will work, but if you want real speed it will take some time and tuning.

Best place to start is [https://tools.percona.com/wizard](https://tools.percona.com/wizard) with a fresh database install.

Once you have a base config, use tools such as the phpmyadmin adviser and MySQLtuner to fine tune things further.

If you are using the tmux scripts it is advised to convert you tables to InnoDB to avoid table locks. There is a script under misc/testing/DB_scripts to help you with this.

Unfortunately that's about it for tuning advise. Every machine will need a different config, so dont just blindly use others. Get a base config designed for your system and fine tune it from there. There are more detailed guide out there on the net if you require further info.