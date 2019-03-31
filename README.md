### Учебный стенд для демонтрации репликацци


#### Развернуть 3 машины

    — master
    - slave
    - proxy

```bash
vagrant up
ansible-playbook provisioning/playbook.yml
```

В итоге разворачивается конфигурация master-slave

### Настройка конфигурации master-master

заводим пользователя на  slave для репликации
```bash
mysql-slave> GRANT REPLICATION SLAVE ON *.* TO 'replication'@'%' IDENTIFIED BY 'dummysecret';
```

включаем режим слейв на мастере 
```bash
mysql-master> change master to master_host='slave', master_user='replication', 
master_password='dummysecret', master_port=3306, MASTER_AUTO_POSITION=1;
mysql-master> START SLAVE;
```

проверяем


### Настройка proxysql

устанавливаем
```bash

yum install proxysql
systemctl enable proxysql
```
создаем пользователя для  коннекта с прокси
```mysql-sql
CREATE USER otus identified by 'otusPWD';
GRANT ALL PRIVILEGES on bet.* to otus@'%';

```

настраиваем прокси
```mysql-sql
mysql -u admin -padmin -h 127.0.0.1 -P6032 --prompt='Admin>' --default-auth=mysql_native_password

Admin> INSERT INTO mysql_servers(hostgroup_id,hostname,port, weight) VALUES (0, 'master', 3306, 100);
Admin> INSERT INTO mysql_servers(hostgroup_id,hostname,port, weight) VALUES (0, 'slave', 3306, 1);
Admin> save mysql servers to disk;
Admin> load mysql servers to runtime;
Admin> INSERT INTO mysql_users(username,password,default_hostgroup) VALUES ('otus','otusPWD',0);
Admin> save mysql users to disk;
Admin> load mysql users to runtime;
```
коннектимся
```bash
mysql -h 127.0.0.1 -u otus -potusPWD -P 6033

```


