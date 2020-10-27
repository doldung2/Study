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

#### Install Cloudera Manager

##### 1. Install JAVA
```linux
sudo yum install -y java-1.8.0-openjdk-devel
sudo vi /etc/profile

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk/jre
export PATH=$PATH:$JAVA_HOME/bin
```

##### 2. Install and Configure MariaDB for Cloudera Software
##### 2.1 Install MariaDB Server
```linux
yum install mariadb-server
```
##### 2.2 Configuring and Starting the MariaDB Server
```linux
systemctl stop mariadb
```
##### 2.3 configuration option file (/etc/my.cnf)
```linux
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
transaction-isolation = READ-COMMITTED
# Disabling symbolic-links is recommended to prevent assorted security risks;
# to do so, uncomment this line:
symbolic-links = 0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

key_buffer = 32M
# (deprecated) key_buffer_size = 32M
max_allowed_packet = 32M
thread_stack = 256K
thread_cache_size = 64
query_cache_limit = 8M
query_cache_size = 64M
query_cache_type = 1

max_connections = 550
#expire_logs_days = 10
#max_binlog_size = 100M

#log_bin should be on a disk with enough free space.
#Replace '/var/lib/mysql/mysql_binary_log' with an appropriate path for your
#system and chown the specified folder to the mysql user.
log_bin=/var/lib/mysql/mysql_binary_log

#In later versions of MariaDB, if you enable the binary log and do not set
#a server_id, MariaDB will not start. The server_id must be unique within
#the replicating group.
server_id=1

binlog_format = mixed

read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M

# InnoDB settings
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit  = 2
innodb_log_buffer_size = 64M
innodb_buffer_pool_size = 4G
innodb_thread_concurrency = 8
innodb_flush_method = O_DIRECT
innodb_log_file_size = 512M

[mysqld_safe]
log-error=/var/log/mariadb/mariadb.log
pid-file=/var/run/mariadb/mariadb.pid

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d
```

##### 2.4 Ensure the MariaDB server starts at boot
```linux
systemctl enable mariadb
```

##### 2.5 Start the MariaDB server
```linux
systemctl start mariadb
```

##### 2.6 Run /usr/bin/mysql_secure_installation
```linux
/usr/bin/mysql_secure_installation
```
```linux
[...]
Enter current password for root (enter for none):
OK, successfully used password, moving on...
[...]
Set root password? [Y/n] Y
New password:
Re-enter new password:
[...]
Remove anonymous users? [Y/n] Y
[...]
Disallow root login remotely? [Y/n] N
[...]
Remove test database and access to it [Y/n] Y
[...]
Reload privilege tables now? [Y/n] Y
[...]
All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

##### 2.7 Installing the MySQL JDBC Driver for MariaDB
```linux
wget --no-check-certificate https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.46.tar.gz

tar zxvf mysql-connector-java-5.1.46.tar.gz

sudo mkdir -p /usr/share/java/
cd mysql-connector-java-5.1.46
sudo cp mysql-connector-java-5.1.46-bin.jar /usr/share/java/mysql-connector-java.jar
```

##### 2.8 Creating Databases for Cloudera Software
##### 2.8.1 Login as root & Create Database
```linux
mysql -u root -p
```
```sql
CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE amon DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE rman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE hue DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE metastore DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE sentry DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE nav DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE navms DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE oozie DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;


GRANT ALL ON scm.* TO 'scm'@'%' IDENTIFIED BY 'scm';
GRANT ALL ON amon.* TO 'amon'@'%' IDENTIFIED BY 'amon';
GRANT ALL ON rman.* TO 'rman'@'%' IDENTIFIED BY 'rman';
GRANT ALL ON hue.* TO 'hue'@'%' IDENTIFIED BY 'hue';
GRANT ALL ON metastore.* TO 'hive'@'%' IDENTIFIED BY 'hive';
GRANT ALL ON sentry.* TO 'sentry'@'%' IDENTIFIED BY 'sentry';
GRANT ALL ON nav.* TO 'nav'@'%' IDENTIFIED BY 'nav';
GRANT ALL ON navms.* TO 'navms'@'%' IDENTIFIED BY 'navms';
GRANT ALL ON oozie.* TO 'oozie'@'%' IDENTIFIED BY 'oozie';
```

#### List the following in your GitHub
##### 1. A command and output that shows the hostname of your database server
```linux
show VARIABLES where Variable_name = 'hostname';
```

##### 2. A command and output that reports the database server version
```linux
status
```

##### 3. A command and output that lists all the databases in the server
```linux
SHOW DATABASES;
```




