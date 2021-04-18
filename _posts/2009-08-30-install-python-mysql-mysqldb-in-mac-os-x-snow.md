---
title: "Install python MySQL (MySQLdb) in Mac OS X Snow Leopard 10.6"
date: "2009-08-30"
---

I hate MySQL but I have some … employees … who think it’s a perfectly fine solution when you don’t want to boot a Solr server. Savages. Anyway, of course I expected the MySQLdb connector in Python to totally break and it did not disappoint. You’re dealing with a 64-bit version of Python now and a computer that gets confused as to its architecture (why does /usr/bin/arch return i386 when gcc outputs x86\_64 by default?)

Here’s what you do:

- Download the [Mac OS X 10.5 x86\_64 version of MySQL](http://dev.mysql.com/downloads/mysql/5.1.html#macosx-dmg). You need the 64-bit one. Get the package format installer, it’s easy. Run the installer.
- Add /usr/local/mysql/bin to your PATH and add the ARCHFLAGS variable to your environment. I always edit /etc/bashrc, you can also use ~/.bash\_profile if you want. But you need both environment variables. Something like:
    
    export PATH=$PATH:/usr/local/mysql/bin
    export ARCHFLAGS="-arch x86\_64"
    
- [Download the latest source version of MySQL-python](http://sourceforge.net/projects/mysql-python/files/). It should end in .tar.gz. Right now it’s 1.2.3c1. Unpack it and run sudo python setup.py install. You don’t need to do anything with commenting out uints anymore like you did in Leopard.
