#### The RPM database is made up of files under the /var/lib/rpm/ directory in CentOS and other enterprise Linux distributions such as RHEL, openSUSE, Oracle Linux and more.


#### If the RPM database is corrupted, RPM will not work correctly, thus updates cannot be applied to your system, you encounter errors while updating packages on your system via YUM package manager.

#### First start by backing up your current RPM database before proceeding

```
[root@ip-172-31-26-143 ~]# tar -zcvf /backups/rpmdb-$(date +"%d%m%Y").tar.gz  /var/lib/rpm
```
#### Verify the integrity of the master package metadata file /var/lib/rpm/Packages; this is the file that needs rebuilding, 

```
[root@ip-172-31-26-143 rpm]# /usr/lib/rpm/rpmdb_verify Packages 
BDB5105 Verification of Packages succeeded.
```
#### In case the above operation fails, meaning you still encounter errors, then you should dump and load a new database. Also verify the integrity of the freshly loaded Packages file as follows.

```
[root@ip-172-31-26-143 rpm]# /usr/lib/rpm/rpmdb_dump Packages.bak | /usr/lib/rpm/rpmdb_load Packages
rpmdb_load: BDB1540 configured environment flags incompatible with existing environment

[root@ip-172-31-26-143 rpm]# /usr/lib/rpm/rpmdb_verify Packages
BDB5105 Verification of Packages succeeded.
```

#### Now to check the database headers, query all installed packages using the -q and -a flags, and try to carefully observe any error(s) sent to the stderror. ( output is discarded to enable printing of errors only )

```
[root@ip-172-31-26-143 rpm]# rpm -qa >/dev/null 
```



#### Rebuild the RPM database using the following command, the -vv option allows for displaying lots of debugging information.



```
[root@ip-172-31-26-143 rpm]# rpm -vv --rebuilddb
D: rebuilding database /var/lib/rpm into /var/lib/rpmrebuilddb.3987
D: opening  db environment /var/lib/rpm private:0x401
D: opening  db index       /var/lib/rpm/Packages 0x400 mode=0x0
D: locked   db index       /var/lib/rpm/Packages
D: opening  db environment /var/lib/rpmrebuilddb.3987 private:0x401
D: opening  db index       /var/lib/rpmrebuilddb.3987/Packages (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Packages 0x1 mode=0x42
D: disabling fsync on database
D:  read h#     259 Header SHA1 digest: OK (140b05787184c2fc1efe240966de87e46d3a82a4)
D: opening  db index       /var/lib/rpmrebuilddb.3987/Name (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Name 0x1 mode=0x42
D: adding "jbigkit-libs" to Name index.
D: opening  db index       /var/lib/rpmrebuilddb.3987/Basenames (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Basenames 0x1 mode=0x42
D: adding 7 entries to Basenames index.
D: opening  db index       /var/lib/rpmrebuilddb.3987/Group (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Group 0x1 mode=0x42
D: adding "Development/Libraries" to Group index.
D: opening  db index       /var/lib/rpmrebuilddb.3987/Requirename (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Requirename 0x1 mode=0x42
D: adding 10 entries to Requirename index.
D: opening  db index       /var/lib/rpmrebuilddb.3987/Providename (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Providename 0x1 mode=0x42
D: adding 4 entries to Providename index.
D: opening  db index       /var/lib/rpmrebuilddb.3987/Conflictname (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Conflictname 0x1 mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Obsoletename (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Obsoletename 0x1 mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Triggername (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Triggername 0x1 mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Dirnames (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Dirnames 0x1 mode=0x42
D: adding 3 entries to Dirnames index.
D: opening  db index       /var/lib/rpmrebuilddb.3987/Installtid (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Installtid 0x1 mode=0x42
D: adding 1 entries to Installtid index.
D: opening  db index       /var/lib/rpmrebuilddb.3987/Sigmd5 (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Sigmd5 0x1 mode=0x42
D: adding 1 entries to Sigmd5 index.
D: opening  db index       /var/lib/rpmrebuilddb.3987/Sha1header (none) mode=0x42
D: opening  db index       /var/lib/rpmrebuilddb.3987/Sha1header 0x1 mode=0x42
D: adding "140b05787184c2fc1efe240966de87e46d3a82a4" to Sha1header index.
D:  read h#       4 Header SHA1 digest: OK (71b3a4f558fba0a134ec243eea74d9c66c7b1cc6)
D: adding "kbd-misc" to Name index.
D: adding 702 entries to Basenames index.
D: adding "System Environment/Base" to Group index.
D: adding 4 entries to Requirename index.
D: adding 1 entries to Providename index.
```
