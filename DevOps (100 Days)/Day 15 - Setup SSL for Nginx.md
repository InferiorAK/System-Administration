## <center> Day 15: Setup SSL for Nginx

```
The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 3 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:


1. Install and configure nginx on App Server 3.

2. On App Server 3 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.

3. Create an index.html file with content Welcome! under Nginx document root.

4. For final testing try to access the App Server 3 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.
```

---

- Login:
    ```apache
    ssh banner@stapp03
    ```
    BigGr33n
- Install nginx:
    ```apache
    yum install nginx
    ```
- Make SSL dir:
    ```apache
    mkdir /etc/nginx/ssl
    ```
- Copy the crt and key:
    ```apache
    cp /tmp/nautilus.crt /etc/nginx/ssl/
    cp /tmp/nautilus.key /etc/nginx/ssl/
    ```
- Now, Edit the config:
    ```apache
    cp nginx.conf nginx.conf.bak
    vi nginx.conf
    ```
    ```nginx
    server {
        listen 443 ssl http2;
        listen [::]:443 ssl http2;
        server_name _;

        root /usr/share/nginx/html;

        ssl_certificate "/etc/nginx/ssl/nautilus.crt";
        ssl_certificate_key "/etc/nginx/ssl/nautilus.key";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout 10m;
        ssl_ciphers PROFILE=SYSTEM;
        ssl_prefer_server_ciphers on;

        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
    ```
- Editing index.html:
    ```apache
    cd /usr/share/nginx/html
    cp index.html index.html.bal
    vi index.html
    ```
    ```html
    <!DOCTYPE html>
    <html>
        <body>
            Welcome!
        </body>
    </html>
    ```
- Before starting Nginx, always verify the config:
    ```apache
    nginx -t
    ```
    ```
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful
    ```
- Now, start and enable nginx:
    ```apache
    systemctl start nginx
    systemctl enable nginx
    systemctl status nginx
    ```
- Verify from jump host:
    ```apache
    ssh thor@jump_host
    curl -Ik https://stapp03
    ```
    mjolnir123