http://www.freedesktop.org/wiki/Software/systemd/
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/chap-Managing_Services_with_systemd.html
https://access.redhat.com/articles/754933

Oficial para Ubuntu y Debian (http://www.markshuttleworth.com/archives/1316)
Oficial para RHEL 7 
  https://access.redhat.com/site/articles/754933
  https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7-Beta/html-single/System_Administrators_Guide/#chap-Managing_Services_with_systemd

systemd is a system and service manager for Linux, compatible with SysV and LSB init scripts. systemd provides aggressive parallelization capabilities, uses socket and D-Bus activation for starting services, offers on-demand starting of daemons, keeps track of processes using Linux control groups, supports snapshotting and restoring of the system state, maintains mount and automount points and implements an elaborate transactional dependency-based service control logic. It can work as a drop-in replacement for sysvinit. See Lennart's blog story for a longer introduction, and the three status updates since then. Also see the Wikipedia article. If you are wondering whether systemd is for you, please have a look at this comparison of init systems by one of the creators of systemd.

systemd tambien puede controlar los antiguos scripts de init.d


# Mejoras
Paralelización
  - smarter dependency resolution
  - faster boot time
smarter service control
  - no more hacks in rc.local
  - auto spawn and respawn (arranca el proceso automáticamente si muere)
  - tracking daemons together with Control Groups
99% backwards compatible init scripts
  - excepciones bien documentadas
will benefit cloud and lightweight images (containers)
