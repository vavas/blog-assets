title: Installing Compass on Linux Mint
date: 2016-08-21 11:17:44
tags:
  - linux
  - compass
---

That's a quick tip but can save you some time.

If you are facing some problems in order to install the Compass gem, try the following approach:

<!-- more -->

```sh
sudo gem update --system
sudo apt-get install ruby-ffi
sudo gem install compass
```

Also, check if you have `gcc` installed on your machine.

That's it!
