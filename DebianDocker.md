# DebianConfigs
Configs in debian launched in Qemu to install Docker.

## Qemu initial configuration.
- Create the hard disk `qemu-img create -f qcow2 DebianDockerVD.qcow2 20G`
- Star the installation `qemu-system-x86_64 -cdrom debian.iso -boot order=d -drive file=DevianDockerVD.qcow2,format=qcow2 -m 1024`
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
- Set up sudo
-- Isntall sudo `apt install sudo`
-- Add user to sudo group `usermod -a -G sudo username`
- Set up sshkey
-- On the CLIENT `ssh-keygen -t rsa -b 4096 -C "comment"` and follow de steps.
-- On the SERVER `mkdir -p /home/user_name/.ssh && touch /home/user_name/.ssh/authorized_keys`, `vim authorized_keys` and copy the public key.
-- Change permissions `chmod 700 /home/user_name/.ssh && chmod 600 /home/user_name/.ssh/authorized_keys`
-- Change owner `chown -R username:username /home/username/.ssh`
-- Config sshd `vim /etc/ssh/sshd_config`
--- `PasswordAuthentication no`
--- `AllowUser username`
- Restart service `systemctl restart ssh`
- Check all is fine `systemctl status ssh`
### Docker.
- Installing Docker `sudo apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common`
- Add GPG key `curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -`
- Add the Docker repository APT sources `add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"`
- Update `apt update`
- Install Docker `docker-ce`

-> Enable Key ssh
-> Install vim Import .vimrc
-> Install docker
