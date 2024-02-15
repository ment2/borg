~~~bash
borg create --stats --list borg@192.168.11.160:/var/backup/::"etc-{now:%Y-%m-%d_%H:%M:%S}" /etc

------------------------------------------------------------------------------

Archive name: etc-2024-02-15_13:35:51

Archive fingerprint: 20a4abdecaab26716953725a9a750c7277e9b7a718c99ec224c349cdc919a2c3

Time (start): Thu, 2024-02-15 13:35:51

Time (end):   Thu, 2024-02-15 13:35:57

Duration: 5.61 seconds

Number of files: 1698

Utilization of max. archive size: 0%

------------------------------------------------------------------------------

                       Original size      Compressed size    Deduplicated size

This archive:               28.43 MB             13.49 MB             11.84 MB

All archives:               28.43 MB             13.49 MB             11.84 MB



                       Unique chunks         Total chunks

Chunk index:                    1275                 1690

------------------------------------------------------------------------------

[root@client ~]# borg list borg@192.168.56.100:/var/backup/

etc-2024-02-15_13:35:51              Thu, 2024-02-15 13:35:51 [20a4abdecaab26716953725a9a750c7277e9b7a718c99ec224c349cdc919a2c3]

[root@client ~]# vi /etc/systemd/system/borg-backup.service

[root@client ~]# vi /etc/systemd/system/borg-backup.timer

[root@client ~]# systemctl enable borg-backup.timer

Created symlink from /etc/systemd/system/timers.target.wants/borg-backup.timer to /etc/systemd/system/borg-backup.timer.

[root@client ~]# systemctl start borg-backup.timer

[root@client ~]# systemctl status borg-backup.timer

● borg-backup.timer - Borg Backup

   Loaded: loaded (/etc/systemd/system/borg-backup.timer; enabled; vendor preset: disabled)

   Active: active (elapsed) since Thu 2024-02-15 13:40:46 UTC; 8s ago



Feb 15 13:40:46 client systemd[1]: Started Borg Backup.

root@client ~]# systemctl status borg-backup.service

● borg-backup.service - Borg Backup

   Loaded: loaded (/etc/systemd/system/borg-backup.service; static; vendor preset: disabled)

   Active: inactive (dead) since Thu 2024-02-15 13:58:17 UTC; 28s ago

  Process: 2734 ExecStart=/bin/borg prune --keep-daily 90 --keep-monthly 12 --keep-yearly 1 ${REPO} (code=exited, status=0/SUCCESS)

  Process: 2729 ExecStart=/bin/borg check ${REPO} (code=exited, status=0/SUCCESS)

  Process: 2725 ExecStart=/bin/borg create --stats ${REPO}::etc-{now:%%Y-%%m-%%d_%%H:%%M:%%S} ${BACKUP_TARGET} (code=exited, status=0/SUCCESS)

 Main PID: 2734 (code=exited, status=0/SUCCESS)



Feb 15 13:58:15 client borg[2725]: Number of files: 1700

Feb 15 13:58:15 client borg[2725]: Utilization of max. archive size: 0%

Feb 15 13:58:15 client borg[2725]: ------------------------------------------------------------------------------

Feb 15 13:58:15 client borg[2725]: Original size      Compressed size    Deduplicated size

Feb 15 13:58:15 client borg[2725]: This archive:               28.43 MB             13.49 MB            127.01 kB

Feb 15 13:58:15 client borg[2725]: All archives:               56.85 MB             26.98 MB             11.97 MB

Feb 15 13:58:15 client borg[2725]: Unique chunks         Total chunks

Feb 15 13:58:15 client borg[2725]: Chunk index:                    1282                 3384

Feb 15 13:58:15 client borg[2725]: ------------------------------------------------------------------------------

Feb 15 13:58:17 client systemd[1]: Started Borg Backup.

[root@client ~]# borg list borg@192.168.56.100:/var/backup/

etc-2024-02-15_13:58:13              Thu, 2024-02-15 13:58:14 [9e73d444ad95d897ec719374a7af6c811f22d0463175945ceca448c7aa217333]

[root@client ~]# systemctl list-timers --all

NEXT                         LEFT          LAST                         PASSED      UNIT                         ACTIVATES

Thu 2024-02-15 14:03:13 UTC  1min 59s left n/a                          n/a         borg-backup.timer            borg-backup.service

Fri 2024-02-16 12:54:03 UTC  22h left      Thu 2024-02-15 12:54:02 UTC  1h 7min ago systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service

n/a                          n/a           n/a                          n/a         systemd-readahead-done.timer systemd-readahead-done.service
~~~
