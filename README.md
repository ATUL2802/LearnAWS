# LearnAWS

#Learn EC2 
==============================================
# SSH into your server, after you have attached the data volume to the instance

# Check if data volume exists, device with name xvdf should exist:
lsblk

# Install file system on data volume
sudo mkfs -t ext4 /dev/xvdf

# Create mount point:
sudo mkdir /data

# Mount the volume to the mount point:
sudo mount /dev/xvdf /data

# Add this line to /etc/fstab, so volume is mounted after reboot:
sudo vi /etc/fstab

/dev/xvdf   /data       ext4    defaults,nofail 0       2  #Copy this whole line and paste to bottom of fstab file, and save

# Test for mount errors:
sudo mount -a

# Check if volume is mounted using df command, /dev/xvdf should show up as mounted on /data:
df -h

# Reboot machine so we can see volume is mounted after reboot:
sudo reboot

# After reboot, SSH again and use df again to verify volume is mounted after reboot as well:
df -h




=================================
# SSH into your EC2 instance and run following commands:

#Stop mysql:
sudo systemctl stop mysql

# Copy mysql db to /data, mount point of data vol, note that file permissions are also copied:
sudo rsync -av /var/lib/mysql /data

# Edit mysql config to change datadir, edit /etc/mysql/mysql.conf.d/mysqld.cnf:
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

datadir=/data/mysql # change datadir to this

# Edit AppArmor access rules, add this line below to /etc/apparmor.d/tunables/alias:
sudo vi /etc/apparmor.d/tunables/alias

alias /var/lib/mysql/ -> /data/mysql/,

# Restart Apparmor:
sudo systemctl restart apparmor

# Restart Mysql:
sudo mkdir /var/lib/mysql/mysql -p # this is to pass env checks
sudo systemctl start mysql
