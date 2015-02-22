HHVM can be used to run the CLI scripts, there might be some issues however as HHVM is not 100% compliant with PHP.

You need to edit the php.ini for HHVM, in linux by default it should be in /etc/hhvm/php.ini

In a command line, type the following: `php -r 'var_dump(get_include_path());'`

You should get something like this: `string(17) ".:/usr/share/pear"`

In the HHVM php.ini, add `include_path = ".:/usr/share/pear"` to the php options section, changing `".:/usr/share/pear"` based on the output of the above command.

Again, in the php.ini, under the php options section add:

`date.timezone = America/New_York` changing the timezone for your own (see PHP website).
`memory_limit = 1024M` change to whatever you require.
`max_execution_time = 120`


