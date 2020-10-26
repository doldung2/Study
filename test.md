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
sudo user -aG skcc training
sudo chmod +w /etc/sudoers
```
