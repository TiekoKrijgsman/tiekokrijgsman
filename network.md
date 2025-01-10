# Network commands

Related [Security](../security/README.md).

<abbr>DNS</abbr> (Domain Name System) are resolved using the `/etc/resolv.conf` file.

`/etc/nsswitch.conf` is used to configure the order of the name resolution services.

## `ping`

`ping` is a command-line utility that can be used to test the reachability of a host on an Internet Protocol (<abbr>IP</abbr>) network.

```sh
$ ping theoludwig.fr
PING theoludwig.fr (76.76.21.21) 56(84) bytes of data.
64 bytes from 76.76.21.21 (76.76.21.21): icmp_seq=1 ttl=122 time=12.6 ms
64 bytes from 76.76.21.21 (76.76.21.21): icmp_seq=2 ttl=122 time=13.7 ms
64 bytes from 76.76.21.21 (76.76.21.21): icmp_seq=3 ttl=122 time=14.7 ms
64 bytes from 76.76.21.21 (76.76.21.21): icmp_seq=4 ttl=122 time=15.1 ms
64 bytes from 76.76.21.21 (76.76.21.21): icmp_seq=5 ttl=122 time=14.6 ms
64 bytes from 76.76.21.21 (76.76.21.21): icmp_seq=6 ttl=122 time=15.5 ms
^C
--- theoludwig.fr ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5007ms
rtt min/avg/max/mdev = 12.635/14.379/15.453/0.942 ms
```

## Making HTTP requests

- `wget`: retrieves content from web servers made by GNU (e.g: `wget https://example.com/file.txt`)
- `curl`: transferring data using various network protocols (e.g: `curl https://example.com/file.txt`)

## IP information

`ifconfig` is part of the package `net-tools`, which is not installed by default, because it's deprecated and superseded by the command `ip` from the package `iproute2`.

So instead to use `ifconfig`, use `ip addr show`:

```sh
$ ip address show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
        valid_lft forever preferred_lft forever
2: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 2c:8d:b1:df:a1:fb brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.64/24 brd 192.168.1.255 scope global dynamic noprefixroute wlan0
        valid_lft 83016sec preferred_lft 83016sec
    inet 192.168.1.66/24 brd 192.168.1.255 scope global secondary dynamic noprefixroute wlan0
        valid_lft 83024sec preferred_lft 72224sec
    inet6 fe80::6b58:2a73:9b5f:aa3f/64 scope link noprefixroute
        valid_lft forever preferred_lft forever
    inet6 fe80::462e:7cdf:7591:1b81/64 scope link
        valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:da:b4:88:b8 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
        valid_lft forever preferred_lft forever
```

## `netstat`

`netstat` is a command-line tool that can provide information about network connections.

To list all TCP or UDP ports that are being listened on, including the services using the ports and the socket status use the following command:

```sh
netstat -tunlp
```

- `-t`: Show TCP ports.
- `-u`: Show UDP ports.
- `-n`: Show numerical addresses instead of resolving hosts.
- `-l`: Show only listening ports.
- `-p`: Show the PID and name of the listener's process. This information is shown only if you run the command as root or sudo user.

## `dig`

`dig` is a command-line tool for querying DNS name servers for information about host addresses, mail exchanges, name servers, and related information.

```sh
# Get the IP address of a domain name
dig +short theoludwig.fr

# Get the public/external IP address (using OpenDNS)
dig +short myip.opendns.com @resolver1.opendns.com
```

## `traceroute`

`traceroute` is a command-line tool that can be used to trace the route that an Internet Protocol (<abbr>IP</abbr>) packet takes to reach an end host.

If you want to use IPv6, use the option `-6`: `traceroute -6 google.fr`, by default it uses IPv4.

```sh
$ traceroute google.fr
traceroute to google.fr (216.58.215.35), 30 hops max, 60 byte packets
    1  box (192.168.1.1)  2.416 ms  2.450 ms  2.785 ms
    2  * * *
    3  134.254.69.86.rev.sfr.net (86.69.254.134)  7.402 ms  7.390 ms  8.688 ms
    4  71.146.6.194.rev.sfr.net (194.6.146.71)  16.147 ms 220.147.6.194.rev.sfr.net (194.6.147.220)  16.116 ms  15.746 ms
    5  71.146.6.194.rev.sfr.net (194.6.146.71)  16.853 ms 220.147.6.194.rev.sfr.net (194.6.147.220)  18.030 ms  18.013 ms
    6  72.14.194.30 (72.14.194.30)  16.584 ms 74.125.146.198 (74.125.146.198)  14.132 ms 72.14.194.30 (72.14. 194.30)  14.068 ms
    7  108.170.244.193 (108.170.244.193)  14.468 ms 108.170.245.1 (108.170.245.1)  15.389 ms  15.350 ms
    8  108.170.235.15 (108.170.235.15)  14.328 ms  17.061 ms  16.772 ms
    9  par21s17-in-f3.1e100.net (216.58.215.35)  16.076 ms  15.470 ms  15.906 ms
```

`***` means that the router doesn't respond to the request.

In this example, it took 9 hops to reach the server.

To understand how it works, see [Network - Understanding the Internet](../network/README.md#understanding-traceroute).
