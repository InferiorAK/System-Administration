## <center> Day 14: Linux Process Troubleshooting

```
The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.


Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don't need to worry if Apache isn't serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 3002 on all app servers.
```

---

- Login:
    ```apache
    ssh tony@stapp01
    ssh steve@stapp02
    ssh banner@stapp03
    ```
    Ir0nM@n  
    Am3ric@  
    BigGr33n
- In CentOS, and other Red Hat-based Linux distributions, the Apache HTTP Server is commonly referred to and managed by the service name `httpd`.
- Check for service:
    ```apache
    systemctl status httpd
    ```

### App Server 1

- Check for error logs:
    ```apache
    journalctl | grep httpd
    journalctl -xe | grep httpd
    ```
    ```
    Aug 21 07:03:20 stapp01.stratos.xfusioncorp.com httpd[795]: (98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:3002
    Aug 21 07:03:20 stapp01.stratos.xfusioncorp.com httpd[795]: no listening sockets available, shutting down
    ```
- Then checking for the port running on what:
    ```apache
    netstat -tulnp | grep 3002
    ```
    ```
    tcp        0      0 127.0.0.1:3002          0.0.0.0:*               LISTEN      770/sendmail: accep
    ```
- Now let's stop the `sendmail` service:
    ```apache
    systemctl status sendmail
    systemctl stop sendmail
    ```
- Now enable, restart and see status of `httpd` again:
    ```apache
    systemctl enable httpd
    systemctl restart httpd
    systemctl status httpd
    ```
- Verify again:
    ```apache
    netstat -tulnp | grep 3002
    ```

### App Server 2

- Enable httpd:
    ```apache
    systemctl enable httpd
    systemctl status httpd
    ```
    Due to no other errors, only doing this is enough.
- Verify port (netstat isn't in this server so I am using curl to check):
    ```apache
    curl -I localhost:3002
    ```
    ```yml
    [REDACTED]
    Server: Apache/2.4.57 (CentOS Stream)
    [REDACTED]
    ```

### App Server 3

This one is will be the same as server 2.
