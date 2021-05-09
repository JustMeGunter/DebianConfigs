# DebianConfigs
Configs in debian launched in Qemu to install Docker.

## Qemu initial configuration.
- Create the hard disk `qemu-img create -f qcow2 DebianDockerVD.qcow2 20G`
`qemu-system-x86_64 -cdrom debian.iso -boot order=d -drive file=DevianDockerVD.qcow2,format=qcow2 -m 1024`
- Follow the installation.

## Debian configuration.
- Run the machine `qemu-system-x86_64 -m 2048 -enable-kvm DebianDockerVD.qcow2`
- Set up Firewall:
-- `apt install ufw`
-- Enable rules to 1pv6 too 
	`nano /etc/default/ufw` 
	`IPV6=yes`
-- Setting default polices 
	`ufw default deny incoming`
	`ufw default allow outgoing`
-- Allow ssh
	`ufw allow ssh` or `ufw allow 22` depends /etc/services
-- Enabling UFW
	`ufw enable`
- Installing OpenSSH
-- apt `install openssh-server` and check service is running `systemctl status sshd`
-- Enable ssh on system boot, check the service is enable with `systemctl list-units-files | grep enabled | grep ssh`, if no results shown on the terminal run `systemctl enable ssh`
-- Disabling root login nano `nano /etc/ssh/sshd_config` uncoment `PermitRootLogin no`
-- Power off the machine `poweroff`, and now can run with `qemu-system-x86_64 DebianDockerVD.qcow2 -nic user,hostfwd=tcp::2222-:22 -m 2048 enable-kvm` and connect from host with `ssh gunter@127.0.0.1 -p 2222`

-> Enable Key ssh
-> Install vim Import .vimrc
-> Sudo user gunter
-> Install docker
