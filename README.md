# ansible-arch-install

An Ansible playbook to help install Arch Linux and Kubernetes.

# Prepare

```console
IP="192.168.1.21"
ssh-keygen -R "${IP}"; ssh-copy-id -i ~/.ssh/id_ed25519.pub "root@${IP}"
```

# Usage

There are two Ansible scripts:

* site.yml
* kubernetes.yml

## Arch Linux

To install Arch Linux you need site.yml.

After booting from the Arch installation media, you will need to:
1. Set the root password using the `passwd` command.
2. Use `vim` to edit the file `/etc/ssh/sshd_config` by changing the
   `ChallengeResponseAuthentication` setting to `yes`.
3. Restart the ssh service using `systemctl restart sshd`.
4. Create a keyfile on your local host containing the password for
   your LUKS root volume via `echo -n "your_password" > keyfile`.
5. Generate a hash for the password to be used on your personal
   account using `mkpasswd --method=sha-512`.

At this point we are able to login remotely as root, so we can
populate `inventory.yml` and run `site.yml`:

```console
ansible-playbook -i inventory.yml site.yml
```

## Kubernetes

To install Arch Linux you need kubernetes.yml.
It is able to install Controller and Worker nodes.

On Controller node you will find a file */var/log/kubeadm.init.log*
with instruction how to join additional nodes.

How to start with Kubernetes, read this: https://kubernetes.io/

# inventory.yml

Set either
```console
install_drive
```
if you are using a single drive for installation

or

```console
raid_a: /dev/sda
raid_b: /dev/sdb
```

if you are using a Linux software Raid.

NEVER BOTH!

# Status

pre alpha: Do not use if you do not exactly know what you are doing

# Parameters

```console
efi:            [True|False]            Install Arch Linux for UEFI or BIOS based system
install_drive:	/dev/sd...		If no SW Raid, device from which system should boot
raid_a, raid_b:	/dev/sd...              If SW Raid: Raid1 devices
kube_ctl:	[True|False]		Role in Kubernetes (controller=true, worker=false)
pod_address:	x.x.x.x/y               IP Address in Pod network
pod_network:    x.x.x.x/y               *Network* for your Pods
```

# Bugs

*Many* of them:

* Documentation is incomplete
* Scripts are bad hacks for now
* Scripts works in our environments, but possibly not in Yours.

