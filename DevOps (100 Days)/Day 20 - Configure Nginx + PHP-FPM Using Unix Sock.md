## <center> Day 20: Configure Nginx + PHP-FPM Using Unix Sock

```
The Nautilus application development team is planning to launch a new PHP-based application, which they want to deploy on Nautilus infra in Stratos DC. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:


a. Install nginx on app server 1 , configure it to use port 8098 and its document root should be /var/www/html.

b. Install php-fpm version 8.3 on app server 1, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist).

c. Configure php-fpm and nginx to work together.

d. Once configured correctly, you can test the website using curl http://stapp01:8098/index.php command from jump host.

NOTE: We have copied two files, index.php and info.php, under /var/www/html as part of the PHP-based application setup. Please do not modify these files.
```

---

- Login to App Server 1:
    ```apache
    ssh tony@stapp01
    ```
    Ir0nM@n
- Install nginx:
    ```apache
    yum install -y nginx
    ```
- In nginx, Set port **8098** and document root to **/var/www/html**:
    ```apache
    vi /etc/nginx/nginx.conf
    ```
    or,
    ```apache
    vi /etc/nginx/conf.d/app.conf
    ```
    ```nginx
    server {
        listen 8098;
        # server_name stapp01;

        root /var/www/html;
        index index.php index.html;

        location / {
            try_files $uri $uri/ =404;
        }

        location ~ \.php$ {
            include fastcgi_params;
            fastcgi_pass unix:/var/run/php-fpm/default.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        }
    }
    ```
    Save and Exit `:wq`
- Verify and check if there is any issue in configuration:
    ```apache
    nginx -t
    ```
- Enable and Start nginx:
    ```apache
    systemctl enable --now nginx
    systemctl status nginx
    ```

---

<!-- - Install EPEL for CentOS Stream 9:
    ```apache
    dnf install -y epel-release
    ```
    *Install EPEL for CentOS Stream 9*
- Install Remi repo for EL9:
    ```apache
    dnf install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
    ```
    Check the repo is enabled:
    ```apache
    dnf repolist
    ```
- Enable PHP 8.3 module:
    ```apache
    dnf module reset php -y
    dnf module enable php:remi-8.3 -y
    ```
- Install php-fpm for centos 9:
    ```apache
    dnf install -y php-fpm php-cli php-mysqlnd php-common
    ``` -->

---

- Install PHP-FPM 8.3:
    ```apache
    sudo dnf module reset php -y
    sudo dnf module enable php:8.3 -y
    sudo dnf install -y php php-fpm
    ```
- Create socket directory and configure PHP-FPM:
    ```apache
    mkdir -p /var/run/php-fpm
    chown -R nginx:nginx /var/run/php-fpm
    ```
- Edit **/etc/php-fpm.d/www.conf**:
    ```apache
    mv /etc/php-fpm.d/www.conf /etc/php-fpm.d/www.conf~bak
    vi /etc/php-fpm.d/www.conf
    ```
    ```apache
    [www]
    listen = /var/run/php-fpm/default.sock
    listen.owner = nginx
    listen.group = nginx
    listen.mode = 0660

    user = nginx
    group = nginx

    pm = dynamic
    pm.max_children = 5
    pm.start_servers = 2
    pm.min_spare_servers = 1
    pm.max_spare_servers = 3
    ```
- Test PHP-FPM config:
    ```apache
    php-fpm -t
    ```
- Start PHP-FPM:
    ```apache
    systemctl enable --now php-fpm
    systemctl status php-fpm
    ```
- Check that the socket exists:
    ```apache
    ls -l /var/run/php-fpm/default.sock
    ```
- Now run and verify that the server is done:
    ```apache
    curl -Iv http://stapp01:8098/index.php
    curl -Iv http://stapp01:8098/info.php
    ```
