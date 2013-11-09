# Intro

The primary purpose is to reduce the number of connections nZEDB has to make to update.  A proxy connection is established with the news server, and then nZEDB will use that established connection to request files for downloads. This an important concept to understand how the configuration works, in case this guide is unclear.

Currently you can either run this under tmux or you must manually run this script under screen.

There are a number of configuration changes to make this work.