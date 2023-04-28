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
