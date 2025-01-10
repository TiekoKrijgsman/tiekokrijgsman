# SSH (Secure Shell)

SSH is a network protocol that allows you to connect from one computer to another (e.g: remote command-line execution).

Default port: `22`.

For learning purpose, we will connect 2 VMs (Virtual Machines) using [multipass](./multipass-ubuntu-vm.md) with SSH.

```sh
multipass launch --name primary
multipass shell primary
```

```sh
multipass launch --name secondary
multipass shell secondary
```

We will connect from `primary` (local) VM to `secondary` (remote) VM.

## Create a new user on `secondary` VM

```sh
# Create a new user
# -m: create a home directory
# -s: set the login shell
# -g: set the primary group (same permissions as `ubuntu` user)
# username: the name of the user (e.g: bob)
sudo useradd -m -s /bin/bash -g ubuntu bob

# Set a password for the user
sudo passwd bob

# Connect as `root` user
su root
# or
su -

# Connect as `bob` user
su bob
```

## Generate a SSH key pair on `primary` VM

```sh
# Generate a SSH key pair
# -t: type of key (e.g: rsa, dsa, ecdsa, ed25519)
# -b: number of bits in the key (e.g: 4096)
# -C: comment (e.g: VM primary)
$ ssh-keygen -t rsa -b 4096 -C "Primary VM"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ubuntu/.ssh/id_rsa
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:YwCqu0v+8+p0HZ6+98C2XoHK7ahm5iA2uY9UkxZNTxA Primary VM
The key's randomart image is:
+---[RSA 4096]----+
|    .Eo.         |
|   .o.o          |
|  .. ...         |
| .  o  .  .      |
|.  =   .S. .     |
| .+ . +.*.  .    |
|.B o . * = .     |
|=.*.o+. +.+      |
|o=+=O+.+++..     |
+----[SHA256]-----+
```

```sh
# List files in the `~/.ssh` directory (`id_rsa` is the private key and `id_rsa.pub` is the public key)
$ ls ~/.ssh
authorized_keys  id_rsa  id_rsa.pub

# Display the content of the public key
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC3Tm20fgMOHconEU0hj0rU2widtT8wSkhKnFd4vNrWYXywVaxEbnzI9rnoye0a2SJYeRFMRXsADxaG9Fl51vwd5bkFZuFfw6+m8495/1HDFCfGPxAom09Iuv5tnb4g2keS7T8edhhNFj5oKcZ7V7wpkEZMq3FBcRlfI/bkPl+jmvmXYwcqEkmHWFnz7nNZpcAyIehdT6C7hEDf18qCj/iR4wVHhqkRPlaxW0GXhVtSMM3zF2cltHq4lNZCaReFDMybjShuiEqYXkGD7wUStOyVVxbSfu1XZF7u/4tChI9UqA9aePWPgyMKSaWk3BbungFtkFDAYlwi6efbvN4TzU9lMrPu/J8L0JSAXWro3V15kMpJGr+SSfuo0o7257OjARf6D0u4BlnHj7+HOqr/6kq8Eq10bNjZBsTa32sI1340nbZfKSN1S4gcxVIl4OVpI+zxZaibXNN75q9DmzOmeXr5Eq057ENeAVkac0YTuBxpx+cmYNfSJPoea+aBEXSmgdRHNyWAsPmcHqRnF3nmtFyWaASsMni3oqeKyHdzDvRscXySgcJCP1vrTV+k15Ba1pWFE9zsdCWOKGGd/gZ76Fq7ktHxZ6H05bZLbJGyGwL4VaqQ6BPhg/hurdCUIS6qt035i2OiNjZprEMg7lB4BMHEar0F62oWwCzIkkEyv+cGCQ== Primary VM
```

## Prepare `secondary` VM

```sh
# Create a `.ssh` directory in the home directory of `bob` user
mkdir ~/.ssh
cd ~/.ssh
```

We will authorize the `primary` VM to connect to the `secondary` VM with SSH (more precisely, we will authorize the public key generated on `primary` VM).

```sh
# Paste the content of `~/.ssh/id_rsa.pub` (public key) from `primary` VM in `~/.ssh/authorized_keys` on `secondary` VM
vim authorized_keys

# Set permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Get the IP address of `secondary` VM:

```sh
$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
        valid_lft forever preferred_lft forever
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:75:f8:31 brd ff:ff:ff:ff:ff:ff
    altname enp0s3
    inet 10.209.175.192/24 metric 100 brd 10.209.175.255 scope global ens3
        valid_lft forever preferred_lft forever
    inet6 fe80::5054:ff:fe75:f831/64 scope link
        valid_lft forever preferred_lft forever
```

The IPv4 address of `secondary` VM on this network is `10.209.175.192`.

## Connect from `primary` VM to `secondary` VM

```sh
# Connect from `primary` VM to `secondary` VM (ssh username@host)
ssh bob@10.209.175.192

# Or by specifying the private key to use for authentication
ssh -i ~/.ssh/id_rsa bob@10.209.175.192
```

## SFTP (Secure File Transfer Protocol)

SFTP is a network protocol that allows you to transfer files between computers.

```sh
# Create a file to transfer on `primary` VM (e.g: `hello-world.txt`)
echo "Hello, world!" >> "hello-world.txt"

# Transfer a file from `primary` VM to `secondary` VM (sftp username@host)
sftp bob@10.209.175.192

# PWD on `primary` (local) VM
lpwd

# PWD on `secondary` (remote) VM
pwd

# List files on `primary` (local) VM
lls

# List files on `secondary` (remote) VM
ls

# Transfer `hello-world.txt` file from `primary` VM to `secondary` VM
put hello-world.txt

# Download `hello-world.txt` file from `secondary` VM to `primary` VM
get hello-world.txt

# Exit
exit
```
