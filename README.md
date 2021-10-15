# Ansible Role: Hetzner Installimage

Set-up a Hetzner Dedicated Server with a chosen distribution and potentially full disk encryption.

## Warnings

All data on the server will be wiped.

## Requirements

- Know the server IP.
- The server must be rebooted to rescue mode. Or ordered with rescue mode if new.
- When ordering or activating rescue mode be sure to select all keys that you want to be able to loging to the boot environment where the encryption password can be entered.

## Role Variables

See `defaults/main.yml`:

```
hetzner_installimage_cryptpassword: "password"

hetzner_installimage_disks:
  - "/dev/nvme0n1"
  - "/dev/nvme1n1"

hetzner_installimage_swraid: 1
hetzner_installimage_swraidlevel: 1

hetzner_installimage_hostname: "custom"

hetzner_installimage_image: "/root/images/Debian-1010-buster-64-minimal.tar.gz"

# list of public ssh keys allowed to login into the BusyBox environment to enter the encryption password and boot the server
# if not defined the keys used when enabling the rescue environment (or ordering the machine if new) are used
#
#hetzner_installimage_sshkeys: 
#  - "key1"
```

## Example Playbook

```
# hetzner-installimage.yml
---
- hosts: all

  roles:
    - role: silviuvulcan.hetzner_installimage
      hetzner_installimage_image: "/root/images/Debian-1010-buster-64-minimal.tar.gz"
```

```
ansible-playbook -i "1.2.3.4," -u root -e "hetzner_installimage_cryptpassword=MY_SECURE_PASS" hetzner-installimage.yml
```

## Mentions

- You don't need to add the server to your ansible inventory.
- You either define the ssh keys variable or comment it out. If you define it and it's empty the role will fail or you will not be able to login to boot the server.
- **No handholding!** be sure you store the password, you use the right SSH keys and that the server is booted into rescue mode and **doesn't contain any data you need**.
- Only tested with Debian 10 and 11. Should work with Ubuntu too.
- Debian 10 does not support ED25519 keys!

## License

MIT / BSD

## Author Information

This role was created by Silviu Vulcan to scratch his own itch.