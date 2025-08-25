## <center> Day 18: Configure LAMP server

```
xFusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter. They have already done infrastructure configuration—for example, on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory. Please perform the following steps to accomplish the task:


a. Install httpd, php and its dependencies on all app hosts.

b. Apache should serve on port 5000 within the apps.

c. Install/Configure MariaDB server on DB Server.

d. Create a database named kodekloud_db9 and create a database user named kodekloud_top identified as password 8FmzjvFU6S. Further make sure this newly created user is able to perform all operation on the database you created.

e. Finally you should be able to access the website on LBR link, by clicking on the App button on the top bar. You should see a message like App is able to connect to the database using user kodekloud_top
```

---

### On App Servers

- Login to an App Server:
    ```apache
    ssh tony@stapp01
    ssh steve@stapp02
    ssh banner@stapp03
    ```
    Ir0nM@n  
    Am3ric@  
    BigGr33n
- Install apache, php (On App Server):
    ```apache
    yum install -y httpd php php-mysqlnd
    ```
    <!-- sudo yum install ncurses -> Install terminal clear -->
- Edit the port on config file of httpd:
    ```apache
    vi /etc/httpd/conf/httpd.conf
    ```
    ```
    Listen 5000
    ```
- Enable, Start and Status of the Apache Server:
    ```apache
    sudo systemctl enable --now httpd
    systemctl status httpd
    ```

---

### Oneshot for App Servers

```sh
#!/bin/bash

for server in tony@stapp01 steve@stapp02 banner@stapp03;
    do ssh -t $server "sudo bash -c '
    yum install -y httpd php php-mysqlnd
    sed -i \"s/Listen.*/Listen 5000/\" /etc/httpd/conf/httpd.conf
    sudo systemctl enable --now httpd
    systemctl status httpd'"
done
```

---

### On DB Server

- Login:
    ```apache
    ssh peter@stdb01
    ```
    Sp!dy
- Install MariaDB Server:
    ```apache
    yum install -y mariadb-server
    ```
<!-- - Get mysql user:
    ```apache
    cat /etc/passwd
    ```
    ```
    [REDACTED]
    mysql:x:27:27:MySQL Server:/var/lib/mysql:/sbin/nologin
    ``` -->
- Enable, Start and Status of the MariaDB Server:
    ```apache
    sudo systemctl enable --now mariadb
    systemctl status mariadb
    ```
- Configure MariaDB to allow remote connections:
    ```apache
    vi /etc/my.cnf.d/mariadb-server.cnf
    ```
    Under `[mysqld]`, add/change:
    ```sql
    bind-address=0.0.0.0
    ```
    Then restart MariaDB:
    ```apache
    systemctl restart mariadb
    ```
- Setup database and user:
    ```apache
    sudo mysql
    ```
    ```sql
    CREATE DATABASE kodekloud_db9;
    CREATE USER 'kodekloud_top'@'%' IDENTIFIED BY '8FmzjvFU6S';
    GRANT ALL PRIVILEGES ON kodekloud_db9.* TO 'kodekloud_top'@'%';
    FLUSH PRIVILEGES;
    SHOW DATABASES;
    SELECT User, Host FROM mysql.user;
    EXIT;
    ```

---

### Highlights

| Part                     | With                                   | Without                                 | Effect                                             |
|--------------------------|----------------------------------------|----------------------------------------|--------------------------------------------------|
| **kodekloud_db9.***          | Grants privileges on all tables in the DB | **kodekloud_db9** → usually syntax error  | Must use `.*`                                    |
| **'kodekloud_top'@'%'**      | User can connect from any host         | kodekloud_top → only localhost         | Must use `@'%'` for remote connections          |

---