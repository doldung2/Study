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
```

#### List the command and output for yum repolist enabled
```linux
yum repolist enabled
```

#### List the /etc/passwd entries for training(only in master namenode)
```linux
cat /etc/passwd | grep training
```

#### List the /etc/group entries for skcc(only in master namenode)
```linux
cat /etc/group | grep skcc
```

### Instsall a MySQL server

#### User MariaDB as the database for all the services. You may choose your own username and passwords but make a record of it so that we may access them.

#### List the following in your GitHub
#### 1. A command and output that shows the hostname of your database server
```linux
show VARIABLES where Variable_name = 'hostname';
```

#### 2. A command and output that reports the database server version
```linux
status
```

#### 3. A command and output that lists all the databases in the server
```linux
SHOW DATABASES;
```

#### Install Cloudera Manager

##### Install JAVA
```linux
sudo yum install -y java-1.8.0-openjdk-devel
sudo vi /etc/profile

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk/jre
export PATH=$PATH:$JAVA_HOME/bin
```

##### Install MariaDB Server
```linux
yum install mariadb-server
```



