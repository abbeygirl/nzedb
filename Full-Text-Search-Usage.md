nZEDb now requires the usage of full-text search indexes for a number of core functions.  Currently, these include PreDB matching via Title Search, PreDB matching via Hash values, accelerated long run blacklist removal, and the majority of site search capabilities.

Depending upon your setup, you may notice increased memory utilization when running these features.  This is due to the way MySQL appropriates cache memory for storing temporary full-text search results.  This is especially apparent in cases where InnoDB is used as the storage engine for the table holding the full-text index and is exacerbated further when using threaded lookups (fixReleaseNames predbft) as the cache is allotted per connection.

By default MySQL sets the InnoDB FT result cache limit quite high at 2000000000 bytes.  nZEDb devs have found that you can safely reduce this amount to something much smaller to allow for less memory allocation per thread.  Simply set the global variable:

`SET GLOBAL innodb_ft_result_cache_limit = 500000000`

Or whatever you find appropriate for your platform.  Be careful, if you set this value too low MySQL will not be able to store the results of the MATCH query and will drop it altogether.  You will see FT result cache limit errors if you have set it too low (and debug is turned on).