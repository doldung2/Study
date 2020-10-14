호스트1
15.164.84.22
10.0.0.150
호스트2
15.164.84.41
10.0.0.205
호스트3
15.165.102.155
10.0.0.175
호스트4
15.165.125.231
10.0.0.22
호스트5
15.165.136.43
10.0.0.196

adduser training --uid 3800
passwd training

groupadd skcc


usermod -a -G skcc training

chmod +w /etc/sudoers

sudo vi /etc/sudoers

https://docs.cloudera.com/documentation/enterprise/5-15-x/topics/install_cm_mariadb.html#install_cm_mariadb

SHOW VARIABLES WHERE Variable_name = 'hostname';

https://docs.cloudera.com/documentation/enterprise/5-15-x/topics/configure_cm_repo.html#cm_repo
