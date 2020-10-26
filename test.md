## 1. Create a CDH Cluster on AWS
### Linux setup
#### Add following linux accounts to all nodes
1. User training with a UID 3800
```linux
sudo adduser training --uid 3800
```
2. Set the password for user "training" to "training"
```linux
sudo passwd training
```
3. Create the group skcc and add training to it
```linux
sudo groupadd skcc
```
4. Give training sudo capabilities
```linux
sudo usermod -aG skcc training
sudo chmod +w /etc/sudoers
```

#### List the your instances by IP address and DNS name (don't user /etc/hosts for this)
```linux
getent hosts
```

#### List the Linux release you are using
```linux
cat /etc/*release*
```

#### List the file system capacity for the first node (master node)
```linux
df -h
fdisk -l
```
