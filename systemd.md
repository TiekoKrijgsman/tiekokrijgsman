# Systemd

[Systemd (System and Service Manager)](https://systemd.io/) is a "Init System", it is the first process (runs as <abbr title="Process IDentifier">PID</abbr> 1) that starts when the computer boots, and it is responsible for starting all other processes and managing them.

Units are something Systemd can manage and a service is a type of unit (there are several types of units: service, socket, device, mount point, automount point, swap file or partition, etc.).

A service is a process that runs in the background and is managed by Systemd (e.g: [nginx](https://nginx.org/)).

## `systemctl`

`systemctl` (System Control) is the command line tool to manage Systemd.

- `systemctl status <service>`: Show the status of a service (e.g: `systemctl status nginx` or `systemctl status nginx.service`). The status can be `active` (running), `inactive` (not running), `enabled` (will start on boot), `disabled` (will not start on boot), `masked` (can not be started).
- `systemctl start <service>`: Start a service.
- `systemctl stop <service>`: Stop a service.
- `systemctl restart <service>`: Restart a service.
- `systemctl enable <service>`: Enable a service (will start on boot).
- `systemctl disable <service>`: Disable a service (will not start on boot).
- `systemctl daemon-reload`: Reload the Systemd configuration files (unit files).

To see all the services that are running on the system, run `systemctl list-units --type=service`.

To see all services that will start on boot, run `systemctl list-unit-files --type=service`.

## Unit files

Unit files describes how to interact with a unit. They are located mostly in `/etc/systemd/system/` and `/usr/lib/systemd/system/` (e.g: `/usr/lib/systemd/system/nginx.service`).

```sh
$ cat /usr/lib/systemd/system/nginx.service
[Unit]
Description=A high performance web server and a reverse proxy server
After=network.target network-online.target nss-lookup.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
PrivateDevices=yes
SyslogLevel=err

ExecStart=/usr/bin/nginx -g 'pid /run/nginx.pid; error_log stderr;'
ExecReload=/usr/bin/nginx -s reload
KillMode=mixed

[Install]
WantedBy=multi-user.target
```

We can create our own unit files to manage our own services so that we have fine-grained control over the lifecycle of our applications. With systemd, we can automatically handle crashes, restart our applications, start them on boot, run them in the background, and have a centralized and structured log management.

## `journalctl`

`journalctl` is the command line tool to manage the logs of Systemd.

- `journalctl -u <service>`: Show the logs of a service (e.g: `journalctl -u nginx`).
- `journalctl -u <service> -f`: Show the logs of a service and follow the logs (e.g: `journalctl -u nginx -f`).
