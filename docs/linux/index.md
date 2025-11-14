# Clark's Linux Exam Prep notes

## Account expiry information of a user

apropos account info | grep expiry


## Kill a process

apropos kill process | grep kill
man kill
/pid
sudo kill -9 [process_id]


## Change file permissions

apropos change file permissions | grep mode


## Check free sectors

sudo parted /dev/sda unit MiB print free

sudo cfdisk /dev/sda


## No free sector

delete sda2


## Create parent directory if not available

use -p option
mkdir -p /backup/data


## Install CentOS in VirtualBox

https://youtu.be/4GwJVW4vDYo


## Find list of search commands

apropos search


## Search inside "man"

Use /

example:
/case


## Return text line from file with matching text ignoring case

man -i hello myfile


## documentation for libraries, system utilities, and other software packages

/usr/share/doc


## Linux combination

Combination of Linux kernel and GNU Software


## List directory contents and show hidden files in long format

ls -al


## Check what the command does with a short description

whatis [command]


## Principle of least privilege

Provide users the minimum access required to perform its functions


## List but only show the directory and not the contents

sudo ls -ld [directory]


## Different types of accounts

root, standard, service


## Superuser

root


## Edit with elevated privileges

sudoedit


## Contains rules that users should follow when using the "sudo" command

/etc/sudoers


## Directly edit /etc/sudoers

sudo visudo


## Group that has root level privileges

wheel


## Elevate privileges to those of root

su root


## Elevate your credentials and context to those of root

su - root


## Where the account is stored

/etc/passwd


## Where the account is configured to various options

/etc/login.defs


## Account's home directory

/etc/home


## Account's home directory populate using files from

/etc/skel


## Tell the system to reboot after 15 minutes and cancel it

sudo /sbin/shutdown -r 15
ctrl + c
sudo shutdown -c


## Modern storage location for hashed password

/etc/shadow


## View default settings for newly created users

sudo useradd -D


## Another command to view default settings for newly created users

less /etc/login.defs


## View what files will be copied to home directory of newly created users

ls -a /etc/skel


## Add new user

sudo useradd username


## Add new user with comment to add full name

sudo useradd -c "Clark Ngo" cngo


## Add new user with account expiry

sudo useradd -e 2025/12/31 [username]


## Change password expiration

chage -E 2025/12/31 [username]


## Configure password of a user

sudo passwd [username]


## Attach a real name to an existing user

sudo usermod -c "[User Name]" [username]


## List details of a user password information

sudo chage -l [username]


## Modify user account's expiry

sudo chage -E 2026/12/31 [username]


## Lock a user's account

sudo passwd -l [username]


## Unlock a user's account

sudo passwd -u [username]


## Another way to lock a user's account

sudo usermod -L [username]


## Another way to unlock a user's account

sudo usermod -U [username]


## Three commands to edit /etc/passwd file

usermod, useradd, userdel


## Change default shell of user to KornShell

sudo usermod -s /bin/ksh [username]


## Associate accounts with similar security requirements

Groups


## Groups location

/etc/group


## Three ways to edit groups file

groupadd, groupmod, groupdel


## Add user to an existing group

sudo usermod -aG [group_name] [username]


## Change group name

sudo groupmod -n [new_name] [group_name]


## Create a new group

sudo groupadd [GroupName]


## Check current user

whoami


## Check current user details

w


## Check how long user is idle

who -u


## Check the history of user login and logout

last


## Check id of user

id [username]


## Why use the command id?

easy-to-read


## whoami, who, w

whoami - check what user account you are currently using
who - see other users logged in 
w - display user-owned processes


## Customize user environment

/home/[username]/.bashrc


## Storage location for scripts and environment variables

/etc/profile.d


## .bashrc vs .bash_profile

.bashrc - executed on subsequent login
.bash_profile - executed on first login


## profile vs profile.d

profile - system-wide customization for all users
profile.d - best practice to edit profile


## Permission contexts

owner (u): file or directory owner
group (g): file or directory group and all users in the group
others (o): neither user nor group member


## Permission operators

+: Grants permissions
-: Denies permissions
=: Assign permissions exactly as provided


## Permission attributes

read (r) 
- Files: view file contents
- Directories: list the contents of a directory
write (w)
- Files: save changes
- Directories: create, rename, delete
execute (x)
- Files: run script, programs, etc.
- Directories: access the directory, execute a file in the directory, or perform a task on the directory


## Permission string

[type_of_object] [owner_permissions] [group permissions] [other permissions]

For example:
d rwx r-x r-x


## chmod two modes

symbolic mode: set permissions using three components
- permission contexts
- permission operators
- permission attributes
absolute mode: set permissions using octal (base-8) numbers
- 4: read
- 2: write
- 1: execute


## Change permission on a file for all permission attributes on user and groups using absolute mode

chmod 770 [filename]


## Change permission on a file to remove read, write, execute on others using symbolic mode

chmod o-rwx [filename]


## Give everyone execute permission using both symbolic and absolute mode

chmod +x [filename]
chmod 775 [filename]


## Alters default permissions on newly created files

umask

Given 666 on a file
a 022 umask is:
0: owner has no masking. keep current permissions
2: group permission subtracted by 2, only read access available now 
2: others permission subtracted by 2, only read access available now


## Change all files to read-only for everyone in a directory

chmod -R 644 *


## Add umask 022

/home/[username]/.bashrc


## Change ownership for owner, group, or both on a filename/directory

chown [username] [file/directory name]
- change owner but not group
chown [username]:[group_name] [file/directory name]
- change owner and group
chown [username]: [file/directory name]
- change owner and group changes to user's login group
chown :[group_name] [file/directory name]
- change group

Example:
sudo chown -R :DeveloperDept /Developer/


## Change file ownership to a user for all files and directory in the current directory

sudo chown -R [username]:[group_name] *


## Change group without changing owner

chgrp [group_name] [filename]


## SUID vs SGID

SUID: allow user to have similar permission as the owner of the file
- use of chmod
SGID: allow user to have similar permission as the group owner of the file
- use of chmod


## Special permission that provides delete protection to files and directories and only owner can delete

sticky bit

if you want sticky bit on files, sticky bit the directory instead.


## t vs T in sticky bit

t: will replace the value in the execute symbol of others location, if execute symbol exists
T: will replace the value in the execute symbol of others location, if execute symbol does not exist

command: chmod +t [filename/directory]

Example:
rwx-rwx-rwx becomes rwx-rwx-rwt
rwx-rwx-rw- becomes rwx-rwx-rwT


## Add sticky bit using symbolic mode and absolute mode to a directory

chmod +t [directory]
chmod 1### [directory]


## Prevent file/directory modification even with root

immutable flag

lowercase i character in the file attributes


## List attributes of file/directory with version number

lsattr -v [file/directory]


## Change attribute of file/directory to immutable

chattr +i [file/directory]


## List of permissions attached to an object

Access Control Lists (ACL)


## Command to retrieve or set ACLs of files and directories

getfacl
setfacl


## Set group for file as the same as target directory's group name whenever a user creates a file inside

sudo chmod -R g+s [directory]

+s: add SUID (set user ID)/SGID (set group ID)

Note: when you verify this, the group attribute (rwx) becomes (rws)


## Add immutability to multiple txt files starting with "notes"

Given: notes1.txt, notes2.txt, notes3.txt
sudo chattr -i notes*.txt


## Show if a file has immutable attribute

sudo lsattr -l [file]

-l: long name instead of abbreviations


## Add read access to a file/directory for a user

sudo setfacl -m u:[username]:r [file/directory]

-m: modify


## Verify ACL of a file/directory

getfacl [file/directory]


## Grant read-only permissions to a group name to a directory

sudo setfacl -R -m g:[group_name]:r [directory]

Note: Immutable files will not be affected and the command will process other files/directories


## Owner of file, denied read permission

use chmod to grant read privileges to owner context


## User can't remove directory even with write permission

use chmod to add execute permission to directory for the user context


## User can't access directory even with write permission

use chmod to add execute permission to the directory for the user context


## User can't remove file even with full permissions on file

use chmod to add write permission to the directory of the file


## User can't create files in a directory even with write and execute permissions

check immutable flag
remove immutable flag as root using chattr command


## Root user can't modify a file

check immutable flag
remove immutable flag as root using chattr command


## All users having ability to list directory contents when only owner, group members, and specific service account should only have the ability

remove read permission for others context
add service account to directory ACL using setfacl command to grant account read access


## User can't change script contents even if user has execute permission for the script

use chmod to write permission for script to appropriate context


## User can't execute script even if user has execute permission for the script

use chmod to write permission for script to appropriate context


## All users can delete files when only owner or root should be able to delete files

Add sticky bit


## User can't access file even with full permission for owner context

change the ownership of the file to the user using chown command


## User can't delete file even with full permissions to the group for the directory

change the directory's group to the same of the user's group using chgrp command


## Some users can edit a file even with read only permission

change group ownership of the directory to the correct owning group using chgrp command


## Whenever a file is created by a user, the user's group ID is taken when it should've been the directory's group ID

set the SGID permission on the containing directory to inherit the directory's group ID whenever a file is created

use chmod command with symbolic mode


## Whenever a file is created by a user, the directory's group ID is taken when it should've been the user's group ID

disable the SGID permission to remove the inheritance
use chmod


## User can't remove a directory

user must have execute permission on the directory


## User can't cd to a directory

User should be assigned with execute permission


## storage devices and 4 types of storage devices

Storage device - physical component to record and hold data persistently

Hard disk drive (HDD) - magnetic storage
Solid-state drive (SSD) - non-mechanical. Quick access than HDDs.
USB thumb drive - portable. uses flash technology. smaller compared to HDD and SSD.
External storage drive - portable.


## Block devices vs character devices

Block - storage devices
Character - input devices such as keyboards, mouse


## File systems and types of file systems

File system - data structure used by OS to store, retrieve, organize, manage files and directories on storage devices.

FAT - File Allocation Table. old file system. compatible with Unix, Windows, and MacOS.
ext2 - native Linux file system
ext3 - improved ext2. faster recovering of data and better data integrity.
ext4 - improved ext3. add ed journaling. bigger volumes. default for Ubuntu.
XFS - 64-bit. high-performance journaling file system. default for CentOS/RHEL 7.


## NTFS

New Technology File System (NTFS)
Built by Microsoft for Windows OS
Includes file- and folder-level security, file encryption, drive compression, scalability

Not supported by Linux by default. NTFS-3G for Linux systems


## Network File Systems and Types

enables data sharing over a network
Types
- SMB (Server Message Block): provides shared access across LAN. SMB clients requests for resources to SMB servers, which respond and grant appropriate access level. Used with Windows. Good for mix of Windows and Linux protocols
- CIFS (Common Internet File Systems): an implementation of SMB. Designed by Microsoft as next to SMB v1 but SMB v2 and v3 are better. Linux uses some of CIFS.
- NFS (Network File System): similar to SMB. Protocols not compatible. NFS preferred for Linux client access to Linux server.


## INODES

Index node (inode): object that stores metadata and identified by a unique integer called inode number.
includes
- time-based values such as created and last modified
- permission and ownership information
- block locations of file data
- misc information


## Check inode for contents of directory

ls -i


## Journaling and Journaling phases

process of how file system records changes that have not been made to the file system. It will be saved to an object called journal. Allows quick recovery for unexpected interruptions.

Phases
- describe all the changes on the drive.
- makes each change as it is entered in the journal.
- pending changes are performed when rebooted 
- incomplete entries are discarded.


## Virtual File System and Examples

Common software system that is located between the kernel and real file systems
Translates real file system details to the kernel
Mount different File Systems and looks same to the user

Examples:
- proc
- devtmpfs
- debugfs


## File System Label

assigned for easy identification
labels can be up to 16 character long
can be displayed or changed by command e2label


## Partition and Partition Types

section of the storage drive that logically acts as a separate drive
covert large drive to smaller manageable chunks
identified through partition table

Types
- Primary: can contain 1 file system or logical drive. referred to as volume. includes swap FS and boot partition.
- Extended: can contain many file system. referred to as logical drives. can only have 1 extended partition can be subdivided. has separate partition table.
- Logical: part of physical drive that has been partitioned and allocated. created within an extended partition. subset of extended partition.


## Swap space

partition on storage device that will be used when system runs out of physical memory. Pushes files from RAM to the swap space to free up memory.


## Create, modify, delete partitions

fdisk /dev/sda


## GNU parted menu options

parted


## partprobe command

update changes to kernel in partition table


## mkfs command

build a Linux file system on a device


## Configuration FILE that stores information about storage devices and partitions

/etc/fstab
- this is a file


## Configuration FILE that stores information about encrypted devices and partitions

/etc/crypttab


## Storage device setup process

1. partition storage device with fdisk or parted
2. format partition with a file system using mkfs
3. add formatted partition to the stab file


## Contains files that represent and support devices attached to the system

/dev/


## Identifiers for devices

/dev/disk/by-id
/dev/disk/by-path
/dev/disk/by-uuid


## Setup new partition on a storage device

sudo fdisk /dev/sdb


## Create a file system ext3 on sdb3 partition

sudo mkfs.ext3 /dev/sdb3


## Create a new logical partition with 4095M size

sudo fdisk /dev/sda
Create new with "n"
Default for partition and first sector
+4096M in last sector
Write with "w" to alter the partition table
update Linux kernel with partition table with sudo partprobe


## Update Linux kernel with partition table

sudo partprobe


## Create XFS file system at sda2

mkfs.xfs /dev/sda2


## Apply label to XFS new partitions

sudo xfs_admin -l /dev/sda


## Device mapping

process to abstract virtual storage devices


## DM-multipath

Feature of Linux kernel that provides redundancy and improved performance for block storage devices


## Configuration FILE for DM-Multipath

/etc/multipath.conf


## Command to manage software-based RAID arrays

mdadm


## Logical Volume Manager

Dynamically create, delete, and resize volumes with having to reboot systems


## Contains all logical volumes on a system managed by Logical Volume Manager

/dev/mapper/


## 3 categories of Logical Volume Manager tools

Physical volume (PV) tools
- pvscan, pvcreate, pvdisplay,  pvchange, pvs, pvck, pvremove
Volume group (VG) tools
- vgscan, vgcreate, vgdisplay, vgchange, vgs, vgck, vgrename, vgreduce, vgextend, vgmerge, vgsplit, vgremove
Logical volume (LV) tools
- lvscan, lvcreate, lvdisplay, lvchange, lvs, lvrename, lvreduce, lvextend, lvresize, lvremove


## Create or remove a physical volume on /dev/sdd2

pvcreate /dev/sdd2
pvdelete /dev/sdd2


## Create a shared volume group on /dev/sdd1 and /dev/sdd2

vgcreate shared /dev/sdd1 /dev/sdd2


## Create logical volume name sales and volume group vg_sales with 20 GB

lvcreate --name sales --size 20G vg_sales


## Show current logical volumes

ls /dev/mapper


## Show details on physical volume, volume group, and logical volume

sudo pvscan


## Show more details on physical volume, volume group, and logical volume

sudo pvdisplay


## Show volume group details

sudo vgscan


## Show more details on volume group

sudo vgdisplay


## Show details on logical volumes

sudo lvscan


## Show more details on logical volumes

sudo lvdisplay 
sudo lvdisplay [path]
sudo lvdisplay /dev/centos/home


## Create physical volume on a partition

sudo pvcreate [path]

sudo pvcreate /dev/sda2


## Increase logical volume

sudo lvextend -L[number][size_type] [path]

sudo lvextend -10G /dev/backup/databk


## Decrease logical volume

sudo lvreduce -L[number][size_type] [path]

sudo lvreduce -L1G /dev/backup/logbk


## Access point to information stored on a local or remote device

mount points


## Command to load filesystem to a specified directory

mount


## Source code that is compiled to executable program, or assembled to be readable by the computer

binaries


## Command to unload or unmount a mounted filesystem

umount


## Three requirements for mounting a filesystem

A device (i.e. /dev/sda1)
A directory (mount point)
Superuser privileges


## File to add entry to as final step to complete mounting a filesystem to have the filesystem mount at boot time

Add entry to /etc/fstab


## Generic format of /etc/stab file

<device or UUID><mount point><file system type><mount option><dump option><fsck option>


## Unable to unmount

A user must be using the directory that is being unmounted


## Command to check if there are errors in fstab file

sudo mount -a


## File that shows status of currently mounted file systems more accurately and up-to-date

/proc/mounts

Note: not a real file. Part of virtual Linux file system


## File that shows status of currently mounted file systems less accurately

/etc/mtab


## File that contains information about each partition attached to the system

/proc/partitions

Note: not a real file. A virtual file system


## Columns that for the file that contains information about each partition attached to the system

major
minor
blocks
name


## Display block storage devices that are currently available to the system

lsblk


## Display in flat format for block storage devices that are currently available to the system

blkid


## Repair file systems

fsck -r [device/file system name]


## Enlarge or shrink etc2/etc3/etc4 file system to a device

resize2fs


## Configure tunable parameters for etc2/etc3/etc3 file system

tune2fs


## Contains metadata about file system including the size, type, and status

Superblock


## Used to dump information on etc2/etc3/etc4 file system

dump2fs


## Steps to increase size of a file system data/datastore1 to 200 GB

sudo umount /dev/data/datastore1
sudo e2fsck -f /dev/data/datastore1
sudo lvextend -L200G /dev/data/datastore1
sudo resizefs /dev/data/datastore1
sudo mount /dev/data/datastore1 /data1


## Avoid file corruption with fsck command

unmount file system first


## Utility to grow an XFS file system

xfs_growfs


## Five types of files

Directories (d) - container of other files
Special files  - files stored in /dev directory. can be block special files (b) or character special files (c)
Links (l) - makes file accessible in multiple parts of the system's file tree
Domain sockets (s) - provides inter-process networking that is protected by file system access control
Named pipes (p) - enable processes to communicate with each other without using network sockets


## Command to determine type of file

file [option] [file_name]


## Location on the system that you are currently accessing at any point in time

Current working directory (CWD)


## One level above current directory

Parent directory


## Location in a file system

path


## Absolute vs relative paths

Absolute path is path irrespective of the current working directory and starts with a foward slash (/)

Relative path is path relative to the current working directory and can coontain period (.) and double period (..)


## Traverse the directory structure using absolute or relative paths to change working directories

cd


## Print working directory

pwd


## Name directories in the root directory of the file system

/etc
/home
/opt
/mnt
/media
/boot
/dev
/proc
/tmp
/var
/lib
/bin
/sbin
/sys
/root
/usr


## Standard directory with system logs

/var/log/


## /root vs /

/root - directory of root user's home directory
/ - is the root of the file system


## Return to the home directory

cd ~


## Return to the root of the file system

cd /


## File object type of bin

link


## Directory that contains essential command-line utilities

/bin


## Installed third-party package

/opt


## Missing devices

verify physical connectivity of devices
check connections and power cables
detected devices will create device file in /dev


## Missing volumes

verify the volumes created on the storage devices are recognized
use utilities parted and fdisk to verify existence of partitions

also, /proc/partitions


## Missing mount points

There might be mistakes in /etc/fstab
manually attach storage volumes on the file system


## Degraded storage

storage drive in RAID array has failed
rebuild the array with a replacement storage drive


## Performance issues

Servers might require more efficient I/O system and more storage capacity


## Resource exhaustion

Due to very busy server
Resolve by rebooting server
ulimit command can be used to adjust the available number of file descriptors


## Storage integrity/bad blocks

Traditional magnetic hard disk drives may degrade over time
Consider replacing hard disk drive


## Set a limit for the maximum number of open file descriptors

ulimit -n [number]
ulimit -n 512


## Display current limits for open file descriptors

ulimit -a


## Storage space tracking for viewing device's free space, file system, total size, space used, percentage value of space used, and mount point

df


## Storage space tracking for viewing how device is used, including size of directory trees and files within it

du


## Process by which the operating system determines the input and output operators as they pertain to block storage devices

I/O scheduling


## Scheduler types

deadline - performs I/O operations using standard pending request queue, read FIFO queue, write FIFO queue

cfq - Complete Fair Queeing. each process has its own queue and own time slice

noop - simplest scheduler and does not sort I/O requests, but merely merges them


## Setting the scheduler

echo noop >  /sys/block/sda/queue/scheduler


## Utility that generates report on CPU and device usage

iostat


## Generate report of device I/O latency in real-time

ioping


## Storage space that is allotted to a user for file storage on a computer

Storage quota


## Effective allotment and monitoring of quotas for all users




## Stores essential command-line utilities and binaries

/bin


## Stores files require to boot Linux OS

/boot


## Stores hardware and software device drivers

/dev


## Stores basic configuration files

/etc


## Stores users' home directories, includes personal files

/home


## Stores shared libraries required by the kernel, command-line utilities, and binaries

/lib


## Stores mount points for removable media such as CD-ROMs and floppy disks

/media


## Mount point for temporarily mounting file systems

/mnt


## Stores optional files of large software packages

/opt


## Virtual file system (VFS) that represents continually updated kernel information to the user in a file format

/proc


## Home directory of root user

/root


## Stories binaries that are used for completing the booting process and ones that are used by the root user

/sbin


## VFS that stores information about devices

/sys


## Stores temporary files and may be lost on system shutdown

/tmp


## Read-only directory that stores small programs and files accessible to all users

/usr


## Stores variable files, or files that constantly change as system runs. I.e. log files, printer spools, networking services' configuration files

/var


## Directory in /usr that includes executable programs that can be executed by all users

/usr/bin


## Directory in /usr that includes custom build applications

/usr/local


## Directory in /usr that includes object libraries and internal binaries that are needed by executable programs

/usr/lib


## Directory in /usr that includes object libraries and internal binaries that are needed by executable programs for 64-bit systems

/usr/lib64


## Directory in /usr that includes read-only architecture independent files

/usr/share


## Common editors

vi
Vim - default in most Linux distros
Emacs - powerful and popular editor used in Linux and Unix
gVim - graphical editor of vim
gedit - GUI-based text editor in GNOME desktops
GNU nano - small, user-friendly editor


## Modes in Vim

Command Mode
- "i" to go to Insert Mode
- ":" to go to Execute Mode
Insert Mode 
- Esc to go to Command Mode
Execute Mode
- Esc to go to Command Mode


## Execute Mode Commands

:w [file_name] - save a file for the first time
:q - quits
:q! - quits, ignore changes
:qa - quit multiple files
:wq - save and quit
:e! - revert to last saved without quitting
:![any Linux command] - use command and use in Vim interface
:help - access help documentation


## Motions in Vim

h - move left 1 char
j - move down 1 line
k - move up 1 line
l - move right 1 char
^ - move to beginning of the line
$ - move to end of the line
w - move to next word
b - move to previous word
e - move to end of current word or end of next word if at the end of current word
Shift+L - move cursor to the bottom of the screen
Shift+H - move cursor to the first line of the screen
[line_number] Shift+G - move cursor to specified line number
gg - move cursor to first line of file
Shift+G - move cursor to last line of file


## Editing Operators in Vim

x - delete character selected by cursor
d - delete text
dd - delete current line
p - paset text below cursor
P - paste text above cursor
/[text_string] - search through the doc with a specific text
?[text_string] - search backward through the doc with specific text
y - copy text
yy - copy the line directly above the cursor
c[range_of_lines]c - begin change in the specified line
u - undo latest change
U - undo all changes in the current line
ZZ - write the file only if changes were made, then quit.


## nano shorcuts

Ctrl+G      open help screen
Ctrl+X      exit 
Ctrl+O      save currently opened file
Crtl+J      justify current paragrah
Ctrl+R      insert another file into current one
Ctrl+W      search the file
Ctrl+K      cut selected line
Ctrl+U      paste line that was cut
Ctrl+C      display cursor position


## Search for files

locate - search in files names and paths stored in "mlocate" database
updatedb  - use for building database of files based on /etc/updatedb.conf
find - search in a specific location for files and directories
which - search the complete path of a specified command
whereis - command to display various details associated with a command

"find" search produces updated results while "locate" might produce outdated results


## Search command that shows absolute path and starts searching from the root directory

find [directory] -name '[directory/file_name]' -print

Example:
find / -name 'file*' -print

To restrict with a specified type use "-type":
b   block special
c   character special
d   directory
f   regular file
l   symbolic link
p   FIFO
s   socket

Example:
sudo find / -type d -name 'log'
sudo find / -type f -name 'messages'

To filter with size use "-size [operator][number][character]"
Example:
-size +100k

Filter by updated period with '-mmin [number]'
Example:
-mmin 30

"-or" operator:
search for size empty or above 100 KB and updated in the last 30 minutes
Example: -size 0 -or +100k -mmin -30


## Concatenate command

cat

-n   output with line numbers
-b   number of lines excluding blank lines
-s   suppress output of repeated empty lines
-v   display non-printing characters, other than tabs, new lines, and form feeds
-e   print $ on end of each line
-t   print tabs ^I and form feeds as ^L


## Display first 10 lines

head


## Display last 10 lines

tail


## Arranges lines in a file




## Extracts specified lines of text from a file

cut


## Program to modify text files according to various parameters

sed


## Display contents and navigate up and down

less


## Display contents but not able to navigate up and down

more


## Changes time of access or modification time of a file to the current time

touch


## Remove files and directories

rm


## Remove one file at a time only

unlink


## ls colors

default: normal/text file
blue: directory
sky blue: symbolic link
green: executable file
yellow with black background: device
pink: image file
red: archive file
red with black background: broken link


## Create and remove directories

mkdir
rmdir


## Display text on the terminal

echo


## Display text on the terminal with various format characters

printf


## Translate string of characters

tr


## Extracts specified lines of text from a file

cut


## Merge lines from text files horizontally

paste


## Display difference between files

diff


## Search contents of a file for a particular string of text

grep


## Pattern matching on the contents of the files

grep


## Pattern matching on files

awk


## Program to modify text files according to various parameters

sed


## Create a link to a file

ln


## Sequence of one or more lines of text

Text streams


## Text stream that acts as source for command input

Standard input
stdin


## Text stream that acts as the destination for command output

Standard output
stdout


## Text stream that used as the destination for error messages

Standard error
stderr


## Process of accepting input data from a source other than the keyboard

Redirection


## Redirection Operators

>      redirect the standard output
>>    append the standard output to end of the destination file
2>    redirect the standard error message to a file
2>>  append the standard error message to the end of the destination file
&>    redirect both the standard output and error message to a file
<      read input from the file rather than from the keyboard or mouse
<<string      provide input data form current source, stopping when a line containing the provided string occurs. This is called a here document


## Process of combining standard I/O streams of command

piping


## Reads from standard input and executes a command for each argument provided

xargs


## Reads the standard input and sends the output to the CLI and copies the output to specified file

tee


## Discards all data written to it

/dev/null

also known as null device


## Elements managed by kernel

Memory
File System Access
Processes
Resource Allocation
Devices


## Two memory spaces for running software divided by the kernel

kernel space
user space


## Usefulness of splitting two memory regions

More stability and security


## Kernel types

monolithic kernel
device driver


## All system modules, such as device drivers or file systems, run in kernel space

monolithic kernel


## Kernel itself runs the minimum amount of resources necessary to run a fully functional operating system




## Prints the kernel name

uname 
-r    show version number
-i     show hardware platform
-a   print all


## kernel layer that handles system calls sent from user applications ot the kernel

System Call Interface (SCI)


## Kernel layer that handless different processes

Process management


## Kernel layer that manages computer memory

Memory management


## Kernel layer that manages the file system

File system management


## Kernel layer that manages devices

Device management


## System-level objects that extends the functionality of the kernel

kernel module


## Contains shared libraries and binaries for general programs and software packages

/usr/lib/


## Location of kernel module subdirectories

/usr/lib/modules/[kernel_version]/kernel


## Kernel module subdirectory that contains modules for architecture-specific support

arch

/usr/lib/modules/[kernel_version]/kernel/arch


## Kernel module subdirectory that contains modules for encryption and other cryptographic functions

crypto

/usr/lib/modules/[kernel_version]/kernel/crypto


## Kernel module subdirectory that contains modules for various types of hardware

drivers

/usr/lib/modules/[kernel_version]/kernel/drivers


## Kernel module subdirectory that contains modules for various types of file systems

fs

/usr/lib/modules/[kernel_version]/kernel/fs


## Kernel module subdirectory that contains modules for networking components such as firewalls and protocols

net

/usr/lib/modules/[kernel_version]/kernel/net


## Display current loaded kernel modules

lsmod


## Display information about a particular module

modinfo


## Install a module into a currently running kernel

insmod


## Remove a module from a currently running kernel

rmmod


## Add or remove modules from a kernel

modprobe - preferred over insmod and rmmod because it is capable of loading all the dependent modules before inserting the specified module

-f   force module to inserted or removed
-n  conduct dry run to output results without actually executing
-s   print errors to system log (syslog) rather than stderr
-v   enable verbose mode


## Updates modules.dep file (database of depencies) to identify linking of modules and searches contents of /lib/modules/[kernel_version]/ for each module

depmod


## Provide a way for modules to call upon functions or other programming objects of other modules

Symbols


## Configuration file that contains settings that apply persistently to all modules loaded on the system

/etc/modprobe.conf


## Directory that lists the parameters that you can configure on your system

/proc/sys/

directory divided into several categories:
crypto    encryption and other cryptographic services
debug    debugging the kernel
dev         specific hardware devices
fs            file system data
kernel    miscellaneous kernel functionality
net         networking functionality
user       user space limitations
vm         virtual memory management


## View or set kernel parameters at runtime

sysctl

options:
-a    display all parameters
-w [paramter] = [value]      set a parameter value
-p [file_name]                      load sysctl settings from the specified file
                                              or /etc/sysctl.conf if no file name is provided
-e                                          ignore errors about unknown keys
-r [pattern]                           apply a command to parameters matching a 
                                               given pattern, using extended regex


## File enables configuration changes to a running linux kernel

/etc/sysctl.conf


## Process of starting or restarting a computer loading an operating system for the user to access

Booting


## Small program stored in ROM that loads the kernel from the storage device, and then starts the operating system

Boot loader


## Able to protect the boot process with a password to prevent unauthorized booting of the system

Boot loader


## Components of boot loader

Boot sector program
- first component of boot loader. loaded by a boot environment on startup and has a fixed size of 512 bytes. Main function is to load second stage boot loader but can load another sector or kernel as well.

Second stage boot loader
- loads the operating system and contains a kernel loader

Boot loader installer
- controls the installation of drive sectors and can be run only when booting from a drive. coordinates activities of the boot sector and the boot loader.


## Standard firmware interfaces and is stored on a computer motherboard's ROM chip

Basic Input/Output System (BIOS)


## Newer firmware that replaced BIOS. Runs faster than BIOS and can operator with greater amount of memory

Unified Extensible Firmware Interface (UEFI)


## Security feature both BIOS and UEFI have

Password protection


## Other boot options

Boot from ISO
PXE (Preboot Execution Environment)
Boot from HTTP/FTP
Boot from NFS


## Smallest unit of storage read from or written to a drive

Sector
- stores 512 bytes of data by default
- can be altered when formatting the  drive


## First physical sector on a storage drive and a type of partition structure

Master Boot Record (MBR)


## Successor to MBR. Employs more modern design and is part of the UEFI

GUID Partition Table (GPT)
- advantage of storing boot data in multiple locations on a drive


## Enables users and applications to read from and write to a block storage device directly, without using system cache

Raw partition


## Root file system that is temporarily loaded into memory upon system boot

initial ramdisk (initrd)


## Archive file containing all the essential files that are required for booting the operating system

initrd image


## Used to create the initrd image for preloading the kernel modules

mkinitrd

--preload=[module_name]       
       load a module in the initrd image before the loading
--with=[module_name]
       load a module in the initrd image after the loading of other modules
-f    
       overwrite an existing initrd image file
--nocompress
       disable the compression of the initrd image


## Directory that contains files that are used to facilitate the Linux boot process

/boot/


## /boot/ directory containing configuration files for GRUB boot loader

/boot/grub/


## /boot/ directory containing configuration files for GRUB 2 boot loader

/boot/grub2/


## /boot/ directory for EFI system partition (ESP)

/boot/efi/


## Image file alternative to initrd that uses different methods to do the same thing

/boot/initramfs-[kernel_version].img


## Compressed executable file that contains the Linux kernel itself

/boot/vmlinuz-[kernel_version]


## Command to generate initramfs image, similar to how mkinitrd is used to generate an initrd image

dracut


## Process that is repeated each time your computer is started by loading the operating system from a storage device

Boot process


## High-level components involved in the Linux boot process

|   BIOS/UEFI
|   MBR/GPT
|   GRUB 2
|   initrd / Kernel
v   systemd process


## 12 steps of the boot process

1. Processor checks for BIOS/UEFI
2. BIOS/UEFI checks for bootable media from internal storage devices or peripherals
3. BIOS/UEFI loads the boot loader from the MBR/GPT
4. User is prompted by GRUB 2 to select operating system to boot
5. Boot loader determines the kernel and locates the corresponding kernel binary
6. Kernel configures the available hardware drivers
7. Kernel mounts the main root partition and releases unused memory
8. systemd program searches for the default.target file, which contains details about the services to be started. It mounts the file system based on the /etc/fstab file and begins the process of starting services. Most systems will use multi-user.target or graphical.target
9. If graphical mode is selected, then a display manager like XDM or KDM is started on the login window
10. The user enters a user name and password to log in to the system
11. System authenticates the user. If user is valid, then various profile files are executed
12. Shell is started and the system is ready for the user to work on


## Mechanism by which the system detects there has been a fatal error and responds to it

Kernel panic


## Common causes of kernel panic

- kernel itself is corrupted or not configure properly
- systemd program is not executed during boot, leaving system usable
- kernel cannot find or otherwise cannot mount the main root file system
- malfunctioning or incompatible hardware is loaded into kernel on boot


## Process used for systems with /boot/efi/ directory

UEFI boot process


## Boot options that enables users to mount a file share as the root file system

Boot from NFS


## Boot loader developed by the GNU Project

GNU GRand Unified Bootloader (GNU GRUB)


## Offers administrators more control over the boot process, boot devices, and boot behavior

GRUB 2
- support for non-x86 architecture platforms
- support for live booting
- partition UUIDs
- support for dynamically loading modules that extend GRUB's functionality
- support for partition UUIDs
- support for dynamically loading modules that extend GRUB's functionality
- ability to configure the boot loader through scripts
- rescue mode, which attempts to fix boot issues like corrupted or missing configurations
- support for custom graphical boot menus and themes


## Command to install GRUB 2 boot loader on a storage device

grub2-install


## grub2-install command options

--modules [module_names]
preloads the specified kernel modules with the GRUB 2 boot loader

--install-modules [module_names]
install only the specified modules and their dependencies

--directory [directory_name]
install files from the specified directory

--target [target_platform]
specify target platform to install GRUB 2

--boot-directory [directory_name]
specify boot directory to install GRUB 2

--force
install GRUB 2 regardless of detected issues


## Main configuration file for the GRUB 2 boot loader

grub.cfg


## Location of the main configuration file for the GRUB 2 boot loader

/boot/efi/EFI/[distro]/

/boot/efi/EFI/centos/grub.cfg


## Directory that contains scripts that are used to build the main grub.cfg file

/etc/grub.d/


## File that enables customization of the menu presented to the user during the boot process

/etc/grub.d/40_custom


## File contains GRUB 2 display settings that are read by /etc/grub.d/ scripts and built into grub.cfg

/etc/default/grub


## Generates a password hash to protect the boot menu

grub2-mkpasswd-pbkdf2


## Generates a new grub.cfg configuration file

grub2-mkconfig


## Process to configure GRUB 2 from boot menu

|  BIOS/UEFI
|  MBR/GPT
V GRUB 2    ---> Press Esc ---> Press e ---> edit config


## Process of adapting system components for use

localization


## Container for all of the regional time zones that you can configure the system to use

/usr/share/zoneinfo/


## File to view time zone in Debian-based distros

/etc/timezone


## Print date in a specified format

date


## Formatting options in date command

%A   full weekday name
%B   full month name
%F   YYYY-MM-DD format
%H   24-hour format
%h    12-hour format
%j      day of the year
%S     seconds
%V     week of the year
%x     date represented based on locale
%X    time represented based on locale
%Y    year


## Set system date and time information

timedatectl


## Subcommands for system date and time information command

status
- show current date and time information
set-time
- set system's time to the time provided
set-timezone
- set system's time zone to the time zone provided
list-timezones 
- list all available time zones in the format specified by /usr/share/zoneinfo structure
set-ntp [0|1]
- enable synchronization with a Network Time Protocol (NTP) server


## Options of system date and time information command

-H [remote_host]
Execute operation on the remote host specified by IP address or hostname

--no-ask-password
Prevent user from being asked to authenticate when performing a privileged task

--adjust-system-clock
Synchronize local (system) clock based on the hardware clock when setting the hardware clock

-M [local_container]
Execute the operation on a local container


## View and set the hardware clock

hwclock


## Options of view and set the hardware clock command

--set
Set hardware clock to the provided date and time

-u
Set the hardware clock to UTC

-s
Set the system time from the hardware clock

--adjust
Add or subtract from the hardware clock to account for systematic drift


## View and configure system locale and keyboard layout settings

localectl


## Subcommands for view and configure system local and keyboard layout settings command

status
- show current locale and keyboard layout

set-locale
- set system locale to the locale provided

list-locales
- list all available locales on the system

set-keymap
- set keyboard layout to provided layout

list-keymaps
- list all available keyboard layouts on the system


## Options of view and configure system local and keyboard layout settings command

-H [remote_host]
Execute operation on the remote host specified by IP address or hostname

--no-ask-password
Prevent user from being asked to authenticate when performing a privileged task

--no-pager
Prevent the output from being piped into a paging utility

--no-convert
Prevent a keymap change for the console from also being applied to the X display server, and vice versa


## Process of converting text into bytes

Character encoding


## Process of converting bytes into text

Character decoding


## Default encoding for many systems

UTF-8 Unicode character set


## Enables users to interact with a system or application through visual design elements rather than pure text as in a command-line interface (CLI)

Graphical User Interface (GUI)


## Component of GUI that constructs and manages the windowing system and other visual elements that can be drawn on the screen

Display server


## Platform-independent display server and windowing systems developed by MIT in 1984

X Window System
also known as X11 or X


## Free and open source reference implementation of the X Window System for Linux and other Unix-like operating systems

X.Org Server


## Both a display server and its reference implementation in Unix-like operating systems that is meant to improve upon and replace the X Window System

Wayland


## Client to a display server that tells the server how to draw graphical elements on the screen

Desktop environment
also known as a window manager


## Default desktop environment in most Linux distributions

GNOME


## Second-most common desktop environment and is included in distributions like RHEL and CentOS

KDE Plasma


## Desktop environment that is a fork of GNOME 3 and default for Linux MINT distro

Cinnamon


## Desktop environment that is a fork of GNOME in respond to changes in GNOME 3

MATE


## Concept on which a client connects to a remote system over a network, and is able to sign in to and use the desktop environment of that system

Remote desktop


## Process of forwarding input and output through a serial connection

Console redirection


## Remote access protocol that encrypts transmissions over a network

Secure Shell (SSH)
SSH Port Forwarding


## Network-aware and can enable clients to access GUI elements over a network

X Forwarding


## Options for accommodating people with disabilities

Accessibility options


## Primary architectural difference between X and Wayland display servers

In X, the compositor is separate from the display server
In Wayland, the compositor and display server are one

Wayland compositor can interact directly with clients minimizing latency


## Location where a user can switch GUI

Graphical login screen


## Software that responds to requests from other programs to provide some sort of specialized functionality

Linux service


## Form in which most services are implemented and that form lies dormant until an event triggers them into activity

daemons


## Lifecycle process of starting services, modifying their running state, and stopping them

Service management


## Process that begins when the kernel first loads

System initialization


## Daemon for system initialization

init


## Software suite that provides an init method for initializing a system

systemd


## Configuration files that systemd uses to determine how it will handle units

Unit files


## Method of grouping unit configuration files together in systemd

targets


## Enables control of systemd init daemon

systemctl


## Subcommand of the command that enables control of systemd init daemon

status [service]
- retrieve current status
enable [service]
- enable service on boot
disable [service]
- disable service to start on boot
start [service]
- start service immediately
stop [service]
- stop service immediately
restart [service]
- restart service immediately
set-default [target]
- set default target for the system to use on boot
isolate [target]
- force system to immediately change to the provided target
mask [unit_file]
- prevent provided unit file from being enabled or activated
daemond-reload
- reload the systemd init daemon


## Options of the command that enables control of systemd init daemon

-t [unit_file_type]
specify unit file types to perform the operation on
-a
list all unit files or properties, regardless of state
--no-reload
prevent reloading of configuration changes when enabling or disabling a service
--no-ask-password
prevent users from being asked to authenticate when performing privileged operations
--runtime
make changes temporary so that they will not be present after a reboot
-H [remote_host]
execute operation on remote host specified by IP address or hostname
--no-pager
prevent the output from being piped into a paging utility


## Command used to control services in most cases

hostnamectl


## An older init method that has been largely replaced by systemd

SysVinit


## Major difference between it and SysVinit

It has runlevels


## Runlevels vs Targets

SysVinit Runlevel    -  systemd Target - description
0 - poweroff.target - shut down system
1 - rescue.target - start single-user mode
2 - multi-user.target - starts multi-user, without remote networking. loads CLI
3 - multi-user.target - starts multi-user, with remote networking. loads CLI
4 - multi-user.target - not used
5 - graphical.target - start multi-user mode with networking and GUI capabilities. loads desktop environment
6-  reboot.target - reboots the system


## Boots the operating system into an environment where the superuser must log in

single-user mode


## Command enables you to switch the current runlevel of the system

telinit


## Stores details of various processes related to system initialization on a SysVinit system

/etc/inittab


## Directory that stores scripts for services

/etc/init.d/


## File executed at the end of the init boot process

/etc/rc.local


## Command used to control services in each runlevel

chkconfig


## Subcommand/Option of command used to control services in each runlevel

[service] on
enable service to be started on boot

[service] off
disable service to be started on boot

[service] reset
reset status of service

--level [runlevel]
specify runlevel in which to enable or disable a service


## Command to control SysVinit services

service


## Subcommand of command to control SysVinit services

[service] status
- print the current state of a service

[service] start
- start a service immediately

[service] stop
- stop a service immediately

[service] restart
- restart a service immediately

[service] reload
- re-read a service's configuration files while the service remains running


## Common process issues

process hangs....
- causing instability
- consuming resources that should be allocated to other processes
- causing general system sluggishness and instability

process terminates before it can perform its intended tasks
process fails to terminate when it is no longer needed
process should be allocated most CPU resources but other processes are instead
process is causing the system to take a long time to boot
process has a file open, preventing you from modifying that file
process has spawned multiple child processes that are hard to keep track of
process is unidentifiable or potentially malicious


## Five process states

Running - process currently executing in user space or kernel

Interruptible sleep - process relinquishes access to the CPU and waits to be reactivated by the scheduler

Uninterruptible sleep - process will only wake when the resource it's waiting for is made available to it

Zombie - state indicates a process was terminated but not yet released by its parent process

Stopped - stopped by debugger or through a kill signal


## Identified assigned to each process

Process id


## Command to display PID of processes

pgrep


## Record that summarizes the current running processes on a system

process table


## Command to show record that summarizes the current running processes on a system

ps


## Options of command to show record that summarizes the current running processes on a system

a      list all user-triggered processes
-e    list all processes
-l      list processes using a long list format
u      list processes along with the user name and start time
r       exclude processes that are not running currently
x       include processes without a terminal
T       exclude processes that were started by any terminal other than the current one
-U [user_name] display the processes based on the specified user
-p [PID]  display the process associated with the specified PID
-C [command] display all processes by command name
--tty [terminal_number]   display all processes running on the specified terminal


## Lists all processes running on the Linux system. Enables to prioritize, sort, or terminate processes interactively

top

Enter - refresh
Shift+N - sort by decreasing order of PID
M - sort by memory usage
P - sort by CPU usage
u - display processes belonging to the user specified at the prompt
k - terminate the process for specified PID
r - renice the process for specified PID
q - exit the process list


## Command used to retrieve performance statistics for boot operations

systemd-analyze


## Command prints a list of all files that are currently opened to all active processes

losf


## Enables you to run a command with a different nice value than the default

nice


## Command that enables you to alter a scheduling priority of an already running process

renice


## Command that prevents a process from ending when the user logs off

nohup

known as "no hangup"


## Differents commands to terminate processes

kill
- kills a specified PID (one or more processes)
pkill
- uses a pattern matching
killall
- all processes matching name specified and extra functionality to match exactly


## Determines how to kill the process

kill signals


## Common kill signals

Signal - Value - Use
SIGNIT - 2
SIGKILL -9
SIGTERM - 15
SIGSTOP 17,19,23
SIGSTP   18,20,24


## File contains information about the system's processor

/proc/cpuinfo


## Command displays time when a system started running

uptime


## Command displays system usage reports based on data collected from system activity

sar


## Common memory issues

not enough total memory or free memory
processes unable to access memory
processes accessing too much memory
system cannot quickly access files from a cache or buffer
system is killing critical processes when memory is low
swap partitions taking up too much or not enough storage space


## File containing great deal of information about system's memory usage

/proc/meminfo


## Parses the file containing great deal of information about system's memory usage

free


## Displays various statistics about virtual memory

vmstat


## Feature of the Linux kernel that determines what processes to kill when the system is extremely low on memory

out-of-memory (OOM) killer


## Alleviate memory-related issues, especially when system and applications request more memory than the system has

swap space configuration


## Three swap types

Device swap
- configured when you partitioned the storage device
File system swap
- Configured when you install Linux
Pseudo-swap
- Enables large applications to run on computers with limited RAM


## Created for storing data thas is to be transferred from a system's memory to a storage device

Swap file


## Area of virtual memory on a storage device to complement the physical RAM




## Command to create swap space on a storage partition

mkswap

-c
verify device is free from bad sectors before mounting the swap space

-p
set page size to be used. Page is a chunk of memory that is copied to the storage device during swap process

-L [label]
activate the swap space using labels applied to partitions or file systems


## Command to activate swap partition on a storage device

swapon

swapon -e
skip devices that do not exist

swapon -a
activate all of the swap space

swapoff -a
deactivate all of the swap space


## CPU and memory issues

identify key information about CPI and logic cores using file /proc/cpuinfo
identify CPU load averages with command uptime
identify heavy load on CPU with command sar
identify key memory information using file /proc/meminfo
easily analyze memory usage information using command free
retrieve more info on CPU and memory usage with command vmstat
tweak OOM killer to spare or sacrifice specific processes when memory is low
create more swap space if adding physical memory is not possible


## Lightweight computing device that connects to a more powerful server

Thin client


## Peripheral interface technology that is the standard for connecting devices

Universal Serial Bus (BUS)


## Transmit and receive signals over the air

Wireless devices
- Wi-Fi
- Bluetooth
- Near Field Communication (NFC)


## Device that provides interface for hosts to exchange data over network

Network adapters
also known as Network Interface Card (NIC)


## Refers to pins on a circuit board that have no designated purpose

General-purpose input/output (GPIO)


## Computer bus interface standard for attaching storage devices to traditional computers

Serial AT Attachment (SATA)


## Computer bus interface for connecting peripheral devices to traditional computers

Small Computer System Interface (SCSI)


## Hardware component that connects a host system to a storage device

Host Bus Adapter (HBA)


## Connection interface standard that is primarily used as an expansion bus for attaching peripheral devices

Peripheral Component Interconnect (PCI)


## Device file locations

/proc/ - contains various system info reported by the kernel
/sys/ - similar to /proc/ with more focus on hierarchical view
/dev/ - contains device driver files that enable the system and users to access the devices themselves
/etc/ - includes configuration files for many components


## Can be physically added or removed from the system without requiring a reboot in order to use

Hotpluggable device


## Manages automatic detection and configuration of hardware devices

Device manager udev


## Directory used to configure rules for how udev functions

/etc/udev/rules.d/


## Directory with udev rules but generated by the system

/usr/lib/udev/rules.d/


## Location of udev rules files

/run/udev/rules.d/


## Command used to manage udev

udevadm


## Subcommands of command used to manage udev

info - retrieve device info
control - modify running state of udev
trigger - execute rules that apply to any device that is currently plugged in
monitor - watch for events sent by the kernel or by a udev rule
test - simulate udev event running for a device


## Print management system for Linux

CUPS


## Submit files for printing

lpr


## Subcommands of command for submitting files printing

-E 
Force encryption when connecting to server
-P [destination]
Send print job to the destination printer 
-# [copies]
Set number of copies from 1 to 100
-T [name]
Set the job name
-l
Specify print file that is already formatted
-o [option]
Set a job option
-p
Print specified files with shaded headers
-r 
Specify print files to be deleted after printing


## Command to display various information about a system's hardware as reported by the kernel

lsdev


## Three files compiled for the command to display various information about a system's hardware as reported by the kernel

/proc/interrupts
- lists each logical CPU core and associated interrupt requests(IRQ)
/proc/ioports
- lists I/O ports and hardware devices mapped to them
/proc/dma
- lists all Industry Standard Architecture (ISA) director memory access (DMA) channels on the system


## Command to display information about devices that are connected to the system's USB buses

lsusb


## Command to display information about devices connected to the system's PCI buses

lspci


## Common hardware issues

issues on...
keyboard mapping
communications port
printer
memory
video
storage adapter


## Command to manage RAID arrays

mdadm


## Lists each detected hardware component on the system

lshw


## Dumps the system's Desktop Management Interface (DMI) table

dmidecode


## Utility for reportings problems detected during system runtime and usually used on Fedora and RHEL distros

Automatic Bug Reporting Tool (ABRT)


## OSI Model

7 Application - applications, end-users
6 Presentation - formats data use
5 Session - establish, maintain, terminates connection
4 Transport - enables reliable transmission of information
3 Network - enables logical addressing (IP address)
2 Data Link - enables physical addressing (MAC)
1 Physical - enables physical network connectivity


## Layers of TCP/IP suite




## Network identities

MAC address - each NIC has unique ID
IP address - each NIC may be assigned na IP
Hostname - human-readable name


## Network devices and components

Switch - device acts as concentrator, centralizing all network connections
Router - device acts as control point for communications between
Media - twisted pair Ethernet cable


## Frame vs packet

network layer (layer 3) - packet
data link layer (layer 2) - frame


## DNS and DHCP

DNS - name resolution
DHCP - provides dynamic configuration


## Common port numbers

22: Secure Shell (SSH)
25: Simple Mail Transfer Protocol (SMTP)
80: Hypertext Transfer Protocol (HTTP)
110: Post Office Protocol version 3 (POP3)
443: Hypertext Transfer Protocol Secure (HTTPS)


## Enables user to view current IP addressing configuration

ifconfig


## Replaces ifconfig in many distributions

ip


## Directory containing network device configuration files

/etc/sysconfig/network-scripts/


## Directory where network configuration files are located in Debian-derived distros

/etc/network/


## File to configure whether networking should be enabled at boot, as well as hostname info, gateway info, etc.

/etc/sysconfig/network


## File enables configuration of DHCP client settings

/etc/dhcp/dhclient.conf


## File where name resolution could be managed

/etc/hosts


## File including several configuration options. Option related to name resolution defines the order in which name resolution methods will be used by the system

/etc/nsswitch.conf


## Cloud models

Software as a Service (SaaS)
Platform as a Service (PaaS)
Infrastructure as a Service (IaaS)


## Templates for different deployments

Open Virtualization Format (OVF)
JavaScript Object Notation (JSON)
YAML Ain't Markup Language (YAML)
Container images


## Store data in an unstructured manner

Blob


## Data itself is written to the storage device in small chunks

Blocks


## Interactive shell to KVM virtual machines

virsh


## Sending test packets between two systems

ping


## Commands to report network path between source and destination computers

traceroute 
tracepath


## Command to gather information about TCP connections to the system

netstat


## Command that is an information gathering utility that provides simpler output and syntax than netstat

ss


## Powerful tool for gathering information and testing name resolution. Installed in most Linux distribution

dig


## Tool for gathering name resolution information and testing information. Available in most Linux distrbutions and Microsoft Windows

nslookup


## Another simple tool for gathering information and testing resolution and is installed on most Linux installation

host


## Tool for exploring a network environment

nmap


## Common packet sniffer and network analyzer

Wireshark


## Command that is most popular packet sniffer and installed in most Linux distribution

tcpdump


## Command to test connectivity and send data across network connections

netcat


## Display bandwidth usage information for the system

iftop


## Command to test maximum throughput an interface will support

iperf


## Potential amount of data that may move through a network

Bandwidth


## Amount of data that actually moves through a network connection in the given amount of time

throughput


## Utility that is a combination of ping and traceroute

mtr


## Command to discover information about known MAC addressses

arp


## Command provides information on Internet DNS registrations for organizations

whois


## Programs that install, update, inventory, and uninstall packaged software

Package managers


## Package manager for RHEL, CentOS, Fedora, Scientific Linux

Red Hat Package Manager (RPM)


## Package manager for Debian, Ubuntu, Linux Mint, Kali Linux

dpkg


## Newer and advanced package manager for RHEL derivatives

Yellowdog Updater, Modified (YUM)


## Command to manager RPM packages

rpm


## Options of command to manager RPM packages

-i [package_name]
install specified software
-e [package_name]
uninstall the package
-v
enable verbose mode to provide more details
-h
print hash marks to indicate progress bar
-V [package_name]
verify software components of the package exist


## Commands for rpm querying

rpm -qa
list all installed software

rpm -qi [package_name]
list information about a particular package

rpm -qc [package_name]
list configuration files for a particular package


## Subcommands for yum

install [package_name]
- install from any configured repository
localinstall [package_name]
- install from the local repository
remove [package_name]
- uninstall package
update [package_name]
- update package, if none provided, updates all packages
info [package_name]
- report information on a package
provides [file_name]
- report what package provides
repolist
- see all available repositories
makecache
- locally cache information bout available repositories
clean all
- clear out-of-date cache information


## Options of dpkg command (Debian-based distribution)

-i [package_name]
install package
-r [package_name] 
remove package
-l [package_name] 
list information of package, if none provided, all packages are listed
-s [package_name]
report whether the package is installed


## Subcommands for apt

install [package_name]
remove [package_name]
- uninstall package and leave its configuration files
purge [package_name]
- uninstall package and remove its configuration files
show [package_name]
- report information about the package
version [package_name]
- display version information about the package
update
- update APT database of available packages
upgrade [package_name]
- upgrade the package, if none provided, all packages are updated


## Commands used for downloading

wget
curl


## Tape archiver

tar


## Compression utility

gzip


## Options for tar command

-c
create tarball
-x
extract tarball
-v
enable verbose mode
-r
append more files to an existing tarball
-t
test the tarball or see what files are included in the tarball
-f
specific the name of the tarball in the next argument


## Translate source code written in human-friendly programming language, such as C or C++, into machine-readable binaries

Compilers


## Chunks of compiled code that can be used in programs to accomplish specific common tasks

Libraries


## Enables user to view shared library dependencies for an application

ldd


## Command that automatically looks for makefile when makefile is created

make


## Discovering what package dependencies exist before attempting a deployment and ensuring that the needed dependencies are stored in the repositories

Dependency troubleshooting


## CIA Triad

Confidentiality
Integrity
Availability


## Vertification of individual's activity

Authentication


## Authentication methods

PINs, passwords, and passphrases
Tokens and one-time passwords (OTP)
Biometrics
RADIUS and TACACS+
LDAP
Kerberos


## Linux Kerberos commands

kinit
- authenticates with kerberos, granting user a ticket granting ticket (TGT) is successful
kpassword
- changes the user's Kerberos password
klist
- lists the user's ticket cache
kdestroy
- clears the user's ticket cache


## Occurs when a user is able to obtain additional resources or functionality that they are not normally not allowed access to

Privilege escalation


## Technique for controlling what a process a user can access on a file system by changing the root directory of that process's environment

chroot jail


## Platform independent FDE solution that is commonly used to encrypt storage devices in a Linux environment

Linux Unified Key Setup (LUKS)


## Securely wipe a storage device

shred


## Command used as front-end to LUKS and dm-crypt

cryptsetup


## Networking security

Enable SSL/TLS in web technology
Configure SSH to disable root access
For remote access and receiving other types of network connections from clients, configure the system to deny hosts you don't recognize and whitelist acceptable hosts


## Security that provides identity, authentication, and authorization

Identity and Access Management (IAM)


## List of files used to configure SSH key-based authentication

~/.ssh/ - directory
id_rsa - user's private key
id_rsa.pub - user's public key
authorized keys - file on the remote server that lists public keys that the server accepts
known_hosts - file on the client that lists the public keys that the client accepts
config - file on the client you can use to configure SSH connection settings


## Generate public/private key pair

ssh-keygen


## Append the user's public keys to the remote server's authorized_keys file

ssh-copy-id


## Identifies to the SSH key agent

ssh-add


## Add private key identities to the SSH key agent

ssh-add


## File used to configure an SSH server

/etc/ssh/sshd_config


## System that is composed of certificate authorities, certificates, software, services, and other cryptographic components

Public key infrastructure


## PKI components

Digital signature
Digital certificate
Certificate authority (CA)
Certificate signing request (CSR)


## Open source implementation of SSL/TLS protocol for securing data in transit using cryptography

OpenSSL


## Popular utility for implementing IPSec tunnels for VPN clients

StrongSwan


## Describe multiple types of information about processes and files that are used in combination to make decisions related to access control

Context-based permissions


## Model in which access is controlled by comparing an object's security designation and a subject's (users or other entities) security clearance

Mandatory access control (MAC)


## Default context-based permissions scheme provided with CentOS and Red Hat Enterprise Linux

SELinux


## Defines access parameters for every process and resource on the system

SELinux security policy


## SELinux commands

semanage 
- configure SELinux policies
sestatus 
getenforce 
- display which SELinux mode
setenforce
- change SELinux mode
getsebool
- Display on/off status of SELinux boolean values
setsebool
- change on/off status of SELinux boolean values
ls -Z
- list directory contents
ps -Z
- list running processes
chcon
- change security context of file


## Software program or hardware that protects a system or network from unauthorized access by blocking unwanted traffic

firewall


## Three main generations of firewall

Packet filters (first generation)
Stateful firewall (second generation)
Application-layer firewalls (third generation)


## Linux kernel implementation of network routing functionality

IP forwarding


## Stored collections of IP addresses

IP sets


## Ports in the well-known range (0-1023)

Trusted ports
also known as privileged ports


## Records of system activities and events

System logs


## Log file locations

/var/log/syslog
/var/log/messages
/var/log/auth.log
/var/log/secure
/var/log/kern.log
/var/log/[application]


## Enables you to view and log query files

journalctl


## Display history of user login and logout events

last


## Lists all users and the last time they logged in

lastlog


## Copy data to another logical or physical location of an original data

Backup


## Three main backup types

Full backup
Differential backup
Incremental backup


## Procedure to reduce the amount of bits that are used to represent data

Compression


## Compression utility

gzip
GNU zip


## Compression utility that features file archiving functionality

zip


## Policy that runs objects on  Mandatory Access Control (MAC)

SELinux targeted
