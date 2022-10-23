# User & Group Management

### List users.
```
cat /etc/passwd
```
```
sudo cat /etc/shadow
```

### List groups.
```
cat /etc/group
```

### Delete a group.
```
sudo groupdel <groupname e.g. "mygroup">
```

### Add a user to a group.
```
sudo usermod -aG <groupname e.g. "sudo"> <username e.g. "myuser">
```

### Remove a user from a group.
```
sudo gpasswd -d <username e.g. "myuser"> <groupname e.g. "sudo">
```

### Switch user.
```
su - <username e.g. "myseconduser">
```

### Enable the root account.
```
sudo passwd root
```

### Disable the root account.
```
sudo passwd -l root
```



# Package Management

### Search for a package in the repository.
```
apt search <package e.g. "lvm2">
```

### See if a package is already installed.
```
apt search <package e.g. "lvm2"> | grep installed
```

### List installed packages.
```
dpkg --get-selections > installed_packages.txt
```

### List the reverse dependencies a package has, i.e. list the packages that depend on a package.
```
apt-cache rdepends <package name e.g. "tmux">
```



# Process Management

### List the top 10 CPU consuming processes.
```
ps aux --sort=-pcpu | head -n 10
```

### List the top 10 RAM consuming processes.
```
ps aux --sort=-pmem | head -n 10
```

### Show the status of a systemd unit.
```
systemctl status <unit name e.g. "ssh">
```

### Enable a systemd unit.
```
sudo systemctl enable <unit name e.g. "ssh">
```

### List systemd units.

#### List units that systemd currently has in memory.
```
sudo systemctl list-units
```

#### List unit files installed on the system, in combination with their enablement state.
```
sudo systemctl list-unit-files
```

### List jobs.
```
jobs
```

### Background a job.

#### Run the job in the background.
```
<command e.g. "sudo apt update"> &
```

#### Background the current job and suspend it.
```
Ctrl + z
```

### Resume the suspended job in the background.
```
bg %<job number e.g. "1">
```

### Foreground a job.
```
fg %<job number e.g. "1">
```

### Show process resource limits
```
prlimit --pid <pid e.g. "123">
```

### Show kernel space stack of a given thread or process
```
sudo cat /proc/<pid>/stack
```

### Attach GDB to a running process
```
gdb -p <pid>
```

### Look up processes based on name
```
pgrep <pattern e.g. "bash">
```



# Resource Management

## General

### Show overall resource utilization.
```
htop
```

## CPU

### Show load average as well as uptime.
```
uptime
```

## Memory

### Show memory usage.
```
free -m # in MB
```

### Create a swap file and activate it.
```
sudo fallocate -l <file size e.g. "4G"> <file path e.g. "/swapfile">
sudo chmod 0600 <file path>
sudo mkswap <file path>

# add `<file path> none swap sw 0 0` to /etc/fstab file.

sudo swapon -a
```

### Show swappiness value.
```
cat /proc/sys/vm/swappiness
```

### Change swappiness value.
```
sudo sysctl vm.swappiness=<value>

# add `vm.swappiness = <value>` to /etc/sysctl.conf in order to make a permanent change.
```

### Disable swap.
```
sudo swapoff -a

# Temporary solution, i.e. this doesn't persist after reboot. Edit /etc/fstab file to disable it permanently.
```

### Check if swap is enabled.
```
free -m

# Swap is disabled if the line for "Swap" is all zeros.
```

## Storage

### List all mounted filesystems presented in a tabbed layout.
```
mount | column -t
```

### List all the mounted filesystems and see how much space is left on each.
```
df -h
```
```
df -i # inode information
```

### Show how much space a directory is using.
```
du -hsc *
```

### NCurses Disk Usage - a curses-based version of the 'du'.
```
ncdu
```

### List block devices.
```
lsblk
```
```
sudo fdisk -l
```

### Show block device attributes.
```
blkid # list
```
```
blkid <device e.g. "/dev/sdb1"> # particular device
```

### Create partitions in a disk.
```
sudo fdisk <device e.g. "/dev/sdb">
```

### Format a partition / Build a filesystem.
```
sudo mkfs.<filesystem type e.g. "ext4"> <partition / device e.g. "/dev/sdb1">
```

### Mount a filesystem.
```
sudo mount -t <filesystem type e.g. "ext4"> <device e.g. "/dev/sdb1"> <dir e.g. "/mnt/vol1">

# This command tells the kernel to attach the filesystem found on <device> at the directory <dir>.
```

### Unmount a filesystem.
```
sudo umount <dir e.g. "/mnt/vol1">
```

### Initialize a physical volume for use by LVM.
```
sudo pvcreate <device e.g. "/dev/sdb">
```

### Show physical volumes available.
```
sudo pvdisplay
```

### Create a volume group and assign a physical volume to it.
```
sudo vgcreate <volume group name e.g. "vg-test"> <physical volume e.g. "/dev/sdb">
```

### Add a physical volume to a volume group.
```
sudo vgextend <volume group e.g. "vg-test"> <physical volume e.g. "/dev/sdc">
```

### Show volume group information.
```
sudo vgdisplay
```

### Create a logical volume.
```
sudo lvcreate -n <logical volume name e.g. "lv1"> -L <logical volume size e.g. "5g"> <volume group e.g. "vg-test">
```

### Show logical volume information.
```
sudo lvs
```
```
sudo lvdisplay
```

### Add space to a logical volume.
```
sudo lvextend <logical volume path e.g. "/dev/vg-test/lv1"> -l <new size e.g. "+100%FREE", "+10g">
```

### Resize ext2/ext3/ext4 filesystem.
```
sudo resize2fs <filesystem e.g. "/dev/mapper/vg--test-lv1">
```

### Remove a logical volume.
```
sudo lvremove <volume group and logical volume seperated by a slash e.g. "vg-test/lv1">
```

### Remove a volume group.
```
sudo vgremove <name e.g. "vg-test">
```

### Create a snapshot of a logical volume.
```
sudo lvcreate -s -n <snapshot name e.g. "mysnapshot"> -L <snapshot max size e.g. "5g"> <volume group and logical volume seperated by a slash e.g. "vg-test/lv1">
```

### Roll back to a logical volume snapshot.
```
sudo lvconvert --merge <volume group and logical volume snapshot seperated by a slash e.g. "vg-test/mysnapshot">
```



# Networking

### Change hostname.
```
# Edit /etc/hostname file.
# Edit /etc/hosts file accordingly to resolve the new hostname.
```

### Show IP address information.
```
ip addr show
```
```
ip a
```

### Bring down network interface.
```
sudo ip link set <network interface name e.g. "enp0s3"> down
```

### Bring back up network interface.
```
sudo ip link set <network interface name e.g. "enp0s3"> up
```

### Show DNS servers.
```
systemd-resolve --status | grep DNS\ Servers
```

### List the listening sockets/ports.
```
sudo ss -tulpn

# Using sudo will make the list exhaustive.
```

### Show the listening sockets/ports of a service.
```
sudo ss -tulpn | grep <service e.g. "sshd">
```

### List attempted accesses to a certain port number blocked by UFW.
```
sudo cat /var/log/ufw.log* | grep "DPT=<port number>"
```


## SSH

### Copy a public ssh key to the remote server.
```
ssh-copy-id -i <public key path e.g. "~/.ssh/id_rsa.pub"> -p <port> <remote user>@<remote host>
```

### Start SSH agent.
```
eval $(ssh-agent)
```

### Add an SSH key to a running SSH agent.
```
ssh-add <ssh private key path e.g. "~/.ssh/id_rsa">
```

### Lists fingerprints of all identities currently represented by the agent.
```
ssh-add -l
```

### Deletes all identities from the agent.
```
ssh-add -D
```

### Change SSH key passphrase.
```
ssh-keygen -p
```


## SSHFS

### Unmount SSHFS

#### as a root user
```
sudo unmount <mount point e.g. "/mnt/myfiles">
```

#### as a non-root user
```
fusermount -u <mount point e.g. "/mnt/myfiles">
```


## DNS

### Interrogate DNS name server.
```
dig <name server> <resource record name> <record type>
```



# Tmux

### Detach from tmux.
```
Ctrl + b -> d (press Ctrl + b, release, then press d)
```

### Reattach a session.
```
tmux a
```



# Date & Time

### Show the system date.
```
date
```

### Show the system time and date.
```
timedatectl
```

### List timezones.
```
timedatectl list-timezones
```

### Set a timezone.
```
sudo timedatectl set-timezone <timezone e.g. "Europe/Paris">
```



# Files & Directories
### Create a symbolic link
```
ln -s <target e.g. /etc/nginx/sites-available/sample.com> <link name e.g. /etc/nginx/sites-enabled/sample.com>
```



# Rsync

### Archive (copy everything and retain the metadata) and verbose mode.
```
rsync -av <source> <destination>
```

### Synchronizing directories (NOTE: This deletes files and directories in the destination that are not found in the source).
```
rsync -av --delete <source> <destination>
```

### Taking a backup of the contents that are overwritten.
```
rsync -avb --delete <source> <destination>
```

```
rsync -avb --delete --backup-dir=<backup directory> <source> <destination>

# Specifying the directory where the backup will be placed. This allows us to take incremental backups.
```

### Over SSH at a non-standard port.
```
rsync -av -e "ssh -p <port number>" <source> <remote user>@<remote host>:<destination>
```



# Cron

### Edit the crontab file.

#### For tasks meant to run only for the current user.
```
crontab -e
```

#### Edit the root crontab. For tasks meant to be run with sudo privileges.
```
sudo crontab -e
```



# MariaDB

### Login as a root database user.
```
sudo mariadb
```

### Backup a database.
```
mysqldump -u <mysql user> -p --databases <database name> > <dump file e.g. "dump.sql">
```



# Fail2ban

### List enabled jails.
```
sudo fail2ban-client status
```



# UFW

### Show the status.
```
sudo ufw status
```

### Set default.
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### Set a specific rule.
```
sudo ufw <action e.g. "allow"> proto <protocol e.g. "tcp"> from <source ip/network e.g. "10.0.0.1", "10.0.0.0/8"> to <destination ip e.g. "192.168.0.1"> port <port e.g. "25">
```



# LUKS

### Initialize a LUKS partition and sets the initial passphrase on a block device.
```
sudo cryptsetup luksFormat <device e.g. "/dev/sdb">
```

### Open the LUKS device after successful verification of the supplied passphrase.
```
sudo cryptsetup luksOpen <device e.g. "/dev/sdb"> <name e.g. "backup_drive">
```

### Create a filesystem on the encrypted LUKS partition.
```
sudo mkfs.<filesystem type e.g. "ext4"> <partition / device e.g. "/dev/mapper/backup_drive">
```

### Mount the filesystem

```
sudo mount -t <filesystem type e.g. "ext4"> <device e.g. "/dev/mapper/backup_drive"> <dir e.g. "/media/backup_drive">

# This command tells the kernel to attach the filesystem found on <device> at the directory <dir>.
```

### Unmount the filesystem
```
sudo umount <device e.g. "/dev/mapper/backup_drive">
```

### Close the LUKS device and wipes the key from kernel memory.
```
sudo cryptsetup luksClose <device e.g. "/dev/mapper/backup_drive">
```

### Change the passphrase
```
sudo cryptsetup luksChangeKey <device e.g. "/dev/mapper/backup_drive"> -S 0
```



# Ansible

### Check connection to the servers.
```
ansible all -m ping
ansible all -m ping --ask-pass # with password authentication
```

### Run a playbook.
```
ansible-playbook <playbook e.g. "packages.yml">
ansible-playbook <playbook e.g. "packages.yml"> --ask-pass # with password authentication
```

### Run a playbook in the pull mode.

#### Default to run the playbook inside the repo named as "local.yml".
```
ansible-pull -o -U <git repo url e.g. "https://github.com/myusername/ansible.git">
```

#### Specify the playbook to run.
```
ansible-pull -o -U <git repo url e.g. "https://github.com/myusername/ansible.git"> <playbook e.g. "myplaybook.yml">
```



# Virtualization

### Check if the CPU supports virtualization extensions
```
egrep -c '(vmx|svm)' /proc/cpuinfo

# Supported if the result is 1 or more.
```

## QEMU

### Create a disk image for a vm
```
qemu-img create -f <file_format e.g. "qcow2"> <image_name e.g. "test-img.qcow2"> <image_size e.g. "5G">
```
### Install an OS in a disk image
```
qemu-system-x86_64 -cdrom <iso_image_path> -enable-kvm -m <RAM size e.g. "2M"> -smp <number_of_cores e.g. "2"> -drive file=<disk_image_path>
```

### Run a vm without an installation media attached
```
qemu-system-x86_64 -enable-kvm -m <RAM size e.g. "2M"> -smp <number_of_cores e.g. "2"> -drive file=<disk_image_path>
```

## Libguestfs

### Mount a guest filesystem on the host
```
sudo guestmount -a <disk_image_path> -m <partition> <mount_point e.g. "/mnt">
```

### Unmount a guest filesystem
```
sudo guestunmount <mount_point e.g. "/mnt">
```



# Kernel

### Show configuration of the currently running kernel
```
cat /boot/config-$(uname -r)
```



# eBPF

## BCC
