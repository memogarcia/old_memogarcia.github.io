---
layout: post
title:  "Linux commands"
date:   2015-06-09 19:23:00
categories: linux shell
---

# Kernel and system information

{% highlight bash %}
uname -a                           # Get the kernel version (and BSD version)
uptime                             # Show how long the system has been running + load
hostname                           # system's host name
man hier                           # Description of the file system hierarchy
last reboot                        # Show system reboot history
{% endhighlight %}


# Hardware information

{% highlight bash %}
dmesg                              # Detected hardware and boot messages
lsdev                              # information about installed hardware
cat /proc/cpuinfo                  # CPU model
cat /proc/meminfo                  # Hardware memory
grep MemTotal /proc/meminfo        # Display the physical memory
watch -n1 'cat /proc/interrupts'   # Watch changeable interrupts continuously
free -m                            # Used and free memory (-m for MB)
cat /proc/devices                  # Configured devices
lspci -tv                          # Show PCI devices
lsusb -tv                          # Show USB devices
lshal                              # Show a list of all devices with their properties
dmidecode                          # Show DMI/SMBIOS: hw info from the BIOS
{% endhighlight %}


# Load, statistics and messages

{% highlight bash %}
top                                # display and update the top cpu processes
mpstat 1                           # display processors related statistics
vmstat 2                           # display virtual memory statistics
iostat 2                           # display I/O statistics (2 s intervals)
systat -vmstat 1                   # BSD summary of system statistics (1 s intervals)
systat -tcp 1                      # BSD tcp connections (try also -ip)
systat -netstat 1                  # BSD active network connections
systat -ifstat 1                   # BSD network traffic through active interfaces
systat -iostat 1                   # BSD CPU and and disk throughput
ipcs -a                            # information on System V interprocess
tail -n 500 /var/log/messages      # Last 500 kernel/syslog messages
tail /var/log/warn                 # System warnings messages see syslog.conf
{% endhighlight %}


# Users

{% highlight bash %}
id                                 # Show the active user id with login and group
last                               # Show last logins on the system
who                                # Show who is logged on the system
groupadd admin                     # Add group "admin" and user colin (Linux/Solaris)
useradd -c "Memo Garcia" -g admin -m memo
usermod -a -G <group> <user>       # Add existing user to group (Debian)
groupmod -A <user> <group>         # Add existing user to group (SuSE)
userdel memo                       # Delete user colin (Linux/Solaris)
adduser memo                       # FreeBSD add user joe (interactive)
rmuser memo                        # FreeBSD delete user joe (interactive)
pw groupadd admin                  # Use pw on FreeBSD
pw groupmod admin -m newmember     # Add a new member to a group
pw useradd memo -c "Memo Garcia" -g admin -m -s /bin/tcsh
pw userdel memo; pw groupdel admin

echo "Sorry no login now" > /etc/nologin  # prevent logins in the system but root

# Encrypted passwords are stored in /etc/shadow for Linux. 
# If the master.passwd is modified manually (say to delete a password),
# to rebuild the database run::
pwd_mkdb -p master.passwd

{% endhighlight %}


# Limits

{% highlight bash %}
ulimit -n 10240                    # This is only valid within the shell

cat /etc/security/limits.conf
*   hard    nproc   250              # Limit user processes
asterisk hard nofile 409600          # Limit application open files

sysctl -a                          # View all system limits
sysctl fs.file-max                 # View max open files limit
sysctl fs.file-max=102400          # Change max open files limit
echo "1024 50000" > /proc/sys/net/ipv4/ip_local_port_range  # port range
cat /etc/sysctl.conf
fs.file-max=102400                   # Permanent entry in sysctl.conf
cat /proc/sys/fs/file-nr           # How many file descriptors are in use
{% endhighlight %}


# Reset root password

{% highlight bash %}
# At the boot loader (lilo or grub), enter the following boot option:
init=/bin/sh

# The kernel will mount the root partition and init will start the 
# bourne shell instead of rc and then a runlevel. Use the command 
# passwd at the prompt to change the password and then reboot. 
# Forget the single user mode as you need the password for that.
# If, after booting, the root partition is mounted read only, remount it rw:

mount -o remount,rw /
passwd                             # or delete the root password (/etc/shadow)
sync; mount -o remount,ro /        # sync before to remount read only
reboot
{% endhighlight %}


# Kernel modules

{% highlight bash %}
lsmod                              # List all modules loaded in the kernel
modprobe isdn                      # To load a module (here isdn)
{% endhighlight %}


# Compile kernel

{% highlight bash %}
cd /usr/src/linux
make mrproper                      # Clean everything, including config files
make oldconfig                     # Reuse the old .config if existent
make menuconfig                    # or xconfig (Qt) or gconfig (GTK)
make                               # Create a compressed kernel image
make modules                       # Compile the modules
make modules_install               # Install the modules
make install                       # Install the kernel
reboot
{% endhighlight %}


# Repair grub

So you broke grub? Boot from a live cd, [find your linux partition under /dev and use fdisk to find the linux partion] mount the linux partition, add /proc and /dev and use grub-install /dev/xyz. Suppose linux lies on /dev/sda6:

{% highlight bash %}

mount /dev/sda6 /mnt               # mount the linux partition on /mnt
mount --bind /proc /mnt/proc       # mount the proc subsystem into /mnt
mount --bind /dev /mnt/dev         # mount the devices into /mnt
chroot /mnt                        # change root to the linux partition
grub-install /dev/sda              # reinstall grub with your old settings
{% endhighlight %}


# Misc Mac

Disable OSX virtual memory (repeat with load to re-enable). Faster system, but a little risky.

{% highlight bash %}

sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.dynamic_pager.plist
sleep 3600; pmset sleepnow           # go to standby in one hour (OSX)
defaults write -g com.apple.mouse.scaling -float 8
                                     # OSX mouse acceleration (use -1 to reverse)
{% endhighlight %}


# Processes

{% highlight bash %}
ps -auxefw                         # Extensive list of all running process
ps axww | grep cron
  586  ??  Is     0:01.48 /usr/sbin/cron -s
ps axjf                            # All processes in a tree format (Linux)
ps aux | grep 'ss[h]'              # Find all ssh pids without the grep pid
pgrep -l sshd                      # Find the PIDs of processes by (part of) name
echo $$                            # The PID of your shell
fuser -va 22/tcp                   # List processes using port 22 (Linux)
pmap PID                           # Memory map of process (hunt memory leaks) (Linux)
fuser -va /home                    # List processes accessing the /home partition
strace df                          # Trace system calls and signals
truss df                           # same as above on FreeBSD/Solaris/Unixware
{% endhighlight %}


# Priority

{% highlight bash %}
renice -5 586                      # Stronger priority

# nice changes the CPU scheduler
nice -n -5 top                     # Stronger priority (/usr/bin/nice)
nice -n 5 top                      # Weaker priority (/usr/bin/nice)
nice +5 top                        # tcsh builtin nice (same as above!)

# ionice changes the IO scheduler
ionice c3 -p123                    # set idle class for pid 123 (Linux only)
ionice -c2 -n0 firefox             # Run firefox with best effort and high priority
ionice -c3 -p$$                    # Set the actual shell to idle priority
{% endhighlight %}


# Background and foreground

When started from a shell, processes can be brought in the background and back to the foreground with [Ctrl]-[Z] (^Z), bg and fg. List the processes with jobs. When needed detach from the terminal with disown.

{% highlight bash %}

ping cb.vu > ping.log
^Z                                   # ping is suspended (stopped) with [Ctrl]-[Z]
bg                                 # put in background and continues running
jobs -l                            # List processes in background
    [1]  - 36232 Running                       ping cb.vu > ping.log
    [2]  + 36233 Suspended (tty output)        top
fg %2                              # Bring process 2 back in foreground

make                               # start a long compile job but need to leave the terminal
^Z                                   # suspended (stopped) with [Ctrl]-[Z]
bg                                 # put in background and continues running
disown -h %1                       # detatch process from terminal, won't be killed at logout
{% endhighlight %}


# Top

The program top displays running information of processes. See also the program htop from htop.sourceforge.net (a more powerful version of top) which runs on Linux.
While top is running press the key h for a help overview. Useful keys are:

{% highlight bash %}
u [user name] To display only the processes belonging to the user. Use + or blank to see all users
k [pid] Kill the process with pid.
1 To display all processors statistics (Linux only)
R Toggle normal/reverse sort.
{% endhighlight %}


# Signals/Kill

{% highlight bash %}
ping -i 60 cb.vu > ping.log &
    [1] 4712
kill -s TERM 4712                  # same as kill -15 4712
killall -1 httpd                   # Kill HUP processes by exact name
pkill -9 http                      # Kill TERM processes by (part of) name
pkill -TERM -u www                 # Kill TERM processes owned by www
fuser -k -TERM -m /home            # Kill every process accessing /home (to umount)

# Important signals are:
    1       HUP (hang up)
    2       INT (interrupt)
    3       QUIT (quit)
    9       KILL (non-catchable, non-ignorable kill)
    15     TERM (software termination signal)
{% endhighlight %}


## File system

# Permissions

Change permission and ownership with chmod and chown. The default umask can be changed for all users in /etc/profile for Linux. The default umask is usually 022. The umask is subtracted from 777, thus umask 022 results in a permission 0f 755.

{% highlight bash %}

1 --x execute                        # Mode 764 = exec/read/write | read/write | read
2 -w- write                          # For:       |--  Owner  --|   |- Group-|   |Oth|
4 r-- read
  ugo=a                              u=user, g=group, o=others, a=everyone
  
chmod [OPTION] MODE[,MODE] FILE    # MODE is of the form [ugoa]*([-+=]([rwxXst]))
chmod 640 /var/log/maillog         # Restrict the log -rw-r-----
chmod u=rw,g=r,o= /var/log/maillog # Same as above
chmod -R o-r /home/*               # Recursive remove other readable for all users
chmod u+s /path/to/prog            # Set SUID bit on executable (know what you do!)
find / -perm -u+s -print           # Find all programs with the SUID bit
chown user:group /path/to/file     # Change the user and group ownership of a file
chgrp group /path/to/file          # Change the group ownership of a file
chmod 640 `find ./ -type f -print` # Change permissions to 640 for all files
chmod 751 `find ./ -type d -print` # Change permissions to 751 for all directories

{% endhighlight %}


# Disk information

{% highlight bash %}

hdparm -I /dev/sda                 # information about the IDE/ATA disk (Linux)
fdisk /dev/ad2                     # Display and manipulate the partition table
smartctl -a /dev/ad2               # Display the disk SMART info

{% endhighlight %}


## System mount points/Disk usage

{% highlight bash %}

mount | column -t                  # Show mounted file-systems on the system
df                                 # display free disk space and mounted devices
cat /proc/partitions               # Show all registered partitions (Linux)

{% endhighlight %}

# Disk usage

{% highlight bash %}

du -sh *                           # Directory sizes as listing
du -csh                            # Total directory size of the current directory
du -ks * | sort -n -r              # Sort everything by size in kilobytes
ls -lSr                            # Show files, biggest last

{% endhighlight %}


# Who has which files opened

This is useful to find out which file is blocking a partition which has to be unmounted and gives a typical error of:

{% highlight bash %}

umount /home/
umount: unmount of /home             # umount impossible because a file is locking home
   failed: Device busy


fuser -m /home                     # List processes accessing /home
lsof /home

COMMAND   PID    USER   FD   TYPE DEVICE    SIZE     NODE NAME
tcsh    29029 memo    cwd    DIR   0,18   12288  1048587 /home/memo (guam:/home)
lsof    29140 memo    cwd    DIR   0,18   12288  1048587 /home/memo (guam:/home)
{% endhighlight %}


# Mount/remount a file system

{% highlight bash %}
mount -t auto /dev/cdrom /mnt/cdrom   # typical cdrom mount command
mount /dev/hdc -t iso9660 -r /cdrom   # typical IDE
mount /dev/scd0 -t iso9660 -r /cdrom  # typical SCSI cdrom
mount /dev/sdc0 -t ntfs-3g /windows   # typical SCSI
{% endhighlight %}


# remount

{% highlight bash %}
mount -o remount,ro /              # Linux
{% endhighlight %}


# virtualbox

{% highlight bash %}
VBoxManage 

# allow share on the host
VBoxManage sharedfolder add "GuestName" --name "share" --hostpath "C:\hostshare"
{% endhighlight %}


# Add swap on the fly

{% highlight bash %}

dd if=/dev/zero of=/swap2gb bs=1024k count=2000
mkswap /swap2gb                    # create the swap area
swapon /swap2gb                    # activate the swap. It now in use
swapoff /swap2gb                   # when done deactivate the swap
rm /swap2gb

{% endhighlight %}


# Mount an image

{% highlight bash %}

hdiutil mount image.iso                               # OS X
mount -t iso9660 -o loop file.iso /mnt                # Mount a CD image
mount -t ext3 -o loop file.img /mnt                   # Mount an image with ext3 fs

{% endhighlight %}


# Disk perfomance

Read and write a 1 GB file on partition ad4s3c (/home)

{% highlight bash %}

time dd if=/dev/ad4s3c of=/dev/null bs=1024k count=1000
time dd if=/dev/zero bs=1024k count=1000 of=/home/1Gb.file
hdparm -tT /dev/hda      # Linux only

{% endhighlight %}

