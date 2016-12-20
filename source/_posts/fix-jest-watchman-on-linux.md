title: Fix Jest/Watchman on Linux
date: 2016-12-20 07:47:11
tags:
	- jest
	- watchman
	- linux
---

If you are trying to run Jest on your Linux machine and getting a weird error about Watchman, you can fix it increasing the number of files that Watchman will be able to... _watch_!

All you need to do is:

1. Open the file `/etc/sysctl.conf` E.g. `sudo vim /etc/sysctl.conf`
1. Add the following lines to it:
```
# Config to run Jest
fs.inotify.max_user_watches = 10485760
fs.file-max = 10485760
```

And that's it! :D