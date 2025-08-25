## <center> Day 9: MariaDB Troubleshooting

```
There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server.



Look into the issue and fix the same.
```

---

- Logins:
    ```apache
    sshpass -p 'Sp!dy' ssh peter@172.16.239.10
    ```
- Check and Try to start mariadb:
    ```apache
    systemctl status mariadb
    systemctl start mariadb
    ```
- Go check for `/var/lib/mysql`.
- In my case it doesn't exist, instead there is `/var/lib/mysqld`.
    ```
    Here's the key point:
    MariaDB by default expects its data directory at /var/lib/mysql, but in your case the data files are under /var/lib/mysqld. That mismatch is why it says:

    Database MariaDB is not initialized, but the directory /var/lib/mysql is not empty


    Actually, /var/lib/mysql doesn't even exist on your system.
    ```

### How to Fix

**Method 01**:

- Point MariaDB to the correct datadir:  
    Edit the config file (usually `/etc/my.cnf` or `/etc/my.cnf.d/server.cnf`):
    ```apache
    [mysqld]
    datadir=/var/lib/mysqld
    socket=/var/lib/mysqld/mysql.sock
    ```
- Restart MariaDB:
    ```apache
    systemctl restart mariadb
    ```

**Method 02**:
```apache
systemctl stop mariadb
mv /var/lib/mysqld /var/lib/mysql
chown -R mysql:mysql /var/lib/mysql
systemctl start mariadb
```