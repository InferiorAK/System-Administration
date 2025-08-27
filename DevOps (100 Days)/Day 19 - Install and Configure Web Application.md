## <center> Day 19: Install and Configure Web Application

```
xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:


a. Install httpd package and dependencies on app server 3.

b. Apache should serve on port 8086.

c. There are two website's backups /home/thor/news and /home/thor/games on jump_host. Set them up on Apache in a way that news should work on the link http://localhost:8086/news/ and games should work on link http://localhost:8086/games/ on the mentioned app server.

d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:8086/news/ and curl http://localhost:8086/games/
```

---

- Login:
    ```apache
    ssh banner@stapp03
    ```
    BigGr33n
- Install httpd:
    ```apache
    yum install -y httpd
    ```
- Change port:
    ```apache
    vi /etc/httpd/conf/httpd.conf
    ```
    ```apache
    Listen 8086
    ```
    Save and exit `:wq`
- Enable, start and status:
    ```apache
    systemctl enable --now httpd
    systemctl status httpd
    ```
- Verify:
    ```apache
    ss -tlnp | grep 8086
    ```
- Lit's test the server:
    ```apache
    echo "This is a Test Server" > /var/www/html/index.html
    curl -I localhost:8086
    ```
    ```yml
    HTTP/1.1 200 OK
    Date: Wed, 27 Aug 2025 17:31:07 GMT
    Server: Apache/2.4.62 (CentOS Stream)
    Last-Modified: Wed, 27 Aug 2025 17:30:59 GMT
    ETag: "16-63d5c26e6f7bc"
    Accept-Ranges: bytes
    Content-Length: 22
    Content-Type: text/html; charset=UTF-8
    ```
<!-- - Now get the files from jump host to app server 3:
    ```apache
    scp thor@jump_host:/home/thor/news /var/www
    scp thor@jump_host:/home/thor/games /var/www
    ```
    mjolnir123 -->
- Now lets get into jump host server and upload the files to app server 3:
    ```apache
    ssh thor@jump_host
    sudo scp -r /home/thor/* banner@stapp03:/tmp
    ```
    mjolnir123
    BigGr33n
- Then exit it, go back to app server 3 again and then:
    ```apache
    cp -r /tmp/games /tmp/news /var/www
    ```
- Now edit a config file `/etc/httpd/conf.d`:
    ```apache
    <VirtualHost *:8086>
        DocumentRoot /var/www

        Alias /news /var/www/news
        Alias /games /var/www/games

        <Directory "/var/www/news">
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
        </Directory>

        <Directory "/var/www/games">
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
        </Directory>

    </VirtualHost>
    ```
- Now restart apache:
    ```apache
    sudo systemctl restart httpd
    ```
- Now Verify:
    ```apache
    curl -iL localhost:8086/games
    ```