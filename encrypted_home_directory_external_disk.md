# Encrypted Home-Directory on external Disk

### Prerequisite:
+ Encrypted hard disk with LUKS (empty partition or entire disk)
+ Only one user in /home/ otherwise adjust commands

Install the necessary packages:

```
apt-get install cryptsetup cryptsetup-initramfs 
```
Create temporary directory (can be deleted later):
```
sudo mkdir /mnt/tmp
```
Decrypt the disk under the name encyrpted_sdb1:
```
sudo cryptsetup luksOpen /dev/sdb1 encrypted_sdb1
```

Mount the decrypted disk to /mnt/tmp
```
sudo mount /dev/mapper/encrypted_sdb1 /mnt/tmp
```

Copy your complete home directory to /mnt/tmp
```
rsync -avxH --progress /home/ /mnt/tmp
```
Now delete your complete home directory in /home/*
```
sudo rm -rf /home/* 
```
Now we mount our decrypted disk to /home
```
sudo mount /dev/mapper/encrypted_sdb1 /home
```
Check once if everything has worked:
```
du -sh /home; mount|grep /home
```
Now edit `/etc/fstab` and enter the following at the very bottom:
```
\# /dev/sdb1
/dev/mapper/encrypted_sdb1      /home                 ext4    defaults        0 0
```

Now edit `/etc/crypttab` and enter the following at the very bottom:
```
encrypted_sdb1  /dev/sdb1       none
```

Now reboot once and then you should get a password request at boot time
```
reboot
```
