## 1. Create a CDH Cluster on AWS
### Linux setup
#### Add following linux accounts to all nodes
1. User training with a UID 3800
```linux
adduser training --uid 3800
```
2. Set the password for user "training" to "training"
```linux
passwd training
```
