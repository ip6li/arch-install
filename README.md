# ansible-arch-install

An Ansible playbook to help install Arch Linux.

# Prepare

```console
IP="192.168.1.21"
ssh-keygen -R "${IP}"; ssh-copy-id -i ~/.ssh/id_ed25519.pub "root@${IP}"
```

# Usage

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

## BIOS boot

```console
ansible-playbook -i inventory.yml site.dos.yml
```

## UEFI boot

NOT TESTED!!!

```console
ansible-playbook -i inventory.yml site.uefi.yml
```

# Status

alpha release

