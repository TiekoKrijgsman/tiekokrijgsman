# Multipass: Ubuntu VMs on demand

Get an instant Ubuntu VM with a single command. Multipass can launch and run virtual machines and configure them with cloud-init like a public cloud.

<https://multipass.run/>

## Installation

```sh
sudo snap install multipass
```

## Usage

```sh
# Launch an instance (by default you get the current Ubuntu LTS)
multipass launch --name foo

# Open a shell in the instance
multipass shell foo

# See your instances
multipass list

# Stop and start instances
multipass start foo
multipass stop foo

# Delete instances
multipass delete foo
multipass purge
```
