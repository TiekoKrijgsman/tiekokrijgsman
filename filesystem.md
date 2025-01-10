# GNU/Linux Filesystem

[YouTube - Fireship - Linux Directories Explained in 100 Seconds](https://www.youtube.com/watch?v=42iQKuQodW4)

The filesystem is defined by Filesystem Hierarchy Standard (<abbr>FHS</abbr>).

```sh
$ pwd
/

$ ls
bin   dev  home  lib32	libx32	mnt  proc  run	 srv  tmp  var
boot  etc  lib	 lib64	media	opt  root  sbin  sys  usr
```

- `/bin`: Essential user command binaries (for use by all users)
- `/boot`: Static files of the boot loader (Linux kernel, GNU GRUB, etc.)
- `/dev`: Device files
- `/etc`: Host-specific system configuration
- `/lib`: Essential shared libraries and kernel modules (e.g: libc)
- `/media`: Mount point for removable media
- `/mnt`: Mount point for mounting a filesystem temporarily
- `/opt`: Add-on application software packages
- `/run`: Data relevant to running processes
- `/sbin`: Essential system binaries (for use by system administrators)
- `/srv`: Data for services provided by this system
- `/tmp`: Temporary files
- `/usr`: User Binaries
- `/var`: Variable data
- `/home`: Home directories (for use by real users)

## Useful files in `/etc`

- `/etc/passwd`: User account information
- `/etc/shadow`: Secure user account information
- `/etc/group`: Group account information
- `/etc/gshadow`: Secure group account information
- `/etc/sudoers`: List of users who can run commands with `sudo`
- `/etc/hostname`: Hostname of the system
- `/etc/hosts`: Hostname and IP address of the system
- `/etc/resolv.conf`: DNS resolver configuration file
