These instructions were tested on Debian 9 "Stretch", but should work on
any POSIX-compliant system.

### Install mwoffliner

Following the instructions here:
<https://github.com/openzim/mwoffliner>.

### Set Up redis-server for mwoffliner

Modify the `Unix socket` section of the redis-server config
(`/etc/redis/redis.conf`) so that the unix socket is created and has the
correct permissions.

`unixsocket /dev/shm/redis.sock`
`unixsocketperm 777`

(assuming you're on debian)

### Run mwoffliner

`mwoffliner --mwUrl=`[`https://wiki.dfn.net.au`](https://wiki.dfn.net.au)` --adminEmail=-adminEmail=foo@bar.net --mwApiPath=/api.php --mwWikiPath=/ --mwModulePath=/load.php`

This will create a .zim file from the wiki.

**Note: many pages aren't being scraped correctly using this method as
of April 2019.**

### Install Kiwix Reader

Install Kiwix Reader: <https://www.kiwix.org/en/downloads/kiwix-reader/>
on the on the device you want to take offline, and copy the .zim file
from the previous step to the device. Test the wiki in Kiwix before you
go offline.

Using chrome or firefox kiwix extension makes it easier.

__NOTOC__