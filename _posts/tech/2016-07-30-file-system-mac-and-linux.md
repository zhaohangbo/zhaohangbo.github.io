---
layout: post
title: File System of OS X and Linux 
category: 技术
tags: [Flie System]
keywords: File System
---

### What are `/usr` and `/usr/local` used for

```
/usr           ---> contains user commands
/usr/bin       ---> for normal users
/usr/sbin      ---> for administrative users, like root
/usr/lib       ---> shared libraries
/usr/share/man ---> executables that shouldn't be run directly by users
/usr/libexec   ---> other stuff

It also offers a subdirectory, 
/usr/local     ---> to place programs, libraries and other files that don't come with the base OS
```

### /opt vs /usr/local

```
/opt and /usr/local have similar role, and they seem interchangeable. 

From experience working with other Linux/UNIX sysadmins,
there seems to be a preference for /usr/local in BSD-based UNIX OSs (OS X is BSD-based, and there's no /opt in OS X).

Linux have both /opt and /usr/local
OS S  only has  /usr/local, no /opt

OS X is BSD-based and consequently I'd use /usr/local. 
Note that you can create a program directory and then symlink commands to /usr/local/bin, etc.
For example:

/usr/local/mysql
/usr/local/mysql/bin/mysqladmin
/usr/local/mysql/lib/libmysqlclient.so

/usr/local/bin/mysqladmin -> ../mysql/bin/mysqladmin
/usr/local/lib/libmysqlclient.so -> ../mysql/lib/libmysqlclient.so

This used to be usual practice in Linux and UNIX too, but the FHS explicitely forbids it: 
If you wish to install third party packages in their own directory hierarchy,
you should use /opt/<package> instead. 
Note that FHS-compliance requires to:
put configuration files in /etc/opt/<package>
put variable      files in /var/opt/<package>.

So, on OS X, I'd recommend that you stick to /usr/local as described above.
```


### How I organize file system on my own Mac ?

```
~/Downloads/software-packages --> for all softwares downloads, like .dmg/.pkg/.tar.gz/.zip
/usr/local/opt                --> for maven, zookeeper, etc, 
                              --> like /opt in Ubuntu, which is for Optional application software packages.
                              --> config environment variables in ~/.bashrc based on /usr/local/opt/XXX
```

### Command "man hier" in terminal to see OS X filesystem

```
Type cmd "man hier" in terminal

A historical sketch of the filesystem hierarchy.  
The modern OS X filesystem is documented in the
[File System Programming Guide] available on Apple Developer.

 /             root directory of the filesystem

 /bin/         user utilities fundamental to both single-user and multi-user environments

 /dev/         block and character device files

               fd/  file descriptor files; see fd(4)

 /etc/         system configuration files and scripts

 /mach_kernel  kernel executable (the operating system loaded into memory at boot time).

 /sbin/        system programs and administration utilities fundamental to both single-user and multi-
               user environments

 /tmp/         temporary files

 /usr/         contains the majority of user utilities and applications

               bin/      common utilities, programming tools, and applications
               include/  standard C include files

                         arpa/       C include files for Internet service protocols
                         hfs/        C include files for HFS
                         machine/    machine specific C include files
                         net/        misc network C include files
                         netinet/    C include files for Internet standard protocols; see inet(4)
                         nfs/        C include files for NFS (Network File System)
                         objc/       C include files for Objective-C
                         protocols/  C include files for Berkeley service protocols
                         sys/        system C include files (kernel data structures)
                         ufs/        C include files for UFS

               lib/      archive libraries
               libexec/  system daemons & system utilities (executed by other programs)
               local/    executables, libraries, etc. not included by the basic operating system
               sbin/     system daemons & system utilities (executed by users)
               share/    architecture-independent data files

                         calendar/  a variety of pre-fab calendar files; see calendar(1)
                         dict/      word lists; see look(1)

                                    web2        words from Webster's 2nd International
                                    words       common words

                         man/       manual pages
                         misc/      misc system-wide ascii text files
                         mk/        templates for make; see make(1)
                         skel/      example . (dot) files for new accounts
                         tabset/    tab description files for a variety of terminals; used in the term-
                                    cap file; see termcap(5)
                         zoneinfo/  timezone configuration information; see tzfile(5)

 /var/         multi-purpose log, temporary, transient, and spool files

               at/        timed command scheduling files; see at(1)
               backups/   misc. backup files
               db/        misc. automatically generated system-specific database files
               log/       misc. system log files

               mail/      user mailbox files
               run/       system information files describing various info about system since it was
                          booted

                          utmpx       database of current users; see utmpx(5)

               rwho/      rwho data files; see rwhod(8), rwho(1), and ruptime(1)
               spool/     misc. printer and mail system spooling directories

                          mqueue/     undelivered mail queue; see sendmail(8)

               tmp/       temporary files that are kept between system reboots
               folders/   per-user temporary files and caches
```


### Useful Links

* [What is standard for OS X file system? opt vs usr ](https://apple.stackexchange.com/questions/119230/what-is-standard-for-os-x-filesystem-e-g-opt-vs-usr)
* [Filesystem Hierarchy Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)
* [Unix-like](https://www.baidu.com/index.php?tn=20041099_adr)
