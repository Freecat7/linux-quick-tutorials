# Nautilius - Custom Disknames

### Prerequisite:
+ Hard Disk or NVME or SSD

First check the UUID (Universally Unique Identifier) of the hard disk whose display name you want to change. 
```
ls -l /dev/disk/by-uuid/
```
This will display the UUIDs and their corresponding device names. Identify the UUID of the desired disk.
Create a new udev rules file.
```
sudo nano /etc/udev/rules.d/99-custom-disks.rules
```
Add the following line to the empty udev rules file:
```
KERNEL=="sd*", ENV{ID_FS_UUID}=="<insert UUID here>", ENV{UDISKS_NAME}="<new display name>"
KERNEL=="nvme*", ENV{ID_FS_UUID}=="<insert UUID here>", ENV{UDISKS_NAME}="<new display name>"
```
Save the file and close the text editor.
Update the udev rules to make the changes take effect.
```
sudo udevadm control --reload-rules
```
